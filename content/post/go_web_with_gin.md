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
我们这里简单模拟一个用户管理服务，用户管理服务提供对用户的增删改查功能。
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
  - service 真正的实现服务业务逻辑的目录，业务的调用在该目录进行。
  - utils 纯项目无关的工具类的存放目录。
  - config.json 项目配置文件，viper从该文件读取本地配置。
  - main.go 项目的启动入口文件。定义项目的启动流程。
3. main.go入口文件：
