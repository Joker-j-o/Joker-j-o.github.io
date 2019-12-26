---
layout:     post                    # 使用的布局（不需要改）
title:      Mysql read-write separation           # 标题 
subtitle:   Joker #副标题
date:       2019-12-26              # 时间
author:     Joker                     # 作者
header-img: static/img/16.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
categories: [python,django]
---

# **Mysql read-write separation**
# **[官方文档](https://docs.djangoproject.com/en/dev/topics/db/multi-db/)**
```python
# 在settings.py里面 添加一个 slave数据库的配置
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',  # 数据库引擎
        'HOST': '127.0.0.1',  # 数据库主机
        'PORT': 3307,  # 数据库端口 主机映射出来的端口
        'USER': 'root',  # 数据库用户名
        'PASSWORD': 'youpassword',  # 数据库用户密码
        'NAME': 'my_project_db'  # 数据库名字
    },
    'slave': {  # 读（从机）
        'ENGINE': 'django.db.backends.mysql',
        'HOST': '127.0.0.1',
        'PORT': 3308, # 数据库端口 从机映射出来的端口
        'USER': 'root',
        'PASSWORD': 'youpassword',
        'NAME': 'my_project_db'
    }
}
# Configure the read and write routing for the database
DATABASE_ROUTERS = ['utils.db_router.MasterSlaveDBRouter']

```

# **Create and configure database read and write routing** 
```python
# 在utils文件夹里面 新建一个文件 db_router.py 实现MySQL读写分离
import random


class MasterSlaveDBRouter(object):
    """数据库读写路由"""

    def db_for_read(self, model, **hints):
        """读"""
        ret = random.choice(["default", "slave"])
        return ret  # 随机选择一个数据库进行读取操作

    def db_for_write(self, model, **hints):
        """写"""
        return "default"

    def allow_relation(self, obj1, obj2, **hints):
        """是否运行关联操作"""
        return True

```

~~hey miss liu this rose is for you~~