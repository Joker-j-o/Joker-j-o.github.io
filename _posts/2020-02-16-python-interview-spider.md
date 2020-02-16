---
layout:     post                    # 使用的布局（不需要改）
title:      Python-interview(spider-douban-top250)          # 标题 
subtitle:   Joker #副标题
date:       2020-02-15              # 时间
author:     Joker                     # 作者
header-img: static/img/16.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
categories: [interview]
---

# 抓取豆瓣音乐排行榜top250

```python
import re
import time, random

import requests
from bs4 import BeautifulSoup
import csv

headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.106 Safari/537.36'
}


def get_url_music(url):
    """
    获取top250界面的链接
    :param url:
    :return:
    """
    html = requests.get(url, headers=headers)
    soup = BeautifulSoup(html.text, 'lxml')
    # 'class':'nbg' --> 根据界面参数检查获得
    aTags = soup.find_all('a', attrs={'class': 'nbg'})  # 获取所有的a标签节点
    for aTag in aTags:
        # 获取当前页 所有的详情链接
        get_music_info(filename, aTag['href'])


def get_music_info(filename, url):
    """
    获取专辑页面中的详细信息
    :param url:
    :return:
    """
    html = requests.get(url, headers=headers)
    soup = BeautifulSoup(html.text, 'lxml')
    try:
        title = soup.find(attrs={'id': 'wrapper'}).h1.span.text  # 获取title 专辑
    except:
        title = '未知专辑'
    try:
        author = soup.find(attrs={'id': 'info'}).find('a').text  # 获取作者
    except:
        author = '未知作者'

    # 获取流派 正则匹配 根据全文匹配
    fashions = re.findall('<span class="pl">流派:</span>&nbsp;(.*?)<br />', html.text, re.S)
    if len(fashions) == 0:
        fashion = '未知流派'
    else:
        fashion = fashions[0].strip()

    # 获取发行时间
    issue_date = re.findall('发行时间:</span>&nbsp;(.*?)<br />', html.text, re.S)

    # 获取发行商 出版者
    publishers = re.findall('<span class="pl">出版者:</span>&nbsp;(.*?)<br />', html.text, re.S)
    if len(publishers) == 0:
        publisher = '未知出版商'
    else:
        publisher = publishers[0].strip()

    # 数据保存格式 字典
    info = {
        'title': title,
        'author': author,
        'fashion': fashion,
        'issue_date': issue_date,
        'publisher': publisher
    }

    # 数据保存函数
    save_csv(filename, info)


def save_csv(filename, info):
    """
    保存数据
    :param filename:
    :param info:
    :return:
    """
    with open(filename, 'a', encoding='utf-8') as f:
        fieldnames = ['title', 'author', 'fashion', 'issue_date', 'publisher']
        writer = csv.DictWriter(f, fieldnames=fieldnames)
        writer.writerow(info)


if __name__ == '__main__':
    urls = ['https://music.douban.com/top250?start={}'.format(str(i)) for i in range(0, 250, 25)]
    # print(urls)
    filename = 'music.csv'
    with open(filename, 'w', encoding='utf-8') as f:
        fieldnames = ['title', 'author', 'fashion', 'issue_date', 'publisher']
        writer = csv.DictWriter(f, fieldnames=fieldnames)
        writer.writeheader()  # 写入头

    for url in urls:
        get_url_music(url)
        time.sleep(random.randint(0, 2))
    print('成功')

```



~~Hey Miss Liu This Rose Is For You~~