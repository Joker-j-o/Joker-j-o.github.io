---
layout:     post                    # 使用的布局（不需要改）
title:      Python-DRF-Frame-APIView           # 标题 
subtitle:   Joker #副标题
date:       2020-01-06              # 时间
author:     Joker                     # 作者
header-img: static/img/16.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
categories: [python,django]
---

# APIView?

`APIView是REST framework提供的所有视图的基类，继承自Django的View类`

### APIView 与 View的区别

- request: `传入到视图中的request对象 不再是django原始的httpRequest对象 而是REST framework的request对象`
- response: `视图中可以直接返回 REST framework response对象 响应数据会根据客户端请求头Accpet自动转换为对应的格式进行返回`
- Exception: `任何APIException的子异常都会被DRF框架默认的异常处理机制处理成对应的响应信息返回给客户端`
- other: `认证, 权限, 限流`

# Request对象.

1. `data`: 包含解析之后的`请求体数据`，已经解析为了`字典或类字典`，相当于Django原始`request对象的body、POST、FILES属性。`
2. `query_params`: 包含解析之后的`查询字符串数据`，相当于Django原始`request对象的GET属性`

# Response对象.

> `视图继承APIView之后 返回响应时 可以统一返回 Response对象 原始的响应数据 自动根据请求头 进行修改Accept`

```python
from rest_framework.response import Response

# 返回响应数据
response = Response(<原始响应数据>)
```

~~demo:~~

```python
from rest_framework.views import APIView
from rest_framework.response import Response
from django.http import Http404

# url(r'^goods/$', views.GoodsView.as_view()),
class GoodsView(APIView):
    def get(self, request):
        print(request.data) # post files ...数据
        print(request.query_params) # get 查询字符串的数据
        # 抛出异常
        raise Http404

        return Response({'msg': 'hello'})
```

## Accept请求头:
1. `application/json`: 服务器会将`原始响应数据转换为json数据`进行返回,如果没有指定`Accept`时, 默认返回的就是`json`
2. `text/html`: 服务器会将`原始响应数据转换为html网页`进行返回

# 根据继承APIView 改写的RestAPI

```python
from rest_framework.views import APIView
from rest_framework import status
from django.http import Http404

from booktest.serializers import BookInfoSerializer

# /books/
class BookListView(APIView):
    """
    获取所有图书、增加图书
    """
    def get(self, request):
        """
        获取所有的图书数据
        """
        queryset = BookInfo.objects.all()

        # 序列化所有图书数据
        serializer = BookInfoSerializer(queryset, many=True)

        return Response(serializer.data)

    def post(self, request):
        """
        新增一个图书数据
        """
        # 反序列化-数据校验
        serializer = BookInfoSerializer(data=request.data)
        serializer.is_valid(raise_exception=True)

        # 反序列化-数据保存(save内部会调用序列化器类的create方法)
        serializer.save()

        return Response(serializer.data, status=status.HTTP_201_CREATED)

# /books/(?P<pk>\d+)/
class BookDetailView(APIView):
    """
    获取、修改、删除指定图书
    """
    def get(self, request, pk):
        """
        获取指定图书
        """
        try:
            book = BookInfo.objects.get(pk=pk)
        except BookInfo.DoesNotExist:
            raise Http404

        # 将图书数据进行序列化
        serializer = BookInfoSerializer(book)

        return Response(serializer.data)

    def put(self, request, pk):
        """
        修改指定图书
        """
        try:
            book = BookInfo.objects.get(pk=pk)
        except BookInfo.DoesNotExist:
            raise Http404

        # 反序列化-数据校验
        serializer = BookInfoSerializer(book, data=request.data)
        serializer.is_valid(raise_exception=True)

        # 反序列化-数据保存(save内部会调用序列化器类的update方法)
        serializer.save()

        return Response(serializer.data)

    def delete(self, request, pk):
        """
        删除指定图书：
        """
        try:
            book = BookInfo.objects.get(pk=pk)
        except BookInfo.DoesNotExist:
            raise Http404

        book.delete()

        return Response(status=status.HTTP_204_NO_CONTENT)
``` 

# 总结:
- `APIView`和`View`的区别:
    - request: `Request类对象`
        - request.data: `解析之后的请求体数据(post files ...)`
        - request.query_params: `解析之后的查询字符串数据(get)`
    - response: `Response类对象`
        - 可以统一返回Response对象, 原始响应数据根据客户端请求头 Accept转换为相对应的格式 进行返回
    - Exception: `异常处理机制`
        - 默认异常处理 会将视图中出现的异常 处理成合适响应 返回给客户端
    - other: `其他: 认证/权限/限流`


~~Hey Miss Liu This Rose Is For You~~