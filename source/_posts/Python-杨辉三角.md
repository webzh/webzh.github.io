---
title: Python-杨辉三角
date: 2019-03-03 21:19:08
tags: Python
categories: 编程开发
---

最近在学习Python，写了个杨辉三角的程序实现玩玩。

##### 杨辉三角 #####
前提：每行端点与结尾的数为1.
每个数等于它上方两数之和。
每行数字左右对称，由1开始逐渐变大。
第n行的数字有n项。
第n行的m个数可表示为 C(n-1，m-1)，即为从n-1个不同元素中取m-1个元素的组合数。
第n行的第m个数和第n-m+1个数相等 ，为组合数性质之一。
![](/images/yanghui.jpg)

##### 第一种 #####
基本是大家都会想到的办法，按照每一行，一个个迭代计算出各个位置的数字。
代码如下：
<!--more-->
```Python
# -*- coding: utf-8 -*-

import datetime

startTime = datetime.datetime.now()

n = 4000
data = []
for i in range(0, n):
    if i == 0:
        data.append([1])
        continue
    pre = data[i - 1]
    val = [1]
    for j in range(1, i):
        val.append(pre[j - 1] + pre[j])
    val.append(1)
    data.append(val)

endTime = datetime.datetime.now()
print('Run time: ' + str((endTime - startTime).total_seconds()))
#print(data)
```

##### 第二种 #####
利用了每行数字个数固定，且左右数字对称的规律，提前定义每一行的列表长度，折半迭代。
代码如下：
```Python
import datetime

startTime = datetime.datetime.now()

n = 4000
data = []
for i in range(0, n):
    if i == 0:
        data.append([1])
        continue
    pre = data[i - 1]
    line = i + 1
    val = [1] * line
    half = line // 2
    if line % 2 != 0:
        val[half] = pre[half - 1] + pre[half]
    for j in range(1, half):
        number = pre[j - 1] + pre[j]
        val[j] = number
        val[-(j+1)] = number
    data.append(val)

endTime = datetime.datetime.now()
print('Run time: ' + str((endTime - startTime).total_seconds()))
#print(data)
```

##### 总结  #####
两种实现方式，在数量级别上来后，执行效率会有很大的差异，明显第二种优于第一种。
同样，上述代码已经附上了程序运行的时长记录，以便测试运行效率。
