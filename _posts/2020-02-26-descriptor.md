---
layout:     post                    # 使用的布局（不需要改）
title:      Python-descriptor(描述符)           # 标题 
subtitle:   Joker #副标题
date:       2020-02-26              # 时间
author:     Joker                     # 作者
header-img: static/img/16.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
categories: [interview]
---

# `descriptor`

```python
"""
描述符:
    一个类里面的 类属性 = 另一个类的对象
    那么 这个类属性就是 描述符
"""


class NameDes(object):
    """
    名字
    """

    def __init__(self):
        self.__name = None  # 私有属性

    def __get__(self, instance, owner):
        return self.__name

    def __set__(self, instance, value):
        if isinstance(value, str):  # 校验类型
            self.__name = value
        else:
            try:
                raise TypeError('this is name err')
            except:
                print('this is name err')

    def __delete__(self, instance):
        del self.__name


class PasswordDes(object):
    """
    密码
    """

    def __init__(self):
        self.__pwd = None

    def __get__(self, instance, owner):
        return self.__pwd

    def __set__(self, instance, value):
        if isinstance(value, int):  # 校验类型
            self.__pwd = value
        else:
            try:
                raise TypeError('this is pwd err')
            except:
                print('this is pwd err')

    def __delete__(self, instance):
        del self.__pwd


class Person():
    """
    人物
    """
    name = NameDes()  # 类属性指向某个 类
    password = PasswordDes()


p = Person()

# p.name = 123 # 错误示范
# p.password = '123' # 错误示范
p.name = 'joker'
p.password = 123456

print(p.name)
print(p.password)

```


~~Hey Miss Liu This Rose Is For You~~