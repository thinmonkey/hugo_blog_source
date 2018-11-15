---
title: "goweb项目开发环境搭建"
date: 2018-11-15T10:36:43+08:00
categories:
- 服务端开发
- go语言
tags:
- go
- 环境安装
keywords:
- goweb开发环境搭建
#thumbnailImage: //example.com/image.jpg
---

# 前言
今年是go语言开源的第十个年头，作为21世纪的新语言，go已经在很多领域有了一席之地，特别是随着近几年云计算的兴起，基于go开发的各种开源产品在社区引起了开发者的广泛关注，可以说目前go是云计算的第一语言。当然，除了云基础设施领域，随着微服务的兴起，go语言在web服务开发方面也有众多的追随者。今天我们讲一下如何基于go语言搭建一个简单的web服务。

## 选择开发工具
go语言本身对外部环境依赖比较少，所以实际上，任何开发编辑器加上go插件都可以用来快速的开发go项目。因为工作中习惯了各种idea的ide，刚好他们出了专门开发go的ide叫Goland,所以这里我选择使用Goland开发go项目。

## go项目的创建
1. 打开Goland，选择New Project新建项目。
![image](/go_language02/goweb_new_project.png)
2. 选择项目存放目录，输入项目名称和选择go sdk。
![image](/go_language02/create_goweb.png)
3. 创建完了项目之后，可以看到开发面板上有一个空目录goweb_learn，这是项目的go目录。
![image](/go_language02/goland_word_plane.png)
4. 创建项目入口文件main.go,编辑go服务器代码。
![image](/go_language02/create_main.go.png)

```
package main

import (
  "net/http"
  "fmt"
)

func main() {
  //注册一个处理http请求的handler
  http.HandleFunc("/", handler)
  //启动http服务监听addr ip:port
  http.ListenAndServe("localhost:8000", nil)
}

// handler echoes the Path component of the request URL r.
func handler(w http.ResponseWriter, r *http.Request) {
  fmt.Fprintf(w, "URL.Path = %q\n:helloworld", r.URL.Path)
}
```

5. 创建idea运行配置,创建完了之后，点击运行项目，此时，本机已经起了一个go服务，监听在localhost:8000端口。
![image](/go_language02/goweb_create_run_config.png)
6. 打开浏览器，输入localhost:8000,可以看到页面上输出了内容。
![image](/go_language02/goweb_show_http.png)
