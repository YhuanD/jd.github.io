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

```sh
which python
whereis python
```

2\. 前面带下划线的变量的意义：**最近的两个输出结果**：分别保存在\_(一个下划线)和__(两个下划线)变量中。

3\. 字符串拆分

``` python
>>> line = 'aaa,bbb;ccc,ddd'
# 按照某个字符（串）拆分
>>> line.split(',')
['aaa', 'bbb;ccc', 'ddd']
>>> line.split('bb')
['aaa,', 'b;ccc,ddd']
## 使用正则拆分
# e.g. 按","或";"拆分，以下两种方法都可以
>>> re.split(r';|,',line)
['aaa', 'bbb', 'ccc', 'ddd']
>>> re.split(r'[;,]',line)
['aaa', 'bbb', 'ccc', 'ddd']
# e.g. 按","或"bb"拆分
>>> re.split(r',|bb',line)
['aaa', '', 'b;ccc', 'ddd']
# e.g. 按一个或多个英文字母拆分
>>> re.split(r'[a-z]+',"5se3dr23")
['5', '3', '23']
# 加"()"保留拆分字符串，对比如下
>>> re.split(r'(dr)',"ddr23")     
['d', 'dr', '23']
>>> re.split(r'dr',"ddr23")  
['d', '23']
```

4\. re.search, re.findall(), re.match(), .find()

```python
# re.search()
re.search(r'a|b','bba')
<_sre.SRE_Match object; span=(0, 1), match='b'>
>>> re.search(r'a|b','bba').group(0) 
'b'
>>> re.findall(r'a|b','bba')
['b', 'b', 'a']
>>> re.match(r'.*bb(\d+)a',"中文bb23a").group(1)
'23'
>>> "aa34bb".find('b') 
4
>>> "aa34bb".find('34')
2
```

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

```python
In [19]: df = pd.DataFrame({'col1':['a','b'],'data1':range(2)})
In [20]: df
Out[20]:
  col1  data1
0    a      0
1    b      1
```

3\. 替换dataframe内容

```python
# 例1
data.replace(';', ',', inplace=True)
# 例2： 正则替换
data = data.replace(to_replace=r'\n|\r|\?|\t', value=' ', regex=True)
```

4\. 读入文件

```python
pd.read_csv('上市公司名单.csv', sep=',', header=0) 
# 注：header=0，表示第一行为标题行
```

5\. 两个dataframe取补集

```python
# df中去掉df1
df.append(df1).drop_duplicates(keep=False).copy()
# 注：如果没有keep=False就是简单的去重
```

6\. dataframe增加一行数据

```python
# e.g. 
df.append([[1,2,3]])
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

