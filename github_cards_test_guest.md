## 2024/3/8 00:53:42:
	- #reading #文艺复兴
	  #que 欧洲的历史演变，从封建社会到现代文明
	- #ans
	  文艺复兴大致发生在14世纪末至17世纪初，具体时间范围通常被认为是从14世纪的1300年左右开始，到17世纪的1600年左右结束。这一时期，以意大利为中心，随后影响扩散到欧洲其他地区，标志着从中世纪向现代早期的过渡，是欧洲文化、艺术、政治和科学的重大复兴和发展时期。
	  文艺复兴时期大致从欧洲的14世纪持续到17世纪，其鼎盛期出现在15世纪和16世纪。它始于中世纪晚期的意大利，后来传播到欧洲其他地区。
	  
	  文艺复兴的兴起也得益于君士坦丁堡的陷落，大批艺术家流落到意大利罗马，并得到富商的支持，从事绘画与雕塑，后来这波浪潮遍及欧洲，在此期间贸易得以发展，并成立银行，这是早期资本主义的特征
	  
	  在基督教和封建主义的深刻影响下，欧洲自14世纪至17世纪的文艺复兴期间，重新探索了古希腊罗马的科学、技术与人文精神，这一时期标志了欧洲从中世纪向现代文明过渡的初步尝试。文艺复兴不仅重燃了对古典文化的兴趣，也为后续的科学和社会变革奠定了基础。
	  
	  随后，16世纪的宗教改革挑战了教会的权威，促进了个人主义和自由思想的发展。这一变革不仅破坏了欧洲的宗教统一性，更为个人权利和自由的普及提供了思想基础。
	  
	  1492年哥伦布发现新大陆标志着大航海时代到来，这一时期持续到17世纪末，大航海时代的到来极大地增强了欧洲国家的军事和经济力量，通过促进跨文化交流和科技传播，为现代文明的形成和全球互联互通奠定了坚实的基础。这一时期的探索和殖民活动不仅带来了新的资源和贸易路线，也促进了科学技术的发展和传播。
	  
	  1688年的光荣革命标志着从绝对君主制向君主立宪制的转变，这一事件为个人自由和法治原则奠定了更坚实的基础，并对后续政治思想和民主制度的发展产生了深远影响。
	  
	  光荣革命之后，18世纪的启蒙运动进一步强调了理性、科学和个人自由的重要性，为科学进步和社会变革奠定了基础。同时，17世纪至18世纪的贸易和投资的兴旺不仅催化了资本主义的成长，而且随着法律制度的进一步完善和王权的削弱，保障了资本与民权的发展。这些因素共同推动了18世纪末至19世纪的科学和技术的巨大进步，并引发了工业革命。18世纪末的美国独立战争与之后的繁荣便是现代文明展现巨大威力的一个典型例证。
	  
	  这一波变革浪潮最终触及全球，特别是在19世纪，亚洲的日本和中国等国家开始进行变法图强，推翻君主集权制，借鉴资本主义制度，大力发展科学技术，以建立新国家并增强国力。中国在1911年的辛亥革命后，开始了近代与现代文明的进程。
	  
	  这段历史展示了欧洲从中世纪封建社会到现代文明的独特演变轨迹，以及其对全球历史进程的深远影响。历经一系列关键事件和变革，欧洲不仅塑造了自身的现代化面貌，也促进了全球范围内的科技、政治和社会变革。而中国与其他大部分古文明一样早早从封建社会进入高度集权的帝国时代，虽然发展了完善的官僚体制，但该体制主要是为皇权服务，这束缚了所有国民，导致民间的活力的丧失，无法发展出适合贸易的人权与自由体制，更谈不上科学与技术的发展，因此尽管人口众多军事实力也远远落后于西方，直到今天才大为改善。
- ## 2024/3/1 04:44:08:
	- #test
	  #que 这是一个测试 9
	- #ans
	  test
	  5456
		- sdjlfsjd
	  sjdfsljfsl
