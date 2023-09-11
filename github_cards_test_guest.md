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
- ## 2023/9/6 18:16:58:
	- #que lzp 454754654765786545465
- ## 2023/9/5 16:42:38:
	- #que lzp 454754654765786987
- ## 2023/9/5 16:41:46:
	- #que lzp 45476567567
- ## 2023/9/5 16:40:38:
	- #que lzp 5657686789
- ## 2023/9/5 16:22:24:
	- #que lzp 36945-6954-7
- ## 2023/9/5 16:20:38:
	- #que lzp 36945-6954-7
- ## 2023/9/5 02:16:18:
	- #que lzp 32-43-045905
	  #sque lzp 85347853485
- ## 2023/9/5 02:15:17:
	- #que lzp 32-43-045905
- ## 2023/9/5 02:14:37:
	- #que lzw 345340346546
- ## 2023/9/5 02:14:05:
	- #que lzw 345340
- ## 2023/9/5 00:59:02:
	- #que lzw 5793457934593
- ## 2023/9/5 00:11:25:
	- #que lzw 3045830583406
- ## 2023/9/5 00:02:37:
	- #que lzw 345834058340
- ## 2023/9/2 10:04:49:
	- #code
	  #que 查看 docker 容器的所属网络
	- #ans
	  ```
	  docker inspect -f '{{.NetworkSettings.Networks}}' eee7698d23e6574c369f4e7fa039e0a7f94b23173169740d411c7ec6ad984297
	  map[dockerize-chromadb_net:0xc00041a000]
	  ```
- ## 2023/8/27 10:19:32:
	- #que test84783
- ## 2023/8/22 00:00:55:
	- #que ddl
- ## 8/21/2023 11:55:50 PM:
  ddl
- ## 8/21/2023 11:55:33 PM:
  dddd
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
- ## 2023/7/23 15:21:08:
  浮空
- ## 2023/7/23 11:17:36:
  test 1234
- ## 2023/7/23 10:45:21:
	- #tools 
	  #que memoflow 仓库
	- #ans
	  https://github.com/qyzhizi/MemoFlow
- ## 2023/7/23 10:44:27:
	- #tools
	  #que memoflow 使用方法
	- #ans
	  https://github.com/qyzhizi/MemoFlow/blob/main/docs/usage.md
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
- ## 2023/7/23 10:38:15:
	- #github #同步
	  #que 当前页面对应的远程仓库文件
	- #ans
	  https://github.com/qyzhizi/MemoFlowGuest/blob/main/github_cards_test_guest.md
- ## 2023/7/23 10:36:16:
  - TODO  编辑、删除卡片笔记，导出当前同步文件（mardown 格式）
- ## 2023/7/23 10:34:54:
	- #博客
	  #que Paul Graham 《如何做好的工作》
	- #ans
	  http://paulgraham.com/greatwork.html
- ## 2023/7/23 10:33:57:
	- #数据库
	  #que 向量数据库
	- #ans
	  https://twitter.com/dotey/status/1682861683742572546?s=20
- ## 2023/7/23 10:32:33:
  hello world !
