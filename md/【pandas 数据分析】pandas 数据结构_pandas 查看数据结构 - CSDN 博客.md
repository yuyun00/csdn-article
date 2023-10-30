> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/zhaoyuanh/article/details/126987684)

#### 文章目录

*   [Series](#Series_17)
*   *   [创建 Series](#Series_26)
    *   *   [从 dict 创建](#dict_44)
        *   [从 ndarray 创建](#ndarray_71)
        *   [从标量创建](#_100)
    *   [Series 的特性](#Series_115)
    *   *   [类 ndarray](#ndarray_117)
        *   [类 dict](#dict_195)
    *   [向量化操作与标签对齐](#_239)
    *   [名称属性](#_289)
*   [DataFrame](#DataFrame_323)
*   *   [创建 DataFrame](#DataFrame_330)
    *   *   [从 Series 的字典创建](#Series_352)
        *   [从字典对象的字典创建](#_402)
        *   [从 ndarray 或列表的字典创建](#ndarray_423)
        *   [从结构化或记录数组创建](#_457)
        *   [从字典列表创建](#_490)
        *   [从元组字典创建](#x20_515)
        *   [从一个 Series 创建](#x20Series_537)
        *   [从命名元组列表创建](#_552)
        *   [从数据类列表中创建](#_581)
        *   [其他创建方式](#_598)
        *   *   [DataFrame.from_dict](#DataFramefrom_dict_602)
            *   [DataFrame.from_records](#DataFramefrom_records_627)
    *   [DataFrame 操作](#DataFrame_646)
    *   *   [列的选择、添加和删除](#_648)
        *   [在方法链中分配新列](#_752)
        *   [索引 / 选择](#_826)
        *   [数据对齐和算数操作](#_905)
        *   [转置](#_1030)
        *   [DataFrame 与 NumPy 函数的互操作](#DataFrameNumPy_1044)

众所周知，数据结构在类库、编程语言甚至是整个计算机科学中都是极其重要的存在，它决定了数据的表达和承载能力、对数据的处理和操作的灵活度和高效性，也是一个类库或一门语言强大功能的其中一种表现。对于 pandas 这样一个专门做数据分析的类库而言，数据结构无疑是整个工具的基石，所有强大的功能和操作都是基于其数据结构实现的。前面的文章 ([pandas 概述](https://blog.csdn.net/zhaoyuanh/article/details/126741324)) 中简单提到了 pandas 中主要有两种数据结构：

*   用于表示一维数据的`Series`
    
*   用于表示二维数据的`DataFrame`
    

在这里，我们对这两种数据结构做进一步的了解。

按照惯例，现在 notebook 或者 IPython 中导入 NumPy 和 pandas：

```
import numpy as np
import pandas as pd

```

Series
------

`Series`是一维的带有标签的数组，能够保存任何数据类型的数据（整数、字符串、浮点数、Python 对象等）。`Series`中每个元素都对应一个标签，这些标签统称为**索引**。

![](https://img-blog.csdnimg.cn/f3d4a5bdcfaa4780b2d872dad5e92ec7.png#pic_center)

> 备注：虽然一个`Series`中的元素可以是不同数据类型的（异构），但通常情况下很少这么使用，大部分情况下其中的元素都是具有相同数据类型的（同质），所以当提到`Series`时，没有特别说明的话，一般指的是同质的`Series`。

### 创建 Series

创建`Series`的基本方法如下：

```
s = pd.Series(data, [index=index])

```

这里，`data`可以是很多不同的数据类型：

*   Python 字典
    
*   `ndarray`
    
*   标量
    

传递的`index`是一个标签的列表，该参数是可选的，最终如何生成索引则取决于`data`是什么类型的数据。

#### 从 dict 创建

```
In [3]: d = {'b': 1, 'a': 0, 'c': 2}

In [4]: pd.Series(d)
Out[4]:
b    1
a    0
c    2
dtype: int64

```

从上面的代码和输出可以看出，当没有传递`index`参数时，`Series`的索引为字典的`key`，索引对应的值为字典的`value`。另外根据官方文档的说法，如果 Python 版本≥3.6 且 pandas 版本≥0.23，那么索引将按`dict`的插入顺序排序。如果 Python 版本 < 3.6 或 pandas 版本 < 0.23，则索引将按字典键的字母顺序排序。

如果指定了`index`，则只有和`index`中的标签对应的字典中的值才会被取出用于创建`Series`，如果某个标签不是字典的键，则这个标签对应的值为`NaN`（not a number，在 pandas 中用于标记缺失数据）

```
In [5]: pd.Series(d, index=['b', 'c', 'd', 'a'])
Out[5]:
b    1.0
c    2.0
d    NaN
a    0.0
dtype: float64

```

#### 从 [ndarray](https://so.csdn.net/so/search?q=ndarray&spm=1001.2101.3001.7020) 创建

如果`data`是一个`ndarray`，如果要指定`index`，那么其长度必须和`data`长度一样。如果没有传递`index`，则会自动创建一个整数索引，其值为`[0, ..., len(data) - 1]`。

```
In [6]: s = pd.Series(np.random.randn(5), index=['a', 'b', 'c', 'd', 'e'])

In [7]: s
Out[7]:
a    0.554556
b    0.153393
c   -0.052892
d    0.153844
e   -0.428457
dtype: float64

In [8]: s.index
Out[8]: Index(['a', 'b', 'c', 'd', 'e'], dtype='object')

In [9]: pd.Series(np.random.randn(5))
Out[9]:
0   -0.189954
1   -1.135869
2    0.034719
3    1.603313
4    0.654774
dtype: float64

```

#### 从标量创建

如果`data`是标量值，则必须提供索引。该值将会重复以匹配索引的长度。

```
In [10]: pd.Series(5.0, index=['a', 'b', 'c', 'd', 'e'])
Out[10]:
a    5.0
b    5.0
c    5.0
d    5.0
e    5.0
dtype: float64

```

### Series 的特性

#### 类 ndarray

`Series`的行为和`ndarray`非常类似，它是大多数 NumPy 函数的有效参数。但是，切片等操作也会对索引进行切片。

```
In [11]: s
Out[11]:
a    0.554556
b    0.153393
c   -0.052892
d    0.153844
e   -0.428457
dtype: float64

In [12]: s[0]
Out[12]: 0.5545560665697387

In [13]: s[:3]
Out[13]:
a    0.554556
b    0.153393
c   -0.052892
dtype: float64

In [14]: s[s > s.median()]
Out[14]:
a    0.554556
d    0.153844
dtype: float64

In [15]: s[[4, 3, 1]]
Out[15]:
e   -0.428457
d    0.153844
b    0.153393
dtype: float64

In [16]: np.exp(s)
Out[16]:
a    1.741168
b    1.165783
c    0.948482
d    1.166309
e    0.651514
dtype: float64

```

与 NumPy 数组一样，`Series`也有`dtype`属性，并且一般情况下其类型就是 NumPy 的 dtype 类型：

```
In [17]: s.dtype
Out[17]: dtype('float64')

In [18]: type(s.dtype)
Out[18]: numpy.dtype[float64]


```

如果需要获取`Series`底层的数组，可以用`Series.array`方法：

```
In [19]: s.array
Out[19]:
<PandasArray>
[  0.5545560665697387,  0.15339337717045246, -0.05289228660105809,
  0.15384372819622366,  -0.4284568761531815]
Length: 5, dtype: float64

```

当你需要在没有索引的情况下执行某些操作时（例如，禁用自动对齐），访问数组会很有用。

你也可以通过`Series.to_numpy()`方法获取对应的 NumPy 数组`ndarray`：

```
In [20]: s.to_numpy()
Out[20]: array([ 0.55455607,  0.15339338, -0.05289229,  0.15384373, -0.42845688])

```

#### 类 dict

前面看到了可以从字典创建`Series`，很巧合的是（或者也是 pamdas 开发者别有用心的设计），原始的字典和通过该字典创建的`Series`在数据的获取和设置方面基本一样（除了字典可以添加新的`key`）。也就是说，`Series`类似于固定大小的`dict`，可以通过索引标签获取和设置值：

```
In [21]: s['a']
Out[21]: 0.5545560665697387

In [22]: s['e'] = 12.0

In [23]: s
Out[23]:
a     0.554556
b     0.153393
c    -0.052892
d     0.153844
e    12.000000
dtype: float64

# 注意，这里是查看e是否在s的索引中，而不是s的值
In [24]: 'e' in s
Out[24]: True

In [25]: 'f' in s
Out[25]: False

```

跟字典一样，如果没有相应的标签，也会抛出`KeyError`的异常：

```
In [25]: s['f']
KeyError: 'f'


```

使用`get`方法，获取`Series`中没有的标签的值将返回`None`或指定的默认值：

```
In [26]: s.get('f')

In [27]: s.get('f', np.nan)
Out[27]: nan

```

### [向量化](https://so.csdn.net/so/search?q=%E5%90%91%E9%87%8F%E5%8C%96&spm=1001.2101.3001.7020)操作与标签对齐

使用原始 NumPy 数组时，通常不需要逐个值循环。在 pandas 中使用 Series 时也是如此。Series 也可以传递给大多数需要 ndarray 的 NumPy 方法。

```
In [28]: s + s
Out[28]:
a     1.109112
b     0.306787
c    -0.105785
d     0.307687
e    24.000000
dtype: float64

In [29]: s * 2
Out[29]:
a     1.109112
b     0.306787
c    -0.105785
d     0.307687
e    24.000000
dtype: float64

In [30]: np.square(s)
Out[30]:
a      0.307532
b      0.023530
c      0.002798
d      0.023668
e    144.000000
dtype: float64

```

`Series`和`ndarray`之间的一个关键区别是`Series`之间的运算会根据标签自动对齐数据，也就是运算是在具有相同标签的元素之间进行的。

```
In [31]: s[1:] + s[:-1]
Out[31]:
a         NaN
b    0.306787
c   -0.105785
d    0.307687
e         NaN
dtype: float64

```

未对齐`Series`之间的运算结果的索引是所有涉及到的`Series`的索引的并集。如果有标签在其中一个`Series`中没找到，那么在结果中该标签对应的值为`NaN`。无需进行任何明确的数据对齐即可编写代码，这为交互式数据分析和研究提供了极大的自由度和灵活性。pandas 数据结构集成的数据对齐功能使 pandas 有别于其他大多数用于处理标签数据的相关工具。

通常，为了避免信息丢失，我们会选择具有不同索引的对象做计算之后的默认结果，这个结果的索引为参与计算的对象索引的并集，结果中会有某些标签的值被标记为确实数据（`NaN`），因为这些标签不是其中一个或多个对象中的标签。而这样的数据通常也是计算结果的重要信息。如果你不需要这些缺失数据，可以通过`dropna`函数将这些缺失数据极其标签丢弃掉。

### 名称属性

`Series`有一个`name`属性：

```
In [32]: s = pd.Series(np.random.randn(5), name='something')

In [32]: s
Out[32]:
0   -0.033113
1   -1.375810
2    1.026355
3    1.023575
4   -0.522689
Name: something, dtype: float64

In [33]: s.name
Out[33]: 'something'

```

在很多情况下，`Series`的`name`属性可以被自动赋值，特别是当从`DataFrame`中选择单个列时，`name`将被赋值为对应的列标签。

可以用方法`Series.rename()`对`Series`重命名，该方法会返回一个新的`Series`对象：

```
In [34]: s2 = s.rename('different')

In [35]: s2.name
Out[35]: 'different'

In [36]: s.name
Out[36]: 'something'

```

DataFrame
---------

`DataFrame`是最常使用的 pandas 对象。它是一种二维的有标签的数据结构，其列可能具有不同的数据类型。你可以将其视为电子表格、SQL 表或者`Series`对象的字典。

![](https://img-blog.csdnimg.cn/cb22c498d5e14f85b13c1d6e89cd599a.png#pic_center)

### [创建 DataFrame](https://so.csdn.net/so/search?q=%E5%88%9B%E5%BB%BADataFrame&spm=1001.2101.3001.7020)

创建`DataFrame`的基本方法如下：

```
df = pd.DataFrame(data, [index=index, columns=columns])

```

与`Series`一样，`DataFrame`接受多种不同类型的输入`data`：

*   一维`ndarray`、列表、字典、`Series`等对象的字典
    
*   二维 NumPy `ndarray`
    
*   [结构化或记录 ndarray](https://numpy.org/doc/stable/user/basics.rec.html "结构化或记录ndarray")
    
*   一个`Series`
    
*   另一个`DataFrame`
    

除了`data`之外，可选的，还可以传递`index`（行标签）和`columns`（列标签）参数。如果传递了索引和 / 或列参数，那么生成的`DataFrame`对象的索引和 / 或列就是你所指定的索引和 / 或列。如果没有传递轴标签，那么将根据默认规则从输入数据中构造出来。

#### 从 Series 的字典创建

在这里，结果的索引是各个`Series`索引的并集。因此，可能存在索引的某些标签不在某些`Series`中，那么在生成的`DataFrame`中，这些标签对应的这些`Series`列的值会被设置为`NaN`。

```
In [37]: d = {
    ...:     "one": pd.Series([1.0, 2.0, 3.0], index=["a", "b", "c"]),
    ...:     "two": pd.Series([1.0, 2.0, 3.0, 4.0], index=["a", "b", "c", "d"]),
    ...: }

# 没有传递索引和列，则结果的索引为各个Series索引的并集，列是字典的键
In [38]: df = pd.DataFrame(d)

In [39]: df
Out[39]:
   one  two
a  1.0  1.0
b  2.0  2.0
c  3.0  3.0
d  NaN  4.0

```

```
# 指定index，Series中匹配标签的数据会被取出，没有匹配的标签的值为NaN
In [40]: pd.DataFrame(d, index=["d", "b", "a"])
Out[40]:
   one  two
d  NaN  4.0
b  2.0  2.0
a  1.0  1.0

# 同时指定了索引和列，同样的，如果字典中没有和指定列标签匹配的键，则结果中该列标签对应的列值都为NaN
In [41]: pd.DataFrame(d, index=["d", "b", "a"], columns=["two", "three"])
Out[41]:
   two three
d  4.0   NaN
b  2.0   NaN
a  1.0   NaN

```

行标签和列标签（也是列名）分别可以通过`index`和`columns`属性访问：

```
In [42]: df.index
Out[42]: Index(['a', 'b', 'c', 'd'], dtype='object')

In [43]: df.columns
Out[43]: Index(['one', 'two'], dtype='object')

```

#### 从字典对象的字典创建

这里，这些嵌套字典会先被转换为`Series`，再从`Series`的字典创建`DataFrame`。

```
In [44]: dd = {
    ...:     'one': {'a':1, 'b':2},
    ...:     'two': {'c':3, 'd':4}}

In [45]: dd
Out[45]: {'one': {'a': 1, 'b': 2}, 'two': {'c': 3, 'd': 4}}

In [46]: pd.DataFrame(dd)
Out[46]:
   one  two
a  1.0  NaN
b  2.0  NaN
c  NaN  3.0
d  NaN  4.0

```

#### 从 ndarray 或列表的字典创建

`ndarray`或列表的长度必须相同。如果指定了索引，则索引的长度也必须和数组 / 列表的长度相同。如果没有传递索引，则会自动创建一个整数索引`range(n)`，n 为数组 / 列表的长度。

```
In [47]: d = {'one': np.array([1.0, 2.0, 3.0, 4.0]), 'two': np.array([4.0, 3.0, 2.0, 1.0])}

In [48]: pd.DataFrame(d)
Out[48]:
   one  two
0  1.0  4.0
1  2.0  3.0
2  3.0  2.0
3  4.0  1.0

In [49]: pd.DataFrame(d, index=['a', 'b', 'c', 'd'])
Out[49]:
   one  two
a  1.0  4.0
b  2.0  3.0
c  3.0  2.0
d  4.0  1.0

In [50]: d = {'one': [1.0, 2.0, 3.0, 4.0], 'two': [4.0, 3.0, 2.0, 1.0]}

In [51]: pd.DataFrame(d)
Out[51]:
   one  two
0  1.0  4.0
1  2.0  3.0
2  3.0  2.0
3  4.0  1.0

```

#### 从结构化或记录数组创建

这种情况的处理方式与数组的字典相同。

```
In [52]: data = np.array([(1, 2.0, "Hello"), (2, 3.0, "World")], dtype=[("A", "i4"), ("B", "
    ...: f4"), ("C", "a10")])

In [53]: data
Out[53]:
array([(1, 2., b'Hello'), (2, 3., b'World')],
      dtype=[('A', '<i4'), ('B', '<f4'), ('C', 'S10')])

In [54]: pd.DataFrame(data)
Out[54]:
   A    B         C
0  1  2.0  b'Hello'
1  2  3.0  b'World'

In [55]: pd.DataFrame(data, index=["first", "second"])
Out[55]:
        A    B         C
first   1  2.0  b'Hello'
second  2  3.0  b'World'

In [56]: pd.DataFrame(data, columns=["C", "A", "B"])
Out[56]:
          C  A    B
0  b'Hello'  1  2.0
1  b'World'  2  3.0


```

#### 从字典列表创建

```
In [57]: data2 = [{"a": 1, "b": 2}, {"a": 5, "b": 10, "c": 20}]

In [58]: pd.DataFrame(data2)
Out[58]:
   a   b     c
0  1   2   NaN
1  5  10  20.0

In [59]: pd.DataFrame(data2, index=["first", "second"])
Out[59]:
        a   b     c
first   1   2   NaN
second  5  10  20.0

In [60]: pd.DataFrame(data2, columns=["a", "b"])
Out[60]:
   a   b
0  1   2
1  5  10


```

#### 从元组字典创建

可以通过传递元组字典来自动创建有多级索引的`DataFrame`

```
In [61]: pd.DataFrame(
    ...:     {
    ...:         ('a', 'b'): {('A', 'B'): 1, ('A', 'C'): 2},
    ...:         ('a', 'a'): {('A', 'C'): 3, ('A', 'B'): 4},
    ...:         ('a', 'c'): {('A', 'B'): 5, ('A', 'C'): 6},
    ...:         ('b', 'a'): {('A', 'C'): 7, ('A', 'B'): 8},
    ...:         ('b', 'b'): {('A', 'D'): 9, ('A', 'B'): 10},
    ...:     }
    ...: )
Out[61]:
       a              b
       b    a    c    a     b
A B  1.0  4.0  5.0  8.0  10.0
  C  2.0  3.0  6.0  7.0   NaN
  D  NaN  NaN  NaN  NaN   9.0

```

#### 从一个 Series 创建

从一个`Series`创建的`DataFrame`只有一列数据，列的名称是`Series`的原始名称（在没有提供其他列名时），其索引与输入的`Series`相同。

```
In [62]: ser = pd.Series(range(3), index=list('abc'), name='ser')

In [63]: pd.DataFrame(ser)
Out[63]:
   ser
a    0
b    1
c    2

```

#### 从命名元组列表创建

列表中第一个`namedtuple`的字段名决定了`DataFrame`的列。之后的命名元组（或元组）就只是简单地将值拆包并填充到`DataFrame`的行。如果这些元组中的任何一个的长度比第一个`namedtuple`短，则相应行中后面的列将标记为缺失值。但如果有任意一个比第一个`namedtuple`长，则会抛出`ValueError`。

```
In [64]: from collections import namedtuple

In [65]: Point = namedtuple("Point", "x y")

# 列由第一个命名元组Point(0, 0)的字段决定，后续的元组可以是命名元组，也可以是普通元组
In [66]: pd.DataFrame([Point(0, 0), Point(0, 3), (2, 3)])
Out[66]:
   x  y
0  0  0
1  0  3
2  2  3

In [67]: Point3D = namedtuple("Point3D", "x y z")

# 第一个元组决定了生成的DataFrame由3列（x，y，z），而列表中第三个元组长度为2
# 所以在DataFrame的第三行中，第三列的值为NaN
In [68]: pd.DataFrame([Point3D(0, 0, 0), Point3D(0, 3, 5), Point(2, 3)])
Out[68]:
   x  y    z
0  0  0  0.0
1  0  3  5.0
2  2  3  NaN

```

#### 从数据类列表中创建

向`DataFrame`的构造函数传递数据类列表等价于传递字典列表，但是要注意的是，列表中的所有值都应该是数据类，在列表中混合类型会导致`TypeError`异常。

```
In [69]: from dataclasses import make_dataclass

In [70]: Point = make_dataclass('Point', [('x', int), ('y', int)])

In [71]: pd.DataFrame([Point(0, 0), Point(0, 3), Point(2, 3)])
Out[71]:
   x  y
0  0  0
1  0  3
2  2  3

```

#### 其他创建方式

除了使用类构造函数创建`DataFrame`对象，`DataFrame`类本身也提供了一些类方法用于创建对象

##### DataFrame.from_dict

接受一个字典的字典或类数组序列的字典作为参数并放回一个`DataFrame`。它的行为类似于`DataFrame`构造函数，不同的是，它有一个`orient`参数，默认值为`columns`，也可以设置为`index`，从而将字典的`key`用作行标签。

```
In [72]: pd.DataFrame.from_dict(dict([('A', [1, 2, 3]), ('B', [4, 5, 6])]))
Out[72]:
   A  B
0  1  4
1  2  5
2  3  6

# 传递index给orient参数，字典key会变成行标签
In [73]: pd.DataFrame.from_dict(
     ...:     dict([('A', [1, 2, 3]), ('B', [4, 5, 6])]),
     ...:     orient='index',
     ...:     columns=['one', 'two', 'three'],
     ...: )
Out[73]:
   one  two  three
A    1    2      3
B    4    5      6


```

##### DataFrame.from_records

接受一个元组或结构化数组的列表作为参数，其工作原理和普通的`DataFrame`构造函数类似，不同的是，生成的`DataFrame`的索引可以是结构化数据类型的特定字段。

```
In [74]: data
Out[74]:
array([(1, 2., b'Hello'), (2, 3., b'World')],
      dtype=[('A', '<i4'), ('B', '<f4'), ('C', 'S10')])

# 指定字段C的数据作为index
In [75]: pd.DataFrame.from_records(data, index='C')
Out[75]:
          A    B
C
b'Hello'  1  2.0
b'World'  2  3.0

```

### DataFrame 操作

#### 列的选择、添加和删除

和`Series`一样，`DataFrame`也和字典类似，你可以在语义上将其视为`Series`对象的字典，这些`Series`对象共享相同的索引。获取、设置和删除列的语法与对应的字典操作相同：

```
In [76]: df['one']
Out[76]:
a    1.0
b    2.0
c    3.0
d    NaN
Name: one, dtype: float64

In [77]: df['three'] = df['one'] * df['two']

In [77]: df['flag'] = df['one'] > 2

In [78]: df
Out[78]:
   one  two  three   flag
a  1.0  1.0    1.0  False
b  2.0  2.0    4.0  False
c  3.0  3.0    9.0   True
d  NaN  4.0    NaN  False

```

可以像字典操作那样将列删除或弹出：

```
In [79]: del df['two']

In [80]: three = df.pop('three')

In [81]: df
Out[81]:
   one   flag
a  1.0  False
b  2.0  False
c  3.0   True
d  NaN  False

In [82]: three
Out[82]:
a    1.0
b    4.0
c    9.0
d    NaN
Name: three, dtype: float64

```

插入标量值时，这个值会填充整个列：

```
In [83]: df
Out[83]:
   one   flag  foo
a  1.0  False  bar
b  2.0  False  bar
c  3.0   True  bar
d  NaN  False  bar

```

当插入一个和`DataFrame`索引不同的`Series`时，只有匹配的标签的值会被保留，不在`DataFrame`索引中的标签值则被丢弃，`Series`索引中没有的标签值则设为`NaN`。

```
In [84]: df['one_trunc'] = pd.Series([1,2,3,4], index=list('acef'))

In [85]: df
Out[85]:
   one   flag  foo  one_trunc
a  1.0  False  bar        1.0
b  2.0  False  bar        NaN
c  3.0   True  bar        2.0
d  NaN  False  bar        NaN

```

也可以插入原始`ndarray`，但其长度必须与`DataFrame`索引的长度匹配。

```
In [86]: df['array'] = np.array([5, 6, 7, 8])

In [87]: df
Out[87]:
   one   flag  foo  one_trunc  array
a  1.0  False  bar        1.0      5
b  2.0  False  bar        NaN      6
c  3.0   True  bar        2.0      7
d  NaN  False  bar        NaN      8

```

默认情况下，列被插入到末尾，可以使用`DataFrame.insert()`方法在列中的特定位置插入：

```
In [88]: df.insert(1, 'bar', df['one'])

In [89]: df
Out[89]:
   one  bar   flag  foo  one_trunc  array
a  1.0  1.0  False  bar        1.0      5
b  2.0  2.0  False  bar        NaN      6
c  3.0  3.0   True  bar        2.0      7
d  NaN  NaN  False  bar        NaN      8

```

#### 在方法链中分配新列

`DataFrame`有一个`assign()`方法，可以轻松地创建从现有列派生的新列。

```
In [90]: iris = pd.DataFrame({
     ...:     'SepalLength': [5.1, 4.9, 4.7, 4.6, 5.0],
     ...:     'SepalWidth': [3.5, 3.0, 3.2, 3.1, 3.6],
     ...:     'PetalLength': [1.4, 1.4, 1.3, 1.5, 1.4],
     ...:     'PetalWidth': [0.2, 0.2, 0.2, 0.2, 0.2],
     ...:     'Name': 'Iris-setosa'
     ...: })

In [91]: iris
Out[91]:
   SepalLength  SepalWidth  PetalLength  PetalWidth         Name
0          5.1         3.5          1.4         0.2  Iris-setosa
1          4.9         3.0          1.4         0.2  Iris-setosa
2          4.7         3.2          1.3         0.2  Iris-setosa
3          4.6         3.1          1.5         0.2  Iris-setosa
4          5.0         3.6          1.4         0.2  Iris-setosa

In [92]: iris.assign(sepal_ratio=iris['SepalWidth'] / iris['SepalLength'])
Out[92]:
   SepalLength  SepalWidth  PetalLength  PetalWidth         Name  sepal_ratio
0          5.1         3.5          1.4         0.2  Iris-setosa     0.686275
1          4.9         3.0          1.4         0.2  Iris-setosa     0.612245
2          4.7         3.2          1.3         0.2  Iris-setosa     0.680851
3          4.6         3.1          1.5         0.2  Iris-setosa     0.673913
4          5.0         3.6          1.4         0.2  Iris-setosa     0.720000

```

也可以传递一个只接受一个参数的函数对象，在这个过程中，调用`assign`方法的`DataFrame`对象会被传递给这个函数，由这个函数产生新列：

```
In [93]: iris.assign(sepal_ratio=lambda x: (x['SepalWidth'] / x['SepalLength']))
Out[93]:
   SepalLength  SepalWidth  PetalLength  PetalWidth         Name  sepal_ratio
0          5.1         3.5          1.4         0.2  Iris-setosa     0.686275
1          4.9         3.0          1.4         0.2  Iris-setosa     0.612245
2          4.7         3.2          1.3         0.2  Iris-setosa     0.680851
3          4.6         3.1          1.5         0.2  Iris-setosa     0.673913
4          5.0         3.6          1.4         0.2  Iris-setosa     0.720000

```

`assing()`返回的是**数据的副本**并插入新列，原始 DataFrame 保持不变。

当你没有对`DataFrame`的引用时，传递可调用对象（而不是要插入的实际值）非常有用，在操作链中使用`assign()`时这是很常见的事情。例如下面的操作（过滤、计算新列、绘图）：

```
In [94]: (
     ...:     iris.query("SepalLength > 5")
     ...:     .assign(
     ...:         SepalRatio=lambda x: x.SepalWidth / x.SepalLength,
     ...:         PetalRatio=lambda x: x.PetalWidth / x.PetalLength,
     ...:     )
     ...:     .plot(kind="scatter", x="SepalRatio", y="PetalRatio")
     ...: )
Out[94]: <AxesSubplot:xlabel='SepalRatio', ylabel='PetalRatio'>

```

`assign()`方法还有一个特性，它允许依赖赋值，在提供给`assign`方法的参数中，参数表达式是按顺序从左到右进行计算的，后面的表达式可以引用前面已经创建的列：

```
In [95]: dfa = pd.DataFrame({'A': [1, 2, 3], 'B': [4, 5, 6]})

In [96]: dfa.assign(C=lambda x: x['A'] + x['B'], D=lambda x: x['A'] + x['C'])
Out[96]:
   A  B  C   D
0  1  4  5   6
1  2  5  7   9
2  3  6  9  12

```

#### 索引 / 选择

基本的索引方式如下：

<table><thead><tr><th>操作</th><th>语法</th><th>结果</th></tr></thead><tbody><tr><td>选择列</td><td><code>df[col]</code>或<code>df.col</code></td><td><code>Series</code></td></tr><tr><td>按标签选择行</td><td><code>df.loc[label]</code></td><td><code>Series</code></td></tr><tr><td>按整数位置选择行</td><td><code>df.iloc[loc]</code></td><td><code>Series</code></td></tr><tr><td>对行切片</td><td><code>df[5:10]</code></td><td><code>DataFrame</code></td></tr><tr><td>按布尔向量选择行</td><td><code>df[bool_vec]</code></td><td><code>DataFrame</code></td></tr></tbody></table>

可以看到选择列还有一个语法`df.col`，而且列名`col`必须为有效的 Python 变量名才可以用这种语法。

下面是一些示例：

```
# 选择列
In [97]: df['one']
Out[97]:
a    1.0
b    2.0
c    3.0
d    NaN
Name: one, dtype: float64

In [98]: df.one
Out[98]:
a    1.0
b    2.0
c    3.0
d    NaN
Name: one, dtype: float64

# 按标签选择行
In [99]: df.loc['a']
Out[99]:
one            1.0
bar            1.0
flag         False
foo            bar
one_trunc      1.0
array            5
Name: a, dtype: object

# 按整数位置选择行
In [100]: df.iloc[0]
Out[100]:
one            1.0
bar            1.0
flag         False
foo            bar
one_trunc      1.0
array            5
Name: a, dtype: object

# 对行切片
In [101]: df[0:2]
Out[101]:
   one  bar   flag  foo  one_trunc  array
a  1.0  1.0  False  bar        1.0      5
b  2.0  2.0  False  bar        NaN      6

# 按布尔向量选择行
In [102]: df
Out[102]:
   one  bar   flag  foo  one_trunc  array
a  1.0  1.0  False  bar        1.0      5
b  2.0  2.0  False  bar        NaN      6
c  3.0  3.0   True  bar        2.0      7
d  NaN  NaN  False  bar        NaN      8

In [103]: df[df.flag]
Out[103]:
   one  bar  flag  foo  one_trunc  array
c  3.0  3.0  True  bar        2.0      7


```

#### 数据对齐和算数操作

`DataFrame`对象之间的数据对齐在列和索引上自动对齐。与`Series`的数据对齐一样，生成的对象将具有列和行标签的并集。

```
In [104]: df = pd.DataFrame(np.random.randn(10, 4), columns=['A', 'B', 'C', 'D'])

In [105]: df2 = pd.DataFrame(np.random.randn(7, 3), columns=['A', 'B', 'C'])

In [106]: df + df2
Out[106]:
          A         B         C   D
0 -0.422414 -3.829047  0.155791 NaN
1  0.405465 -1.537037 -0.395309 NaN
2 -0.599319  0.584357 -2.368686 NaN
3 -0.931744 -1.190795 -2.078240 NaN
4  0.565281  1.239038 -0.777809 NaN
5  0.805762 -0.429504  1.629823 NaN
6  2.091268 -0.019281 -0.444887 NaN
7       NaN       NaN       NaN NaN
8       NaN       NaN       NaN NaN
9       NaN       NaN       NaN NaN

```

在`DataFrame`和`Series`之间执行操作时，默认行为是在`DataFrame`列上的对齐`Series`索引，然后逐行广播。例如：

```
In [107]: df - df.iloc[0]
Out[107]:
          A         B         C         D
0  0.000000  0.000000  0.000000  0.000000
1 -1.411174  3.171864 -0.180068 -0.764811
2 -1.774370  4.247326 -1.126468 -0.328530
3 -1.239705  3.417047 -2.341824 -1.944903
4 -0.934275  4.544993 -0.259307 -1.441780
5 -1.152844  3.030318 -0.367320 -0.017702
6  1.249936  4.775528 -0.056807 -1.413703
7 -1.148255  3.154681  1.145846 -0.666049
8 -2.037174  5.424244  1.108562  1.573868
9 -2.774255  3.373770 -0.089912  0.600061

```

与标量的算术运算则是按元素操作：

```
In [108]: df * 5 + 2
Out[108]:
           A          B         C         D
0   7.703396 -18.667260  3.099865  1.053464
1   0.647527  -2.807940  2.199523 -2.770593
2  -1.168453   2.569370 -2.532477 -0.589185
3   1.504869  -1.582027 -8.609254 -8.671051
4   3.032019   4.057706  1.803329 -6.155438
5   1.939174  -3.515669  1.263263  0.964955
6  13.953077   5.210381  2.815829 -6.015049
7   1.962119  -2.893855  8.829094 -2.276782
8  -2.482472   8.453959  8.642675  8.922804
9  -6.167879  -1.798413  2.650303  4.053769

In [109]: 1 / df
Out[109]:
            A         B          C         D
0    0.876671 -0.241929   4.546014 -5.282420
1   -3.696930 -1.039946  25.059785 -1.048088
2   -1.578057  8.781640  -1.103150 -1.931110
3  -10.098344 -1.395858  -0.471287 -0.468557
4    4.844874  2.429891 -25.423112 -0.613088
5  -82.202122 -0.906508  -6.786678 -4.830706
6    0.418302  1.557448   6.128733 -0.623826
7 -131.993123 -1.021689   0.732162 -1.169103
8   -1.115456  0.774718   0.752709  0.722251
9   -0.612154 -1.316339   7.688727  2.434549

In [110]: df ** 4
Out[110]:
              A           B          C          D
0  1.692989e+00  291.911773   0.002341   0.001284
1  5.353465e-03    0.854980   0.000003   0.828723
2  1.612534e-01    0.000168   0.675247   0.071907
3  9.616108e-05    0.263412  20.270259  20.746686
4  1.814973e-03    0.028685   0.000002   7.077976
5  2.190120e-08    1.480856   0.000471   0.001836
6  3.266171e+01    0.169960   0.000709   6.603052
7  3.294540e-09    0.917750   3.479945   0.535289
8  6.459373e-01    2.776034   3.115243   3.674923
9  7.121266e+00    0.333065   0.000286   0.028466

```

布尔运算符也是按元素操作，对相同位置的元素做布尔运算：

```
In [111]: df1 = pd.DataFrame({'a': [1, 0, 1], 'b': [0, 1, 1]}, dtype=bool)

In [112]: df2 = pd.DataFrame({'a': [0, 1, 1], 'b': [1, 1, 0]}, dtype=bool)

In [113]: df1 & df2
Out[113]:
       a      b
0  False  False
1  False   True
2   True  False

In [114]: df1 | df2
Out[114]:
      a     b
0  True  True
1  True  True
2  True  True

In [115]: df1 ^ df2
Out[115]:
       a      b
0   True   True
1   True  False
2  False   True

In [116]: -df1
Out[116]:
       a      b
0  False   True
1   True  False
2  False  False


```

#### 转置

和`ndarray`一样，要进行转置，访问`T`属性或者调用`DataFrame.transpose()`方法：

```
In [117]: df.T
Out[117]:
          0         1         2         3  ...         6         7         8         9
A  1.140679 -0.270495 -0.633691 -0.099026  ...  2.390615 -0.007576 -0.896494 -1.633576
B -4.133452 -0.961588  0.113874 -0.716405  ...  0.642076 -0.978771  1.290792 -0.759683
C  0.219973  0.039905 -0.906495 -2.121851  ...  0.163166  1.365819  1.328535  0.130061
D -0.189307 -0.954119 -0.517837 -2.134210  ... -1.603010 -0.855356  1.384561  0.410754

```

#### DataFrame 与 NumPy 函数的互操作

大多数 NumPy 函数可以在`Series`和`DataFrame`上直接调用

```
In [118]: np.abs(df)
Out[118]:
          A         B         C         D
0  1.140679  4.133452  0.219973  0.189307
1  0.270495  0.961588  0.039905  0.954119
2  0.633691  0.113874  0.906495  0.517837
3  0.099026  0.716405  2.121851  2.134210
4  0.206404  0.411541  0.039334  1.631088
5  0.012165  1.103134  0.147347  0.207009
6  2.390615  0.642076  0.163166  1.603010
7  0.007576  0.978771  1.365819  0.855356
8  0.896494  1.290792  1.328535  1.384561
9  1.633576  0.759683  0.130061  0.410754

In [119]: np.asarray(df)
Out[119]:
array([[ 1.14067923, -4.13345204,  0.21997292, -0.18930719],
       [-0.27049469, -0.961588  ,  0.03990457, -0.95411851],
       [-0.63369057,  0.11387395, -0.90649543, -0.51783703],
       [-0.09902614, -0.71640548, -2.12185072, -2.13421017],
       [ 0.20640373,  0.41154111, -0.03933429, -1.63108755],
       [-0.01216514, -1.10313382, -0.14734749, -0.20700908],
       [ 2.39061544,  0.64207621,  0.16316587, -1.60300982],
       [-0.00757615, -0.97877097,  1.36581882, -0.85535631],
       [-0.89649441,  1.29079181,  1.32853493,  1.38456089],
       [-1.63357585, -0.75968252,  0.13006054,  0.41075372]])

```

当传递两个 pandas 对象给 NumPy 函数时，会先进行对齐再执行函数操作：

```
In [120]: ser1 = pd.Series([1, 2, 3], index=['a', 'b', 'c'])

In [121]: ser2 = pd.Series([1, 3, 5], index=['b', 'a', 'c'])

In [122]: ser1
Out[122]:
a    1
b    2
c    3
dtype: int64

In [123]: ser2
Out[123]:
b    1
a    3
c    5
dtype: int64

In [124]: np.remainder(ser1, ser2)
Out[124]:
a    1
b    0
c    3
dtype: int64

```

与`Series`一样，可以使用`DataFrame.to_numpy()`方法获得相应的`ndarray`：

```
In [125]: df.to_numpy()
Out[125]:
array([[ 1.14067923, -4.13345204,  0.21997292, -0.18930719],
       [-0.27049469, -0.961588  ,  0.03990457, -0.95411851],
       [-0.63369057,  0.11387395, -0.90649543, -0.51783703],
       [-0.09902614, -0.71640548, -2.12185072, -2.13421017],
       [ 0.20640373,  0.41154111, -0.03933429, -1.63108755],
       [-0.01216514, -1.10313382, -0.14734749, -0.20700908],
       [ 2.39061544,  0.64207621,  0.16316587, -1.60300982],
       [-0.00757615, -0.97877097,  1.36581882, -0.85535631],
       [-0.89649441,  1.29079181,  1.32853493,  1.38456089],
       [-1.63357585, -0.75968252,  0.13006054,  0.41075372]])

```