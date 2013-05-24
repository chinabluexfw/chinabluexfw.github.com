---
layout : post
category : lessons
tags :  play
title :  Play Framework java or scala

---


1 MVC模型

Play应用遵循Web架构使用的MVC架构模式。

它将应用分离到不同的层中：表现层(Presentation)和模型层(Model)。表现层进一步分为视图(View)和控制器(Controller)。

Model 是应用所处理信息的领域表述(Domain-Specific Representation)。绝大多数应用使用持久化机制如数据库存储数据。但是MVC并没有特别提到数据访问层，因为它属于下层，由模型封装。
View 将模型渲染成适合交互的表单，通常是用户界面。一个模型可有多个不同目的视图。Web应用中，View通常以HTML，XML或JSON形式呈现，也可能是二进制的Chart。
Controller处理事件（通常是用户动作），并对模型做相应改变。Web应用中，事件通常是HTTP请求：Controller监听HTTP请求，从其中提取数据，如查询字符串参数，请求头，然后改变下层模型对象。


Play将此三层分在app目录下的不同package中。

app/controllers

一个Controller就是一个Java 类，它的静态公共方法则是动作(Action)。动作是接收HTTP请求后的Java处理入口点。Controller类实际是面向过程的，非OO。Action从HTTP请求中提取数据，读或更新Model对象，然后返回一个包装成HTTP响应(HTTP Response)的结果。

app/models

Model是一组具有所有OO特性的Java类。包含数据结构和应用可使用的操作。（译注：即充血模型）。支持通过JPA持久化。

app/views

应用的视图由Play的模板系统生成。Controller从Model获取数据，然后使用模板呈现它。此package包含HTML,XML等模板文件，用作动态生成模型的表述(Representation)。

	

2 请求生命周期

Play是完全无状态的(stateless)，且仅面向请求-应答(Request-Response)。所有请求遵循相同路径：

    1. 框架收到一个HTTP请求
	2. Router匹配请求和Controller、Action，执行动作方法。
	3. 应用代码执行
	4. 绘制模型，呈现视图
	5. 动作方法的结构作为HTTP响应返回。



3 应用程序布局
    app目录
包含所有可执行的工件：Java源代码和视图模板。其下有三个标准package,每个代表MVC的一层。你也能添加你自己的包,如示例的utils包。
View pacakge可以在分子packages:

    * tags 存储应用的ta。如可重用的模板片段。
	* 一个Controller一个view目录，按惯例，每个Controller的相关模板存储在自己的子目录中。

.class文件在哪儿？
Play在运行时编译Java源代码，并将编译的类缓存在tmp\bytecode目录下。Play应用的可执行工件时.java源文件，不是已编译的.class（译注：包括jar文件）。
public目录
存储静态的、由Web服务器直接处理的资源。分为三个子目录：images,stylesheets和javascripts，分别放图片，css和js。
conf目录
包含应用的所有配置文件。两个必须文件为：

	* application.conf： 应用主配置文件，包含所有标准的配置选项。
	* routes：url路由规则定义文件。

此目录包含在Java ClassPath中。
lib目录
存放应用依赖的标准Java类库。此目录自动添加到Java classpath中。


	
4 开发生命周期

使用Play开发时没有编译，打包和部署这些阶段，代之两个不同环境：用于开发阶段的DEV模式和用户部署阶段的PROD模式。
Java源代码在运行时编译和加载。如果Java源文件在应用运行时发生改变，代码会重新编译并热加载(hot-swapped)到JVM中。模板文件也是如此。
关于DEV/PROD模式
通过application.mode配置属性切换DEV或PROD。DEV模式下，Play会检查并在必要时热加载。PROD模式为产品做了优化:Java源文件和模板仅编译一次。

5 调试

开发模式下通过JDPA连接到8000端口远程调试。

	
	