- ## 2024/3/1 03:15:15:
  #code
  ```
  function splitStringWithPattern(inputString, pattern_t) {
      // 检查 pattern_t 是否是带有全局标志的正则表达式
      if (!pattern_t.global) {
      console.error("Error: pattern_t must have the global (g) flag.");
      return;
      }
      let match;
      let match_list = [];
      let old_index = 0;
      
      while ((match = pattern_t.exec(inputString)) !== null) {
          match_list.push(inputString.substring(old_index, match.index));
          match_list.push(match[0]);
          // pattern_t.lastIndex 用于记录正则表达式匹配的最后一个字符的位置
          // 这要求pattern_t 是带有 g (global) 标志的正则表达式，否则一直是 0
          old_index = pattern_t.lastIndex;
      }
      if (old_index != 0){
          match_list.push(inputString.substring(old_index));}
      return match_list;
  }
  ```
- ## 2024/3/1 00:27:45:
  #task
	- gemma 模型 ：gemma:7b-instruct-fp16 是什么意思？
	- https://x.com/rasbt/status/1761394305661321255?s=20
	  #que sdfjlsfjw
	- #ans
		- dsljfsljfsd
		- djfsdljfsldfj
			- dlfjlsdfjsdlf
			  sdfjslfjsdlfjsldf
			  
			  sdlfjsldfjsldfjs
	- #sque dlfsjlfjsdfjs
	- #ans
		- djfslfjslfj
		- djslfjsdlfjs
- ## 2024/2/29 09:44:08:
	- #code
	  #que 支持 LaTeX 公式
	- #ans
	  公式使用 `$$...$$` 或者 `$$...$$` 来标识， 然后点击右侧下拉按钮 `latexView` 即可展示
	  
	  Here is a LaTeX equation: $$\int_{-\infty}^{\infty} e^{-x^2} \, dx = \sqrt{\pi}$$
	  
	  $$\int_{-\infty}^{\infty} e^{-x^2} \, dx = \sqrt{\pi}$$  
	  $$\int_{a}^{b} x^2 \,dx = \frac{1}{3}x^3 \Big|_{a}^{b}$$
	  
	  https://chat.openai.com/c/4e717e2d-4714-42ba-a1f1-94ff54499f44
	  让我们解释一下：
		- `(?<= ... )`：这是正向后顾断言的语法。
		- `^|\s`：这是断言的内容。`^` 表示行首，`\s` 表示空白字符（包括空格、制表符、换行符等）。
		- `|`：表示逻辑或操作符。
- ## 2023/7/23 10:33:57:
	- #数据库
	  #que 向量数据库
	- #ans
	  https://twitter.com/dotey/status/1682861683742572546?s=20
	  ertyuio
- ## 2023/7/23 10:34:54:
	- #博客
	  #que Paul Graham 《如何做好的工作》博文
	- #ans
	  http://paulgraham.com/greatwork.html
- ## 2023/7/23 10:38:15:
	- #github #同步
	  #que 当前页面对应的远程仓库文件
	- #ans
	  https://github.com/qyzhizi/MemoFlowGuest/blob/main/github_cards_test_guest.md
- ## 2023/7/23 10:42:58:
	- #code
	  #que hello world Go 代码
	- #ans
	  ```
	  package main
	  
	  import "fmt"
	  
	  func main() {
	      fmt.Println("Hello, World!")
	  }
	  
	  ```
- ## 2023/7/23 10:44:27:
	- #tools
	  #que memoflow 使用方法
	- #ans
	  https://github.com/qyzhizi/MemoFlow/blob/main/docs/usage.md
- ## 2023/7/23 10:45:21:
	- #tools 
	  #que memoflow 仓库
	- #ans
	  https://github.com/qyzhizi/MemoFlow
- ## 2023/8/21 22:59:51:
	- #code
	  #que docker 构建镜像, 并推送到dockerhub
	- #ans
	  dockerfile 安装软件, 并清理安装包, python:3.10-slim-bookworm 是不错的python的, debain 基础镜像.
	  ```
	  FROM python:3.10-slim-bookworm as builder
	  
	  RUN apt-get update --fix-missing && apt-get install -y --fix-missing \
	  build-essential \
	  gcc \
	  g++ && \
	  rm -rf /var/lib/apt/lists/*
	  ```
	  
	  不使用缓存构建镜像
	  ```
	  docker build -t memoflow:v0.1.2 --no-cache .
	  ```
	  给镜像打标签
	  ```
	  docker tag 01c67cc7606d92c846c378cee220eff42b7621355bcd6d90c8524e37095dd2a2 qyzhizi/memoflow:v0.1.2
	  ```
	  推送镜像到dockerhub
	  ```
	  docker push qyzhizi/memoflow:v0.1.2
	  ```
