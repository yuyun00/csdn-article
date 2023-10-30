> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/qq_41068877/article/details/125662190)

### Matplotlib 是什么？

Matplotlib 是一个全面的库，用于在 Python 中创建静态，动画和交互式的可视化图像。

目前（22 年 6 月）最新稳定版是 3.5.2

### 安装:

使用 pip 进行安装

`pip install matplotlib`

### 快速上手

我们首先要导入 matplotlib

```
import matplotlib as mpl
import matplotlib.pyplot as plt
import numpy as np  #用numpy生成数据，作为示例演示。

```

Matplotlib 在图 (Figure)（例如，窗口，Jupyter 小部件等）上绘制数据，图可以包含一个或多个轴域 ([Axes](https://so.csdn.net/so/search?q=Axes&spm=1001.2101.3001.7020))。

Axes 是一个可以根据 x-y 坐标指定点的绘图区域（或[极坐标](https://so.csdn.net/so/search?q=%E6%9E%81%E5%9D%90%E6%A0%87&spm=1001.2101.3001.7020)图中的θ-r，3D 图中的 x-y-z 等）。创建带有 Axes 的图的最简单方法是使用 [`pyplot.subplots`](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.subplots.html#matplotlib.pyplot.subplots "matplotlib.pyplot.subplots")。然后，我们可以使用 [`Axes.plot`](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.plot.html#matplotlib.axes.Axes.plot "matplotlib.axes.Axes.plot")在 Axes 上绘制一些数据：

```
x = [1,2,3,4]
y = [1,4,3,2]
fig,ax = plt.subplots() # 创建一个图fig， 默认包含一个axes
ax.plot(x,y) # 绘制x-y的折线图
plt.show()  # 显示绘制的图。请注意，如果使用save保存图片，需要在show前面保存

```

运行后将显示折线图：

![](https://img-blog.csdnimg.cn/50e3a8756c09411f9c1e94f7ad0cd741.png)

### 图（Figure）的结构

一幅图有下面这些部分：标题 (Title)、图例（Legend）、x、y 轴标签(xlabel、ylabel) 等等…

![](https://img-blog.csdnimg.cn/fa178060587e457cbebf11247445b28e.png)

下面逐个部分介绍。

#### 图 Figure

完整的图像。该图跟踪所有子轴域（Axes）---- 一组 “特殊” 画作（标题，图例，彩条等），甚至嵌套的子图。

创建新图的最简单方法是使用 pyplot：

```
fig = plt.figure()  # 空图，没有Axes
fig, ax = plt.subplots()  #有1个Axes
fig, axs = plt.subplots(2, 2)  # 有2x2（两行两列） 个Axes

```

通常，将轴域（Axes）与 Figure 一起创建很方便，但您也可以在以后手动添加轴域。

#### 轴域 Axes

轴域（Axes）是附加到图（Figure）上，包含用于绘制数据的图形。

通常包括两个轴（Axis）对象。两个轴分别提供刻度（ticks）和标签 (tick label)，以便为轴中的数据提供比例。每个[`轴域`](https://matplotlib.org/stable/api/axes_api.html#matplotlib.axes.Axes "matplotlib.axes.Axes")还有一个标题（通过 [`set_title（））`](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.set_title.html#matplotlib.axes.Axes.set_title "matplotlib.axes.Axes.set_title")设置）、一个 x 标签（通过 [`set_xlabel（））`](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.set_xlabel.html#matplotlib.axes.Axes.set_xlabel "matplotlib.axes.Axes.set_xlabel") 设置）和一个 y 标签（通过 [`set_ylabel（））`](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.set_ylabel.html#matplotlib.axes.Axes.set_ylabel "matplotlib.axes.Axes.set_ylabel") 设置）。

[`Axes`](https://matplotlib.org/stable/api/axes_api.html#matplotlib.axes.Axes "matplotlib.axes.Axes") 类及其成员函数是使用 OOP 接口的主要入口点，并且在其上定义了大多数绘图方法（例如，如上所示，使用 [`plot`](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.plot.html#matplotlib.axes.Axes.plot "matplotlib.axes.Axes.plot")方法）`ax.plot()`

#### 轴 Axis

轴设置刻度（scale）和限制（limits）并且生成刻度（ticks，轴上的标记）和刻度标签（ticklabels，标记刻度的字符串）ticks 位置由定位器（Locator）确定，ticklabel 字符串由[`格式器（Formatter）`](https://matplotlib.org/stable/api/ticker_api.html#matplotlib.ticker.Formatter "matplotlib.ticker.Formatter")设置。正确的[`定位器（Locator）`](https://matplotlib.org/stable/api/ticker_api.html#matplotlib.ticker.Locator "matplotlib.ticker.Locator")和[`格式器（Formatter）`](https://matplotlib.org/stable/api/ticker_api.html#matplotlib.ticker.Formatter "matplotlib.ticker.Formatter")的组合可以对刻度位置和标签进行非常精细的控制。

#### Artist

Artist 这里翻译成艺术家或画家了。

基本上，图形上可见的所有内容都是艺术家（甚至是[`图形`](https://matplotlib.org/stable/api/figure_api.html#matplotlib.figure.Figure "matplotlib.figure.Figure")，[`轴域`](https://matplotlib.org/stable/api/axes_api.html#matplotlib.axes.Axes "matplotlib.axes.Axes")和[`轴`](https://matplotlib.org/stable/api/axis_api.html#matplotlib.axis.Axis "matplotlib.axis.Axis")对象）。这包括[`文本`](https://matplotlib.org/stable/api/text_api.html#matplotlib.text.Text "matplotlib.text.Text")、[`Line2D`](https://matplotlib.org/stable/api/_as_gen/matplotlib.lines.Line2D.html#matplotlib.lines.Line2D "matplotlib.lines.Line2D") 、[`集合`](https://matplotlib.org/stable/api/collections_api.html#module-matplotlib.collections "matplotlib.collections")、[`Patch`](https://matplotlib.org/stable/api/_as_gen/matplotlib.patches.Patch.html#matplotlib.patches.Patch "matplotlib.patches.Patch")等。渲染图形时，所有艺术家都将被绘制到**画布**上。大多数艺术家都与轴域关联; 这样的艺术家不能由多个轴域共享，也不能从一个轴移动到另一个轴。

### 绘图函数的输入数据类型

绘图函数接收 `numpy.array`或`numpy.ma.masked_array`作为输入，或者是可以被传递给`numpy.asarray`的数据。pandas 数据或`numpy.matrix`可能不会正常工作。常见的约定是在绘图前将数据转换成`numpy.array`。例如：

```
b = np.matrix([[1, 2], [3, 4]])
b_asarray = np.asarray(b) # 使用np.asarray()将其转换成np.array类型

```

大多数方法还可以解析可寻址对象，如 `dict`,`np.recarray`或`pandas.DataFrame`。

Matplotlib 允许使用关键字参数生成图像，传递和 x,y 相应的字符串。

```
np.random.seed(19680801)  # seed the random number generator.
data = {'a': np.arange(50),
        'c': np.random.randint(0, 50, 50),
        'd': np.random.randn(50)}
data['b'] = data['a'] + 10 * np.random.randn(50)
data['d'] = np.abs(data['d']) * 100

fig, axes = plt.subplots(2,1) # fig 拥有2行1列个子图，存放在axes数组（np.array类型）中。
ax = axes[0]

ax.scatter('a', 'b', c='c', s='d', data=data)
ax.set_xlabel('entry a')
ax.set_ylabel('entry b')
# 轴域2 ，去掉颜色c，形状s参数。
ax2 = axes[1]
ax2.scatter('a', 'b', data=data)
plt.show()

```

绘制出来的图，第二幅是去掉了 s,c 参数后的图像：  
![](https://img-blog.csdnimg.cn/100b72d3d6bf4f29a8e3e4a22f34f6b6.png)

### 编码风格 Coding Styles

#### 面向对象（OO）和 pyplot 函数接口。

基本上有两种使用 Matplotlib 的方法：

*   显式创建 “图形（Figures）” 和“轴域（Axes）”，并在其上调用方法（“面向对象 （OO） 样式”）。
    
*   依靠 pyplot 自动创建和管理图形和轴，并使用 pyplot 函数进行绘图。
    

使用 OO 样式 (笔者觉得 OO 样式比较好，只需要在轴域（Axes）对象上设置就可以，非常清晰)：

```
x = np.linspace(0, 2, 100)  # 产生一些数据

# 使用OO风格，先产生两个对象 图和轴域 （fig, ax）
fig, ax = plt.subplots(figsize=(5, 2.7), layout='constrained')
# 调用ax对象的方法
ax.plot(x, x, label='linear')  # Plot some data on the axes.
ax.plot(x, x**2, label='quadratic')  # Plot more data on the axes...
ax.plot(x, x**3, label='cubic')  # ... and some more.
ax.set_xlabel('x label')  # Add an x-label to the axes.
ax.set_ylabel('y label')  # Add a y-label to the axes.
ax.set_title("Simple Plot")  # Add a title to the axes.
ax.legend()  # Add a legend.
plt.show() a legend.

```

或者使用 pyplot 函数风格：

```
x = np.linspace(0, 2, 100)  # Sample data.

plt.figure(figsize=(5, 2.7), layout='constrained')
plt.plot(x, x, label='linear')  # Plot some data on the (implicit) axes.
plt.plot(x, x**2, label='quadratic')  # etc.
plt.plot(x, x**3, label='cubic')
plt.xlabel('x label')
plt.ylabel('y label')
plt.title("Simple Plot")
plt.legend();

```

![](https://img-blog.csdnimg.cn/72db7297bda94bec9d3d576863c59c0c.png)

（此外，还有第三种方法，用于在 GUI 应用程序中嵌入 Matplotlib 的情况，即使对于图形创建也是如此。有关详细信息，请参阅库中的相应部分：[在图形用户界面中嵌入 Matplotlib](https://matplotlib.org/stable/gallery/index.html#user-interfaces)。

### 常用的绘图类型

从官网的首页 进入`Plot types`，就可以看到常用的不同类型的图怎么绘制了。

![](https://img-blog.csdnimg.cn/137cc8d8d3454b838de448bdfd313af7.png)

可以看到，常见的折线图、散点图、柱状图等。点击相应的图就可以进入对应的案例了。

![](https://img-blog.csdnimg.cn/f3772d5269ce491b8315a07c0137d804.png)

主要用法基本就是这样，后面的一些样式的调整我们下回再说。

### 参考：

[Matplotlib documentation — Matplotlib 3.5.2 documentation](https://matplotlib.org/stable/index.html)

`https://matplotlib.org/stable/index.html`