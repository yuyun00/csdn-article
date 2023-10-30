> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/qq_19528953/article/details/79348929)

### [Pandas](https://so.csdn.net/so/search?q=Pandas&spm=1001.2101.3001.7020) 最好用的函数

[Pandas](http://pandas.pydata.org/) 是`Python`语言中非常好用的一种数据结构包，包含了许多有用的数据操作方法。而且很多算法相关的库函数的输入数据结构都要求是`pandas`数据，或者有该数据的接口。

仔细看 [pandas 的 API 说明文档](http://pandas.pydata.org/pandas-docs/stable/api.html)，就会发现有好多有用的函数，比如非常常用的文件的读写函数就包括如下函数：

<table border="1"><colgroup><col width="12%"><col width="40%"><col width="24%"><col width="24%"></colgroup><thead><tr><th>Format Type</th><th>Data Description</th><th>Reader</th><th>Writer</th></tr></thead><tbody><tr><td>text</td><td><a target="_blank" href="https://en.wikipedia.org/wiki/Comma-separated_values" rel="noopener noreferrer">CSV</a></td><td><a target="_blank" href="http://pandas.pydata.org/pandas-docs/stable/io.html#io-read-csv-table" rel="noopener noreferrer">read_csv</a></td><td><a target="_blank" href="http://pandas.pydata.org/pandas-docs/stable/io.html#io-store-in-csv" rel="noopener noreferrer">to_csv</a></td></tr><tr><td>text</td><td><a target="_blank" href="http://www.json.org/" rel="noopener noreferrer">JSON</a></td><td><a target="_blank" href="http://pandas.pydata.org/pandas-docs/stable/io.html#io-json-reader" rel="noopener noreferrer">read_json</a></td><td><a target="_blank" href="http://pandas.pydata.org/pandas-docs/stable/io.html#io-json-writer" rel="noopener noreferrer">to_json</a></td></tr><tr><td>text</td><td><a target="_blank" href="https://en.wikipedia.org/wiki/HTML" rel="noopener noreferrer">HTML</a></td><td><a target="_blank" href="http://pandas.pydata.org/pandas-docs/stable/io.html#io-read-html" rel="noopener noreferrer">read_html</a></td><td><a target="_blank" href="http://pandas.pydata.org/pandas-docs/stable/io.html#io-html" rel="noopener noreferrer">to_html</a></td></tr><tr><td>text</td><td>Local clipboard</td><td><a target="_blank" href="http://pandas.pydata.org/pandas-docs/stable/io.html#io-clipboard" rel="noopener noreferrer">read_clipboard</a></td><td><a target="_blank" href="http://pandas.pydata.org/pandas-docs/stable/io.html#io-clipboard" rel="noopener noreferrer">to_clipboard</a></td></tr><tr><td>binary</td><td><a target="_blank" href="https://en.wikipedia.org/wiki/Microsoft_Excel" rel="noopener noreferrer">MS Excel</a></td><td><a target="_blank" href="http://pandas.pydata.org/pandas-docs/stable/io.html#io-excel-reader" rel="noopener noreferrer">read_excel</a></td><td><a target="_blank" href="http://pandas.pydata.org/pandas-docs/stable/io.html#io-excel-writer" rel="noopener noreferrer">to_excel</a></td></tr><tr><td>binary</td><td><a target="_blank" href="https://support.hdfgroup.org/HDF5/whatishdf5.html" rel="noopener noreferrer">HDF5 Format</a></td><td><a target="_blank" href="http://pandas.pydata.org/pandas-docs/stable/io.html#io-hdf5" rel="noopener noreferrer">read_hdf</a></td><td><a target="_blank" href="http://pandas.pydata.org/pandas-docs/stable/io.html#io-hdf5" rel="noopener noreferrer">to_hdf</a></td></tr><tr><td>binary</td><td><a target="_blank" href="https://github.com/wesm/feather" rel="noopener noreferrer">Feather Format</a></td><td><a target="_blank" href="http://pandas.pydata.org/pandas-docs/stable/io.html#io-feather" rel="noopener noreferrer">read_feather</a></td><td><a target="_blank" href="http://pandas.pydata.org/pandas-docs/stable/io.html#io-feather" rel="noopener noreferrer">to_feather</a></td></tr><tr><td>binary</td><td><a target="_blank" href="https://parquet.apache.org/" rel="noopener noreferrer">Parquet Format</a></td><td><a target="_blank" href="http://pandas.pydata.org/pandas-docs/stable/io.html#io-parquet" rel="noopener noreferrer">read_parquet</a></td><td><a target="_blank" href="http://pandas.pydata.org/pandas-docs/stable/io.html#io-parquet" rel="noopener noreferrer">to_parquet</a></td></tr><tr><td>binary</td><td><a target="_blank" href="http://msgpack.org/index.html" rel="noopener noreferrer">Msgpack</a></td><td><a target="_blank" href="http://pandas.pydata.org/pandas-docs/stable/io.html#io-msgpack" rel="noopener noreferrer">read_msgpack</a></td><td><a target="_blank" href="http://pandas.pydata.org/pandas-docs/stable/io.html#io-msgpack" rel="noopener noreferrer">to_msgpack</a></td></tr><tr><td>binary</td><td><a target="_blank" href="https://en.wikipedia.org/wiki/Stata" rel="noopener noreferrer">Stata</a></td><td><a target="_blank" href="http://pandas.pydata.org/pandas-docs/stable/io.html#io-stata-reader" rel="noopener noreferrer">read_stata</a></td><td><a target="_blank" href="http://pandas.pydata.org/pandas-docs/stable/io.html#io-stata-writer" rel="noopener noreferrer">to_stata</a></td></tr><tr><td>binary</td><td><a target="_blank" href="https://en.wikipedia.org/wiki/SAS_%28software%29" rel="noopener noreferrer">SAS</a></td><td><a target="_blank" href="http://pandas.pydata.org/pandas-docs/stable/io.html#io-sas-reader" rel="noopener noreferrer">read_sas</a></td><td>&nbsp;</td></tr><tr><td>binary</td><td><a target="_blank" href="https://docs.python.org/3/library/pickle.html" rel="noopener noreferrer">Python Pickle Format</a></td><td><a target="_blank" href="http://pandas.pydata.org/pandas-docs/stable/io.html#io-pickle" rel="noopener noreferrer">read_pickle</a></td><td><a target="_blank" href="http://pandas.pydata.org/pandas-docs/stable/io.html#io-pickle" rel="noopener noreferrer">to_pickle</a></td></tr><tr><td>SQL</td><td><a target="_blank" href="https://en.wikipedia.org/wiki/SQL" rel="noopener noreferrer">SQL</a></td><td><a target="_blank" href="http://pandas.pydata.org/pandas-docs/stable/io.html#io-sql" rel="noopener noreferrer">read_sql</a></td><td><a target="_blank" href="http://pandas.pydata.org/pandas-docs/stable/io.html#io-sql" rel="noopener noreferrer">to_sql</a></td></tr><tr><td>SQL</td><td><a target="_blank" href="https://en.wikipedia.org/wiki/BigQuery" rel="noopener noreferrer">Google Big Query</a></td><td><a target="_blank" href="http://pandas.pydata.org/pandas-docs/stable/io.html#io-bigquery" rel="noopener noreferrer">read_gbq</a></td><td><a target="_blank" href="http://pandas.pydata.org/pandas-docs/stable/io.html#io-bigquery" rel="noopener noreferrer">to_gbq</a></td></tr></tbody></table>

读取数据后，对于数据处理来说，有好多有用的相关操作的函数，但是我认为其中最好用的函数是下面这个函数：

#### apply 函数

[apply 函数](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.apply.html#pandas.DataFrame.apply)

是 `pandas` 里面所有函数中自由度最高的函数。该函数如下：

`DataFrame.apply(func, axis=0, broadcast=False, raw=False, reduce=None, args=(), **kwds)`

该函数最有用的是第一个参数，这个参数是函数，相当于`C/C++`的函数指针。

这个函数需要自己实现，函数的传入参数根据`axis`来定，比如`axis = 1`，就会把一行数据作为`Series`的数据结构传入给自己实现的函数中，我们在函数中实现对`Series`不同属性之间的计算，返回一个结果，则`apply`函数会自动遍历每一行`DataFrame`的数据，最后将所有结果组合成一个`Series`数据结构并返回。

比如读取一个表格：  
![](https://img-blog.csdnimg.cn/20200429183747719.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzE5NTI4OTUz,size_16,color_FFFFFF,t_70#pic_center)  
假如我们想要得到表格中的`PublishedTime`和`ReceivedTime`属性之间的时间差数据，就可以使用下面的函数来实现：

```
import pandas as pd
import datetime   #用来计算日期差的包

def dataInterval(data1,data2):
    d1 = datetime.datetime.strptime(data1, '%Y-%m-%d')
    d2 = datetime.datetime.strptime(data2, '%Y-%m-%d')
    delta = d1 - d2
    return delta.days

def getInterval(arrLike):  #用来计算日期间隔天数的调用的函数
    PublishedTime = arrLike['PublishedTime']
    ReceivedTime = arrLike['ReceivedTime']
#    print(PublishedTime.strip(),ReceivedTime.strip())
    days = dataInterval(PublishedTime.strip(),ReceivedTime.strip())  #注意去掉两端空白
    return days

if __name__ == '__main__':    
    fileName = "NS_new.xls";
    df = pd.read_excel(fileName) 
    df['TimeInterval'] = df.apply(getInterval , axis = 1)

```

有时候，我们想给自己实现的函数传递参数，就可以用的`apply`函数的`*args`和`**kwds`参数，比如同样的时间差函数，我希望自己传递时间差的标签，这样每次标签更改就不用修改自己实现的函数了，实现代码如下：

```
import pandas as pd
import datetime   #用来计算日期差的包

def dataInterval(data1,data2):
    d1 = datetime.datetime.strptime(data1, '%Y-%m-%d')
    d2 = datetime.datetime.strptime(data2, '%Y-%m-%d')
    delta = d1 - d2
    return delta.days

def getInterval_new(arrLike,before,after):  #用来计算日期间隔天数的调用的函数
    before = arrLike[before]
    after = arrLike[after]
#    print(PublishedTime.strip(),ReceivedTime.strip())
    days = dataInterval(after.strip(),before.strip())  #注意去掉两端空白
    return days


if __name__ == '__main__':    
    fileName = "NS_new.xls";
    df = pd.read_excel(fileName) 
    df['TimeInterval'] = df.apply(getInterval_new , 
      axis = 1, args = ('ReceivedTime','PublishedTime'))    #调用方式一
    #下面的调用方式等价于上面的调用方式
    df['TimeInterval'] = df.apply(getInterval_new , 
      axis = 1, **{'before':'ReceivedTime','after':'PublishedTime'})  #调用方式二
    #下面的调用方式等价于上面的调用方式
    df['TimeInterval'] = df.apply(getInterval_new , 
      axis = 1, before='ReceivedTime',after='PublishedTime')  #调用方式三

```

修改后的`getInterval_new`函数多了两个参数，这样我们在使用`apply`函数的时候要自己传递参数，代码中显示的三种传递方式都行。

最后，本篇的全部代码在下面这个网页可以下载：

[https://github.com/Dongzhixiao/Python_Exercise/tree/master/pandas_apply](https://github.com/Dongzhixiao/Python_Exercise/tree/master/pandas_apply)