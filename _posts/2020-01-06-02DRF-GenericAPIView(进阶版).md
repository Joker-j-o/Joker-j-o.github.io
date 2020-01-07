---
layout:     post                    # 使用的布局（不需要改）
title:      Python-DRF-Frame-GenericAPIView           # 标题 
subtitle:   Joker #副标题
date:       2020-01-06              # 时间
author:     Joker                     # 作者
header-img: static/img/16.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
categories: [DRF-serializer]
---

# GenericAPIView介绍

`GenericAPIView`继承自`APIView`，在`APIView功能基础上`，主要`增加了操作序列化器`和`数据库查询的属性和方法`。

> 注：`GenericAPIView`为之后要学的`Mixin扩展类`的执行`提供`方法支持，通常在使用时，可`搭配一个或多个Mixin扩展类`。

# 提供的关于序列化器使用的属性与方法

   - 属性:
      - `serializer_class`: 指明视图使用的 `序列化器`
   - 方法:
      - `get_serializer_class(self)`: 返回序列化器类 默认返回`serializer_class` 可以重写
      - `get_serializer(self,args,**kwargs)`: 返回创建序列化器类的 对象 如果我们在视图中 想要创建 序列化对象 可直接调用该方法

`使用方法`

```python
from booktest.serializers import BookInfoSerializer

class BookListView(GenericAPIView):
    serializer_class = BookInfoSerializer # 指明使用的是哪个序列化器类
```

# 提供的关于数据库查询的属性与方法

   - 属性:
      - `queryset`: 指明 视图使用的 查询集
   - 方法:
      - `get_queryset(self)`: 返回视图使用的查询集 默认返回`queryset`属性,可以重写
      - `get_object(self)`: 返回从视图使用的查询集中查询指定的对象(默认根据url地址中提取的pk进行查询)，如查询不到，此方法会抛出Http404异常。 

`使用方法`

```python
  from booktest.models import BookInfo

  class BookListView(GenericAPIView):
    queryset = BookInfo.objects.all() # 指明 视图中使用的 查询集
```

# 其他可以设置的属性

   - `pagination_class`：指明分页控制类
   - `filter_backends`：指明过滤控制后端


`使用GenericAPIView进一步改写APIView`

```python
from rest_framework.generics import GenericAPIView
from rest_framework import status
from rest_framework.response import Response

from booktest.serializers import BookInfoSerializer
from booktest.models import BookInfo

# /books/
class BookListView(GenericAPIView):
    """
    获取所有图书、增加图书
    """
    # 指定视图所使用的序列化器类
    serializer_class = BookInfoSerializer
    # 指定视图所使用的查询集
    queryset = BookInfo.objects.all()

    def get(self, request):
        """
        获取所有的图书数据
        """
        queryset = self.get_queryset()

        # 序列化所有图书数据
        serializer = self.get_serializer(queryset, many=True)

        return Response(serializer.data)

    def post(self, request):
        """
        新增一个图书数据
        """
        # 反序列化-数据校验
        serializer = self.get_serializer(data=request.data)
        serializer.is_valid(raise_exception=True)

        # 反序列化-数据保存(save内部会调用序列化器类的create方法)
        serializer.save()

        return Response(serializer.data, status=status.HTTP_201_CREATED)

# /books/(?P<pk>\d+)/
class BookDetailView(GenericAPIView):
    """
    获取、修改、删除指定图书
    """
    # 指定视图所使用的序列化器类
    serializer_class = BookInfoSerializer
    # 指定视图所使用的查询集
    queryset = BookInfo.objects.all()

    def get(self, request, pk):
        """
        获取指定图书
        """
        instance = self.get_object()

        # 将图书数据进行序列化
        serializer = BookInfoSerializer(instance)

        return Response(serializer.data)

    def put(self, request, pk):
        """
        修改指定图书
        """
        instance = self.get_object()

        # 反序列化-数据校验
        serializer = BookInfoSerializer(instance, data=request.data)
        serializer.is_valid(raise_exception=True)

        # 反序列化-数据保存(save内部会调用序列化器类的update方法)
        serializer.save()

        return Response(serializer.data)

    def delete(self, request, pk):
        """
        删除指定图书：
        """
        instance = self.get_object()
        instance.delete()
        return Response(status=status.HTTP_204_NO_CONTENT)
```

# 与 APIView的区别

### 类的区别
   - 添加了2个类属性，`serializer_class` `queryset` 分别指定`序列化器类`与`查询结果集`

```python
class BookListView(GenericAPIView):
    """
    获取所有图书、增加图书
    这两个类属性 就是 GenericAPIView特有的
    """
    # 指定视图所使用的序列化器类
    serializer_class = BookInfoSerializer
    # 指定视图所使用的查询集
    queryset = BookInfo.objects.all()
```

### get(获取所有数据)方法的变化

   - `使用get_queryset来获取所有数据而不是使用BookInfo.objects.all`

```python
# 序列化所有图书数据
serializer = self.get_serializer(queryset, many=True)
```

### post方法的变化

   - `使用get_serializer创建序列化器对象`

```python
    # 反序列化-数据校验
    serializer = self.get_serializer(data=request.data)
```

### get(获取一个数据)方法的变化

   - `使用get_object方法查询一个对象，且自带异常判断`

```python
    instance = self.get_object()

    # 将图书数据进行序列化
    serializer = BookInfoSerializer(instance)
```

### put方法的变化

   - `使用get_object方法，且这个方法自带异常判断`

```python
    instance = self.get_object()

    # 反序列化-数据校验
    serializer = BookInfoSerializer(instance, data=request.data)
```

### delete方法的变化

   - `使用get_object方法，且这个方法自带异常判断`

```python
    instance = self.get_object()
    instance.delete()
    return Response(status=status.HTTP_204_NO_CONTENT)
```

# 总结:
   - `GenericAPIView` & `APIView` 区别:
      - 继承自 `APIView`
      - 封装了操作 序列化器的 属性和方法
         - 属性: `serializer_class`
         - 方法: `get_serializer_class` & `get_serializer`
      - 封装了 数据库操作的 属性和方法
         - 属性: `queryset`
         - 方法: `get_queryset` & `get_object`
      - 其他: 过滤 & 分页

`每个版本不一样 建议都试一下`

# [Mixin扩展类进阶版](https://joker-j-o.github.io/drf-serializer/2020/01/06/DRF-Mixin.html)


~~Hey Miss Liu This Rose Is For You~~