---
layout:     post                    # 使用的布局（不需要改）
title:      Python-arithmetic(算法)           # 标题 
subtitle:   Joker #副标题
date:       2020-02-26              # 时间
author:     Joker                     # 作者
header-img: static/img/16.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
categories: [interview]
---

# `arithmetic`

```python
'''
最大子序列 整数类型nums, 找出一个序列中 乘积最大的连续子序列(该序列至少包含一个数)
num(i,j) = num(0,j) / num(0,i)

0: 需要重新开始
< 0: 应该找到前面最大的负数
> 0: 最小的正数

查找最大的正数

'''


def maxMul(nums):
    if not nums: return
    # 目前的累乘
    cur_mul = 1
    # 前面最小的正数
    min_pos = 1
    # 前面最大的负数
    max_neg = float('-inf')
    # 结果
    result = float('-inf')

    for num in nums:
        cur_mul *= num

        if cur_mul > 0:
            result = max(result, cur_mul // min_pos)
            min_pos = min(min_pos, cur_mul)
        elif cur_mul < 0:
            if max_neg != float('-inf'):
                result = max(result, cur_mul // max_neg)
            else:
                result = max(result, num)
            max_neg = max(max_neg, cur_mul)
        else:
            cur_mul = 1
            min_pos = 1
            max_neg = float('-inf')
            result = max(result, num)
    return result


data = [1, 2, -2, -1, 5, -4, 1]
print(maxMul(data))

```

# 二进制中 1 出现的次数

```python
"""
统计 二进制中 1 出现的次数

"""


def oneNumber(n):
    print(bin(n))
    if n < 0:
        n = n & 0xfffffffff

    m = len(bin(n)) - 2
    count = 0

    for i in range(0, m):
        if n % 2 ** i != 0:
            count += 1
    return count


num = int(input('输入数字:'))

print(oneNumber(num))

```

~~Hey Miss Liu This Rose Is For You~~