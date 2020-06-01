---
layout: post
title: Python数据预处理Cheatsheet (持续更新)
<!-- date: 2020-05-30 12:25:06 -0700 -->
tags: python cheatsheet
<!-- comments: true -->
categories: python
---


* TOC
{:toc}

#  Python basics

1\. 查找python所在位置：

```python
which python
whereis python
```

2\. 前面带下划线的变量的意义：**最近的两个输出结果**：分别保存在\_(一个下划线)和__(两个下划线)变量中。

#  Ipython

1\. ipython下运行linux shell命令

```python
In [1]: !ls
```

2\. magic function %: 

- `%paste`: 带格式粘贴一段代码到ipython
- `%run`: 运行python script. e.g. `%run test.py`

#  Pandas

1\. 删除DataFrame中某列／行

```python
In [2]: df = pd.DataFrame({"c1":[1,2],"c2":[3,4]})
In [3]: df
Out[3]:
   c1  c2
0   1   3
1   2   4
# 按列名删除
In [4]: df.drop(['c1'],axis=1)
Out[4]:
   c2
0   3
1   4
# 按列编号删除 - e.g. 删除第一列
In [5]: df.drop(df.columns[0],axis=1)
Out[5]:
   c2
0   3
1   4
# e.g. 删除倒数第一列
In [6]: df.drop(df.columns[-1],axis=1)
Out[6]:
   c1
0   1
1   2
# inplace删除：加inplace=True
In [7]: df.drop(df.columns[-1],axis=1,inplace=True)
```

2\. 用range构造df

```
In [19]: df = pd.DataFrame({'col1':['a','b'],'data1':range(2)})
In [20]: df
Out[20]:
  col1  data1
0    a      0
1    b      1
```

#  Numpy

1\. np.zeros, np.ones

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
In [13]: arr2d = np.zeros((3,2))
# 以下两种方式等价：
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

# 作图

1\. 加载图片并展示

```python
import matplotlib.pyplot as plt
# 读取图片
img = plt.imread('/filepath/myimage.jpg')
# 展示图片
plt.imshow(img)
```

