---
layout:     post                    # 使用的布局（不需要改）
title:      Python-interview(json时间格式转换)          # 标题 
subtitle:   Joker #副标题
date:       2020-02-15              # 时间
author:     Joker                     # 作者
header-img: static/img/16.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
categories: [interview]
---

# 1. Json序列化时 处理日期类型的值

```python
"""
python interview

Json时间转换格式

"""

import json
from datetime import datetime, date


class DateToJson(json.JSONEncoder):
    """
    时间转换json格式
    """

    def default(self, obj):
        if isinstance(obj, datetime):
            return obj.strftime('%Y:%m:%d %H:%M:%S')
        elif isinstance(obj, date):
            return obj.strftime('%Y:%m:%d %H:%M:%S')
        else:
            return json.JSONEncoder.default(self, obj)


d = {'name': 'joker', 'date': datetime.now()}
print(json.dumps(d, cls=DateToJson, ensure_ascii=False))


```


# 2. 统计文本文件中 出现字符频率最大的数

```python
"""
python interview

统计文本文件中 出现字符频率最大的数

"""

with open('test-json.py', 'r', encoding='utf-8') as f:
    data = f.read()

num = {}
maxText = ''  # 表示最大数的 值
for i in data:
    if i.isspace():
        # 校验 如果是空格 就跳过
        continue
    if num.get(i) is None:
        # 此处不使用[i] 因为没找到会报错 而使用get() 未找到会出现None
        num[i] = 1
        if maxText == '':
            maxText = i
    else:
        num[i] += 1
        if num[maxText] < num[i]:
            maxText = i

print('出现频率最大的值 %s' % maxText)
print('出现的次数为 %s' % num[maxText])

```

# 3. 装饰器的简单测试

```python
from functools import wraps


def log(flag):
    def decorate(func):
        @wraps(func)
        def _wrap(*args, **kwargs):
            try:
                func(*args, **kwargs)
                print('name', func.__name__)
            except Exception as e:
                pass

        return _wrap

    return decorate


@log(True)
def count_add(num1, num2, num3):
    print('sum', '=', num1 + num2 + num3)


count_add(1, 2, 3)

"""
装饰器:
    装饰器 本身就是一个函数
    主要用于: 为函数添加额外的功能.
        例如:
            日志, 性能测试, 清理缓存 等

"""

```

# 4. 将一个二维列表(多维列表) 转换为一个一维列表

```python
"""
python 生成器(迭代)

    yield

将一个二维列表 转换为一维列表
二维列表: [[],[]] 多个列表嵌套
一维列表: []

多维列表:[[[[[]],[[]]],[[[]],[[]]]]]

"""

two_d_list = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]


def list_of(two_d_list):
    for subList in two_d_list:
        for element in subList:
            yield element


for num in list_of(two_d_list):
    print(num, end=' ')

'''
将一个多维列表转换为一维列表
'''

many_dimensions_list = [1, [2, 3, [4, 5], [6, 7]], [2, [1998, 2, 15, [1999, 1, 1]], [1999, 1, 2], [2000, 2, 2]]]


def many_dimensions(many_dimensions_list):
    try:
        for sub_list in many_dimensions_list:
            for element in many_dimensions(sub_list):
                yield element
    except TypeError:
        yield many_dimensions_list


for num in many_dimensions(many_dimensions_list):
    print(num, end=' ')

```

# 5. 进程通信方式 线程通信方式

```python
"""
python 进程之间如何通信

可以使用队列在进程之间进行通信

"""

from multiprocessing import Queue, Process
import threading
import time, random

list1 = ['python', 'java', 'php', 'c++', 'c#', 'javascript']


def write(queue):
    """
    添加数据
    :param queue:
    :return:
    """
    for value in list1:
        print(f'正在向队列中添加数据 {value}')
        queue.put_nowait(value)  # 写入数据
        time.sleep(random.random())


def read(queue):
    """
    读取数据
    :param queue:
    :return:
    """
    while True:
        if not queue.empty():
            value = queue.get_nowait()  # 获取数据
            print(f'正在向队列中获取数据 {value}')
            time.sleep(random.random())
        else:
            break  # 如果队列为空 那么就结束


if __name__ == '__main__':
    queue = Queue()  # 创建队列

    write_data = Process(target=write, args=(queue,))  # 创建 write 进程
    read_data = Process(target=read, args=(queue,))  # 创建 read 进程

    # 开启进程
    write_data.start()
    write_data.join()  # 直到进程结束 join 等待

    read_data.start()
    read_data.join()

'''
线程之间通信

'''


def func1(s, func2):
    print('正在执行func1')
    func2(s)


def ff(s):
    print('ff输出的值 %s' % s)


t = threading.Thread(target=func1, args=('hello world', ff))

t.start()

```

# 6. 读取文件中的url 使用多线程下载图片

```python
from urllib3 import *
import threading

http = PoolManager()

disable_warnings()

urlList = []
with open('urls.txt', 'r') as f:
    while True:
        url = f.readline()
        if url == '':  # 如果读取完成 就退出
            break
        urlList.append(url.strip()) # 截取空格


class DownloadThread(threading.Thread):
    """
    自定义 创建下载类
    """

    def __init__(self, func, args):
        super().__init__(target=func, args=args)


def download(filename, url):
    """
    下载函数
    """
    response = http.request('GET', url)
    with open(filename, 'wb') as f:
        f.write(response.data)
        print('<', url, '>', '下载完成')


for i in range(len(urlList)):
    thread = DownloadThread(download, (str(i) + '.jpg', urlList[i]))
    thread.start()

```

> `文章中的 urls.txt 需要自己定义 内容如下:`

```s
http://img5.imgtn.bdimg.com/it/u=2104351895,3299575660&fm=26&gp=0.jpg
http://n.sinaimg.cn/sinacn20116/600/w940h1260/20190429/c7ab-hwfpcxm9497537.jpg
http://img.pconline.com.cn/images/upload/upc/tx/photoblog/1312/04/c5/29292154_1386139829396_mthumb.jpg
http://img1.imgtn.bdimg.com/it/u=252488915,2067705788&fm=26&gp=0.jpg
http://www.magicalmaths.org/wp-content/uploads/2012/11/conclusion-introduction-starter-plenary1.jpg
http://hbimg.b0.upaiyun.com/4e12e5eb2ea26c9b579e6726eb9648e323d8e7b1106c3-phLyTv_fw658
http://getdrawings.com/images/bamboo-tree-drawing-7.jpg
```



~~Hey Miss Liu This Rose Is For You~~