## 2026/1/31 16:02:44:
你这个函数**正是导致 409 的根因**，而且这是一个**GitHub Contents API 的经典误用点**。

先给结论，再给你**可直接替换的正确版本**。

---

## 一、为什么你现在“创建的是文件而不是目录”

你现在的代码是：

```ts
PUT /contents/{folderPath}
```

```json
{
  "message": "Create folder xxx",
  "content": ""
}
```

👉 **GitHub 的语义是：**

> 在 `folderPath` 这个路径上，创建一个「文件」

⚠️ GitHub **根本不存在“创建目录”的 API**
目录只是「路径前缀的抽象」

所以你实际上创建了一个文件：

```
vault/MyVault/Folder_0   ← 文件 ❌
```

然后你再尝试写：

```
vault/MyVault/Folder_0/File_0.md
```

就必然触发：

```
409: a file exists where you’re trying to create a subdirectory
```

---

## 二、正确的思路（必须牢记）

> ✅ **创建一个“目录” = 在该目录下创建一个文件**

行业标准做法是用占位文件：

* `.gitkeep`
* `.keep`
* `_init.md`

---

## 三、修改后的正确版本（直接可用）

### ✅ 推荐版本：`.gitkeep`（最通用）

```ts
async createEmptyFloderPathInRepo(
    accessToken: string,
    githubUserName: string,
    repoName: string,
    folderPath: string
) {
    const normalizedFolderPath = folderPath
        .replace(/\\/g, '/')
        .replace(/\/+/g, '/')
        .replace(/^\/|\/$/g, '');

    const placeholderFilePath = `${normalizedFolderPath}/.gitkeep`;

    const res = await fetch(
        `https://api.github.com/repos/${githubUserName}/${repoName}/contents/${placeholderFilePath}`,
        {
            method: 'PUT',
            headers: {
                Authorization: `Bearer ${accessToken}`,
                'User-Agent': 'Hono-Worker',
                'Content-Type': 'application/json',
            },
            body: JSON.stringify({
                message: `Init folder ${normalizedFolderPath}`,
                content: Buffer.from('').toString('base64'),
            }),
        }
    );

    // 201 = created, 200 = updated（幂等）
    if (!res.ok && res.status !== 200) {
        throw new Error(await res.text());
    }
}
```

---

## 四、这个版本解决了什么问题

### ✅ 1. 永远不会创建“同名文件”

```txt
Folder_0/.gitkeep  ✔
Folder_0           ✖
```

---

### ✅ 2. 幂等（多次调用不会炸）

* 已存在 → GitHub 返回 200
* 不存在 → GitHub 返回 201

---

### ✅ 3. 路径安全（防 422）

* 去掉 `//`
* 去掉首尾 `/`
* 禁止 `\`

---

## 五、⚠️ 非常重要的使用约定（强烈建议你遵守）

### ❌ 永远不要再做：

```ts
PUT /contents/{folderPath}
```

### ✅ 永远只做：

```ts
PUT /contents/{folderPath}/.gitkeep
```

---

## 六、一句话总结（记住这一条就够）

> **GitHub 没有“目录”，只有“文件路径前缀”**
> 👉 想要目录，就在目录里创建一个文件

---

如果你愿意，我可以帮你下一步把整个流程升级成：

* ✅ 自动检测路径是否被文件占用
* ✅ 自动修复脏 repo（可选）
* ✅ 并发安全（不会多个 DO 同时写炸）

只要说一句「继续」。