- ## 2023/8/21 23:29:33:
	- #vim #code
	  #que vim 区域选择
	- #ans
	  假设你有一个字符串 `(map (+) ("foo"))` .而光标键在第一个 o 的位置。
	  ```
	  vi" → 会选择 foo.
	  va" → 会选择 "foo".
	  vi) → 会选择 "foo".
	  va) → 会选择("foo").
	  v2i) → 会选择 map (+) ("foo")
	  v2a) → 会选择 (map (+) ("foo"))
	  ```
	  ![](http://qyzhizi.cn/img/202308061414532.png)
- ## 8/21/2023 11:55:33 PM:
  dddd
- ## 8/21/2023 11:55:50 PM:
  ddl
- ## 2023/8/22 00:00:55:
	- #que ddl
- ## 2023/8/27 10:19:32:
	- #que test84783
- ## 2023/9/2 10:04:49:
	- #code
	  #que 查看 docker 容器的所属网络
	- #ans
	  ```
	  docker inspect -f '{{.NetworkSettings.Networks}}' eee7698d23e6574c369f4e7fa039e0a7f94b23173169740d411c7ec6ad984297
	  map[dockerize-chromadb_net:0xc00041a000]
	  ```
- ## 2023/9/12 05:48:31:
	- #code 
	  #que Installing Docker and Docker Compose
	- #ans
	  using Docker's official helper script:
	  curl -L https://get.docker.com | sh
	  To run Docker commands without using sudo
	  ```
	  sudo usermod -aG docker $USER
	  sudo reboot
	  ```
- ## 2023/9/13 07:59:18:
	- #tools
	  #que MobaXterm ssh pem文件 连接?
	- #ans
	  要通过MobaXterm使用SSH密钥（.pem文件）连接到AWS实例，可以按照以下步骤进行操作：
	  
	  1. 启动 MobaXterm 并打开 "Session" 窗口。
	  2. 在 "Session" 窗口中，选择 "SSH" 类型。
	  3. 在 "Remote host" 字段中，输入 AWS 实例的公共 IP 地址或公共 DNS。
	  4. 在 "Specify username" 字段中，输入 SSH 用户名。例如，对于 Amazon Linux 实例，用户名是 "ec2-user"，对于 Ubuntu 实例，用户名是 "ubuntu"。
	  5. 在 "Advanced SSH settings" 部分的 "Use private key" 选项中，选择 "Browse"。
	  6. 在弹出的对话框中，导航到存储了 .pem 密钥文件的位置，并选择该文件。
	  7. 确认其他连接设置，如端口号等。
	  8. 单击 "OK" 开始 SSH 连接。
	  
	  MobaXterm 将使用提供的 SSH 密钥文件进行身份验证，并与 AWS 实例建立安全连接。
	  
	  请注意，确保已将 .pem 密钥文件下载到本地计算机，并且具有适当的权限（例如，只有所有者具有读取权限）。此外，还需要确保 AWS 实例的安全组配置允许通过 SSH 连接。
- ## 2023/9/13 23:36:44:
	- #code
	  #que 记录docker某个容器的内存消耗的脚本
	- #ans
	  ```
	  #!/bin/bash
	  
	  # 容器名称或ID
	  CONTAINER_NAME="836ae4856280"
	  
	  # 输出文件路径
	  OUTPUT_FILE="./output.txt"
	  
	  # 创建输出文件的目录（如果不存在）
	  mkdir -p "$(dirname "$OUTPUT_FILE")"
	  
	  # 循环获取容器内存消耗并写入文件
	  while true; do
	    # 获取容器的内存使用情况
	    MEMORY_USAGE=$(docker stats --no-stream --format "{{.MemUsage}}" $CONTAINER_NAME)
	  
	    # 获取当前时间戳
	    TIMESTAMP=$(date +"%Y-%m-%d %H:%M:%S")
	  
	    # 写入时间戳和内存消耗到文件
	    echo "$TIMESTAMP $MEMORY_USAGE" >> $OUTPUT_FILE
	  
	    # 等待一段时间后继续循环
	    sleep 60
	    done
	  ```
- ## 2024/2/11 02:32:46:
  1234