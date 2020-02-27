---
layout:     post                    # 使用的布局（不需要改）
title:      Python-spider(BeautifulSoup)          # 标题 
subtitle:   Joker #副标题
date:       2020-02-15              # 时间
author:     Joker                     # 作者
header-img: static/img/16.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
categories: [interview]
---

# 关于 BeautifulSoup的节点选择器获取节点的信息

> `pip install beautifulsoup4 ` 安装 beautifulsoup4 

```python
"""

关于 使用 BeautifulSoup 获取节点参数
    要解析文档内容之前，先要用BeautifulSoup实例一个对象。它的类型为<class 'bs4.BeautifulSoup'>

节点选择器: 只能获得第一个满足的节点
css选择器: 可以得到多个节点

"""

from bs4 import BeautifulSoup

html = '''
<html>
<body>
<div>
    <span>这是一句测试语句</span>
    <ul>
        <li class="item1" value1="1234" value2="hellow world"><a href="http://papiboy.cn">joker</a></li>
        <li class="item"><a href="https://joker-j-o.github.io/">joker</a></li>
    </ul>
    <button id="button">确定</button>
    <ul>
        <li class="item3"><a href="https://joker-j-o.github.io/">joker</a></li>
        <li class="item" ><a href="https://joker-j-o.github.io/">joker</a></li>
        <li id = "my_li" class="item2"><a href="https://joker-j-o.github.io/">joker</a></li>
        <p> 
            this is engineer_p
            <span class="span_css"> 
                this is engineer_span 
            </span> 
        </p>
    </ul>
</div>
</body>
</html>

'''

soup = BeautifulSoup(html, 'lxml')
# print(soup.span.text) # 获取 span 标签的值


"""节点选择器"""
print(soup.find_all('li')[0])  # 获取所有的 li 标签 返回一个列表
print(soup.find_all('li', class_='item'))  # 限制属性class的
print(soup.find_all('li', id='my_li'))  # 获取id值

"""css选择器"""
# tags = soup.select('.item') # 根据类获取
# tags = soup.select('#my_li') # 根据id获取

print(soup.select('ul .item3'))  # 组合查找: 标签名与类名、id名进行的组合原理是一样的 两者之间用 空格分开
print(soup.select('ul>p'))  # 子标签查找 使用 > 分隔

"""
属性查找 需要用中括号括起来[]
    当然 属性查找也可以根据上述 组合查找结合使用
    注意:同一节点的属性 标签之间不用加空格 否则会匹配不到
        不在同一节点的情况下 使用空格隔开
"""
print(soup.select('a[href="http://papiboy.cn"]'))  # 同一节点
print(soup.select('p span', class_="span_css"))  # 加属性限制的 不同一节点

tags = soup.select('a')  # 获取所有的 a 标签

for tag in tags:
    print(tag.text)  # 获取所有文本内容
    print(tag['href'])  # 获取所有 a 标签链接 href值

"""获取内容 可以使用 get_text()"""

```



~~Hey Miss Liu This Rose Is For You~~