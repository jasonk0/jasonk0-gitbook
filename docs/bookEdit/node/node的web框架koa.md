[TOC]

## 简单使用

### 常用node Web框架

![常用nodeweb框架](https://raw.githubusercontent.com/jasonk0/myImage-blog/master/imgs/nodejs.png)

### Koa Web Server

#### 概览

- Express很简洁，Koa更加简洁，是同一个开发团队

- Koa应用程序是一个包含中间件函数的对象，按照堆栈的方式组织执行。

- Koa提供了一个壳子，中间的内容由中间件决定

洋葱模型

Koa里对请求和相应的处理，是一层层递进，再一层层递出的。

![洋葱模型](https://raw.githubusercontent.com/jasonk0/myImage-blog/master/imgs/洋葱模型.png)

#### 常用中间件

![常用中间件](https://raw.githubusercontent.com/jasonk0/myImage-blog/master/imgs/Koa常见中间件.png)

#### koa的框架实战

https://github.com/posquit0/koa-rest-api-boilerplate

### Koa 与 Express 的对比

Koa

- 更优雅的编程体验
- 核心轻量，插件生态庞大
- 内置异步流控制
- 与Epxress生态不兼容，有自己的开发生态
- 入手简单，便于企业级实践

Express

- Nodejs至今最流行框架
- 提供了Web中间件的标准
- 简单快捷可拓展
- 维护成本高，对系统设计能力要求高
- 学习门槛低，入手简单。



## 原理解析

方法论-四个问题

- 问什么要读源码？
- 应该去读那些源码？
- 手写源码应该注意什么？

读源码是一个程序员必须的素养。

首先要看「项目依赖」、「版本」、「目录结构」

![koa的关键依赖](https://raw.githubusercontent.com/jasonk0/myImage-blog/master/imgs/koa的关键依赖.png)

源码架构图

![koa架构](https://raw.githubusercontent.com/jasonk0/myImage-blog/master/imgs/koa的源码架构.png)

### Application





## 常见中间件及原理