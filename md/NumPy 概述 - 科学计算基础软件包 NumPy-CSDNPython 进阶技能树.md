> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [edu.csdn.net](https://edu.csdn.net/skill/python02/python-3-195?category=882&typeId=17493)

> CSDNNumPy 概述社区, NumPy 概述论坛, 为中国软件开发者打造学习和成长的家园

1. NumPy 家族
-----------

NumPy 是 Python 科学计算的基础软件包，提供多了维数组对象，多种派生对象（掩码数组、矩阵等）以及用于快速操作数组的函数及 API，它包括数学、逻辑、数组形状变换、排序、选择、I/O 、离散傅立叶变换、基本线性代数、基本统计运算、随机模拟等。

![](https://img-blog.csdnimg.cn/8b9a840935f645338bfe451f2826a685.png#pic_center)

NumPy 是 SciPy 家族的成员之一。SciPy 家族是一个专门应用于数学、科学和工程领域的开源 Python 生态圈，其家族成员如上图所示。SciPy 家族的核心成员为 Matplotlib、SciPy 和 NumPy，可以概括为 MSN 这三个字母。

2. NumPy 在 Python 生态圈中的地位
-------------------------

在 Python 的世界里，没有一个模块能够像 NumPy 那样支撑并影响着整个生态系统：从科学计算到数据处理，从视觉识别到机器学习，从神经网络到虚拟现实，处处都有它的身影。无论是 OpenCV、OpenGL，还是 Pandas、Matplotlib，抑或是 Scikti-learn、TensorFlow、Keras、Theano、PyTorch，无不依赖于 NumPy，尤其是依赖它所创造的数组对象（numpy.ndarray）。在 Python 生态圈中，NumPy 的重要性和普遍性日趋增强。换句话说，为了高效地使用当今机器学习和数据处理等基于 Python 的工具包，只知道如何使用 Python 的列表是不够的，还需要熟练使用 NumPy 数组。

![](https://img-blog.csdnimg.cn/20191217085329637.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly94dWZpdmUuYmxvZy5jc2RuLm5ldA==,size_16,color_FFFFFF,t_70)  
上面这一张图，展示的是 Python 生态圈中用于图像视觉处理、2D 绘图、3D 渲染、数据处理、机器学习等领域比较有名的 5 个工具模块，都深度依赖 NumPy 数组。可以说，没有 NumPy 的基础，任何人都很难用好上述这 5 个工具库。

*   OpenCV：目前以人脸识别、自动驾驶等技术为代表的人工智能方兴未艾，其背后的图像和视觉处理，几乎都离不开 OpenCV，而 OpenCV 库中图像的数据结构，从 CV2 之后，全面转向了 NumPy，用 OpenCV 打开图像文件，得到的就是 NumPy 数组
*   OpenGL：在三维领域大名鼎鼎的 OpenGL，更是深度依赖 NumPy，如果没有 NumPy，我们无法想象如何操作动辄几万、几十万，甚至几百万的顶点数据集
*   Pandas：这个是当下非常流行的数据分析工具包，相信很多人都是从 Pandas 开始接触数据处理的，而 Pandas 整个就是基于 NumPy 之上的扩展
*   Matplotlib：作为 NumPy 生态圈的重要成员，二者关系自然是密不可分的
*   scikit-learn：机器学习领域应用最广泛的工具包，则是建立在 NumPy/SciPy/Matplotlib 之上的，同样深度依赖 NumPy

3. NumPy 的组织架构
--------------

NumPy 的核心是多维数组类 numpy.ndarray，矩阵类 numpy.matrix 是多维数组类的派生类。以多维数组类为数据组织结构，NumPy 提供了众多的数学、科学和工程函数。NumPy 的组织结构如下图所示。

![](https://img-blog.csdnimg.cn/1fabd4c66fec495a8c2e954c9fd5589a.png#pic_center)

图中红色的两个子模块，numpy.core 是 NumPy 的核心，包含 ndarray、 ufuncs 和 dtypes 等，numpy.lib 提供 NumPy 的函数。这个两个子模块是私有的，所有的函数和对象都可以在 numpy 的命名空间中使用，使用时无需使用子模块名。

除了 core 和 lib 外，NumPy 的其他常用子模块有：

*   numpy.random ：随机抽样子模块
*   numpy.ma：掩码数组子模块，用于处理包含无效或丢失的数据的数组
*   numpy.linalg ：线性代数子模块
*   numpy.fft ：离散傅里叶变换子模块
*   numpy.math ：由 C 标准定义的数学函数子模块
*   numpy.emath ：具有自动域的数学函数子模块
*   numpy.rec ：记录数组子模块，数组元素是多个不同类型的数据的组合，类似结构体
*   numpy.matrixlib ：矩阵类和函数子模块
*   numpy.ctypeslib ：ctypes 外部函数接口子模块
*   numpy.polynomial：多项式子模块
*   numpy.char：向量化字符串操作子模块
*   numpy.testing ：测试支持子模块