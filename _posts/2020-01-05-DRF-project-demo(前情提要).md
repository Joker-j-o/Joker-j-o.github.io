---
layout:     post                    # 使用的布局（不需要改）
title:      Python-DRF-project-demo-APIView(前情提要部分)           # 标题 
subtitle:   Joker #副标题
date:       2020-01-05              # 时间
author:     Joker                     # 作者
header-img: static/img/16.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
categories: [DRF-serializer]
---

# BookProject ------demo

```s
# 首先 安装django 安装依赖
pip install django==1.11.11 -i https://pypi.tuna.tsinghua.edu.cn/simple # 安装django
pip install djangorestframework # 安装依赖

# 创建django项目 创建子应用 别忘了虚拟环境
django-admin startproject BookProject # 创建项目
django-admin startapp booktest # 创建子应用

```

# 正文

`创建好项目之后 在 项目文件夹里面的settings.py里面 注册一下子应用 不然在数据库迁移的时候 会报错`
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'rest_framework', # 注册 DRF
    'booktest', # 注册子应用
]
```

> 注意: `DRF 是在django的基础下 衍生的一个功能 该功能 简化了序列化&数据库之间数据转换操作 需要注册一下`

`测试阶段 将csrf关闭`

```python
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    # 'django.middleware.csrf.CsrfViewMiddleware', # 测试阶段 不使用csrf
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]
```

`创建数据模型类 使用django 自带默认的数据库`

```python
from django.db import models # 这是django 自带的数据库


class BookInfo(models.Model):
    """图书模型类"""
    btitle = models.CharField(max_length=20, verbose_name='标题')
    bpub_date = models.DateField(verbose_name='发布日期')
    bread = models.IntegerField(default=0, verbose_name='阅读量')
    bcomment = models.IntegerField(default=0, verbose_name='评论量')
    is_delete = models.BooleanField(default=False, verbose_name='删除标记')

    class Meta:
        db_table = 'tb_books'
        verbose_name = '图书'
        verbose_name_plural = verbose_name

    def __str__(self):
        return self.btitle


class HeroInfo(models.Model):
    """英雄模型类"""
    GENDER_CHOICES = (
        (0, '男'),
        (1, '女')
    )
    hname = models.CharField(max_length=20, verbose_name='名称')
    hgender = models.SmallIntegerField(choices=GENDER_CHOICES, default=0, verbose_name='性别')
    hcomment = models.CharField(max_length=200, null=True, verbose_name='备注')
    is_delete = models.BooleanField(default=False, verbose_name='删除标记')
    hbook = models.ForeignKey('BookInfo', on_delete=models.CASCADE, verbose_name='所属图书')

    class Meta:
        db_table = 'tb_heros'
        verbose_name = '英雄'
        verbose_name_plural = verbose_name

    def __str__(self):
        return self.hname

```

> 注意: `导包的时候 注意`

`在子应用里面 新建 serializers.py`

```python
from rest_framework import serializers

from booktest.models import BookInfo


class BookInfoSerializer(serializers.ModelSerializer):
    """图书数据序列化器"""

    class Meta:
        model = BookInfo # 对应的数据模型类
        fields = '__all__' # 所有的字段

```

# [ModelSerializer的使用](https://joker-j-o.github.io/python/django/2020/01/06/DRF-ModelSerializer.html)

# 数据库迁移

```shell
python manage.py makemigrations # 生成迁移文件
python manage.py migrate # 执行迁移文件
```

`可能会出现以下报错 及 解决方案`

![解决方案](/static/img/pycharm-items.png)

# 引入测试数据

`关于 pycharm里面的db.sqlite3的使用`

![db1](/static/img/django-db1.png)

![db2](/static/img/django-db2.png)

![db3](/static/img/django-db3.png)

> 不会使用 请自行百度 [baidu](https://www.baidu.com)

```s
# 测试数据 tb_books
insert into tb_books(btitle,bpub_date,bread,bcomment,is_delete) values
('射雕英雄传','1980-5-1',12,34,0),
('天龙八部','1986-7-24',36,40,0),
('笑傲江湖','1995-12-24',20,80,0),
('雪山飞狐','1987-11-11',58,24,0);

# 测试数据 tb_heros
insert into tb_heros(hname,hgender,hbook_id,hcomment,is_delete) values
('郭靖',1,1,'降龙十八掌',0),
('黄蓉',0,1,'打狗棍法',0),
('黄药师',1,1,'弹指神通',0),
('欧阳锋',1,1,'蛤蟆功',0),
('梅超风',0,1,'九阴白骨爪',0),
('乔峰',1,2,'降龙十八掌',0),
('段誉',1,2,'六脉神剑',0),
('虚竹',1,2,'天山六阳掌',0),
('王语嫣',0,2,'神仙姐姐',0),
('令狐冲',1,3,'独孤九剑',0),
('任盈盈',0,3,'弹琴',0),
('岳不群',1,3,'华山剑法',0),
('东方不败',0,3,'葵花宝典',0),
('胡斐',1,4,'胡家刀法',0),
('苗若兰',0,4,'黄衣',0),
('程灵素',0,4,'医术',0),
('袁紫衣',0,4,'六合拳',0);
```

`每个版本不一样 建议都试一下`

# [APIView-v1](https://joker-j-o.github.io/drf-serializer/2020/01/06/01DRF-APIView(%E7%BC%96%E5%86%99views).html)


~~Hey Miss Liu This Rose Is For You~~