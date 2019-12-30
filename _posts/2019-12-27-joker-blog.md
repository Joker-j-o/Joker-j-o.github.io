---
layout:     post                    # 使用的布局（不需要改）
title:      MVC & MVT           # 标题 
subtitle:   Joker #副标题
date:       2019-12-30              # 时间
author:     Joker                     # 作者
header-img: static/img/16.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
categories: [python,django]
---

# MVC
> 有一种程序设计模式叫MVC，其核心思想是分工、解耦，让不同的代码块之间降低耦合，增强代码的可扩展性和可移植性，实现向后兼容。
![mvc工作流程]('../static/img/_posts/mvc.png')
- `M` 全拼为 `Model` 主要封装对数据库层的访问, 对数据库中的 数据进行`增 删 改 查` 操作
- `V` 全拼为 `View` 用于封装结果, 生成页面展示的 html内容
- `C` 全拼为 `Controller` 用于接收请求,处理业务逻辑,与`Model`和`View`交互, 返回结果

# MVT
![mvt工作流程]('../static/img/_posts/mvt.png')
> mvc & mvt的差别就是黑色箭头部分
- `M` 全拼为 `Model` 跟MVC中的M功能一样 主要用于 负责和数据库进行交互 进行数据处理
- `V` 全拼为 `View` 跟MVC中的C功能一样 主要用于 接收请求 进行业务处理 返回应答
- `T` 全拼为 `Template` 跟MVC中的V功能相同 负责封装构造要返回的html


~~Hey Miss Liu This Rose Is For You~~