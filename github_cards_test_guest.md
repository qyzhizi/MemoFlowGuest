## 2023/7/23 10:33:57:
	- #数据库
	  #que 向量数据库
	- #ans
	  https://twitter.com/dotey/status/1682861683742572546?s=20
		- 456789
			- dljflsja
		- sjdflsjfls
- ## 2023/7/23 10:34:54:
	- #博客
	  #que Paul Graham 《如何做好的工作》
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