---
title: "基于gin框架的goweb开发简介"
date: 2018-11-21T21:48:35+08:00
categories:
- 服务器开发
- go语言
tags:
- go web开发
- web服务开发
keywords:
- gin
- go web开发
#thumbnailImage: //example.com/image.jpg
---

<!--more-->
# 简介
前面讲了基于go语言搭建一个简单的web服务，让大家可以看到go原生api起一个web服务是非常方便的。当然，那个项目仅仅停留在学习和实验中，真实的项目的需求是复杂的，我们要考虑的东西很多，这个时候仅仅使用原生的api是很难满足企业级的需求的。所以我们需要使用社区开源的成熟的框架来帮助我们满足基本的项目需求开发。今天我就给大家讲一下，在go语言中，如何用web框架gin + orm框架gorm + 配置管理框架viper + 日志框架logrus + mysql搭建一个简单的企业级的web api服务。
## 项目引用第三方库介绍
### [gin](https://github.com/gin-gonic/gin "gin")
gin是用go写的高性能web框架，官方形容为martini-like API提供者。如果你开发的项目有高性能的指标要求，那么它将是你的首选。gin提供丰富http请求方法，高效灵活的路由配置功能和动态配置的中间件处理以及比较全面的用法示例。
### [gorm](http://gorm.io/ "gorm")
gorm是go开发的高性能的orm库，提供丰富的功能。功能如下：全功能 ORM (无限接近),关联 (Has One, Has Many, Belongs To, Many To Many, 多态),钩子 (在创建/保存/更新/删除/查找之前或之后)，预加载，事务，复合主键，SQL 生成器，数据库自动迁移，自定义日志，可扩展性, 可基于 GORM 回调编写插件，所有功能都被测试覆盖，开发者友好。
## [viper](https://github.com/spf13/viper "viper")
viper是功能强大的配置管理库。支持多种格式，多种来源的配置管理。支持配置的动态改变监听。
## [logrus](https://github.com/sirupsen/logrus)
Logrus是Go的结构化日志记录器（golang），与标准库日志记录器完全兼容。

## 项目启动
### 需求描述
我们简单模拟创建一个用户管理服务，用户管理服务提供对用户的增删改查功能。
### 项目开发阶段
1. 在goland下创建user-manager项目
![image](/go_language03/create_user_manager_project.png)
2. 创建项目目录结构
![image](/go_language03/user_manager_project_struct.png)
  #### 项目结构说明：
  - config 配置启动目录，在该目录中进行环境配置，log日志配置，数据库配置。
  - handle web路由的处理入口目录，gin的所有路由配置的handle函数均定义在该目录中。
  - locallog 本地开发的日志存放目录。
  - models 数据模型目录，数据库实体模型，数据库引擎初始化，数据实体dao均在该目录进行。
  - router 路由配置目录，路由相关的配置均在该目录进行。
  - service 实现服务业务逻辑的目录，业务的调用在该目录进行。
  - utils 纯项目无关的工具类的存放目录。
  - config.json 项目配置文件，viper从该文件读取本地配置。
  - main.go 项目的启动入口文件。定义项目的启动流程。
  
### 关键模块源码：
1.项目目录下的启动入口文件main.go，类似java的带有启动main函数的class文件：

    package main
    //项目依赖引入
    import (
    	"github.com/thinmonkey/user-manager/config"
    	"github.com/gin-gonic/gin"
    	"net/http"
    	"github.com/thinmonkey/user-manager/router"
    	"github.com/thinmonkey/user-manager/utils/log"
    )
    
    func main() {//程序入口文件
    
    	config.Init()//执行配置初始化逻辑
    
    	// 创建  Gin engine.
    	gin := gin.New()
    
    	// 进行路由模块配置
    	router.Load(
    		// Cores.
    		gin,
    		// Middlwares.
    	)
    	
		//启动Gin http服务器，监听在localhost:8088,打印启动错误日志
    	log.Info(http.ListenAndServe(":8088", gin).Error())
    }
    
2.http路由配置文件router.go，在此文件中配置程序提供的http请求的路由和处理handler。

    package router
    
    import (
    	"github.com/gin-gonic/gin"
    	"net/http"
    	"github.com/thinmonkey/user-manager/router/middlewares"
    	"github.com/thinmonkey/user-manager/handle"
    )
    
    // 配置gin框架的middlewares（中间件，统一处理请求和响应）, routes（服务路径配置）, handlers（路由的处理函数）
    func Load(router *gin.Engine, mw ...gin.HandlerFunc) *gin.Engine {
    	// 配置Middlewares。
    	router.Use(gin.Recovery())
    	router.Use(middlewares.LoggerMiddlerware())
    	router.Use(mw...)
    	// 配置找不到路由时返回404的处理Handler.
    	router.NoRoute(func(c *gin.Context) {
    		c.String(http.StatusNotFound, "The incorrect API route.")
    	})
    	// 配置健康检查的处理handler
    	router.GET("/healthcheck", func(context *gin.Context) {
    		context.JSON(http.StatusOK, gin.H{"code": 200})
    	})
    	//配置路由组apiV1
    	apiV1 := router.Group("/api/v1/")
    	{
    		//按照rest风格配置匹配各个路由的handler
    		apiV1.GET("user/:userId", handle.GetUser)
    		apiV1.DELETE("user/:userId",handle.DeleteUser)
    		apiV1.PUT("user/:userId", handle.UpdateUser)
    		apiV1.POST("user", handle.CreateUser)
    	}
    	//返回路由对象
    	return router
    }

3.http用户模块的handler文件user_handler.go
 
    package handle
    
    import "github.com/gin-gonic/gin"
    
    func CreateUser(c *gin.Context) {
    	
    }
    
    func UpdateUser(c *gin.Context) {
    
    }
    
    func DeleteUser(c *gin.Context) {
    
    }
    
    func GetUser(c *gin.Context) {
    
    }


## 总结
本文从实际出发，以应用的角度讲述了一个基于gin框架的goweb项目总体结构和各个模块的功能，并且提供了示例项目源码[user-manager](https://github.com/thinmonkey/user-manager)。在如今后端盛行的微服务架构中，以前的集中式大项目基本上不存在了，现在的服务都是由一个个小的应用和服务组成的，所以每个项目的功能和代码并不会太复杂，一个高性能的web框架加上一个支持动态化的配置工具基本上能满足基本的业务需求。其他的功能就是通过基本的orm框架完成数据增删改查，基本的日志框架实现日志打印功能，方便问题定位和排查。再复杂的需求可能就是加redis实现分布式缓存和rpc实现服务间的调用功能，这些框架go语言都有开源的，大家可以自己去查看。

