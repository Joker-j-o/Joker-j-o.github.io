---
layout:     post                    # 使用的布局（不需要改）
title:      Python-DRF-Frame-GenericViewSet(中配视图集v1.4)           # 标题 
subtitle:   Joker #副标题
date:       2020-01-07              # 时间
author:     Joker                     # 作者
header-img: static/img/16.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
categories: [DRF-serializer]
---

# `使用GenericViewSet 改写 Rest API`

```python

from rest_framework import mixins
from rest_framework.viewsets import GenericViewSet

class BookInfoViewSet(mixins.ListModelMixin,
                      mixins.CreateModelMixin,
                      mixins.RetrieveModelMixin,
                      mixins.UpdateModelMixin,
                      mixins.DestroyModelMixin,
                      GenericViewSet):
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

`每个版本不一样 建议都试一下`

# [ModelViewSet-v1.6](https://joker-j-o.github.io/drf-serializer/2020/01/07/03DRF-ModelViewSet.html)



~~Hey Miss Liu This Rose Is For You~~