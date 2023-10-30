> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/Itsme_MrJJ/article/details/126101002?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-1-126101002-blog-122668922.235%5Ev38%5Epc_relevant_sort_base3&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-1-126101002-blog-122668922.235%5Ev38%5Epc_relevant_sort_base3&utm_relevant_index=2)

#### Pandas 知识点超全总结

*   [一、数据结构](#_13)
*   *   [1、Series](#1Series_16)
    *   *   [1. 创建](#1_18)
        *   [2. 切片、修改](#2_33)
        *   [3. 其他属性](#3_58)
    *   [2、DataFrame](#2DataFrame_74)
    *   *   [1. 创建](#1_76)
        *   [2. 切片](#2_89)
        *   [3. 增加、修改](#3_122)
        *   [4. 删除](#4_146)
        *   [5. 查看](#5_170)
*   [二、读写数据](#_199)
*   *   [1、读数据](#1_202)
    *   *   [1.excel 文件](#1excel_203)
        *   [2.csv 文件](#2csv_216)
        *   [3.sql 文件](#3sql_227)
    *   [2、写数据](#2_242)
*   [三、数据处理、分析](#_262)
*   *   [1、基础操作](#1_263)
    *   [2、分组聚合](#2_265)
    *   *   [1. 分组聚合](#1_268)
        *   *   [同种聚合方式](#_272)
            *   [多种聚合方式](#_289)
        *   [2. 透视和透视提取](#2_301)
    *   [3、数据拼接、合并](#3_309)
    *   *   [1.concat](#1concat_311)
        *   [2.merge](#2merge_324)
    *   [4、数据映射](#4_343)
    *   *   [1.map](#1map_345)
        *   [2.apply](#2apply_352)
*   [四、数据可视化](#_357)
*   *   [1、线图](#1_374)
    *   [2、条形图](#2_387)
    *   [3、饼图](#3_406)
    *   [4、散点图](#4_410)
    *   [5、直方图](#5_415)
    *   [6、箱图](#6_420)

Pandas，python+data+analysis 的组合缩写，是 python 中基于 numpy 和 matplotlib 的第三方数据分析库，与后两者共同构成了 python 数据分析的基础工具包，享有数分三剑客之名。

  正因为 pandas 是在 numpy 基础上实现，其核心数据结构与 numpy 的 [ndarray](https://so.csdn.net/so/search?q=ndarray&spm=1001.2101.3001.7020) 十分相似，但 pandas 与 numpy 的关系不是替代，而是互为补充。pandas 就数据处理上比 numpy 更加强大和智能，而 numpy 比 pandas 更加基础和强大。个人感觉，它们关系很像 excel 和 wps。

  Pandas 的导入也很简单，语法如下：

> import pandas as pd

  关于 pandas 的知识点，总结如下图：

![](https://img-blog.csdnimg.cn/af6a66cae2ec492796ca4a53d2350841.png)

一、数据结构
------

  Pandas 核心数据结构有两种，即一维的 series 和二维的 dataframe，就像 excel 中的列和表的关系。下面就这两种数据结构展开更加详细的介绍。

### 1、Series

  Series，一维数组。与 Python 标准的数据结构 List 列表很像，你甚至可以把它理解成 excel 表的列。想象一下，对于列数据，我们可以做什么？可以取任意行的数据、可以取指定行的数据、还可以修改相应的数据，这在 Series 中也可以实现，**对应的分别是 Series 的切片、索引和修改**。

#### 1. 创建

```
# 导入Pandas库
import pandas as pd

# 构建Series，其中index和name是可选项
series = pd.Series([1,2,"da"],index=["a","b","c"],)
series 
# 结果输出
	a     1
	b     2
	c    da
	Name: ss, dtype: object

```

#### 2. 切片、修改

```
# 切片
series[:2]
# 结果输出
	a    1
	b    2
	Name: ss, dtype: object

# 索引
series["a"]
# 结果输出
	1

# 修改值
series["a"] = 6
series[1] = 88
series
# 结果输出
	a     6
	b    88
	c    da
	Name: ss, dtype: object

```

#### 3. 其他属性

```
# Series的属性：dtype, index, values, name
series.dtype
series.index
series.values
series.name

# 结果输出
	dtype('O')
	Index(['a', 'b', 'c'], dtype='object')
	array([6, 88, 'da'], dtype=object)
	'ss'


```

### 2、DataFrame

  DataFrame，二维数组，也可以说是表。同样的，你可以把它理解成 excel 表的表格。想象一下，对于表格数据，我们可以做什么？可以对列增删改、可以对行增删改、当然对 “单元格” 数据也可以操作。那是不是我们可以对 DataFrame 实现上述的操作呢？答案是肯定的。话不多说，直接上代码：

#### 1. 创建

```
# 导入Pandas库
import pandas as pd

# 构建DataFrame，其中index、dtype和columns是可选项,
dataFrame = pd.DataFrame([[1,2,3],[4,5,6]],index=["a","b"],columns=["第一列","第二列","第三列"])
dataFrame

第一列	第二列	第三列
a	1	2	3
b	4	5	6

```

#### 2. 切片

```
# 切片,常规切片只能切行，要想切列，就得使用loc和iloc，loc使用索引和列名切，iloc使用行号和列号切
dataFrame[0:1]
第一列	第二列	第三列
a	1	2	3

dataFrame.loc['a']
第一列    1
第二列    2
第三列    3
Name: a, dtype: int64

dataFrame.loc[::,"第一列"]
a    1
b    4
Name: 第一列, dtype: int64

dataFrame.loc["a","第一列"]
1

dataFrame.iloc[:1]
第一列	第二列	第三列
a	1	2	3

dataFrame.iloc[::,1]
a    2
b    5
Name: 第二列, dtype: int64

dataFrame.iloc[1,1]
5

```

#### 3. 增加、修改

```
# 修改、增加，修改只要把切到的位置赋值即可，不再赘述，主讲增加
# 新增列
dataFrame["新增列"] = 6
# 新增行
dataFrame.loc["c"] = [55,88,99,66]
dataFrame

	第一列	第二列	第三列	新增列
a	1		2		3		6
b	4		5		6		6
c	55		88		99		66

# 另外，append也可以增加，但是它增加的索引是默认值，而且之前的index会变列
dataFrame.append([55])

	0	新增列	第一列	第三列	第二列
a	NaN	6.0 	1.0	 	3.0		2.0
b	NaN	6.0 	4.0		 6.0	5.0
c	NaN	66.0	55.0	 99.0	88.0
0	55.0 NaN 	 NaN	NaN		NaN

```

#### 4. 删除

```
# 删除可以分好几种，下面一一介绍
# 删除行
dataFrame.drop(index="c",inplace=True)

# 删除列,注意这里的0是列名哦
dataFrame.drop(columns=0,inplace=True)

# 删除nan值,参数axis指定删除行还是列，0代表行，1代表列
#	how指定是有一个旧删除，还是所有都为nan值菜删除，可选值'any', 'all'
# 	thresh阈值，指定行或者列超过指定的个数旧删除
# 	inplace是否在源数据删除，True代表是，False代表否

dataFrame.dropna(inplace=True)

# 删除重复值
# 参数subset: 指定去重依据，多条件用[]括起来
#    keep: 保留第一个还是最后一个，默认第一个，可选值first、last
#     inplace:是否在源数据删除
dataFrame.drop_duplicates()


```

#### 5. 查看

```
# 查看数据可以对数据有个大体的认知
# 前N行,不写参数N，则是前五行
dataFrame.head(N)

# 后N行,不写参数N，则是后五行
dataFrame.tail(N)

# 总体信息,包括索引，各列类型，整体数据类型，内存大小等信息
dataFrame.info()

<class 'pandas.core.frame.DataFrame'>
Index: 4 entries, a to 0
Data columns (total 5 columns):
 #   Column  Non-Null Count  Dtype  
---  ------  --------------  -----  
 0   0       1 non-null      float64
 1   新增列     3 non-null      float64
 2   第一列     3 non-null      float64
 3   第三列     3 non-null      float64
 4   第二列     3 non-null      float64
dtypes: float64(5)
memory usage: 192.0+ bytes


```

二、读写数据
------

  Pandas 支持了非常丰富的文件类型，也就是说，它可以读取和保存多种类型的数据，比如：excel 文件、csv 文件、或者 json 文件、sql 文件，甚至 html 文件等，这对我们获取数据很方便，这里挑常用的几个讲解，其他基本大同小异，注意特殊参数就行，读者自行查阅。

### 1、读数据

#### 1.excel 文件

  excel 无疑是接触最多的数据源，所有首先讲解 [pandas 读取 excel 文件](https://so.csdn.net/so/search?q=pandas%E8%AF%BB%E5%8F%96excel%E6%96%87%E4%BB%B6&spm=1001.2101.3001.7020)的方法。

```
pd.read_excel()

```

**参数详解：**

> io: 文件路劲，可以是相对路径，可以是绝对路径  
> sheet_name：excel 存在多个 sheet 的情况，所以可以指定 sheet 名（列 1，列 2…）或者指定第几个（0，1，2…），默认第一个。  
> usecols：提取特定的列，可以是列名，也可以是列号，[] 括起来  
> header: 以数据的第几行做标题行，和后面的参数 skiprows 很像，都能是跳过几行  
> dtype: 指定读取到的数据格式  
> skiprows : 跳过几行，和 header 类似

#### 2.csv 文件

  excel 虽然是使用做多的数据格式，但是由于 excel 存在诸多问题，如：数据加载慢、多 sheet 问题，没法保存大数据（excel 单个表只能 100w 多一点），所以 csv 就显得优秀很多，上述问题都不存在。

```
pd.read_csv()

```

  读取时的参数很多都是一样的，所以不再赘述，只讲解不一样的参数。

**和 Excel 不同参数详解：**

> sep：csv 文件并不是表格格式，所以要指定分隔符，默认是逗号，有事还可能是”\t“  
> encoding：csv 文件不像 excel 只有一种编码，所以使用时有时候需要指定编码格式。utf8 或者 gb2312 等

#### 3.sql 文件

  不得不说，现在好多数据都是直接在数据库提取的，好在 pandas 也有专门的获取方法，代码如下：

```
pd.read_sql(sql, con)

```

**参数详解：**

> sql:sql 语句，比如 "select * from test"  
> con: 链接数据库的信息，包括 ip、user、密码、数据库等，
> 
> > con = pymysql.connect(  
> > host=‘182.225.32.220’, # 本地数据库  
> > user=‘root’,  
> > password=‘***’,  
> > db=‘financial’,  
> > charset=‘utf8’)

### 2、写数据

  对于写文件，重要参数几乎一样，这里只使用 excel 做演示：

```
dataFrame.to_excel("输出.xlsx",index=False,sheet_)

```

参数说明：

> excel_writer: 保存文件路径，不写绝对路径则保存在当前目录下。注意文件后缀（必须）  
> sheet_name：保存 excel 文件中 sheet 的名字（excel 特有）  
> index：是否输出索引列，一般不输出（通用）  
> encoding：指定输出文件编码（通用）  
> startrow/startcol：输出文件在 excel 文件第几行几列开始 (excel 特有）  
> sep：输出的 csv 文件以什么分割，默认逗号（csv 特有）

  以上基本是常用参数，其他参数用的不多，默认即可，有特殊需求就百度查询。

  需要注意的是，保存文件要选择适应的类型，具体如下：

![](https://img-blog.csdnimg.cn/31a16210d1e340b2845277c2384c64c2.png)

三、数据处理、分析
---------

### 1、基础操作

  利用 Pandas 进行数据分析，基础操作就是 Data Frame 的各种操作，这里不再赘述。

### 2、分组聚合

  应该都用过 Excel 的透视表吧，很强大对不对，幸好，pandas 也有相应的功能，且不必 Excel 的差，甚至更加强大一些。

#### 1. 分组聚合

  假设我们有一份数据（如下图），我们想计算大部维度的销售额、订单数量以及订单量；或者大部小组两个维度的三个聚合值，怎么快速计算呢？这就用到 pandas 中的 groupby 函数，具体用法往下看。

![](https://img-blog.csdnimg.cn/dcdb0aff663d4e0b868e398812dc59c7.png#pic_center)

##### 同种聚合方式

![](https://img-blog.csdnimg.cn/1fd5de55995c4925bb672b82459d9ce9.png)  
  上图我们分别就大部维度、大部小组维度计算了销售额和订单数量的求和，看了效果，咱们在细细讲解下 groupby 函数。

**常用参数：**

> by：指定的分组依据，多个的时候用 [] 括起来  
> axis：聚合方向，默认对列聚合，可选值 0（纵向，默认）和 1（横向）  
> level：当聚合依据是索引时使用，默认 None

**聚合的列**  
  上图中的 [[“销售额”,“订单数量”]]，就是需要聚合的列，根据实际使用，如果是所有列，可以不写这个。

**聚合方式：**  
  上图中，使用的聚合方式是 sum，代表在给定的维度进行求和。当然不只能求和，其他聚合方式如下图：

![](https://img-blog.csdnimg.cn/4169d251d6194c50b3240a4c280aa0f0.png)

##### 多种聚合方式

  可能很多人想说，我需要聚合的方式要根据列选择，上面这只能一种，不是很方便，别慌，咱接着看：

![](https://img-blog.csdnimg.cn/d8388e7475d744b0bb71fee46934e3e4.png)  
**说明：**

1.  agg 函数：可以一次计算多个聚合值，除了上图的样式，还有 agg([“sum”,“mean”])，效果如下图

![](https://img-blog.csdnimg.cn/74920f1768ae43bfb0cd587906e75605.png)

2.  rename 函数：重命名，最常用参数就是 columns 和 inplace。columns 以字典形式给出要重命名的列名和修改后的列名
3.  其他几个和聚合有关系的函数：不知大家发现没，聚合之后索引变成了聚合的依据列，这时候要变成列就需要对结果重置索引，使用 reset_index()，结果如下图：  
    ![](https://img-blog.csdnimg.cn/1703e1debccb4585a027a12edf335660.png)

#### 2. 透视和透视提取

  上面讲到的分组聚合和 Excel 中的透视表很像，但是又不能对列和行一起做透视，那有没有和 Excel 中的透视表功能一样的功能呢，当然是有的，那就是 pandas 中的 pivot_table。

  关于 pivot_table 的使用，这里不再赘述，读者可以看下面两篇文章。

[Pandas 透视表](https://blog.csdn.net/Itsme_MrJJ/article/details/108514722?spm=1001.2014.3001.5502)  
[Pandas 透视表的提取](https://blog.csdn.net/Itsme_MrJJ/article/details/108517600?spm=1001.2014.3001.5502)

### 3、数据拼接、合并

  想象这么一个场景，你有两个表，你需要把里面的内容合在一张表上，你会怎么做？手动复制嘛？那如果是一百张表呢？这就要用到接下来讲解的方法了。

#### 1.concat

> concat 函数参数：  
> objs：需要合并的数据  
> axis：合并方向，可以横向拼接，也可以纵向拼接  
> join：合并方式，类似内联外联，默认外联，不一样的 nan 补充

![](https://img-blog.csdnimg.cn/316238dec0c64c029bbf4a53e1a6660f.png)

**注意：**

*   需要合并的两个表使用 [] 括起来
*   使用 concat 一般不做横向链接，横向使用下面介绍的 merge 函数
*   合并的时候要保持列名一致，不一致的以新列存在，并且值使用 NAN 填充

#### 2.merge

  merge 函数和 sql 中的内外联一致，可以实现左联、右联、内联和外联。

> merge 函数详解  
> left: 数据 1  
> right：数据 2  
> how: 连接方式，可选 inner（默认），outer，left，right  
> on：用来连接的列，使用 on 时，数据 1 和数据 2 用来连接的列名要相同，不相同时用下面参数  
> left_on：单独指定左边表的连接列,  
> right_on：单独指定右边表的连接列,  
> left_index: 是否使用左边表的索引连接，默认 False  
> right_index: 是否使用右边表的索引连接，默认 False

**效果演示：**

![](https://img-blog.csdnimg.cn/9081e87607ae420ca317a77c1de93a90.png)

  所以总的来说，拼接有两种方法可以选择，纵向拼接就用 concat，横向拼接就用 merge。

### 4、数据映射

  实际工作中，我们在利用 pandas 进行数据处理的时候，经常会对数据框中的单行、多行（列也适用）甚至是整个数据进行某种相同方式（单一逻辑或者自定义函数）的处理，这个时候虽然可以用 For 来实现，但是 for 的效率太低了，好在 Pandas 提供更加高效的函数，map、apply、applymap，三者的区别在于 map 是针对单列，apply 是针对多列，applymap 是针对全部元素（可以理解成单元格）。

#### 1.map

  例如根据大部构建新的列，构建规则是在原来大部的基础上加上成都，这样的规则。

![](https://img-blog.csdnimg.cn/a1aa7441311945c18a1bcc3e76d17c61.png)

需要说明的是，上面例子虽然是 map 实现的，但是 apply 也可以实现哈。

#### 2.apply

上面已经说了，apply 是可以应用于多列的，这是 map 无法实现的，看下面的例子：

![](https://img-blog.csdnimg.cn/b2d8c8c2217749fbaa314537a2931731.png)

四、数据可视化
-------

  大家都知道，Matplotlib 是众多 Python 可视化包的鼻祖，也是 Python 最常用的标准可视化库，其功能非常强大，同时也非常复杂，想要搞明白并非易事。虽然现在 Python 可视化的包有很多，视觉效果也都很好，但是有时候我们只是需要看看数据的分布，没必要多么的美观，因为美观的代价是复杂。所幸 pandas 本身就有数据可视化的功能已经可以满足我们大部分的要求了，让我们看看 pandas 自带的可视化操作。

  在开始之前，我们说一下，Pandas 在绘图时, 会显示中文为方块，效果如下：

![](https://img-blog.csdnimg.cn/1731b89b24c244d68b86e0143ac84697.png#pic_center)  
 **显然，这个问题是不能忽视的，所以利用如下代码即可修正。**

```
import matplotlib as mpl
mpl.rcParams['font.sans-serif'] = ['KaiTi']
mpl.rcParams['font.serif'] = ['KaiTi']
mpl.rcParams['axes.unicode_minus'] = False # 解决保存图像是负号'-'显示为方块的问题,或者转换负号为字符串

```

![](https://img-blog.csdnimg.cn/c55dbfe816ca4a09b4c8c771b32dc3d7.png#pic_center)  
  好的，问题解决完，我们正式开始学习 pandas 的画图功能。画图数据如下：  
![](https://img-blog.csdnimg.cn/46527eb44f7240daa9d8dfc0f95d720d.png)

### 1、线图

**常规线图：**

> data2.plot.line()  
> ![](https://img-blog.csdnimg.cn/6411249896ec45eca85bec7d85f82502.png)

**子图线图：**

> data2.plot.line(subplots=True)

![](https://img-blog.csdnimg.cn/cc3360a762a14fd1b19c65ae80e96a80.png)

### 2、条形图

**常规条形图：**

> data2.plot.bar()

![](https://img-blog.csdnimg.cn/db0002cf76ed432a857ee763261944b5.png)

**堆叠条形图：**

> data2.plot.bar(stacked=True)

![](https://img-blog.csdnimg.cn/c12acce5cf624d528b8083e78a0d5f71.png)

**水平条形图：**

> data2.plot.barh()

![](https://img-blog.csdnimg.cn/51b047247fca4a59a777cf49887cec67.png)

 需要注意的是，前面介绍的子图，也是使用于条形图（不管是横向还是纵向）的，增加相应参数就行，后面的图也不例外，不再赘述。

### 3、饼图

> data2.plot.pie(subplots=True)

![](https://img-blog.csdnimg.cn/f5acd8b1ee504aa190439615ee3ca1d1.png)

### 4、散点图

> data2.plot.scatter(x=‘销售额’,y=‘订单数量’)

![](https://img-blog.csdnimg.cn/ae9dd224ef27449483257e0fdf8a8c96.png)

### 5、直方图

> data2.hist()

![](https://img-blog.csdnimg.cn/d5115b4d7db94c41b3e3e65176939a15.png)

### 6、箱图

> data2.boxplot()

![](https://img-blog.csdnimg.cn/6cf1bcecae20446aafc0169d542d3ca8.png)

这里只是罗列各种图的画法，关于更多细节，诸如坐标轴的设置、图例的设置、标题的设置等美化细节，读者自行查阅。