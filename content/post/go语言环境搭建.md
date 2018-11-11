---
title: "go语言环境搭建"
date: 2018-11-07T10:36:43+08:00
categories:
- 服务端开发
- go语言
tags:
- go
- 环境安装
keywords:
- go环境搭建
#thumbnailImage: //example.com/image.jpg
---

<!--more-->
# go语言开发环境搭建

## go语言环境安装
- 安装go环境（mac上的安装）
  1. 打开官方网页（需要翻墙），我们可以看到，go官方提供了两种形式的安装包，这里我直接选择安装包进行安装。
  2. 一顿傻瓜式的下一步操作之后，go语言环境安装好了，此时可以通过在命令行输入go命令验证一下是否安装成功，我们可以看到window默认的安装目录为C:\Go
- 安装成功之后，我们通过go env查看go环境配置：
```
	set GOARCH=amd64
	set GOBIN=
	set GOCACHE=C:\Users\asus\AppData\Local\go-build
	set GOEXE=.exe
	set GOFLAGS=
	set GOHOSTARCH=amd64
	set GOHOSTOS=windows
	set GOOS=windows
	set GOPATH=C:\Users\asus\go
	set GOPROXY=
	set GORACE=
	set GOROOT=G:\Go
	set GOTMPDIR=
	set GOTOOLDIR=G:\Go\pkg\tool\windows_amd64
	set GCCGO=gccgo
	set CC=gcc
	set CXX=g++
	set CGO_ENABLED=1
	set GOMOD=
	set CGO_CFLAGS=-g -O2
	set CGO_CPPFLAGS=
	set CGO_CXXFLAGS=-g -O2
	set CGO_FFLAGS=-g -O2
	set CGO_LDFLAGS=-g -O2
	set PKG_CONFIG=pkg-config
	set GOGCCFLAGS=-m64 -mthreads -fno-caret-diagnostics -Qunused-arguments -fmessag
	e-length=0 -fdebug-prefix-map=C:\Users\asus\AppData\Local\Temp\go-build936714174
	=/tmp/go-build -gno-record-gcc-switches
```
这里解释一下Go env里面比较重要的几个概念：
	1. GOROOT：GOROOT是go的安装目录，该目录只有一个，这个目录里面包含Go本身提供的官方开发开发库和编译环境以及内置的工具。
	2. GOPATH：GOPATH是go项目的工作目录。go语言开发的一大特点就是遵循统一的配置，简化开发流程。所有项目的源码和第三方库只能放在GOPATH目录下编译和运行（最新的Go1.11版本可以在非GOPATH下编译了，添加了Go MODULE功能）。GOPATH可以有多个，但是每个GOPATH目录下规定有三个子目录：src，bin，pkg。
		* src是存放源码的目录，项目源码和第三方库的代码均放在Src下面。
		* bin是通过 go install 命令安装后生成可执行产物的目录，go项目生成最终产物一般是可执行的二进制流，在不同平台上有不同的格式（window上是exe文件,linux上是可执行的文件）。但是都可以直接运行。
		* pkg是生成中间编译产物的目录，非main包下的go文件生成的产物都放在pkg目录下，中间产物可以直接参与最终可执行文件的生成。
	3. GOBIN：存放可执行文件的目录的绝对路径
