---
layout: post
title: "Python Data Preprocessing Cheatsheet (持续更新)"
date: 2020-05-30 12:25:06 -0700
comments: false
---

Python Basics
============

1\. 查找python所在位置：

```python
which python
whereis python
```

2\. 前面带下划线的变量的意义：**最近的两个输出结果**：分别保存在_(一个下划线)和__(两个下划线)变量中。

Ipython
===============

1\. ipython下运行linux shell命令

e.g.

```python
In [1]: !ls
```

Pandas
===============


Numpy
===============

1\. np.zeros, np.ones

e.g.

```python
In [6]: import numpy as np
In [10]: np.ones((3,2))                                                                                                   
Out[10]: 
array([[1., 1.],
       [1., 1.],
       [1., 1.]])

In [11]: np.zeros((3,2))                                                                                                  
Out[11]: 
array([[0., 0.],
       [0., 0.],
       [0., 0.]])
```

2\. np.array单个元素element选取

```python
# 两种方式等价：
In [13]: arr = np.zeros((3,2))
In [16]: arr2d[0][1]                                                                                                      
Out[16]: 0.0
In [17]: arr2d[0,1]                                                                                                       
Out[17]: 0.0
```

3\. 生成随机数

```python
In [18]: from numpy.random import randn                                                                                   

In [19]: data = {i : randn() for i in range(7)}                                                                           

In [20]: data                                                                                                             
Out[20]: 
{0: 1.153935587569964,
 1: 0.1478806146606029,
 2: -0.08871804387908859,
 3: 0.8873332130508745,
 4: -1.295237891571599,
 5: 1.5232211455713052,
 6: -0.054794039890991186}

In [21]: randn()                                                                                                          
Out[21]: -0.18245305471376677
```

