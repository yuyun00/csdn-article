> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [edu.csdn.net](https://edu.csdn.net/skill/python02/python-3-196?category=882&typeId=17496)

> CSDN 安装配置社区, 安装配置论坛, 为中国软件开发者打造学习和成长的家园

1 安装
----

NumPy 不依赖于任何其他 Python 包，安装 NumPy 的唯一先决条件是 Python 本身， 因此 NumPy 的安装非常简单，可以使用 pip、conda、macOS 和 Linux 上的包管理器来安装，还可以从源代码安装。当然，最简单的安装方式是使用 pip 命令安装，无论运行在哪一种平台上。

```
pip install numpy


```

对于初学者，强烈建议安装 NumPy 的时候像下面这样一并安装 SciPy 模块和 matplotlib 模块，前者是包含图像处理、信号处理、空间计算、积分、聚类等专业性的科学计算工具包，后者是 Python 生态圈中应用最广泛的绘图库。

```
pip install numpy scipy matplotlib


```

由于 pip 默认的 Python 包索引的基本 URL 为 https://pypi.org/simple，安装过程可能会出现网络连接或传输超时，因此建议国内用户使用 -i 参数指定国内的镜像源。

```
pip install numpy 
pip install numpy -i https://pypi.tuna.tsinghua.edu.cn/simple/ 
pip install numpy -i https://mirrors.aliyun.com/pypi/simple/ 
pip install numpy -i https://pypi.mirrors.ustc.edu.cn/simple/ 


```

如果需要特定的版本，也可以从 [Python 模块仓库](https://www.lfd.uci.edu/~gohlke/pythonlibs/)直接下载. whl 文件后在本地安装。该仓库由加州大学欧文分校荧光动力学实验室创建和维护，这是近十年来我见过的最全面最稳定的模块仓库。

2 导入
----

导入 NumPy 时，通常习惯简写成 np——尽管这不是必须的，但几乎所有的程序员都会遵守这一约定俗成的规则。

```
import numpy as np


```

下图是在 IDEL 窗口中导入 NumPy 的示例，在脚本文件中导入也是如此。顺便说一下， 在 IDEL 窗口中可以交互式执行 Python 语句，是学习 Python 的有力工具。

![](https://img-blog.csdnimg.cn/20191217105248636.png#pic_center)

这种导入方式，不止导入了 NumPy 最核心的 ndarray、 ufuncs 和 dtypes 等，也包括了 np.lib 提供 NumPy 的函数，以及包括 random、ma、matrixlib 在内的其他子模块。

```
>>> import numpy as np
>>> np.array([1,2,3])
array([1, 2, 3])
>>> np.random.random(3)
array([0.34897809, 0.78084803, 0.8300295 ])


```

3 配置
----

### 3.1 设置显式格式

NumPy 提供了一个配置字典，用于指定数据的显式格式或方式，比如精度位数，负数、非数字和无穷大的表示符号，是否使用科学计数法等。下面的代码列出了配置字典的默认设置。

```
>>> np.get_printoptions()
{'edgeitems': 3, 'threshold': 1000, 'floatmode': 'maxprec', 'precision': 8, 'suppress': False, 'linewidth': 75, 'nanstr': 'nan', 'infstr': 'inf', 'sign': '-', 'formatter': None, 'legacy': False}


```

其中几个最常用的参数含义如下：

*   precisionint - 浮点输出精度的数字数（默认 8）
*   suppress - 如果为 True，始终使用固定点表示法显式浮点数；若为 False，则当最小值的绝对值 <1e-4 或最大绝对值与最小值的比值为> 1e3 时，使用科学计数法
*   edgeitemsint - 数据过多时每个维度只显示开始和结束的数组项数（默认 3）

使用演示了将浮点输出精度从默认的 8 位设置为 3 位的用法。

```
>>> import numpy as np
>>> np.random.random(10)
array([0.03492636, 0.17094003, 0.87078202, 0.39193946, 0.20655205,
       0.52429516, 0.44361299, 0.52315678, 0.10703231, 0.93694037])
>>> np.set_printoptions(precision=3) 
>>> np.random.random(10)
array([0.409, 0.541, 0.785, 0.633, 0.026, 0.629, 0.282, 0.517, 0.065, 0.898])


```

### 3.2 设置警告信息

在数学运算时，如果除数为 0 就会产生溢出错误，程序会异常中断。不过对 NumPy 而言，这个结果仅是个无效值（nan），通常不会导致程序中断，只是发出警告信息。

```
>>> import numpy as np
>>> a = np.array([1,2,3])
>>> b = np.array([1,0,1])
>>> a/b

Warning (from warnings module):
  File "__main__", line 1
RuntimeWarning: divide by zero encountered in true_divide
array([ 1., inf,  3.])


```

在实际工作应用中，产生无效数据的几率是非常高的，会频繁出现忽略警告信息。如果想屏蔽此类警告信息，可以使用 seterr 函数。

```
>>> import numpy as np
>>> np.seterr(invalid='ignore')
{'divide': 'warn', 'over': 'warn', 'under': 'ignore', 'invalid': 'warn'}
>>> a = np.array([1,2,3])
>>> b = np.array([1,0,1])
>>> a/b
array([ 1., inf,  3.])


```

4 基本概念
------

### 4.1 NumPy 数组的数据类型

NumPy 支持的数据类型主要有整型 (integrate)、浮点型(float)、布尔型(bool) 和复数型(complex)，每一种数据类型根据占用内存的字节数又分为多个不同的子类型。当然，NumPy 也支持自定义类型，我们在后面讲解数组排序的时候，再讨论自定义类型。  
![](https://img-blog.csdnimg.cn/20191218173959767.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly94dWZpdmUuYmxvZy5jc2RuLm5ldA==,size_16,color_FFFFFF,t_70#pic_center)  
下面的例子演示了如何查看和指定数组的数据类型。

```
>>> a = np.array([0,1,2,3])
>>> a.dtype
dtype('int32')
>>> a = np.array([0,1,2,3.0])
>>> a.dtype
dtype('float64')
>>> a = np.array([0,1,2,3+0j])
>>> a.dtype
dtype('complex128')
>>> a = np.array([0,1,2,3], dtype=np.int16)
>>> a.dtype
dtype('int16')
>>> a = np.array([0,1,2,3], dtype=np.uint8)
>>> a.dtype
dtype('uint8')


```

dtype 是数组的属性之一，可以很方便地查看数组的数据类型。创建数组时，如果不指定数据类型，NumPy 会根据输入数据选择合适的数据类型。在较早期的版本中，指定数据类型的时候允许省略数据类型后面的数字，不过最新的版本已经限制这样的写法了。

### 4.2 NumPy 数组的属性

刚才我们用 dtype 可以查看数组的数据类型，dtype 是数组对象的属性之一，除了 dtype，NumPy 数组还有其他一些属性，详见下表。属性看起来有点多，但我们只需要记住`dtype`和`shape`两个属性就足可应对一般需求了。这两个属性非常重要，重要到你可以忽略其他的属性。

<table><thead><tr><th>属性</th><th>说明</th></tr></thead><tbody><tr><td>ndarray.dtype</td><td>元素类型</td></tr><tr><td>ndarray.shape</td><td>数组结构或形状</td></tr><tr><td>ndarray.size</td><td>数组元素个数</td></tr><tr><td>ndarray.itemsize</td><td>数组元素的大小，以字节为单位</td></tr><tr><td>ndarray.ndim</td><td>数组的维度数，也叫秩</td></tr><tr><td>ndarray.flags</td><td>数组的内存信息</td></tr><tr><td>ndarray.real</td><td>元素的实部</td></tr><tr><td>ndarray.imag</td><td>元素的虚部</td></tr><tr><td>ndarray.data</td><td>元素数组的实际存储区</td></tr></tbody></table>

下面是这些属性的演示操作：

```
>>> a = np.arange(24, dtype=np.complex64).reshape((2,3,4))
>>> a.dtype 
dtype('complex64')
>>> a.shape 
(2, 3, 4)
>>> a.size 
24
>>> a.itemsize 
8
>>> a.flags 
  C_CONTIGUOUS : True
  F_CONTIGUOUS : False
  OWNDATA : False
  WRITEABLE : True
  ALIGNED : True
  WRITEBACKIFCOPY : False
  UPDATEIFCOPY : False

>>> a.real 
array([[[ 0.,  1.,  2.,  3.],
        [ 4.,  5.,  6.,  7.],
        [ 8.,  9., 10., 11.]],

       [[12., 13., 14., 15.],
        [16., 17., 18., 19.],
        [20., 21., 22., 23.]]], dtype=float32)
>>> a.imag 
array([[[0., 0., 0., 0.],
        [0., 0., 0., 0.],
        [0., 0., 0., 0.]],

       [[0., 0., 0., 0.],
        [0., 0., 0., 0.],
        [0., 0., 0., 0.]]], dtype=float32)
>>> a.data 
<memory at 0x00000157D820BC78>
>>> a.ndim 
3


```

### 4.3 维、秩、轴

维，就是维度。我们说数组是几维的，就是指维度，3 维的数组，其维度数自然就是 3。维度数，有一个专用名字，叫做秩，也就是上一节提到的数组属性 ndim。秩这个名字感觉有些多余，不如维度数更容易理解。但是，轴（axis）的概念一定要建立起来，并且要理解，因为轴的概念很重要。简单来说，可以把数组的轴，和笛卡尔坐标系的轴对应一下。

一维数组，类比于一维空间，只有一个轴，那就是 0 轴。  
![](https://img-blog.csdnimg.cn/20191217111747961.png#pic_center)

二维数组，类比于二维平面，有两个轴，我们习惯表示成行、列，那么行的方向就是 0 轴，列的方向就是 1 轴。  
![](https://img-blog.csdnimg.cn/20191217111833313.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly94dWZpdmUuYmxvZy5jc2RuLm5ldA==,size_16,color_FFFFFF,t_70#pic_center)  
三维数组，类比于三维空间，有三个轴，我们习惯表示成层、行、列，那么层的方向就是 0 轴，行的方向就是 1 轴，列的方向就是 2 轴。  
![](https://img-blog.csdnimg.cn/20191217111845146.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly94dWZpdmUuYmxvZy5jc2RuLm5ldA==,size_16,color_FFFFFF,t_70#pic_center)

我们用一个求和的例子来演示一下轴概念的重要性。以三维数组为例，求和的需求会比较复杂，比如，分层求和，逐行求和，逐列求和等。这时候，轴（axis）就可以大显身手了。

```
>>> a = np.arange(18).reshape((3,2,3)) 
>>> a
array([[[ 0,  1,  2],
        [ 3,  4,  5]],

       [[ 6,  7,  8],
        [ 9, 10, 11]],

       [[12, 13, 14],
        [15, 16, 17]]])
>>> np.sum(a) 
153 
>>> np.sum(a, axis=0) 
array([[18, 21, 24],
       [27, 30, 33]])
>>> np.sum(a, axis=1) 
array([[ 3,  5,  7],
       [15, 17, 19],
       [27, 29, 31]])
>>> np.sum(a, axis=2) 
array([[ 3, 12],
       [21, 30],
       [39, 48]])
>>> np.sum(np.sum(a, axis=1), axis=1) 
array([15, 51, 87])
>>> np.sum(np.sum(a, axis=2), axis=1) 
array([15, 51, 87])


```

只要理解了上面的操作，轴的概念就真正建立起来了。

### 4.4 广播和矢量化

在讲两个概念之前，我们先思考两个问题：

1.  整型数组各元素加 1；
2.  求两个等长整型数组对应元素之和组成的新数组。

若用 python 的列表实现的话，代码大约会这样写吧。

```
>>> x = list(range(5))
>>> for i in range(len(x)): 
        x[i] += 1
>>> y = list(range(5,10))
>>> z = list()
>>> for i, j in zip(x, y): 
        z.append(i+j)


```

用 NumPy 数组实现的话，代码就简洁多了，无需循环。

```
>>> a = np.arange(5)
>>> a += 1
>>> b = np.arange(5,10)
>>> c = a + b


```

显然，用 NumPy 数组实现起来，要比 Python 列表更简洁、更清晰。这得益于于 NumPy 的两大特性：`广播`和`矢量化`。

广播（broadcast）和矢量化（vectorization），是 NumPy 最精髓的特性，是 NumPy 的灵魂。所谓广播，就是将对数组的操作映射到每个数组元素上；矢量化可以理解为代码中没有显式的循环、索引等。NumPy 数组最重要的特性是广播和矢量化，体现在性能上，就是接近 C 语言的运行效率，体现在代码上，则有这样的特点：

1.  矢量化代码更简洁，更易于阅读
2.  代码行越少意味着出错的几率越小
3.  代码更接近于标准的数学符号
4.  矢量化代码更 pythonic

![](https://img-blog.csdnimg.cn/20191217113237857.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly94dWZpdmUuYmxvZy5jc2RuLm5ldA==,size_16,color_FFFFFF,t_70#pic_center)