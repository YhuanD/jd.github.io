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

7\. list多重赋值

```python
In [56]: lst = ['a','b','c']
In [57]: v1,v2,v3 = lst
In [58]: v1
Out[58]: 'a'
```

8\. 查看list元素index：index()

```python
In [56]: lst = ['a','b','c']
In [59]: lst.index('b')
Out[59]: 1
# 如果有重复值则返回第一个元素的index
In [60]: lst = ['a','a','b']
In [61]: lst.index('a')
Out[61]: 0
```

9\. list排序sort

```python
In [66]: lst = ['a','A','Z','z']
In [67]: lst.sort()
# .sort()直接作用于原list
# 默认先排序大写，后排序小写
In [68]: lst
Out[68]: ['A', 'Z', 'a', 'z']
# 改成忽略大小写
In [75]: lst = ['a','Z','z','A']
In [76]: lst.sort(key=str.lower)
In [77]: lst
Out[77]: ['a', 'A', 'Z', 'z']
```

10\. 删除变量：`del var_name`

11\. 查看list非重复元素数量

```python
In [190]: lst1 = ['a','a','b']
In [191]: len(set(lst1))
Out[191]: 2
```

12\. 查看两个list中元素是否是子集关系

```python
In [215]: lst1 = ['a','b','c','d']
In [216]: lst2 = ['c','b']
In [217]: set(lst1) > set(lst2)
Out[217]: True
```

13\. 更换目录

```python
import os
os.chdir(path)
# 显示当前目录current working directory
os.getcwd()
```

14\. list元素变tuple类型

```python
In [450]: lst = [['a','b'],['c','d']]
In [451]: list(map(tuple,lst))
Out[451]: [('a', 'b'), ('c', 'd')]
```

15\. 查看string（或pd.Series类型）中是否有小写字母

```python
In [468]: s = 'ABCDe'
In [469]: any(filter(str.islower,s))
Out[469]: True
```

16\. 转换英文字母大小写

```python
In [532]: 'abC'.upper()
Out[532]: 'ABC'
In [533]: 'abC'.lower()
Out[533]: 'abc'
```

17\. 判断string是否包含某substring

```python
In [561]: 'ab' and 'bc' in 'abc'
Out[561]: True
In [562]: 'ab' and 'de' in 'abc'
Out[562]: False
In [563]: 'ab' or 'bc' in 'abc'
Out[563]: 'ab'
```

18\. 列表和元组：元组像字符串一样不可变，其中的值不能被修改、添加或删除。

```python
In [597]: type(('a',))
Out[597]: tuple
In [598]: type(('a'))
Out[598]: str
# 类型转换
In [602]: tuple(['a','b'])
Out[602]: ('a', 'b')
In [603]: list(('a','b'))
Out[603]: ['a', 'b']
```

19\. 查看字典的值

```python
In [641]: dic = {'a':0,'b':1}
In [642]: dic.get('a')  # 'a'存在于字典中，可以用dic['a']
Out[642]: 0
In [643]: dic.get('c') # 注：'c'不在字典中，用dic['c']会报错
In [644]: str(dic.get('c'))
Out[644]: 'None'
# 对于不在字典中的元素赋默认值，对于已有的元素不起作用
In [645]: dic.setdefault('c',3)
Out[645]: 3
In [646]: dic.setdefault('a',3)
Out[646]: 0
```

20\. 字符串方法：.startswith(), .endswith()

```python
In [660]: 'abc'.startswith('a')
Out[660]: True
In [661]: 'abc'.endswith('c')
Out[661]: True
```

21\. 字符串对齐填充

```python
In [671]: s = 'abc'
# 左右对齐，保持长度为5，默认空格' '填充
In [672]: s.rjust(5)
Out[672]: '  abc'
In [673]: s.ljust(5)
Out[673]: 'abc  '
# 设置填充字符，注：只能为单个字符，如'0'
In [674]: s.ljust(5,0)
In [675]: s.ljust(5,'0')
Out[675]: 'abc00'
# 中间对齐
In [678]: s.center(5,'0')
Out[678]: '0abc0'
In [679]: s.center(6,'0')
Out[679]: '0abc00'
```

22\. 字符串删除左右空格

