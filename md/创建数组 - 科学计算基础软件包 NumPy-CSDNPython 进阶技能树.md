> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [edu.csdn.net](https://edu.csdn.net/skill/python02/python-3-197?category=882&typeId=17499)

> CSDN 创建数组社区, 创建数组论坛, 为中国软件开发者打造学习和成长的家园

1. 概述
-----

一般情况下，科学数据都是海量的、层次关系复杂的，是由数据服务机构提供的，不是我们构造出来的。我们创建数组的目的，很多时候是用来做原型验证和算法验证的。NumPy 为创建数组提供了非常丰富的手段，可以无中生有，可以移花接木，还可以举一反三。配合数据类型设置、结构设置，就可以构造出我们想要的任何形式的数组了。

根据工作经验，我把创建数组的方法，分成了创建简单数组和构造复杂数组两大类。其实简单数组和复杂数组并没有严格的分界线，大致上，无中生有创造出来的数组称为简单数组，通过移花接木、举一反三创造出来的数组称为复杂数组。

2. 创建简单数组
---------

归纳一下，常用的构造简单数组的方法可以分为四类，分别是蛮力构造法、特殊数值法、随机数值法和定长分割法。下面我给大家逐一演示一下。

![](https://img-blog.csdnimg.cn/20191217161343475.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly94dWZpdmUuYmxvZy5jc2RuLm5ldA==,size_16,color_FFFFFF,t_70#pic_center)

### 2.1 蛮力构造法

蛮力构造法使用 np.array() 来创建数组，其原型为：

> numpy.array(object, dtype=None, copy=True, order=None, subok=False, ndmin=0)

蛮力构造法就是你想要什么结构，就直接用 Python 数组或元组写出来。这个方法看起来简单，但很容易出错，不适合构造体量较大的数组。

```
>>> a = np.array([[1,2,3],[4,5,6]]) 
>>> a
array([[1, 2, 3],
       [4, 5, 6]])
>>> a.dtype
dtype('int32')


```

也可以像下面这样，在创建数组时，指定元素的数据类型。

```
>>> a = np.array([[1,2,3],[4,5,6]], dtype=np.uint8) 
>>> a
array([[1, 2, 3],
       [4, 5, 6]], dtype=uint8)


```

### 2.2 特殊数值法

特殊值一般指 0，1，空值等。特殊数值法适合构造全 0、全 1、空数组，或者由 0、1 组成的类似单位矩阵（主对角线为 1，其余为 0）的数组。特殊数值法使用的 4 个函数原型如下：

> numpy.zeros(shape, dtype=float, order=‘C’)  
> numpy.ones(shape, dtype=float, order=‘C’)  
> numpy.empty(shape, dtype=float, order=‘C’)  
> numpy.eye(N, M=None, k=0, dtype=float, order='C’)

这几个函数配合 shape 和 dtype 参数，可以很方便的构造一些简单数组：

```
>>> np.zeros(6)
array([0., 0., 0., 0., 0., 0.])
>>> np.zeros((2,3))
array([[0., 0., 0.],
       [0., 0., 0.]])
>>> np.ones((2,3),dtype=np.float)
array([[1., 1., 1.],
       [1., 1., 1.]])
>>> np.empty((2,3))
array([[1., 1., 1.],
       [1., 1., 1.]])
>>> np.eye(2)
array([[1., 0.],
       [0., 1.]])


```

如果我们需要一个 3 行 4 列、初始值都是 5 的数组，该怎么做呢？填充函数 fill() 就是专门解决这个问题的：

```
>>> a = np.empty((3,4), dtype=np.int8)
>>> a.fill(5)
>>> a
array([[5, 5, 5, 5],
       [5, 5, 5, 5],
       [5, 5, 5, 5]], dtype=int8)


```

### 2.3 随机数值法

和 Python 的标准模块 random 类似，NumPy 也有一个 random 子模块，功能更为强大。用随机数值法创建数组，主要就是使用 random 子模块。random 子模块方法很多，我们只介绍 3 个常用函数，其原型如下：

> numpy.random.random(size=None)  
> numpy.random.randint(low, high=None, size=None, dtype=‘l’)  
> numpy.random.normal(loc=0.0, scale=1.0, size=None)

random() 生成 [0,1) 区间内的随机浮点数数，randint() 生成 [low, high) 区间内的随机整数。

```
>>> np.random.random(3)
array([0.4129063 , 0.94242888, 0.10129428])
>>> np.random.random((2,3))
array([[0.80530845, 0.96161533, 0.89166972],
       [0.22920038, 0.84989557, 0.46865645]])
>>> np.random.randint(5)
4
>>> np.random.randint(1,5,size=(2,3))
array([[3, 3, 1],
       [3, 3, 3]])


```

normal() 生成以 loc 为均值、以 scale 为标准差的正态分布数组。下面，我们用正态分布函数模拟生成 1000 位成年男性的身高数据（假定中国成年男性平均身高 170 厘米，标准差 4 厘米），并画出柱状图：

```
>>> import matplotlib.pyplot as plt
>>> tall = np.random.normal(170, 4, 1000)
>>> bins = np.arange(156, 190, 2)
>>> plt.hist(tall, bins)
(array([  3.,   4.,  15.,  31.,  94., 163., 185., 186., 157.,  92.,  43.,
        18.,   6.,   1.,   1.,   0.]), array([156, 158, 160, 162, 164, 166, 168, 170, 172, 174, 176, 178, 180,
       182, 184, 186, 188]), <a list of 16 Patch objects>)
>>> plt.show()


```

![](https://img-blog.csdnimg.cn/20191218092944806.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly94dWZpdmUuYmxvZy5jc2RuLm5ldA==,size_16,color_FFFFFF,t_70#pic_center)

### 2.4 定长分割法

定长分割法最常用的函数是 arange()，非常类似于 Python 的 range() 函数，只是前面多了一个字母 a。另一个定长分割函数 linspace() 类似于 arange()，但功能更强大。两个函数的原型如下：

> numpy.arange(start, stop, step, dtype=None)  
> numpy.linspace(start, stop, num=50, endpoint=True, retstep=False, dtype=None)

arange() 和 Python 的 range() 函数用法相同，还可以接收浮点型参数：

```
>>> np.arange(5)
array([0, 1, 2, 3, 4])
>>> np.arange(5,11)
array([ 5,  6,  7,  8,  9, 10])
>>> np.arange(5,11,2)
array([5, 7, 9])
>>> np.arange(5.5,11,1.5)
array([ 5.5,  7. ,  8.5, 10. ])
>>> np.arange(3,15).reshape(3,4)
array([[ 3,  4,  5,  6],
       [ 7,  8,  9, 10],
       [11, 12, 13, 14]])


```

linspace() 函数需要 3 个参数：一个起点、一个终点，一个返回元素的个数。linspace() 返回的元素包括起点和终点，我们可以通过 endpoint 参数选择是否包含终点。

```
>>> np.linspace(0, 4.5, 5) 
array([0.   , 1.125, 2.25 , 3.375, 4.5  ])
>>> np.linspace(0,4.5,5,endpoint=False) 
array([0. , 0.9, 1.8, 2.7, 3.6])


```

3. 构造复杂数组
---------

构造复杂数组的方法很多，技巧性也很强。我给大家归纳了两类：重复构造法和网格构造法。  
![](https://img-blog.csdnimg.cn/20191218094951574.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly94dWZpdmUuYmxvZy5jc2RuLm5ldA==,size_16,color_FFFFFF,t_70#pic_center)

### 3.1 重复构造法

重复构造法主要使用 repeat() 和 tile() 两个函数，repeat() 用来重复元素，tile() 用来重复数组。

```
>>> a = np.arange(5)
>>> a
array([0, 1, 2, 3, 4])
>>> np.repeat(a,3) 
array([0, 0, 0, 1, 1, 1, 2, 2, 2, 3, 3, 3, 4, 4, 4])
>>> np.tile(a,3) 
array([0, 1, 2, 3, 4, 0, 1, 2, 3, 4, 0, 1, 2, 3, 4])
>>> np.tile(a,(3,2)) 
array([[0, 1, 2, 3, 4, 0, 1, 2, 3, 4],
       [0, 1, 2, 3, 4, 0, 1, 2, 3, 4],
       [0, 1, 2, 3, 4, 0, 1, 2, 3, 4]])


```

对于多维数组 a，repeat() 还有一个默认参数 axis，tile() 也有不同表现，请仔细揣摩。

```
>>> a = np.arange(6).reshape((2,3))
>>> a
array([[0, 1, 2],
       [3, 4, 5]])
>>> np.repeat(a,3)
array([0, 0, 0, 1, 1, 1, 2, 2, 2, 3, 3, 3, 4, 4, 4, 5, 5, 5])
>>> np.repeat(a,3,axis=0)
array([[0, 1, 2],
       [0, 1, 2],
       [0, 1, 2],
       [3, 4, 5],
       [3, 4, 5],
       [3, 4, 5]])
>>> np.repeat(a,3,axis=1)
array([[0, 0, 0, 1, 1, 1, 2, 2, 2],
       [3, 3, 3, 4, 4, 4, 5, 5, 5]])
>>> np.tile(a,3)
array([[0, 1, 2, 0, 1, 2, 0, 1, 2],
       [3, 4, 5, 3, 4, 5, 3, 4, 5]])
>>> np.tile(a,(2,3))
array([[0, 1, 2, 0, 1, 2, 0, 1, 2],
       [3, 4, 5, 3, 4, 5, 3, 4, 5],
       [0, 1, 2, 0, 1, 2, 0, 1, 2],
       [3, 4, 5, 3, 4, 5, 3, 4, 5]])


```

### 3.2 网格构造法

我们知道，研究地球表面需要经纬度坐标，经度从西经 180 度到东经 180，纬度从南纬 90 度到北纬 90 度，把经纬度线画出来，就形成了一个经纬度网格。经纬度网格是科学数据中常用的概念，通常，经度用 Longitude 表示，简写成 lon，纬度用 latitude 表示，简写成 lat。那么，如何用数组表示经纬度网格呢？  
![](https://img-blog.csdnimg.cn/20191218100047875.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly94dWZpdmUuYmxvZy5jc2RuLm5ldA==,size_16,color_FFFFFF,t_70#pic_center)  
用数组表示经纬度网格，一般有两种方式。第一种方式，用两个一维数组表示：

```
>>> lon = np.linspace(-180,180,37) 
>>> lat = np.linspace(-90,90,19) 


```

第二种方式，则是用 np.meshgrid() 生成两个二维数组，分别表示经度网格、纬度网格。np.meshgrid() 以刚才的两个一维数组 lon 和 lat 为参数，生成的网格精度也是 10°。

```
>>> lons,lats = np.meshgrid(lon,lat)
>>> lons.shape
(19, 37)
>>> lats.shape
(19, 37)


```

构造网格，除了 np.meshgrid() 之外，还有一个更牛的方法，可以直接生成纬度网格和经度网格（请注意，纬度在前，经度在后）：

```
>>> lats, lons = np.mgrid[-90:91:5., -180:181:5.] 
>>> lons.shape, lats.shape
((37, 73), (37, 73))
>>> lats, lons = np.mgrid[-90:90:37j, -180:180:73j] 
>>> lons.shape, lats.shape
((37, 73), (37, 73))


```

讲到这里，咱们做一下延申，把这个网格用 3D 的方式画出来，展示一下 NumPy 和 matplotlib 的强大力量。

```
>>> lats, lons = np.mgrid[-90:91:5., -180:181:5.]
>>> lons = np.radians(lons) 
>>> lats = np.radians(lats) 
>>> z = np.sin(lats) 
>>> x = np.cos(lats)*np.cos(lons) 
>>> y = np.cos(lats)*np.sin(lons) 
>>> import matplotlib.pyplot as plt
>>> import mpl_toolkits.mplot3d
>>> ax = plt.subplot(111,projection='3d')
>>> ax.plot_surface(x,y,z,cmap=plt.cm.coolwarm,alpha=0.8)
>>> plt.show()


```

![](https://img-blog.csdnimg.cn/20191218104532508.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly94dWZpdmUuYmxvZy5jc2RuLm5ldA==,size_16,color_FFFFFF,t_70#pic_center)