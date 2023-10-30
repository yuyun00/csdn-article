> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/weixin_39597875/article/details/121387904)

#### 文章目录

*   [前言](#_8)
*   [一、pandas 是什么？](#pandas_16)
*   [二、使用步骤](#_23)
*   *   [1. 引入库](#1_24)
    *   [2. 读写数据](#2_42)
    *   [3. 最基本的索引](#3__57)
    *   [4. 通过 loc 函数索引](#4_loc_67)
    *   [5. 数据的增删](#5__81)
    *   [6. 数据的修改和查找](#6__98)
    *   [7. 时间日期格式处理](#7__115)
    *   [8. 数据堆叠与合并](#8__153)
    *   [9. 字符串处理](#9__165)
    *   [10. 数据统计与排序](#10__201)
    *   [11. 读取 txt 文件](#11_txt_213)
*   [总结](#_232)

前言
--

本文记录自己使用 pandas 的常用方法和心得。

一、[pandas](https://so.csdn.net/so/search?q=pandas&spm=1001.2101.3001.7020) 是什么？
-------------------------------------------------------------------------------

pandas 是基于 NumPy 的一种工具，该工具是为了解决数据分析任务而创建的。特别使用读取 csv 和 excel 格式的额文件。

二、使用步骤
------

### 1. 引入库

代码如下：

```
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import warnings
warnings.filterwarnings('ignore')
import  ssl
ssl._create_default_https_context = ssl._create_unverified_context

```

### 2. 读写数据

代码如下：  
读取 CSV 格式数据，使用 pd.read_csv()，写入文件使用 DataFrame.to_csv()

```
work_path = r'C:\Users\sherry\.spyder-py3'
os.chdir(work_path)#改变当前目录到work_path
print(os.getcwd())#打印当前目录
data = pd.read_csv(
    'sample_data.csv', encoding = 'utf-8', dtype = {'user_id':str, 'count':int})
print(data.head())
data.to_csv('sample_data_out.csv', encoding = 'gbk')#微软的软件一般用gbk编码

```

该处地址也可以使用 url 向网络请求数据。

### 3. 最基本的索引

直接索引时，列更容易定位和索引，似乎行只能连续索引，不能灵活索引。

```
print(data.columns)#显示列标签
print(data['user_id'])#获取一列'user_id'
print(data[['count','user_id']])#获取两列,这是用列表形式获取的两列，所以有2层方括号
print(data[1:5])#获取1:4行,好像无法获取不连续的多行，需要用loc或iloc函数
print(data[['count','user_id']][1:5])#获取两列,再获取这两列组成的dataframe的1:4行

```

### 4. 通过 loc 函数索引

loc 函数定位的是标签 (index)，不是行的位置。loc 函数的第一个参数是对行的操作，第二个参数是对列的操作。

```
print(data.loc[1:3, ['user_id','count']])#获取行标签为1:3,列标签为'user_id','count'的数据
print(data.loc[data.user_id == '1', ['user_id','count']])#获user_id列中值为'1'的行，列标签为'user_id','count'的数据
print(data.loc[data.buy_mount > 9, ])#获buy_mount > 9的所有行
print(data.loc[(data.buy_mount > 90) | (data.user_id == '1'), ])#获buy_mount > 90或user_id='1'的所有行

```

iloc 函数定位的是行的位置。iloc 函数的第一个参数是对行的操作，第二个参数是对列的操作。

```
print(data.iloc[[1, 6], 0:4])#获取第1行和第六行，0到2列
print(data.iloc[[1, 6], [0, 2]])#获取第1行和第六行，0和2列

```

### 5. 数据的增删

列的增加主要使用直接增加法，或者 insert() 方法，删除列主要使用 drop、del 方法。  
行的删除也可以用 drop 方法，行的增加可以用 append 在末尾加入，也有更好的方法。

```
tmp = data['购买量']
data.insert(0, 'copy购买量', tmp)#在数据中插入一列，名称‘copy购买量’，位置第0列，数字tmp
del data['copy购买量']#删除dataframe中名称为copy购买量的列
data.insert(1, 'copy购买量', tmp)#在数据中插入一列，名称‘copy购买量’，位置第0列，数字tmp
data.insert(0, 'copy购买量1', tmp)#在数据中插入一列，名称‘copy购买量’，位置第0列，数字tmp
data.drop(labels = ['copy购买量', 'copy购买量1'], axis = 1, inplace = True)
#上一行在数据中删除名称为'copy购买量', 'copy购买量1'的列，名称‘copy购买量’，axis = 1表示删除列，axis = 0表示删除行，inplace表示真实删除

print(data.drop(labels = [3, 4], axis = 0, inplace = False))#删除行标签为3和4的行
print(data.drop(labels = range(2, 5), axis = 0, inplace = False))#删除行标签为2到4的行
print(data.append(data))#在df的最后附加行

```

### 6. 数据的修改和查找

用 rename 修改行标签或者列名称。  
直接用 df.series 同 ==,>,< 等逻辑运算符获取满足特定关系的 df 数据  
也可以用 between、isin 方法获取在之间，匹配某些数量的数据

```
data.rename(columns = {'user_id':'用户ID'})#用rename修改列标签，使用字典的形式；
data.rename(index = {1:11, 2:22}, inplace = True)#用rename修改行标签，使用字典的形式；

data.loc[data['性别'] == 0, '性别'] = '女性'#将列标签为‘性别’的列中值等于0的元素，改为女性。
data.loc[data['性别'] == 1, '性别'] = '男性'
data.loc[data['性别'] == 2, '性别'] = '未知'

data[data['buy_mount']>80]#获取名称为'buy_mount'列的数据中大于80的数据，会得到所有列
print(data[(data['buy_mount'] < 80) & (data['性别'] == '男性')])
print(data[data['buy_mount'].isin([80, 88, 89, 98])])#buy_mount 列数值属于list[80, 88, 89, 90]的数据

```

### 7. 时间日期格式处理

使用 df.to_datetime() 处理 pandas 的各类字符串形式的时间；  
使用 pd.dt.strftime() 处理 datetime64 或时间戳类型数据的以格式化字符串输出

```
start_datetime = np.arange('2021-11-01', '2021-11-10', dtype = 'datetime64[D]')#生成numpy起始时间序列
end_datetime = np.arange('2021-11-01T12:14:30.789', '2021-11-10T12:14:30.789', 86400000, dtype = 'datetime64[ms]')
segment_time = np.array([start_datetime, end_datetime]).T#建立一个起始时间的numpy数组
df = pd.DataFrame(segment_time, columns = ['起始时间', '结束时间'])#新建dataframe
df['持续时间'] = df['结束时间'] - df['起始时间']#增加一列持续的时间
df['间隔时间'] = np.append((df['起始时间'].values[1:] - df['结束时间'].values[0:-1]), 0)#增加一列每两组之间间隔的时间
df['start_time'] = df['起始时间'].dt.strftime('%A %B %d %Y %H:%M:%S.%f')#把datetime64格式转换成规定格式字符串时间
df['end_time'] = df['结束时间'].dt.strftime('%a %b %d %Y %H:%M:%S.%f')
df['读入起始时间'] = pd.to_datetime(df['start_time'], format = '%A %B %d %Y %H:%M:%S.%f')
df['读入结束时间'] = pd.to_datetime(df["end_time"], format = '%a %b %d %Y %H:%M:%S.%f')

#建立df的另外一种方法：
start_datetime_char = np.datetime_as_string(start_datetime, unit = 'ms')#numpy里的datetime64转换为字符串,改格式不方便
end_datetime_char = np.datetime_as_string(end_datetime, unit = 'ms')#转换为字符串
df_char = pd.DataFrame(start_datetime_char, columns = ['起始时间'])
df_char['结束时间'] = end_datetime_char

```

对 datetime64 使用 pd.dt.date/time/year/month/wedk/weekday/day/hour/min/second  
对 timedelta64 使用 pd.dt.days/total_seconds()  
对 datetime64 使用 pd.dt.dayofyear/weekofyear

```
df_char['date_time'] = pd.to_datetime(df['结束时间'], format = '%Y-%m-%dT%H%M%S.%f')
df_char['年'] = df_char['date_time'].dt.year
df_char['周'] = df_char['date_time'].dt.isocalendar().week
df_char['周几'] = df_char['date_time'].dt.weekday
df_char['分'] = df_char['date_time'].dt.minute
df_char['秒'] = df_char['date_time'].dt.second
df_char['微秒'] = df_char['date_time'].dt.microsecond
df_char['总秒数'] = df['间隔时间'].dt.total_seconds()#不要忘了括号
df_char['总天数'] = (df['间隔时间']).dt.days#不用括号
df_char['加天序列后总天数'] = (df['间隔时间'] + np.arange(0, 9, 1, dtype = 'timedelta64[D]')).dt.days#不用括号
#上面的增加np.arange()主要是用来增加间隔天数，以使显示的天数由变化。


```

### 8. 数据堆叠与合并

使用 concat() 将两个 dataframe 横向堆叠或纵向堆叠  
使用 merge() 将两个 dataframe 按照主键合并

```
merge1 = pd.concat([df, df_char], axis = 0, join = 'outer')#使用列表堆叠，0轴沿着纵向拓展，即行数增加，inner表示不一致的删除，outer表示不一致的保留
merge2 = pd.concat([df, df_char, data], axis = 1, join = 'outer')#使用列表堆叠，0轴沿着横向拓展，即列数增加
merge3 = pd.merge(left = df, right = df_char, how = 'inner', left_on = '结束时间', right_on = 'date_time')#按照df[结束时间]和df[date_time]这两个主键关联
merge4 = pd.merge(left = df, right = df_char, how = 'inner', left_on = '起始时间', right_on = 'date_time')#按照df[起始时间]和df[date_time]这两个主键关联，由于不相等且是inner模式，所以会清空所有数据
merge5 = pd.merge(left = df, right = df_char, how = 'outer', left_on = '起始时间', right_on = 'date_time')#按照df[起始时间]和df[date_time]这两个主键关联，由于不相等但是是outer模式，所以会保留所有数据

```

### 9. 字符串处理

主要用到的函数名称即说明如下：  
pd.str.  
contains() 返回表示各 str 是否含有指定模式的字符串 。  
replace() 替换字符串  
lower() 返回字符串的副本, 其中所有字母都转换为小写。  
upper() 返回字符串的副本, 其中所有字母都转换为大写。  
split() 返回字符串中的单词列表。  
strip() 删除前导和后置空格。  
join() 返回一个字符串, 该字符串是给定序列中所有字符串的连接。  
还有一个判断是否包含空元素的函数 pd.isnull()，经常会需要和 T、any() 联合起来使用  
∙ \bull ∙**使用 df.fillna/df.age.fillna 来替换缺失值**

```
df1 = pd.read_csv('MotorcycleData.csv', encoding = 'gbk')
print(df1['Price'].str[0])#pd.str可以用切片方式实现索引
print(df1['Price'].str[:2])
df1['价格'] = df1['Price'].str.strip('$')#删去头尾的'$'符号
df1['价格'] = df1['价格'].str.replace(',', '')#删去','符号
df1['价格'] = df1['价格'].astype(int)
# print(df1.info())
# print(df1[['Price', '价格']])
df1['位置'] = df1['Location'].str.split(',')#通过字符串中“，”将其分割成一个字符串列表
# print(df1['位置'].str[1])#打印出这个列表中的第一个字符串元素
# print(df1['Location'].str.len())#获取字符串长度
df1.loc[df1[['Location']].isnull().T.any()] = 'aaa'#将'Location'这列中是空数据的填上字符串，避免字符串判断时出现空值而报错
#上面的例子中df1[['Location']]必须用两层中括号，这样结果是DataFrame类，如果是df1['Location']就是series类了
#非转置：df1.isnull().any()，得到的每一列求any()计算的结果，输出为列的Series。
#转置：df1.isnull().T.any()，得到的每一行求any()计算的结果，输出为行的Series。
#这里要知道那一行有Nan，所以要用转置
print(df1[df1[['Location', '价格']].isnull().T.any()]['Location'])#检查'Location和价格'这两列中是是否有空数据 
#替换缺失值用df.fillna更方便
df1.fillna('datamiss', inplace = True)
print(df1.loc[df1['Location'].str.contains('New Hampshire'), 'Location'])#找出数据中包含特定字符串的数据

```

### 10. 数据统计与排序

数据统计信息显示采用 discribe() 方法；  
数据排序使用:  
∙ \bullet ∙ sort_values(by, axis=0, ascending=True, inplace=False, kind=‘quicksort’, na_position=‘last’), 基于某几行或列的值进行排列  
∙ \bullet ∙ sort_index(axis=0, level=None, ascending=True, inplace=False, kind=‘quicksort’, na_position=‘last’, sort_remaining=True, by=None)，基于行或列标签进行排列

```
print(df1.describe())#只对数字类参数有用，输出每一列的均值、标准差、最大、最小值和25%/50%/75%的值
print(df1.sort_index(axis = 1))
print(df1.sort_values(by = ['Bid_Count', 'Price']))#根据指定的列名称及其优先级顺序，对整个列表进行排序。

```

更详细的统计功能可参考博客：  
[https://blog.csdn.net/qq_42067550/article/details/106260512](https://blog.csdn.net/qq_42067550/article/details/106260512)

### 11. 读取 txt 文件

读取 txt 文件可以用 read_csv 也可以用 read_table，前置默认以逗号分隔，后置必须指定分隔符。前者也可以用 sep 参数来指定分隔符  
数据排序使用:

```
dtxt = pd.read_table('sample_data_out.txt', encoding = 'gbk', header = 0, sep = ' ')
dtxt = pd.read_csv('sample_data_out.txt', encoding = 'gbk', header = 0, sep = ' ')
'''
上面的两句功能基本相同
'''
print(dtxt.iloc[:,0].str.split(','))#手动分隔，结果是两层列表
print(dtxt.columns.str.split(',')[0])#手动分隔列名称，结果是两层列表
dtxt_deal = (dtxt.iloc[:,0].str.split(',')).apply(pd.Series, index = dtxt.columns.str.split(',')[0])
dtxt_deal['buy_mount'] = dtxt_deal['buy_mount'].astype(int)
print(dtxt_deal.info())

```

总结
--

```
pandas是非常方便的数据处理工具还有非常多很方便的功能，本文没有涉及，有时间的话慢慢补充。

```