```python
In [683]: s1
Out[683]: '  abc  '
# 删除两边空格
In [684]: s1.strip()
Out[684]: 'abc'
# 删除左边空格
In [685]: s1.lstrip()
Out[685]: 'abc  '
# 删除右边空格
In [686]: s1.rstrip()
Out[686]: '  abc'
# 也可以删除其他指定字符（串）
In [690]: s
Out[690]: 'abc'
In [691]: s.rstrip('c')
Out[691]: 'ab'
In [692]: s.rstrip('bc')
Out[692]: 'a'
```

23\. dict赋值

```python
In [847]: dict(color = '#17BECF')
Out[847]: {'color': '#17BECF'}
```

24\. 带参数print

```python
In [852]: print('Hello, {}!' .format('world'))
Hello, world!
```

25\. 从两个list构造字典

```python
>>> lst1 = list('abc')
>>> lst2 = list(range(3))
>>> lst1
['a', 'b', 'c']
>>> lst2
[0, 1, 2]
>>> dict(zip(lst1,lst2))
{'a': 0, 'b': 1, 'c': 2}
```

26\. re只保留字符串中的中文

```python
>>> p = re.compile(r'[^\u4e00-\u9fa5]', re.S) # re.S表示包括换行符 
>>> re.sub(p,'',"sd中文df")
'中文'
```

27\. string中substring出现次数

```python
>>> "aaabc".count("a")
3
>>> "aaabc".count("aa")
1
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
- `%reset`：删除所有已定义的变量

3\. 查看所有已定义的变量：`who`

4\. pprint 格式化打印，用于打印json变量等

```python
from pprint import pprint
pprint(j)
# 对于dictionary类型，pprint会自动排序，为避免排序，可使用OrderedDict
>>> from collections import OrderedDict
>>> lst1 = list('abcdefg')
>>> lst2 = list(range(7))
>>> dic = OrderedDict(zip(lst1,lst2))
>>> pprint(dic)
OrderedDict([('a', 0),
             ('b', 1),
             ('c', 2),
             ('d', 3),
             ('e', 4),
             ('f', 5),
             ('g', 6)])
```



Pandas
==============

1\. 删除DataFrame中某列／行：（df.drop()或del df['col_name']，注：不能用del df.col_name，inplace=True可用）

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
# e.g. 使用默认列名0,1,2
df.append([[1,2,3]])
# 非默认列名使用字典
In [716]: df
Out[716]:
  col1  col2
0    a   1.0
1    b   2.0
2    c   3.0
In [717]: df = df.append([{'col1':'d','col2':4}]) # 没有inplace选项
In [718]: df
Out[718]:
  col1  col2
0    a   1.0
1    b   2.0
2    c   3.0
0    d   4.0
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
# 按条件选取sub-df
In [575]: df
Out[575]:
  col1  col2
0    a     0
1    b     1
2    c     2
In [576]: df[df.col2 > 1]
Out[576]:
  col1  col2
2    c     2
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
# 字典填充
In [594]: df.fillna({'col1':'b','col2':3.0},inplace=True)
In [595]: df
Out[595]:
  col1  col2
0    a   1.0
1    b   2.0
2    c   3.0
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

13\. 表连接pd.merge

```python
In [31]: df1                                          
Out[31]:
  col1  data1
0    a      0
1    b      1
In [32]: df2                                         
Out[32]:
  col1  data2
0    a      3
1    d      4
# 自动寻找相同名称的列
In [33]: pd.merge(df1,df2)                                         
Out[33]:
  col1  data1  data2
0    a      0      3
# 指定连接key，连接方式
In [47]: pd.merge(df1,df2,on=['col1'],how='left')
Out[47]:
  col1  data1  data2
0    a      0    3.0
1    b      1    NaN
In [48]: pd.merge(df1,df2,on=['col1'],how='outer')
Out[48]:
  col1  data1  data2
0    a    0.0    3.0
1    b    1.0    NaN
2    d    NaN    4.0
```

14\. dataframe查看所有列名称

```python
In [115]: df
Out[115]:
  col1  data1
0    a      0
1    b      1
In [116]: list(df)
Out[116]: ['col1', 'data1']
In [117]: df.columns
Out[117]: Index(['col1', 'data1'], dtype='object')
``` 

15\. 查看dataframe所有列的类型：`df.dtypes`

16\. 查看dataframe行数列数：

```python
In [121]: df1.shape
Out[121]: (2, 2)
```

17\. 统计某一列频数：value_counts()

```python
In [128]: df = pd.DataFrame({'col1':['a','a','b']})
In [129]: df
Out[129]:
  col1
