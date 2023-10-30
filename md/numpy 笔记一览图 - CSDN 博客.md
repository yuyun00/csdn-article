> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/qq_43260356/article/details/108866635?spm=1035.2023.3001.6557&utm_medium=distribute.pc_relevant_bbs_down_v2.none-task-blog-2~default~ESQUERY~Rate-2-108866635-bbs-600546343.264^v3^pc_relevant_bbs_down_v2_opensearchbbsnew&depth_1-utm_source=distribute.pc_relevant_bbs_down_v2.none-task-blog-2~default~ESQUERY~Rate-2-108866635-bbs-600546343.264^v3^pc_relevant_bbs_down_v2_opensearchbbsnew)

#### 文章目录

*   [numpy 中文参考手册](#numpy_1)
*   [numpy 学习笔记 1 一基础知识](#numpy1_7)
*   [numpy 学习笔记 2—常用函数 1](#numpy21_48)
*   [numpy 学习笔记 3—常用函数 2](#numpy32_83)
*   [numpy 学习笔记 4—常用函数 3](#numpy43_114)
*   [numpy 学习笔记 5—常用函数 4](#numpy54_145)
*   [numpy 学习比较 6—常用函数 5](#numpy65_172)
*   [numpy 学习笔记 7—断言函数](#numpy7_211)

[numpy](https://so.csdn.net/so/search?q=numpy&spm=1001.2101.3001.7020) 中文参考手册
-----------------------------------------------------------------------------

[https://www.numpy.org.cn/reference/](https://www.numpy.org.cn/reference/)

numpy 中文参考手册用中文介绍了 numpy 的函数接口，很方便大家的查阅。

numpy 学习笔记 1 一基础知识
------------------

[笔记 1—基础知识](https://blog.csdn.net/qq_43260356/article/details/107969536)

<table><thead><tr><th>作用</th><th>代码</th></tr></thead><tbody><tr><td>基 本 操 作 \color{blue}{基本操作} 基本操作</td><td></td></tr><tr><td>创建数组</td><td>np.arange、np.array</td></tr><tr><td>类型查看</td><td>a.dtype</td></tr><tr><td>维度查看</td><td>a.shape</td></tr><tr><td>指定数组类型</td><td>np.array([1,2],dtype=‘float32’)</td></tr><tr><td>利用字符编码指令类型</td><td>np.array([1,2],dtype=‘f’)</td></tr><tr><td>dtype 属性</td><td>np.dtype(‘float32’)</td></tr><tr><td>索 引 与 切 片 \color{blue}{索引与切片} 索引与切片</td><td>——</td></tr><tr><td>数 组 重 塑 \color{blue}{数组重塑} 数组重塑</td><td></td></tr><tr><td>数组重塑</td><td>reshape、resize</td></tr><tr><td>数组展平</td><td>np.ravel、np.flatten</td></tr><tr><td>数组转置</td><td>array.transpose()</td></tr><tr><td>组 合 \color{blue}{组合} 组合</td><td></td></tr><tr><td>水平组合</td><td>np.hstack</td></tr><tr><td>垂直组合</td><td>np.vstack</td></tr><tr><td>深度组合</td><td>np.dstack</td></tr><tr><td>列组合</td><td>np.column_stack</td></tr><tr><td>行组合</td><td>np.row_stack</td></tr><tr><td>分 割 \color{blue}{分割} 分割</td><td></td></tr><tr><td>水平分割</td><td>np.hsplit</td></tr><tr><td>垂直分割</td><td>np.vsplit</td></tr><tr><td>深度分割</td><td>np.dsplit</td></tr><tr><td>数 组 的 属 性 \color{blue}{数组的属性} 数组的属性</td><td></td></tr><tr><td>维度</td><td>array.ndim</td></tr><tr><td>总个数</td><td>array.size</td></tr><tr><td>元素所占字节</td><td>array.itemsize</td></tr><tr><td>数组所占字节</td><td>array.nbytes</td></tr><tr><td>转置</td><td>array.T</td></tr><tr><td>取实部</td><td>array.real</td></tr><tr><td>取虚部</td><td>array.imag</td></tr><tr><td>flat 属性</td><td>array.flat</td></tr><tr><td>数 组 转 换 \color{blue}{数组转换} 数组转换</td><td></td></tr><tr><td>转列表</td><td>array.tolist()</td></tr><tr><td>类型转换</td><td>array.astype(‘float32’)</td></tr></tbody></table>

numpy 学习笔记 2—常用函数 1
-------------------

[笔记 2—常用函数 1](https://blog.csdn.net/qq_43260356/article/details/108016847)

<table><thead><tr><th>作用</th><th>代码</th></tr></thead><tbody><tr><td>读 写 文 件 \color{blue}{读写文件} 读写文件</td><td></td></tr><tr><td>写文件</td><td>np.savatxt</td></tr><tr><td>读文件</td><td>np.loadtxt</td></tr><tr><td>统 计 方 法 \color{blue}{统计方法} 统计方法</td><td></td></tr><tr><td>最大值</td><td>np.max</td></tr><tr><td>最小值</td><td>np.min</td></tr><tr><td>平均值</td><td>np.mean</td></tr><tr><td>加权平均值</td><td>np.average</td></tr><tr><td>极差</td><td>np.ptp</td></tr><tr><td>中位数</td><td>np.median</td></tr><tr><td>方差</td><td>np.var</td></tr><tr><td>相邻元素差</td><td>np.diff</td></tr><tr><td>标准差</td><td>np.std</td></tr><tr><td>条 件 索 引 \color{blue}{条件索引} 条件索引</td><td></td></tr><tr><td>元素索引</td><td>np.where</td></tr><tr><td>找出索引值</td><td>np.take</td></tr><tr><td>替换最小值</td><td>np.maximum</td></tr><tr><td>其 他 \color{blue}{其他} 其他</td><td></td></tr><tr><td>卷积</td><td>np.convolve</td></tr><tr><td>返回均匀分布数组</td><td>np.linspace</td></tr><tr><td>计算点积</td><td>np.dot</td></tr><tr><td>创建同维度数组</td><td>np.ones_like</td></tr><tr><td>求交集</td><td>np.intersect1d</td></tr><tr><td>最大 / 最小索引</td><td>np.argmax/np.argmin</td></tr><tr><td>限制范围</td><td>array.clip</td></tr><tr><td>根据条件帅选</td><td>array.compress</td></tr><tr><td>计算阶层</td><td>array.prod</td></tr><tr><td>计算累积</td><td>array.cumpord</td></tr><tr><td>填充方法</td><td>array.fill</td></tr></tbody></table>

numpy 学习笔记 3—常用函数 2
-------------------

[笔记 3—常用函数 2](https://blog.csdn.net/qq_43260356/article/details/108249662)

<table><thead><tr><th>作用</th><th>代码</th></tr></thead><tbody><tr><td>矩 阵 处 理 \color{blue}{矩阵处理} 矩阵处理</td><td></td></tr><tr><td>矩阵创建</td><td>np.mat</td></tr><tr><td>转置</td><td>mat.T</td></tr><tr><td>求逆</td><td>mat.T</td></tr><tr><td>矩阵拼接</td><td>np.bmat</td></tr><tr><td>协方差笔记</td><td>np.cov</td></tr><tr><td>查看对角元素</td><td>mat.diagonal</td></tr><tr><td>求迹</td><td>mat.trace</td></tr><tr><td>相关系数</td><td>np.corrcoef</td></tr><tr><td>多 项 式 处 理 \color{blue}{多项式处理} 多项式处理</td><td></td></tr><tr><td>多项式拟合</td><td>np.polyfit</td></tr><tr><td>多项式求值</td><td>np.polyval</td></tr><tr><td>求零点</td><td>np.roots</td></tr><tr><td>多项式导数</td><td>np.polyder</td></tr><tr><td>求极值点（求导数的零点）</td><td>np.root(der)</td></tr><tr><td>其 他 函 数 \color{blue}{其他函数} 其他函数</td><td></td></tr><tr><td>返回元素正负号</td><td>np.sign</td></tr><tr><td>指定条件返回对应符号</td><td>np.piecewise</td></tr><tr><td>函数向量化（避免循环）</td><td>np.vectorize</td></tr><tr><td>平滑处理 (卷积 + 窗函数)</td><td>np.convolve(x,np.hanning)</td></tr><tr><td>求多项式的差</td><td>np.polysub</td></tr><tr><td>判断是否为实数</td><td>np.isreal</td></tr><tr><td>根据布尔值帅选元素</td><td>np.select</td></tr><tr><td>去掉首尾的 0 元素</td><td>np.trim_zeros</td></tr></tbody></table>

numpy 学习笔记 4—常用函数 3
-------------------

[笔记 4—常用函数 3](https://blog.csdn.net/qq_43260356/article/details/108287701)

<table><thead><tr><th>作用</th><th>代码</th></tr></thead><tbody><tr><td>a d d 函 数 的 方 法 \color{blue}{add 函数的方法} add 函数的方法</td><td></td></tr><tr><td>求和 (累加和)</td><td>np.add.reduce</td></tr><tr><td>求和 (累计和)</td><td>np.add.accumulate</td></tr><tr><td>返回的数组的秩为两输入的和</td><td>np.add.outer</td></tr><tr><td>特殊计算</td><td>np.add.reduceat</td></tr><tr><td>算 术 运 算 \color{blue}{算术运算} 算术运算</td><td></td></tr><tr><td>加法</td><td>np.add</td></tr><tr><td>减法</td><td>np.subtract</td></tr><tr><td>乘法</td><td>np.multiply</td></tr><tr><td>除法</td><td>np.divide</td></tr><tr><td>保留小数的除法</td><td>np.true_divide</td></tr><tr><td>向下取整的除法</td><td>np.floor_divide</td></tr><tr><td>取模</td><td>np.remainder、np.mod、np.fmod</td></tr><tr><td>斐 波 那 契 数 列 \color{blue}{斐波那契数列} 斐波那契数列</td><td></td></tr><tr><td>代码实现</td><td></td></tr><tr><td>推导过程</td><td></td></tr><tr><td>波 形 绘 制 \color{blue}{波形绘制} 波形绘制</td><td></td></tr><tr><td>方波、锯齿波、三角波、莉萨如曲线</td><td></td></tr><tr><td>位 操 作 \color{blue}{位操作} 位操作</td><td></td></tr><tr><td>异或运算</td><td>np.bitwise_xor</td></tr><tr><td>与运算</td><td>np.bitwise_and</td></tr><tr><td>移位操作</td><td>np.left_shift</td></tr><tr><td>判断是否小于</td><td>np.less</td></tr><tr><td>判断是否等于</td><td>np.equal</td></tr></tbody></table>

numpy 学习笔记 5—常用函数 4
-------------------

[笔记 5—常用函数 4](https://blog.csdn.net/qq_43260356/article/details/108648631)

<table><thead><tr><th>作用</th><th>代码</th></tr></thead><tbody><tr><td>n p . l i n a l g 模 块 \color{blue}{np.linalg 模块} np.linalg 模块</td><td></td></tr><tr><td>求逆</td><td>np.linalg.inv</td></tr><tr><td>求解线性方程</td><td>np.linalg.solve</td></tr><tr><td>求特征值</td><td>np.linalg.eigvals</td></tr><tr><td>求特征值和特征向量</td><td>np.linalg.eig()</td></tr><tr><td>奇异值分解的定义、证明、举例</td><td></td></tr><tr><td>奇异值分解</td><td>np.linalg.svd</td></tr><tr><td>数组变对角矩阵</td><td>np.diag(array)</td></tr><tr><td>广义逆矩阵</td><td>np.linalg.pinv</td></tr><tr><td>行列式</td><td>np.linalg.det</td></tr><tr><td>快 速 傅 里 叶 \color{blue}{快速傅里叶} 快速傅里叶</td><td></td></tr><tr><td>快速傅里叶变换</td><td>np.fft.fft</td></tr><tr><td>快速傅里叶逆变换</td><td>np.fft.ifft</td></tr><tr><td>频移</td><td>np.fft.fftshift</td></tr><tr><td>逆频移</td><td>np.fft.ifftshift</td></tr><tr><td>概 率 分 布 \color{blue}{概率分布} 概率分布</td><td></td></tr><tr><td>二项分布</td><td>np.random.binomial</td></tr><tr><td>超几何分布</td><td>np.random.hypergeometric</td></tr><tr><td>正态分布</td><td>np.random.normal</td></tr><tr><td>对数正态分布</td><td>np.random.lognormal</td></tr></tbody></table>

numpy 学习比较 6—常用函数 5
-------------------

[笔记 6—常用函数 5](https://blog.csdn.net/qq_43260356/article/details/108861889)

<table><thead><tr><th>作用</th><th>代码</th></tr></thead><tbody><tr><td>排 序 函 数 \color{blue}{排序函数} 排序函数</td><td></td></tr><tr><td>行排序</td><td>np.sort(a,axis=1)</td></tr><tr><td>列排序</td><td>np.sort(a,axis=0)、np.msort</td></tr><tr><td>方法排序</td><td>array.sort</td></tr><tr><td>放回排序后原数组的索引</td><td>np.argsort</td></tr><tr><td>根据字典键值排序</td><td>np.lexsort</td></tr><tr><td>复数排序</td><td>np.sort_complex</td></tr><tr><td>搜 索 函 数 \color{blue}{搜索函数} 搜索函数</td><td></td></tr><tr><td>最大值下标</td><td>np.argmax</td></tr><tr><td>最大值下标（不包括 nan)</td><td>np.nanargmax</td></tr><tr><td>最小值下标</td><td>np.argmin</td></tr><tr><td>最小值下标（不包括 nan)</td><td>np.nanargmin</td></tr><tr><td>寻找满足条件的下标</td><td>np.argwhere</td></tr><tr><td>寻找指定值在数组的位置</td><td>np.searchsorted</td></tr><tr><td>根据条件抽取的数组元素</td><td>np.extract</td></tr><tr><td>抽取非 0 元素</td><td>np.nonzero</td></tr><tr><td>金 融 函 数 \color{blue}{金融函数} 金融函数</td><td></td></tr><tr><td>终值</td><td>np.fv</td></tr><tr><td>现值</td><td>np.pv</td></tr><tr><td>净现值</td><td>np.npv</td></tr><tr><td>内部收益率</td><td>np.irr</td></tr><tr><td>分期付款还款金额</td><td>np.pmt</td></tr><tr><td>分期付款还款期数</td><td>np.nper</td></tr><tr><td>分期付款还款利率</td><td>np.rate</td></tr><tr><td>窗 函 数 \color{blue}{窗函数} 窗函数</td><td></td></tr><tr><td>巴特利特窗</td><td>np.bartlett</td></tr><tr><td>布莱克曼窗</td><td>np.blackman</td></tr><tr><td>汉明窗</td><td>np.hamming</td></tr><tr><td>凯泽窗</td><td>np.kaiser</td></tr><tr><td>重 要 函 数 \color{blue}{重要函数} 重要函数</td><td></td></tr><tr><td>贝塞尔函数</td><td>np.i0</td></tr><tr><td>sinc 函数</td><td>np.sinc</td></tr></tbody></table>

numpy 学习笔记 7—断言函数
-----------------

[笔记 7—断言函数](https://blog.csdn.net/qq_43260356/article/details/108866464)

<table><thead><tr><th>作用</th><th>代码</th></tr></thead><tbody><tr><td>断言精度近似相等</td><td>np.testing.assert_almost_equal</td></tr><tr><td>断言有效位近似相等</td><td>np.testing.assert_approx_equal</td></tr><tr><td>断言数组近似相等</td><td>np.testing.assert_almost_equal</td></tr><tr><td>断言数组相等</td><td>np.testing.assert_array_equal</td></tr><tr><td>断言有条件数组相等</td><td>np.testing.assert_allclose</td></tr><tr><td>断言数组大小</td><td>np.testing.assert_array_less</td></tr><tr><td>断言对象相等</td><td>np.testing.assert_equal</td></tr><tr><td>断言字符串相等</td><td>np.testing.assert_string_equal</td></tr><tr><td>断言浮点数相等 (单 ulp)</td><td>np.testing.assert_array_almost_equal_nulp</td></tr><tr><td>断言浮点数相等 (多 ulp)</td><td>np.testing.assert_array_max_nulp</td></tr><tr><td>unittest 单元测试</td><td></td></tr></tbody></table>