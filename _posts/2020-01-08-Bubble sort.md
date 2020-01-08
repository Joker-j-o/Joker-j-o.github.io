---
layout:     post                    # 使用的布局（不需要改）
title:      Python-Bubble sort(冒泡排序)           # 标题 
subtitle:   Joker #副标题
date:       2020-01-08              # 时间
author:     Joker                     # 作者
header-img: static/img/16.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
categories: [python]
---

# `Bubble sort`

**Bubble sort**: `冒泡排序的主要思想类似于水底的气泡，从水底向水平面移动时，不断变大。`

```python
arr_1 = [11,4,3,55,22,47,66,43,9,8,7]


# 冒泡排序
def bubble_sort(arr):
    for i in range(len(arr)):
        for j in range(len(arr) - i - 1):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
    return arr


print('冒泡排序前：', arr_1)
print('冒泡排序后：', bubble_sort(arr_1))
# 冒泡排序后： [3, 4, 7, 8, 9, 11, 22, 43, 47, 55, 66]
```

### `Implementation steps`:

   - 比较相邻的元素，如果`第一个比第二个大`，就交换他们两个，大的靠后
   - 对每一对相邻元素做同样的工作,从开始第一对到结尾的最后一对。此时，最后的元素应该是最大的数。
   - 针对所有的元素重复以上的步骤，除了最后一个（最后一个已经是最大的了）。 持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较。

# `Quick sort`

**Quick sort**: `快速排序的思想主要是将数列拆成一大、一小的数列，然后再对一大一小继续分割迭代，最后实现数列的快速排序。`

```python
arr_2 = [7, 1, 4, 8, 26, 43, 2, 3]


# 快速排序
def quick_sort(arr):
    if len(arr) <= 1:
        return arr
    mid = arr[len(arr) // 2]
    left, right = [], []
    arr.remove(mid)
    for item in arr:
        if item >= mid:
            right.append(item)
        else:
            left.append(item)
    return quick_sort(left) + [mid] + quick_sort(right)


print('快速排序前：', arr_2)
print('快速排序后：', quick_sort(arr_2))
# 快速排序后： [1, 2, 3, 4, 7, 8, 26, 43]
```

### `Implementation steps`:

   - 先从数列中选取一个数作为基数（此处我们 取了下标为中间对应的数组值作为基数）。
   - 将比这个大的数字全放在右边，比这个数小的数字放在左边，现在得到了左右两个区间。
   - 将左右区间分别再次执行step1，不断迭代，得到最终返回的数组=迭代（左）+[ 基数]+迭代（右）

# `Insertion sort`

**Insertion sort**: `插入排序`的思想就是将`无序数列`插入到`有序数列`中。本质也是`两个元素比大小`，满足条件就`交换位置`，一开始会像冒泡排序一样，但会比冒泡多一步就是`交换后(a[i]=a[i+1]后)原位置(a[i])`，会`继续`和`前面的数`比较`满足条件交换`，直到`a[i+1]`前面的`数组是有序`的。

```python
arr_3 = [16, 20, 4, 9, 2, 98, 1, 7]


# 插入排序
def insert_sort(arr):
    for i in range(1, len(arr)):
        value = arr[i]
        j = i - 1
        while j >= 0:
            if arr[j] > value:
                arr[j + 1] = arr[j]
            else:
                break
            j -= 1
        arr[j + 1] = value
        # print(arr) # 每次比较排序打印一次
    return arr


print('插入排序前：', arr_3)
print('插入排序后：', insert_sort(arr_3))
# 插入排序后： [1, 2, 4, 7, 9, 16, 20, 98]
```

### `Implementation steps`:
   - 认定`第一个元素`为`有序数列`（第一个元素就一个，无需排序）。
   - 选取`无序数列`中的`元素`与`有序数列`的`元素`依次比较，如果`比前面的数小`，那就把它放到`这个数前面`。


~~Hey Miss Liu This Rose Is For You~~