0    a
1    a
2    b
In [130]: df.col1.value_counts() 
Out[130]:
a    2
b    1
Name: col1, dtype: int64
# 归一化参数：normalize=True
# 是否考虑空值：dropna=False
```

18\. 判断dataframe中的空值：`.isnull()`或`.isna()`

```python
In [171]: df = pd.DataFrame({'col1':['a','b',None]})
In [172]: df
Out[172]:
   col1
0     a
1     b
2  None
In [173]: df.isnull()
Out[173]:
    col1
0  False
1  False
2   True
In [174]: df.isna()
Out[174]:
    col1
0  False
1  False
2   True
In [487]: df.isnull().values.any()
Out[487]: True
```

19\. 查看dataframe中所有列的空值数量情况及所有列的类型：

```python
# 查看df整体信息
df.info()
# 查看每列空值数量
df.isnull().sum(axis=0)
# 查看每行空值数量
df.isnull().sum(axis=1)
```

20\. dataframe拼接：

```python
In [203]: df1
Out[203]:
  col1  data1
0    a      0
1    b      1
# 按列拼接
In [205]: pd.concat([df1,df1])
Out[205]:
  col1  data1
0    a      0
1    b      1
0    a      0
1    b      1
# 按行拼接
In [209]: pd.concat([df1,df1],axis=1)
Out[209]: 
  col1  data1 col1  data1
0    a      0    a      0
1    b      1    b      1
# 与merge对比
In [213]: df1.merge(df2)
Out[213]:
  col1  data1  data2
0    a      0      3
```

21\. 查看dataframe数值类型列基本信息

```python
In [218]: df1
Out[218]:
  col1  data1
0    a      0
1    b      1
In [219]: df1.describe()
Out[219]:
          data1
count  2.000000
mean   0.500000
std    0.707107
min    0.000000
25%    0.250000
50%    0.500000
75%    0.750000
max    1.000000
```

21\. 选取dataframe的某一个（组）元素：.iloc, .loc

```python
In [246]: df3 = pd.DataFrame({'col1':range(3),'col2':list('abc'),'col3':range(3,6)})
In [247]: df3
Out[247]:
   col1 col2  col3
0     0    a     3
1     1    b     4
2     2    c     5
# 元素(1,2)
In [248]: df3.iloc[1,2]
Out[248]: 4
# 选取第2行
In [249]: df3.iloc[[1],:] # 或：.loc[[1]]
Out[249]:
   col1 col2  col3
1     1    b     4
# 如果将上例中[1]换成1
In [490]: df3.iloc[1] # 或：.loc[1]
Out[490]:
col1    1
col2    b
col3    4
Name: 1, dtype: object
# 第2行之后的行
In [250]: df3.iloc[1:]
Out[250]:
   col1 col2  col3
1     1    b     4
2     2    c     5
# 选取第2列
In [506]: df3.iloc[:,1]
Out[506]:
0    a
1    b
2    c
Name: col2, dtype: object
# 重置index
In [262]: df3.set_index([['row1','row2','row3']],inplace=True)
In [263]: df3
Out[263]:
      col1 col2  col3
row1     0    a     3
row2     1    b     4
row3     2    c     5
# .loc按行、列名称选取
In [264]: df3.loc['row2':'row3','col1':'col2']
Out[264]:
      col1 col2
row2     1    b
row3     2    c
```

22\. dataframe分组： groupby

```python
In [287]: df = pd.DataFrame({'col1':list('aab'),'col2':range(2,5)})
In [288]: df
Out[288]:
  col1  col2
0    a     2
1    a     3
2    b     4
# 默认分组的列作为index
In [289]: df.groupby(['col1']).sum()
Out[289]:
      col2
col1      
a        5
b        4
# 不将分组列作为index咧
In [290]: df.groupby(['col1'],as_index=False).sum()
Out[290]:
  col1  col2
0    a     5
1    b     4
```

23\. dataframe转换成numpy matrix：np.mat()或者np.matrix()

```python
In [369]: np.mat(df)
Out[369]:
matrix([['a', 2],
        ['a', 3],
        ['b', 4]], dtype=object)
