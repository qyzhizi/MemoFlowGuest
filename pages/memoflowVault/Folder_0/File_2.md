## 2026/2/9 22:55:01:
```
/**
 * SQLite 仓储层
 * 负责处理所有 SQLite 数据库操作
 */

import { NotFoundError } from "@/types/error";
import type {
    TitleIndex,
    ArticleContent,
    InsertTitleIndexParams,
    InsertArticleContentParams,
    UpdateArticleContentParams,
    InsertResult,
} from "./types";

import { v4 as uuidv4 } from "uuid";

export class SqliteRepository {
    private sql: any;
    private maxArticlesToStore: number;
    private static readonly MAX_ENTRIES = 30000;

    constructor(sql: any, maxArticlesToStore: number = 1000) {
        this.sql = sql;
        this.maxArticlesToStore = maxArticlesToStore;
    }

    /**
     * 辅助方法：将 SQLite Cursor 对象转换为可序列化的数组
     * Cloudflare Durable Object 的 SQLite 返回 Cursor 对象，需要转换为标准数组
     * 
     * 目前无法通过配置让 Durable Object SQLite 直接返回数组，
     * 只能通过这个辅助函数进行转换
     */
    private convertCursorToArray(result: any): any[] {
        try {
            if (result && typeof result === 'object' && 'columnNames' in result) {
                // 这是一个 Cursor 对象，转换为数组
                const converted = Array.from(result);
                return converted;
            } else if (Array.isArray(result)) {
                // 已经是数组，直接返回
                console.log(`Already an array, length: ${result.length}`);
                return result;
            } else {
                console.log(`Unexpected result type: ${typeof result}, value:`, result);
                return [];
            }
        } catch (error) {
            console.error('Error in convertCursorToArray:', error);
            return [];
        }
    }

    /**
     * 初始化数据库表结构
     */
    initializeTables(): void {
        // 创建计数表以维护各表的记录数（O(1) 查询复杂度）
        this.sql.exec(`
            CREATE TABLE IF NOT EXISTS tableCounters (
                tableName TEXT PRIMARY KEY,
                count INTEGER DEFAULT 0,
                lastUpdated DATETIME DEFAULT CURRENT_TIMESTAMP
            );
        `);

        // 初始化计数表数据
        this.sql.exec(`
            INSERT OR IGNORE INTO tableCounters (tableName, count)
            VALUES ('titleIndex', 0), ('articleContent', 0);
        `);

        // 创建标题索引表，添加 last_access 字段用于 LRU 缓存
        this.sql.exec(`
            CREATE TABLE IF NOT EXISTS titleIndex (
                id TEXT PRIMARY KEY,
                title TEXT NOT NULL,
                hashOfTitle TEXT NOT NULL UNIQUE,
                remoteArticlePath TEXT NOT NULL,
                createdAt DATETIME DEFAULT CURRENT_TIMESTAMP,
                last_access INTEGER DEFAULT (cast(strftime('%s','now') as int))
            );
        `);

        // 为 hashOfTitle 创建索引以加快查询速度
        this.sql.exec(`
            CREATE INDEX IF NOT EXISTS idx_hashOfTitle ON titleIndex(hashOfTitle);
        `);

        // 为 (last_access, id) 创建索引以支持 LRU 缓存淘汰, (last_access, id)是覆盖索引, 这样子查询完全不回表
        this.sql.exec(`
            CREATE INDEX IF NOT EXISTS idx_last_access_id ON titleIndex (last_access, id);
        `);

        // 创建文章内容表
        this.sql.exec(`
            CREATE TABLE IF NOT EXISTS articleContent (
                id TEXT PRIMARY KEY,
                title TEXT NOT NULL,
                content TEXT NOT NULL,
                createdAt DATETIME DEFAULT CURRENT_TIMESTAMP
            );
        `);

        // 为 title 创建索引以便快速查询
        this.sql.exec(`
            CREATE INDEX IF NOT EXISTS idx_title ON articleContent(title);
        `);
    }

    /**
     * 私有方法：增加表计数
     */
    private incrementTableCount(tableName: string, amount: number = 1): void {
        try {
            console.log(`incrementTableCount: tableName=${tableName}, amount=${amount}`);
            this.sql.exec(
                `UPDATE tableCounters SET count = count + ?, lastUpdated = CURRENT_TIMESTAMP WHERE tableName = ?`,
                amount,
                tableName
            );
            console.log(`incrementTableCount成功: tableName=${tableName}`);
        } catch (error) {
            console.error(`incrementTableCount失败: tableName=${tableName}, amount=${amount}, error=`, error);
            throw error;
        }
    }

    /**
     * 私有方法：减少表计数
     */
    private decrementTableCount(tableName: string, amount: number = 1): void {
        this.sql.exec(
            `UPDATE tableCounters SET count = MAX(0, count - ?), lastUpdated = CURRENT_TIMESTAMP WHERE tableName = ?`,
            amount,
            tableName
        );
    }

    /**
     * 私有方法：获取表计数（O(1) 时间复杂度）
     */
    private getTableCount(tableName: string): number {
        const result = this.sql.exec(
            `
            SELECT count FROM tableCounters
            WHERE tableName = ?
            `,
            tableName
        );

        const resultArray = this.convertCursorToArray(result);
        return resultArray[0]?.count || 0;
    }

    // ===================== titleIndex 表操作方法 =====================

    /**
     * 私有方法：更新记录的 last_access 时间
     */
    private updateLastAccess(id: string): void {
        this.sql.exec(
            `
            UPDATE titleIndex
            SET last_access = cast(strftime('%s','now') as int)
            WHERE id = ?
            `,
            id
        );
    }

    /**
     * 私有方法：LRU 缓存淘汰
     * 当 titleIndex 表中的记录数超过 MAX_ENTRIES 时，删除最久未访问的记录
     */
    private async evictIfNeeded(): Promise<void> {
        const totalCount = this.getTableCount('titleIndex');

        if (totalCount >= SqliteRepository.MAX_ENTRIES) {
            const toDelete = totalCount - SqliteRepository.MAX_ENTRIES + 1;

            // 删除最久未访问的记录
            this.sql.exec(
                `
                DELETE FROM titleIndex
                WHERE id IN (
                    SELECT id FROM titleIndex
                    ORDER BY last_access ASC
                    LIMIT ?
                )`,
                toDelete
            );

            // 更新计数表
            this.decrementTableCount('titleIndex', toDelete);

            console.log(
                `已淘汰 ${toDelete} 条最久未访问的记录以保持在 ${SqliteRepository.MAX_ENTRIES} 条的限制内`
            );
        }
    }

    /**
     * 向 titleIndex 表中插入一条新的标题记录
     * 同时检查是否超过 MAX_ENTRIES 限制，如果超过则通过 LRU 淘汰最久未访问的记录
     */
    async insertTitleIndex(params: InsertTitleIndexParams): Promise<InsertResult> {
        const { title, hashOfTitle, remoteArticlePath } = params;
        const id = uuidv4();

        this.sql.exec(
            `
            INSERT INTO titleIndex (id, title, hashOfTitle, remoteArticlePath)
            VALUES (?, ?, ?, ?)
            `,
            id, title, hashOfTitle, remoteArticlePath
        );

        // 更新计数表
        this.incrementTableCount('titleIndex', 1);

        // 检查是否超过 LRU 缓存限制
        await this.evictIfNeeded();

        return { id };
    }

    /**
     * 根据 hashOfTitle 查询标题记录
     * 访问时更新 last_access 时间（LRU 缓存机制）
     */
    async queryTitleIndexByHash(hashOfTitle: string): Promise<TitleIndex | null> {
        const result = this.sql.exec(
            `
            SELECT id, title, hashOfTitle, remoteArticlePath, createdAt, last_access
            FROM titleIndex
            WHERE hashOfTitle = ?
            LIMIT 1
            `,
            hashOfTitle
        );

        const resultArray = this.convertCursorToArray(result);

        if (resultArray.length > 0) {
            const record = resultArray[0] as TitleIndex;
            // 访问即更新 LRU
            this.updateLastAccess(record.id);
            return record;
        }

        return null;
    }

    /**
     * 根据 ID 查询标题记录
     * 访问时更新 last_access 时间（LRU 缓存机制）
     */
    async queryTitleIndexById(id: string): Promise<TitleIndex | null> {
        const result = this.sql.exec(
            `
            SELECT id, title, hashOfTitle, remoteArticlePath, createdAt, last_access
            FROM titleIndex
            WHERE id = ?
            LIMIT 1
            `,
            id
        );

        const resultArray = this.convertCursorToArray(result);

        if (resultArray.length > 0) {
            const record = resultArray[0] as TitleIndex;
            // 访问即更新 LRU
            this.updateLastAccess(id);
            return record;
        }

        return null;
    }

    /**
     * 获取所有标题记录（按创建时间倒序）
     */
    async getAllTitleIndex(): Promise<TitleIndex[]> {
        const result = this.sql.exec(`
            SELECT id, title, hashOfTitle, remoteArticlePath, createdAt, last_access
            FROM titleIndex
            ORDER BY createdAt DESC
        `);

        return this.convertCursorToArray(result) as TitleIndex[];
    }

    /**
     * 获取标题记录数量（O(1) 时间复杂度）
     */
    async getTitleIndexCount(): Promise<number> {
        return this.getTableCount('titleIndex');
    }

    // ===================== articleContent 表操作方法 =====================

    /**
     * 向 articleContent 表中插入一条新的文章内容记录
     * 同时检查是否超过 MAX_ARTICLES_TO_STORE 限制，如果超过则删除最旧的文章
     */
    async insertArticleContent(
        params: InsertArticleContentParams
    ): Promise<InsertResult> {
        const { title, content } = params;
        const id = uuidv4();
        
        // 参数验证
        if (!id || typeof id !== 'string') {
            throw new Error(`Invalid id: ${id}`);
        }
        if (title === undefined || title === null) {
            throw new Error(`Invalid title: ${title}`);
        }
        if (content === undefined || content === null) {
            throw new Error(`Invalid content: ${content}`);
        }
        
        // 插入新文章
        this.sql.exec(
            `INSERT INTO articleContent (id, title, content)
            VALUES (?, ?, ?)`,
            id,
            title,
            content
        );
        console.log(`Inserting article content with id=${id}, title="${title}", content length=${content.length}`);

        // 更新计数表
        try {
            this.incrementTableCount('articleContent', 1);
        } catch (error) {
            console.error(`计数表更新失败:`, error);
            throw error;
        }

        // 检查是否超过存储限制
        await this.enforceArticleCountLimit();

        return { id };
    }

    /**
     * 私有方法：保证 articleContent 表中最多只存储 MAX_ARTICLES_TO_STORE 篇文章
     * 超过限制时，删除最旧的文章
     */
    private async enforceArticleCountLimit(): Promise<void> {
        const totalCount = this.getTableCount('articleContent');

        if (totalCount > this.maxArticlesToStore) {
            const toDelete = totalCount - this.maxArticlesToStore;

            // 删除最旧的文章
            this.sql.exec(
                `
                DELETE FROM articleContent
                WHERE id IN (
                    SELECT id FROM articleContent
                    ORDER BY createdAt ASC
                    LIMIT ?
                )
            `,
                toDelete
            );

            // 更新计数表
            this.decrementTableCount('articleContent', toDelete);

            console.log(
                `已删除 ${toDelete} 篇旧文章以保持在 ${this.maxArticlesToStore} 篇的限制内`
            );
        }
    }

    /**
     * 根据 title 查询文章内容
     */
    async queryArticleContentByTitle(
        title: string
    ): Promise<ArticleContent | null> {
        const result = this.sql.exec(
            `
            SELECT id, title, content, createdAt
            FROM articleContent
            WHERE title = ?
            LIMIT 1
            `,
            title
        );

        const resultArray = this.convertCursorToArray(result);
        return resultArray.length > 0 ? (resultArray[0] as ArticleContent) : null;
    }

    /**
     * 根据 ID 查询文章内容
     */
    async queryArticleContentById(id: string): Promise<ArticleContent | null> {
        const result = this.sql.exec(
            `
            SELECT id, title, content, createdAt
            FROM articleContent
            WHERE id = ?
            LIMIT 1
            `,
            id
        );

        const resultArray = this.convertCursorToArray(result);
        return resultArray.length > 0 ? (resultArray[0] as ArticleContent) : null;
    }

    /**
     * 获取所有文章内容（按创建时间倒序）
     */
    async getAllArticleContent(): Promise<ArticleContent[]> {
        const result = this.sql.exec(`
            SELECT id, title, content, createdAt
            FROM articleContent
            ORDER BY createdAt DESC
        `);

        return this.convertCursorToArray(result) as ArticleContent[];
    }

    /**
     * 获取特定标题的所有文章内容（按创建时间倒序）
     */
    async getAllArticleContentByTitle(
        title: string
    ): Promise<ArticleContent[]> {
        const result = this.sql.exec(
            `
            SELECT id, title, content, createdAt
            FROM articleContent
            WHERE title = ?
            ORDER BY createdAt DESC
            `,
            title
        );

        return this.convertCursorToArray(result) as ArticleContent[];
    }

    /**
     * 获取文章内容数量（O(1) 时间复杂度）
     */
    async getArticleContentCount(): Promise<number> {
        return this.getTableCount('articleContent');
    }

    /**
     * 更新文章内容
     */
    async updateArticleContent(params: UpdateArticleContentParams): Promise<void> {
        const { id, content } = params;

        const result = this.sql.exec(
            `
            UPDATE articleContent
            SET content = ?
            WHERE id = ?
            `,
            content,
            id
        );

        // 检查是否有行被更新（使用 changes 属性）
        if (result.changes === 0) {
            throw new NotFoundError(`Article content with id=${id} not found`);
        }
    }

    // ===================== 级联删除操作 =====================

    /**
     * 删除标题记录
     * 不再自动删除关联的文章内容
     */
    async deleteTitleIndex(idOfTitleIndex: string): Promise<void> {
        const result = this.sql.exec(
            `
            DELETE FROM titleIndex
            WHERE id = ?
            `,
            idOfTitleIndex
        );

        // 检查是否有行被删除（使用 changes 属性）
        if (result.changes === 0) {
            throw new NotFoundError(
                `Title index with id=${idOfTitleIndex} not found`
            );
        }

        // 更新计数表
        this.decrementTableCount('titleIndex', 1);
    }

    /**
     * 删除单个文章内容
     */
    async deleteArticleContent(id: string): Promise<void> {
        const result = this.sql.exec(
            `
            DELETE FROM articleContent
            WHERE id = ?
            `,
            id
        );

        if (result.changes === 0) {
            throw new NotFoundError(`Article content with id=${id} not found`);
        }

        // 更新计数表
        this.decrementTableCount('articleContent', 1);
    }

    /**
     * 删除特定标题的所有文章内容
     */
    async deleteAllArticlesByTitle(title: string): Promise<void> {
        const result = this.sql.exec(
            `
            DELETE FROM articleContent
            WHERE title = ?
            `,
            title
        );

        // 更新计数表（使用 changes 属性获取实际删除的行数）
        if (result.changes > 0) {
            this.decrementTableCount('articleContent', result.changes);
        }

        console.log(
            `已删除标题 "${title}" 的 ${result.changes} 篇文章内容`
        );
    }

    // ===================== 调试和数据库状态查询方法 =====================

    /**
     * 查询数据库中所有表的信息
     */
    debugGetAllTables(): any {
        try {
            const result: any = {
                section: "数据库表信息",
                success: true,
                data: {}
            };
            
            // 查询所有用户表名（过滤掉 Cloudflare 系统表）
            const tables = this.sql.exec(`
                SELECT name, type 
                FROM sqlite_master 
                WHERE type='table' 
                  AND name NOT LIKE 'sqlite_%' 
                  AND name NOT LIKE '_cf_%'
                ORDER BY name
            `);
            
            const tablesArray = this.convertCursorToArray(tables);
            // console.log("tablesArray: ", JSON.stringify(tablesArray, null, 2));
            // console.log("item type: ", typeof tablesArray[0]);
            result.data.tables = tablesArray;
            
            // 查询每个表的记录数（不查询表结构，因为 PRAGMA 命令在 Cloudflare Durable Object 中受限）
            const tableDetails: any[] = [];
            tablesArray.forEach((table: any) => {
                // console.log(`Processing table: ${table.name}`);
                try {
                    const count = this.sql.exec(`SELECT COUNT(*) as count FROM ${table.name}`);
                    // get column names
                    const columns = this.sql.exec(`PRAGMA table_info(${table.name})`);
                    const columnsArray = this.convertCursorToArray(columns);
                    const columnNames = columnsArray.map((col: any) => col.name);

                    const countArray = this.convertCursorToArray(count);

                    tableDetails.push({
                        name: table.name,
                        type: table.type,
                        recordCount: countArray[0]?.count || 0,
                        columns: columnNames,
                    });
                } catch (error) {
                    console.error(`Error getting count for table ${table.name}:`, error);
                    tableDetails.push({
                        name: table.name,
                        type: table.type,
                        recordCount: 0,
                        error: error instanceof Error ? error.message : String(error),
                        note: "可能是 Cloudflare 系统保护的表"
                    });
                }
            });
            
            result.data.tableDetails = tableDetails;
            // console.log("tableDetails: ", JSON.stringify(tableDetails, null, 2));
            
            // 查询所有索引
            const indexes = this.sql.exec(`
                SELECT name, tbl_name, sql 
                FROM sqlite_master 
                WHERE type='index' AND name NOT LIKE 'sqlite_%'
                ORDER BY tbl_name, name
            `);
            
            const indexesArray = this.convertCursorToArray(indexes);
            // console.log("indexesArray: ", JSON.stringify(indexesArray, null, 2));
            result.data.indexes = indexesArray;

            // console.log("result: ", JSON.stringify(result, null, 2));

            return result;
            
        } catch (error) {
            console.error("Error in debugGetAllTables: ", error);
            return {
                section: "数据库表信息",
                success: false,
                error: error instanceof Error ? error.message : String(error)
            };
        }
    }

    /**
     * 查询表计数器信息
     */
    debugGetTableCounters(): any {
        try {
            const result: any = {
                section: "表计数器信息",
                success: true,
                data: {}
            };
            
            const counters = this.sql.exec(`
                SELECT tableName, count, lastUpdated 
                FROM tableCounters 
                ORDER BY tableName
            `);
            
            const countersArray = this.convertCursorToArray(counters);
            result.data.counters = countersArray;
            
            // 验证计数准确性
            const validationResults: any[] = [];
            countersArray.forEach((counter: any) => {
                if (counter.tableName === 'titleIndex') {
                    const actualCount = this.sql.exec(`SELECT COUNT(*) as count FROM titleIndex`);
                    const actualCountArray = this.convertCursorToArray(actualCount);
                    
                    validationResults.push({
                        tableName: 'titleIndex',
                        counterValue: counter.count,
                        actualCount: actualCountArray[0]?.count,
                        isValid: counter.count === actualCountArray[0]?.count
                    });
                } else if (counter.tableName === 'articleContent') {
                    const actualCount = this.sql.exec(`SELECT COUNT(*) as count FROM articleContent`);
                    const actualCountArray = this.convertCursorToArray(actualCount);
                    
                    validationResults.push({
                        tableName: 'articleContent',
                        counterValue: counter.count,
                        actualCount: actualCountArray[0]?.count,
                        isValid: counter.count === actualCountArray[0]?.count
                    });
                }
            });
            
            result.data.validation = validationResults;
            
            return result;
            
        } catch (error) {
            return {
                section: "表计数器信息",
                success: false,
                error: error instanceof Error ? error.message : String(error)
            };
        }
    }

    /**
     * 查询最近访问的记录（LRU 缓存调试）
     */
    debugGetRecentAccess(): any {
        try {
            const result: any = {
                section: "LRU 缓存状态",
                success: true,
                data: {}
            };
            
            const recentAccess = this.sql.exec(`
                SELECT id, title, last_access, 
                       datetime(last_access, 'unixepoch') as last_access_time,
                       createdAt
                FROM titleIndex 
                ORDER BY last_access DESC 
                LIMIT 10
            `);
            
            const recentAccessArray = this.convertCursorToArray(recentAccess);
            result.data.recentAccess = recentAccessArray;
            
            const oldestAccess = this.sql.exec(`
                SELECT id, title, last_access,
                       datetime(last_access, 'unixepoch') as last_access_time,
                       createdAt
                FROM titleIndex 
                ORDER BY last_access ASC 
                LIMIT 5
            `);
            
            const oldestAccessArray = this.convertCursorToArray(oldestAccess);
            result.data.oldestAccess = oldestAccessArray;
            
            return result;
            
        } catch (error) {
            return {
                section: "LRU 缓存状态",
                success: false,
                error: error instanceof Error ? error.message : String(error)
            };
        }
    }

    /**
     * 完整的数据库状态调试信息
     */
    getDODBStatus(): any {
        const debugResult = {
            timestamp: new Date().toISOString(),
            sections: {} as any
        };

        // 收集所有调试信息
        debugResult.sections.tables = this.debugGetAllTables();
        debugResult.sections.counters = this.debugGetTableCounters();
        debugResult.sections.lruCache = this.debugGetRecentAccess();
        
        // 使用 JSON.stringify 来确保完整的序列化输出，避免 [Object] 显示
        

        return debugResult;
    }    

    // 清空所有数据表的内容，保留表结构和索引结构
    resetTables(): any {
        try {
            const result: any = {
                section: "重置所有表",
                success: true,
                data: {}
            };

            // 删除所有表的数据内容，保留表结构和索引
            this.sql.exec(`DELETE FROM titleIndex`);
            this.sql.exec(`DELETE FROM articleContent`);
            
            // 重新初始化计数器
            this.sql.exec(`DELETE FROM tableCounters`);
            this.sql.exec(`
                INSERT INTO tableCounters (tableName, count)
                VALUES ('titleIndex', 0), ('articleContent', 0)
            `);

            result.data.deletedTables = ['titleIndex', 'articleContent'];
            result.data.preservedStructures = [
                '表结构完全保留',
                '索引结构完全保留：idx_hashOfTitle, idx_last_access_id, idx_title',
                '计数器已重置为0'
            ];

            return result;

        } catch (error) {
            return {
                section: "重置所有表",
                success: false,
                error: error instanceof Error ? error.message : String(error)
            };
        }
    }
}

```