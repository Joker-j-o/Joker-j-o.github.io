---
layout:     post                    # 使用的布局（不需要改）
title:      Python-DRF-Frame-ModelSerializer           # 标题 
subtitle:   Joker #副标题
date:       2020-01-03              # 时间
author:     Joker                     # 作者
header-img: static/img/16.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
categories: [DRF-serializer]
---

# ModelSerializer?

> 如果序列化器类对应的是`Django的某个模型类`，则定义序列化器类时，可以`直接继承于ModelSerializer`
> `ModelSerializer是Serializer类`的子类，相对于`Serializer`，提供了以下功能

- 基于模型类字段`自动生成序列化器类`的`字段`
- 包含`默认的create()`和`update()`方法的实现

# 一） 基本使用

```python
class BookInfoSerializer(serializers.ModelSerializer):
    """图书数据序列化器"""
    class Meta:
        model = BookInfo # 数据模型类
        fields = '__all__' # __all__ 表示所有字段
```

- `model`: `指明序列化器类对应的模型类`
- `fields`: `指明依据模型类的哪些字段生成序列化器类的字段`

# 二） 指明字段

> 1）使用fields来指明依据模型类的哪些字段生成序列化器类的字段，__all__表明包含所有字段，也可以指明具体哪些字段

```python
class BookInfoSerializer(serializers.ModelSerializer):
    """图书数据序列化器"""
    class Meta:
        model = BookInfo # 指明根据 哪个数据模型类
        fields = ('id', 'btitle', 'bpub_date', 'bread', 'bcomment') # 指明这些字段
```

> 2）使用exclude可以指明排除哪些字段

```python
class BookInfoSerializer(serializers.ModelSerializer):
    """图书数据序列化器"""
    class Meta:
        model = BookInfo
        exclude = ('is_delete',) # 排除当前的is_delete字段 其余的都要
```

> 3）可以使用extra_kwargs参数为自动生成的序列化器类字段添加或修改原有的选项参数

```python
class BookInfoSerializer(serializers.ModelSerializer):
    """图书数据序列化器"""
    class Meta:
        model = BookInfo
        fields = ('id', 'btitle', 'bpub_date', 'bread', 'bcomment')
        extra_kwargs = {
            'bread': {'min_value': 0, 'required': True},
            'bcomment': {'min_value': 0, 'required': True},
            'bpub_date': {'required': True}
        } # 修该原有的选项参数
```

# 总结:
   - 如果序列化器类对应的是`django的模型类` 那么 它可以直接`继承ModelSerializer`
   - `继承ModelSerializer`之后 可以依据模型类字段 `自动生成序列化器的字段`
   - ModelSerializer中 已经实现了 `create()`和`update()`方法


~~Hey Miss Liu This Rose Is For You~~