In [370]: np.matrix(df)
Out[370]:
matrix([['a', 2],
        ['a', 3],
        ['b', 4]], dtype=object)
```

24\. 重置行号index # 可用inplace

```python
In [433]: df
Out[433]:
   c1  c2
0   0   1
1   2   3
0   0   1
1   2   3
# 默认原行号变成新列
In [434]: df.reset_index()
Out[434]:
   index  c1  c2
0      0   0   1
1      1   2   3
2      0   0   1
3      1   2   3
# 使用drop=True丢弃原行号
In [435]: df.reset_index(inplace=True,drop=True)
In [436]: df
Out[436]:
   c1  c2
0   0   1
1   2   3
2   0   1
3   2   3
```

25\. dataframe去重

```python
In [452]: df
Out[452]:
   c1  c2
0   0   1
1   2   3
2   0   1
3   2   3
In [455]: df.drop_duplicates(inplace=True)
In [456]: df
Out[456]:
   c1  c2
0   0   1
1   2   3
```

26\. timestamp与date, time相互转换

```python
# time tuple转换成timestamp
In [464]: import time
In [465]: time_tuple = (2008, 11, 12, 13, 59, 27, 2, 317, 0)
In [466]: timestamp = time.mktime(time_tuple)
In [467]: timestamp
Out[467]: 1226469567.0
# timestamp转换成日期时间datetime.datetime类型
In [528]: datetime.fromtimestamp(1463352000)
Out[528]: datetime.datetime(2016, 5, 16, 6, 40)
# 从timestamp中抽取date
In [525]: from datetime import datetime
In [526]: datetime.fromtimestamp(1463352000).date()
Out[526]: datetime.date(2016, 5, 16)
# 表示成string date
In [527]: datetime.fromtimestamp(1463352000).date().strftime('%Y-%m-%d')
Out[527]: '2016-05-16'
# string类型转换成datetime.datetime类型
In [530]: datetime.strptime('2016-05-16 06:40:00Z', '%Y-%m-%d %H:%M:%SZ')
Out[530]: datetime.datetime(2016, 5, 16, 6, 40)
# string 类型转换成timestamp类型
In [560]: time.mktime(datetime.strptime('2016-05-16 06:40:00Z', "%Y-%m-%d %H:%M:%SZ").timetuple())
Out[560]: 1463352000.0
```

27\. 重置dataframe列顺序

```python
In [510]: df3
Out[510]:
   col1 col2  col3
0     0    a     3
1     1    b     4
2     2    c     5
In [514]: df3 = df3[['col2','col1','col3']]
In [515]: df3
Out[515]:
   col2 col1  col3
0     0    a     3
1     1    b     4
2     2    c     5
```

28\. 在某位置插入一列

```python
In [520]: df3.insert(1,'new_col',list('bcd'))
In [521]: df3
Out[521]:
  col1 new_col  col2  col3
0    a       b     0     3
1    b       c     1     4
2    c       d     2     5
```

29\. 增加空列

```python
In [545]: df1
Out[545]:
  col1  col2 col3
0    a     0
1    b     1
# 增加一列np.nan空值
In [546]: df1['col3'] = np.nan
In [547]: df1
Out[547]:
  col1  col2  col3
0    a     0   NaN
1    b     1   NaN
# 增加一列空string''
In [548]: df1['col4'] = ''
In [549]: df1
Out[549]:
  col1  col2  col3 col4
0    a     0   NaN     
1    b     1   NaN
# 增加一列None值
In [552]: df1['col5'] = None
In [553]: df1
Out[553]:
  col1  col2  col3 col4  col5
0    a     0   NaN       None
1    b     1   NaN       None
# 查看是否为空 注：空string''不算空None和np.nan算为空
In [554]: df1.isnull() # 或 .isnan()
Out[554]:
    col1   col2  col3   col4  col5
0  False  False  True  False  True
1  False  False  True  False  True
```

30\. dataframe所有空值用None填充

```python
df = df.where((pd.notnull(df)), None)
```

31\. pd.Series to dataframe

```python
In [850]: df2
Out[850]:
  col1  data2
0    a      3
1    d      4
In [851]: df2.col1.to_frame()
Out[851]:
  col1
