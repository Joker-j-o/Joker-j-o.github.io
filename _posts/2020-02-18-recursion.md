---
layout:     post                    # 使用的布局（不需要改）
title:      Python-recursion(递归算法)           # 标题 
subtitle:   Joker #副标题
date:       2020-02-18              # 时间
author:     Joker                     # 作者
header-img: static/img/16.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
categories: [interview]
---

# `recursion`

```python
"""
用 2 * 1 的小矩形 横放 或者 竖放
    去 覆盖一个更大的矩形 那么 用 n 个 2*1 的小矩形 覆盖 2*n 的 大矩形
    有多少种覆盖方法?
"""


def rect_cover(n):
    """
    递归
    :param n:
    :return:
    """
    if n == 0:
        return 0
    elif n == 1:
        return 1
    elif n == 2:
        return 2
    else:
        return rect_cover(n - 1) + rect_cover(n - 2)  # f(n) = f(n-1) + f(n-2)


num = input('输入需要计算的2*n的大矩形数:')
r1 = rect_cover(int(num))
print(r1)


def rect_cover_two(n):
    """
    非递归
    :param n:
    :return:
    """
    if n == 0:
        return 0
    elif n == 1:
        return 1
    elif n == 2:
        return 2
    else:
        res = [0, 1, 2]
        while len(res) <= n:
            res.append(res[-1] + res[-2])  # f(n) = f(n-1) + f(n-2)
        return res[n]


two_num = input('输入需要计算的值:')
r2 = rect_cover_two(int(two_num))
print(r2)

```



~~Hey Miss Liu This Rose Is For You~~