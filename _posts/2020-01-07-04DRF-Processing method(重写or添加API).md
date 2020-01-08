---
layout:     post                    # 使用的布局（不需要改）
title:      Python-DRF-Frame-Processing method(处理APIv1.6)           # 标题 
subtitle:   Joker #副标题
date:       2020-01-07              # 时间
author:     Joker                     # 作者
header-img: static/img/16.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
categories: [DRF-serializer]
---

# Processing method

`在视图集中 除了action处理方法(API)之外，还可以添加额外的处理方法(API)。`

### `For example`

   - 获取ID最新的一本图书的数据
   - 修改指定图书的阅读量(只修改阅读量)

```python
from rest_framework import mixins
from rest_framework.viewsets import GenericViewSet
from rest_framework.decorators import action

class BookInfoViewSet(ModelViewSet):
    queryset = BookInfo.objects.all()
    serializer_class = BookInfoSerializer

    def latest(self, request):
        """
        返回id最新的图书信息
        """
        book = BookInfo.objects.latest('id')
        serializer = self.get_serializer(book)
        return Response(serializer.data)

    def read(self, request, pk):
        """
        修改指定图书的阅读量
        """
        book = self.get_object()
        book.bread = request.data.get('read')
        book.save()
        serializer = self.get_serializer(book)
        return Response(serializer.data)
```

### `urls配置`

```python
urlpatterns = [
    url(r'^books/$', views.BookInfoViewSet.as_view({
        'get':'list',
        'post': 'create'
    })),
    url(r'^books/(?P<pk>\d+)/$', views.BookInfoViewSet.as_view({
        'get': 'retrieve',
        'put': 'update',
        'delete': 'destroy'
    })),
    url(r'^books/latest/$', views.BookInfoViewSet.as_view({
        'get': 'latest'
    })),
    url(r'^books/(?P<pk>\d+)/read/$', views.BookInfoViewSet.as_view({
        'put': 'read'
    }))
]

```

> 注意: `as_view({'key':'value'})` 根据`key值` 找到`相对应`的`请求方法`



~~Hey Miss Liu This Rose Is For You~~