0    a
1    d
```

32\. 按字符串拼接两列内容

```python
>>> df = pd.DataFrame({'col1':list('abc'),'col2':range(3)})
>>> df
  col1  col2
0    a     0
1    b     1
2    c     2
>>> df['col3'] = df['col1'].astype(str) + df['col2'].astype(str)
>>> df
  col1  col2 col3
0    a     0   a0
1    b     1   b1
2    c     2   c2
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

4\. 计算np.array范数norm（元素平方和开方，（向量）长度）

```python
In [108]: a = np.array([3,4,5,12,7,24]).reshape((3,2))
In [109]: a
Out[109]:
array([[ 3,  4],
       [ 5, 12],
       [ 7, 24]])
# 矩阵范数：矩阵所有元素平方和开方
In [110]: np.linalg.norm(a)                             
Out[110]: 28.61817604250837
# 按列求范数
In [111]: np.linalg.norm(a,axis=0)
Out[111]: array([ 9.11043358, 27.12931993])
# 按行求范数
In [112]: np.linalg.norm(a,axis=1)
Out[112]: array([ 5., 13., 25.])
```

5\. 查看np.ndarray是否含有空值：`np.isnan(df)`

6\. np.dnarray间点乘

```python
In [315]: x = np.array(range(8)).reshape((2,-1))
In [316]: x
Out[316]:
array([[0, 1, 2, 3],
       [4, 5, 6, 7]])
In [317]: y = np.array(range(1,9)).reshape((2,-1))
In [318]: y
Out[318]:
array([[1, 2, 3, 4],
       [5, 6, 7, 8]])
# ndarray转置
In [319]: y.T
Out[319]:
array([[1, 5],
       [2, 6],
       [3, 7],
       [4, 8]])
# ndarray点乘
In [320]: np.dot(x,y.T)
Out[320]:
array([[ 20,  44],
       [ 60, 148]])
```

7\. numpy matrix

```python
In [335]: arr = np.array((range(1,4),range(4,7)))
In [336]: arr
Out[336]:
array([[1, 2, 3],
       [4, 5, 6]])
# numpy ndarray转换成matrix
In [337]: m = np.mat(arr) # 或np.matrx(arr)
In [338]: m
Out[338]:
matrix([[1, 2, 3],
        [4, 5, 6]])
# matrix点乘可以用np.dot()或*的形式。注：ndarray点乘只能用np.dot()
In [339]: np.dot(m,m.T)
Out[339]:
matrix([[14, 32],
        [32, 77]])
In [340]: m*m.T
Out[340]:
matrix([[14, 32],
        [32, 77]])
# 与ndarray类型对比：'*'或'np.multiply()'表示element wise元素相乘
In [363]: arr*arr
Out[363]:
array([[ 1,  4,  9],
       [16, 25, 36]])
In [364]: np.multiply(arr,arr)
Out[364]:
array([[ 1,  4,  9],
       [16, 25, 36]])
# matrix类型的element wise元素相乘也适用np.multiply()
In [365]: np.multiply(x,x)
Out[365]:
matrix([[ 1,  4,  9],
        [16, 25, 36]])
```

8\. 浮点数四舍五入 np.rint()

```python
In [805]: a
Out[805]:
array([[8.5430416 , 3.21420658],
       [9.99185835, 9.1757228 ],
       [7.17641551, 6.84591918]])
In [806]: np.rint(a)
Out[806]:
array([[ 9.,  3.],
       [10.,  9.],
       [ 7.,  7.]])
```

9\. numpy array选取最大值元素的index

```python
In [836]: a
Out[836]:
array([[8.54, 3.21],
       [9.01, 9.18],
       [7.18, 6.85]])
# 将a展平后最大值为9.18，索引为3
In [837]: np.argmax(a)
Out[837]: 3
# 按行查看
In [838]: np.argmax(a,axis=0)
Out[838]: array([1, 1])
# 按列查看
In [839]: np.argmax(a,axis=1)
Out[839]: array([0, 1, 0])
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

2\. plt做hist计数柱状图

```python
# e.g.
plt.hist(df.col1,bins=range(0,11,1))
# 一般可以用
plt.hist(data, bins=range(min(data), max(data) + binwidth, binwidth))
```


