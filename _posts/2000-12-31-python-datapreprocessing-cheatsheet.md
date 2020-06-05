---
layout: post
title: Python数据预处理cheatsheet (持续更新)
<!-- date: 2020-05-30 12:25:06 -0700 -->
tags: python cheatsheet
<!-- comments: true -->
categories: python
---


* TOC
{:toc}

Python basics
==============

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

4\. 匹配搜索： re.search(), re.findall(), re.match(), .find()

```python
# re.search()
>>> re.search(r'a|b','bba')
<_sre.SRE_Match object; span=(0, 1), match='b'>
>>> re.search(r'a|b','bba').group(0) 
'b'
# re.findall()
>>> re.findall(r'a|b','bba')
['b', 'b', 'a']
# re.match()
>>> re.match(r'.*bb(\d+)a',"中文bb23a").group(1)
'23'
# a_string.find()
>>> "aa34bb".find('b') 
4
>>> "aa34bb".find('34')
2
```

5\. 字符串替换re.sub()

```python
>>> re.sub(',|;','','aa,bb;cc')
'aabbcc'
>>> re.sub(',|bb','','aa,bb;cc')
'aa;c'
```

6\. 日期

```python
>>> import datetime
>>> datetime.date.today().strftime("%Y%m%d")
'20200604'
```

Ipython
==============

1\. ipython下运行linux shell命令

```python
In [1]: !ls
```

2\. magic function %: 

- `%paste`: 带格式粘贴一段代码到ipython
- `%run`: 运行python script. e.g. `%run test.py`

Pandas
==============

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

4\. 读入/输出文件

```python
# 读入文件
pd.read_csv('filename', sep=',', header=0) 
# 注：header=0，表示第一行为标题行
# 将df保存到文件
df.to_csv('filename', index=False, sep=';')
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

7\. dataframe部分选取

```python
>>> df = pd.DataFrame({'col1':['ab','bc','cd','de'],'col2':[1,2,3,4]})
>>> df
  col1  col2
0   ab     1
1   bc     2
2   cd     3
3   de     4
# 选取col1中包含'a'或'b'的行
>>> df[df.col1.str.contains('a|b')]
  col1  col2
0   ab     1
1   bc     2
# 选取col1中包含'a'或'b'并且不包含'c'的行
>>> df[df.col1.str.contains('a|b') & ~df.col1.str.contains('c')]
  col1  col2
0   ab     1
```

8\. 填充np.nan处： .fillna() ## inplace=True可用

```python
>>> df = pd.DataFrame({'col1':['a',np.nan,'c'],'col2':[1,2,np.nan]})
>>> df
  col1  col2
0    a   1.0
1  NaN   2.0
2    c   NaN
# 全局fill
>>> df.fillna(3)
  col1  col2
0    a   1.0
1    3   2.0
2    c   3.0
# inplace
>>> df.fillna(3,inplace=True)
>>> df
  col1  col2
0    a   1.0
1    3   2.0
2    c   3.0
# 某一列fill
>>> df.col2.fillna(3)
0    1.0
1    2.0
2    3.0
# 使用df1或df中的另一列fill df中的某一列
>>> df1 = pd.DataFrame({'col3':[4,5,6]})
>>> df1
   col3
0     4
1     5
2     6
>>> df.col2.fillna(df1.col3)
0    1.0
1    2.0
2    6.0
```

9\. 列/行重命名rename ## inplace=True可用

```python
>>> df = pd.DataFrame({'col1':['a','b','c'],'col2':['d','e','f']})
>>> df
  col1 col2
0    a    d
1    b    e
2    c    f
>>> df.rename(columns={'col1':'c1'})  
  c1 col2
0  a    d
1  b    e
2  c    f
# 行重命名
>>> df.rename(index={0:'x',1:'y',2:'z'})
  col1 col2
x    a    d
y    b    e
z    c    f
```

10\. 按列排序 ## inplace=True可用

```python
>>> df = pd.DataFrame({'col1':['b','a','a','c'],'col2':[1,3,2,4]})
>>> df
  col1  col2
0    b     1
1    a     3
2    a     2
3    c     4
>>> df.sort_values(by='col1') # 或 df.sort_values(by=['col1'])
  col1  col2
1    a     3
2    a     2
0    b     1
3    c     4
# 反向排序
>>> df.sort_values(by='col1', ascending=False)
  col1  col2
3    c     4
0    b     1
1    a     3
2    a     2
# 按两列排序
>>> df.sort_values(by=['col1','col2'])
  col1  col2
2    a     2
1    a     3
0    b     1
3    c     4
```

11\. 删除含np.nan的行/列： .dropna() ## inplace=True可用

```python
>>> df = pd.DataFrame({'col1':['a',np.nan,'c'],'col2':[1,2,np.nan],'col3':[4,5,6]})
>>> df
  col1  col2  col3
0    a   1.0     4
1  NaN   2.0     5
2    c   NaN     6
# drop含np.nan的行
>>> df.dropna()
  col1  col2  col3
0    a   1.0     4
# drop含np.nan的列
>>> df.dropna(axis='columns')
   col3
0     4
1     5
2     6
# drop都为np.nan的行
>>> df = pd.DataFrame({'col1':['a',np.nan,'c'],'col2':[1,np.nan,np.nan]})
>>> df
  col1  col2
0    a   1.0
1  NaN   NaN
2    c   NaN
>>> df.dropna(how='all')
  col1  col2
0    a   1.0
2    c   NaN
```

12\. 更改某列位置

```python
>>> df = pd.DataFrame({'col1':['a','b'],'col2':[1,2],'col3':['aa','bb']})           
>>> df
  col1  col2 col3
0    a     1   aa
1    b     2   bb
# 将col3换到第二列（列index=1）## 直接在df原位替换，不用重新赋值给df
>>> df.insert(1,'col3',df.pop('col3'))
>>> df
  col1 col3  col2
0    a   aa     1
1    b   bb     2
```

Numpy
==============

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

作图
==============

1\. 加载图片并展示

```python
import matplotlib.pyplot as plt
# 读取图片
img = plt.imread('/filepath/myimage.jpg')
# 展示图片
plt.imshow(img)
```

