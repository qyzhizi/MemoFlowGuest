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
