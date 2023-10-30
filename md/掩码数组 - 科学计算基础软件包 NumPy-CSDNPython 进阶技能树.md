> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [edu.csdn.net](https://edu.csdn.net/skill/python02/python-3-200?category=882&typeId=17508)

> CSDN 掩码数组社区, 掩码数组论坛, 为中国软件开发者打造学习和成长的家园

在科研活动和实际工作中，我们获得的数据集往往是有缺失或被污染的，如卫星上各种载荷的传感器在某一瞬间甚至某一段时间内可能无法记录数据或记录值被干扰。上一节简单介绍了 NumPy 处理数据缺值或无效值的思路，本节则是针对这个问题的完整的实现方案。

numpy.ma 子模块通过引入掩码数组提供了一种解决数据缺失或无效问题的安全、便捷的方法。numpy.ma 子模块的主体是 MaskedArray 类，它是 numpy.ndarray 的派生类，可以把 numpy.ma 子模块当作 ndarray 来用，且无须考虑数组的无效值是否会给操作带来无法预知的意外。

1. 创建掩码数组
---------

掩码子模块无需单独导入即可使用。

```
>>> import numpy as np
>>> m = np.ma.array([0, 1, 2, 3])


```

也可以像下面这样单独导入掩码子模块，并赋予其一个简短易记的别名。

```
>>> import numpy.ma as ma
>>> m = ma.array([0, 1, 2, 3])


```

### 1.1 由列表生成掩码数组

掩码数组子模块的 ma.array() 函数和 NumPy 的 np.array() 函数类似，可以直接将列表生成掩码数组，默认 mask 参数为 False，生成的数组类型是 MaskedArray 类。数组掩码处理后，无论是查找最大值、最小值，还是计算均值、方差，都不用再担心数据是否无效的问题了。

```
>>> a = ma.array([0, 1, 2, 3], mask=[0, 0, 1, 0]) 
>>> print(a)
[0 1 -- 3]
>>> print(type(a))
<class 'numpy.ma.core.MaskedArray'>
>>> print(a.min(), a.max(), a.mean(), a.var())
0 3 1.3333333333333333 1.5555555555555556


```

### 1.2 由数组生成掩码数组

ma.asarray() 函数可以将普通的 NumPy 数组转成掩码数组。新生成的掩码数组不会对原数组中的 np.nan 或 np.inf 做掩码处理，但是会相应调整填充值（fill_value）。

```
>>> a = np.arange(5)
>>> print(ma.asarray(a))
[0 1 2 3 4]
>>> a = np.array([1, np.nan, 2, np.inf, 3]) 
>>> print(ma.asarray(a))
[ 1. nan  2. inf  3.]


```

### 1.3 对数组中的无效值做掩码处理

ma.asarray() 函数不会对原数组中的 np.nan 或 np.inf 做掩码处理，ma.masked_invalid() 函数则可以实现这个功能。

```
>>> a = np.array([1, np.nan, 2, np.inf, 3])
>>> print(ma.masked_invalid(a))
[1.0 -- 2.0 -- 3.0]


```

### 1.4 对数组中的给定值做掩码处理

有时需要将数组中的某个给定值设置为无效（掩码），ma.masked_equal() 函数可以实现这个功能。

```
>>> a = np.arange(3).repeat(2)
>>> print(ma.masked_equal(a, 1)) 
[0 0 -- -- 2 2]


```

### 1.5 对数组中的给定值做掩码处理

有时需要将数组中符合条件的某些特定值设置为无效（掩码），掩码数组子模块提供了若干函数实现条件掩码。这些可能的筛选条件包括大于、大于等于、小于、小于等于、区间内、区间外等 6 种。下面的代码演示了 6 种筛选条件对应的 6 个掩码函数的使用方法。

```
>>> a = np.arange(8)
>>> print(ma.masked_greater(a, 4)) 
[0 1 2 3 4 -- -- --]
>>> print(ma.masked_greater_equal(a, 4)) 
[0 1 2 3 -- -- -- --]
>>> print( ma.masked_less(a, 4)) 
[-- -- -- -- 4 5 6 7]
>>> print(ma.masked_less_equal(a, 4)) 
[-- -- -- -- -- 5 6 7]
>>> print(ma.masked_inside(a, 2, 5)) 
[0 1 -- -- -- -- 6 7]
>>> print(ma.masked_outside(a, 2, 5)) 
[-- -- 2 3 4 5 -- --]


```

### 1.6 用一个数组的条件筛选结果对另一个数组做掩码处理

a 和 b 是两个结构相同的数组，如果用 a>5 的条件对数组 b 掩码，上面那些函数就失效了。这种情况正是 ma.masked_where() 函数可以大显身手的时候。当然，该函数也可以对数组自身掩码。

```
>>> a = np.arange(8)
>>> b = np.random.random(8)
>>> print( ma.masked_where(a>5, b)) 
[0.576345802773722 0.05825827623828039 0.5220058443822165
 0.23611993998540037 0.28437966672648063 0.5030442621873711 -- --]


```

2. 访问掩码数组
---------

### 2.1 索引和切片

因为掩码数组 MaskedArray 类是 numpy.ndarray 的派生类，所以那些用在普通 NumPy 数组上的索引和切片操作也依然有效。

```
>>> a = np.array([1, np.nan, 2, np.inf, 3])
>>> a = ma.masked_invalid(a)
>>> print(a[0], a[1], a[-1])
1.0 -- 3.0
>>> print(a[1:-1])
[-- 2.0 --]


```

### 2.2 函数应用

掩码数组内置方法的使用和普通数组没有区别，下面的代码演示了最大值、最小值、均值和方差的使用。

使用 NumPy 命名空间的函数则要慎重，如果掩码数组子模块有对应函数，应优先使用掩码数组子模块的对应函数。例如，对掩码数组求正弦，如果使用 np.sin() 函数，会发出警告信息；如果使用 ma.sin() 函数，则无任何问题。

```
>>> a = np.array([1, np.nan, 2, np.inf, 3])
>>> a = ma.masked_invalid(a)
>>> print(a.min(), a.max(), a.mean(), a.var())
1.0 3.0 2.0 0.6666666666666666
>>> print(ma.sin(a)) 
[0.8414709848078965 -- 0.9092974268256817 -- 0.1411200080598672]


```

### 2.3 掩码数组转为普通数组

任何情况下，我们都可以通过掩码数组的 data 属性来获得掩码数组的数据视图，其类型就是 np.ndarray 数组。另外，还可以使用掩码数组的 **array**() 函数或 ma.getdata() 函数来获取掩码数组的数据视图。上述三种方法获得数据视图的操作，本质上都是操作掩码数组本身。如果需要数据视图副本，需使用 copy() 函数。

```
>>> a = ma.array([1, np.nan, 2, np.inf, 3])
>>> print(a)
[ 1. nan  2. inf  3.]
>>> x = a.data
>>> y = a.__array__()
>>> z = ma.getdata(a)
>>> w = np.copy(a.__array__()) 
>>> print(x)
[ 1. nan  2. inf  3.]
>>> print(y)
[ 1. nan  2. inf  3.]
>>> print(z)
[ 1. nan  2. inf  3.]
>>> print(w)
[ 1. nan  2. inf  3.]
>>> a[-1] = 9
>>> print(x)
[ 1. nan  2. inf  9.]
>>> print(y)
[ 1. nan  2. inf  9.]
>>> print(z)
[ 1. nan  2. inf  9.]
>>> print(w)
[ 1. nan  2. inf  3.]


```

### 2.4 修改掩码

通过掩码数组的 mask 属性可以查看当前数组的掩码情况，其代码如下。通常，数组的掩码是一个布尔型数组，或是一个布尔值。

```
>>> a = ma.masked_invalid(np.array([1, np.nan, 2, np.inf, 3]))
>>> print(a.mask)
[False  True False  True False]
>>> print(a)
[1.0 -- 2.0 -- 3.0]


```

如果要对数组切片掩码或对数组的某个元素掩码，直接令该切片或该元素等于 ma.masked 常量即可，其代码如下。

```
>>> a[:2] = ma.masked
>>> print(a)
[-- -- 2.0 -- 3.0]


```

如果要撤销对数组切片或数组中的某个元素的掩码，只需要对该切片或该元素做赋值操作即可，其代码如下。

```
>>> a = ma.masked_invalid(np.array([1, np.nan, 2, np.inf, 3]))
>>> a[1] = 1.5
>>> a[2:4] = 5
>>> print(a)
[1.0 1.5 5.0 5.0 3.0]


```