---
layout:     post                    # 使用的布局（不需要改）
title:      Python-DRF-Frame-field types and option parameters           # 标题 
subtitle:   Joker #副标题
date:       2020-01-04              # 时间
author:     Joker                     # 作者
header-img: static/img/16.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
categories: [DRF-serializer]
---

# 常用字段类型

![序列化常用字段类型](/static/img/drf.png)

# 通用参数
... `无论哪种字段类型都可以使用的选项参数。`
   - `read_only`: 表明该字段`仅用于序列化输出`, 默认为 False
   - `write_only`: 表明该字段`仅用于反序列化输入`, 默认为 False
   - `required`: 表明该字段在`反序列化时 必须输入`, 默认为 True
   - `default`: `序列化 和 反序列化时` 使用的默认值
   - `error_massages`: 包含错误编号与错误信息的字典
   - `label`: 用于`HTML展示API页面时` 显示的字段名称

> 定义序列化器类的字段时，如果没有指定`read_only和write_only`，则这两个`参数默认值都为False`，表明对应的字段既在`序列化时使用`，也在`反序列化时使用`。

# 常用参数:
   - `max_length`: 最大长度
   - `min_length`: 最小长度
   - `max_value`: 最大值
   - `min_value`: 最小值

> `max_length` & `min_length` 是针对字符串类型的参数
> `max_value` & `min_value` 是针对数字类型的参数

# demo ------python

~~测试数据 自定义文件 不在django项目里面~~

### read_only 序列化时 输出

```python
# 设置Django运行所依赖的环境变量
import os

if not os.environ.get('DJANGO_SETTINGS_MODULE'):
    os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'BookProject.settings')

from rest_framework import serializers


class User(object):
    """用户类"""

    def __init__(self, name, age):
        self.name = name
        self.age = age


class UserSerializer(serializers.Serializer):
    """序列化器类"""
    name = serializers.CharField()
    age = serializers.IntegerField(read_only=True)


if __name__ == "__main__":
    # 准备数据
    data = {'name': 'joker', 'age': 18}

    # 数据校验
    serializer = UserSerializer(data=data)
    res = serializer.is_valid()
    print('校验结果：%s' % res)
    # 如果校验失败，获取校验失败的错误原因
    print(serializer.errors)
    # 如果校验成功，获取校验之后的数据
    print(serializer.validated_data)
    """
    以上测试打印结果:
        校验结果: True
        {}
        orderodDict([('name','joker')])
    """
```

### write_only 反序列化时 输入

```python
# 设置Django运行所依赖的环境变量
import os

if not os.environ.get('DJANGO_SETTINGS_MODULE'):
    os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'BookProject.settings')


from rest_framework import serializers


class User(object):
    """用户类"""
    def __init__(self, name, age):
        self.name = name
        self.age = age


class UserSerializer(serializers.Serializer):
    """序列化器类"""
    name = serializers.CharField()
    age = serializers.IntegerField(write_only=True) # 如果将值改为 False 那么表示 序列化时 该字段也会被执行

if __name__ == "__main__":
    # 创建user对象
    user = User('smart', 18)

    # 将user对象序列化为字典{'name': 'smart', 'age': 18}
    serializer = UserSerializer(user)

    # serializer.data获取序列化之后的字典数据
    print(serializer.data)
    """
    以上测试打印结果:
        {'name': 'smart'}
    """

```

### required

```python
# 设置Django运行所依赖的环境变量
import os

if not os.environ.get('DJANGO_SETTINGS_MODULE'):
    os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'JokerProject.settings')

# 让Django进行一次初始化
# import django
# django.setup() # 可有可无

from rest_framework import serializers


class User(object):
    """用户类"""

    def __init__(self, name, age):
        self.name = name
        self.age = age


class UserSerializer(serializers.Serializer):
    """序列化器类"""
    name = serializers.CharField()
    age = serializers.IntegerField()


if __name__ == "__main__":
    # 准备数据
    data = {'name': 'joker'}

    # 数据校验
    serializer = UserSerializer(data=data)
    res = serializer.is_valid()

    print('校验结果：%s' % res)

    # 如果校验失败，获取校验失败的错误原因
    print(serializer.errors)

    # 如果校验成功，获取校验之后的数据
    print(serializer.validated_data)
```
> 注意: 当前是测试数据 并没有使用django项目, 只是引用了django来运行DRF `报错说:No module named 'JokerProject'`
   
   - `只需要 将age字段里面 添加一句: required=False`

> 注意: 如果`不写 required` 它是`默认的` 默认为 `True`

### default 序列化时 使用的 默认值

```python
# 设置Django运行所依赖的环境变量
import os

if not os.environ.get('DJANGO_SETTINGS_MODULE'):
    os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'BookProject.settings')

from rest_framework import serializers


class User(object):
    """用户类"""

    def __init__(self, name, age):
        self.name = name
        self.age = age


class UserSerializer(serializers.Serializer):
    """序列化器类"""
    name = serializers.CharField()
    age = serializers.IntegerField()
    addr = serializers.CharField(default='默认地址')  # 当前使用了默认值 序列化&反序列化都会有


if __name__ == "__main__":
    # 创建user对象
    user = User('smart', 18)

    # 将user对象序列化为字典{'name': 'smart', 'age': 18}
    serializer = UserSerializer(user)

    # serializer.data获取序列化之后的字典数据
    print(serializer.data)

```

> 注意: `如果当前用户有该默认的值 那么使用的就不再是默认值 而是用户自己的`

`反序列化时 使用的默认值 default`

```python
import os

if not os.environ.get('DJANGO_SETTINGS_MODULE'):
    os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'BookProject.settings')

from rest_framework import serializers


class User(object):
    """用户类"""

    def __init__(self, name, age):
        self.name = name
        self.age = age


class UserSerializer(serializers.Serializer):
    """序列化器类"""
    name = serializers.CharField()
    age = serializers.IntegerField(required=False, default=20)


if __name__ == "__main__":
    # 准备数据
    data = {'name': 'joker'}

    # data = {'name': 'joker', 'age': 12} # 如果加上age值 就会覆盖默认值
    
    # 数据校验
    serializer = UserSerializer(data=data)
    res = serializer.is_valid()

    print('校验结果：%s' % res)

    # 如果校验失败，获取校验失败的错误原因
    print(serializer.errors)

    # 如果校验成功，获取校验之后的数据
    print(serializer.validated_data)

```

> 注意: 如果反序列化时传递的data中包含age，则反序列化之后的字典中age的值不再使用default设置的默认值。

~~Hey Miss Liu This Rose Is For You~~