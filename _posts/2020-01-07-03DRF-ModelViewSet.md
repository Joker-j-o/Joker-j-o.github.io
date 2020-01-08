---
layout:     post                    # 使用的布局（不需要改）
title:      Python-DRF-Frame-ModelViewSet(高配视图集v1.5)           # 标题 
subtitle:   Joker #副标题
date:       2020-01-07              # 时间
author:     Joker                     # 作者
header-img: static/img/16.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
categories: [DRF-serializer]
---

# `ModelViewSet 改写 Rest API`

特点: `继承自GenericViewSet`，同时包括了`ListModelMixin`、`RetrieveModelMixin`、`CreateModelMixin`、`UpdateModelMixin`、`DestroyModelMixin。`

```python

from rest_framework import mixins
from rest_framework.viewsets import ModelViewSet

class BookInfoViewSet(ModelViewSet):
    queryset = BookInfo.objects.all()
    serializer_class = BookInfoSerializer
```

## `urls配置`

```python

from booktest import views

urlpatterns = [
    url(r'^books/$', views.BookInfoViewSet.as_view({
        'get':'list',
        'post': 'create'
    })),
    url(r'^books/(?P<pk>\d+)/$', views.BookInfoViewSet.as_view({
        'get': 'retrieve',
        'put': 'update',
        'delete': 'destroy'
    }))
]
```

> 注意: `as_view({'key':'value'})` 根据`key值` 找到`相对应`的`请求方法`

# `ReadOnlyModelViewSet`

> 继承自`GenericViewSet`，同时包括了`ListModelMixin`、`RetrieveModelMixin`。



~~Hey Miss Liu This Rose Is For You~~