---
layout:     post                    # 使用的布局（不需要改）
title:      Django-Rest-Web           # 标题 
subtitle:   Joker #副标题
date:       2020-01-01              # 时间
author:     Joker                     # 作者
header-img: static/img/16.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
categories: [python,django]
---
# Web development model
## **The front and rear ends do not separate**
![前后端不分离](/static/img/web-1.png)
> 注意:前端看到的效果都是由后端控制的，后端渲染页面之后给前端返回完整的页面，前端与后端的耦合度很高。

## **Fore and aft end separation**
![前后端分离](/static/img/web-2.png)
> 1. 后端仅返回前端所需的数据，至于数据怎么进行展示，由前端自己进行控制，前端与后端的耦合度很低。
  2. API: 在前后端分离开发模式中，我们通常将后端开发的每个视图都称为一个接口或者API。

## **Development mode comparison** 
- 前后端不分离: 完整的html页面都是在后端生成的, 后端给前端返回一个完整的页面, 前端只是进行展示
- 前后端分离: 完整的html界面都是在前端生成, 后端只是给前端数据, 前端将数据渲染在页面上

开发模式|优点|排行
--|:--:|--:
前后端不分离|利于SEO(搜索引擎优化)|仅适用于纯网页的应用
前后端分离|可以针对不同客户端|不利于SEO(搜索引擎优化)

> SEO: 搜索引擎优化: 是针对引擎的一种排名优化手段,对对应的关键字 进行提高搜索排名

- 前后端不分离：前端看到的效果是由后端控制的，后端模板渲染返回给客户端完整的页面。
- 前后端分离：后台只返回前端所需的数据，至于数据怎么展示，由前端自己进行控制。
- 前后端不分离适合于纯网页的应用，前后端分离可以适合于不同的客户端。

# What is the Rest?




~~Hey Miss Liu This Rose Is For You~~