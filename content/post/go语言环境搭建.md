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
### 安装go环境（mac上的安装）
  1. 打开官方网页（需要翻墙），go官方提供了两种形式的安装包，可以直接选择安装包进行安装。
  2. 安装包下载成功后，一直下一步操作之后，go语言环境安装好了，此时可以通过在命令行输入go命令验证一下是否安装成功，我们可以看到window默认的安装目录为C:\Go
### 安装成功之后，我们通过go env查看go环境配置：
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
### 这里解释一下Go env里面比较重要的几个概念：
1. GOROOT：GOROOT是go的安装目录，该目录只有一个，这个目录里面包含Go本身提供的官方开发开发库和编译环境以及内置的工具。
2. GOPATH：GOPATH是go项目的工作目录。go语言开发的一大特点就是遵循统一的配置，简化开发流程。**go1.11**版本之前，通常情况下项目的源码和第三方库只能放在GOPATH目录下编译和运行，目前最新的**go1.11**版本为了解决项目自由存放的问题，添加了go module子模块功能，可以把项目源码放在任何目录，运行go build,go run等命令时会去自动找相关依赖库完成编译和运行。GOPATH可以有多个，但是每个GOPATH目录下规定有三个子目录：src，bin，pkg。
	* src是存放源码的目录，项目源码和第三方库的代码均放在src下面。
	* bin是存放在src下的项目通过 go instal 命令生成可执行产物的目录，go项目生成最终产物一般是可执行的二进制流，在不同平台上有不同的格式（window上是exe文件,linux上是可执行的文件）。
	* pkg是生成中间编译产物的目录，非main包下的go文件生成的产物都放在pkg目录下，中间产物可以直接参与最终可执行文件的生成。
3. GOBIN：存放可执行文件的目录的绝对路径。如果该目录没有设置，在非GOPATH里src下的项目里执行go install时会报找到不到GOBIN路径错误。如果该目录设置了，在非GOPATH下的项目中执行go install命令，会把生成的项目可执行产物放在GOBIN目录下。

### 下面我们讲一下如何编写Helloworld:
1. 我们还是按照1.11之前的规则，在GOPATH的src目录下新建learnProject的项目。
2. 在helloworld项目下新建文件main.go,在main.go中输入如下内容：
	    package main
    	
    	import "fmt"
    	
    	func main() {
    		fmt.Print("hello world")
    	}
3. 在helloworld项目所在目录执行 go run helloworld会打印出内容：“hello world”，我们也可以运行go build helloworld命令，会在当前目录下看到helloworld.exe文件，这是go在window下生成的可执行文件。继续执行go install helloworld，可以在当前项目的GOPATH目录下的bin目录中发现helloworld.exe文件，如果该bin目录加入了系统环境中，则在任何目录下都可以执行helloworld.exe文件输出“hello world”。