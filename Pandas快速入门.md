# Pandas快速入门
## 简介
  - 2008 年 WesMcKinney 开发出的库
  - 专门用于数据挖掘的开源 Python 库
  - 以 Numpy 为基础，借力 Numpy 模块在计算方面性能高的优势
  - 基于 matplotlib，能够简便的画图
  - 独特的数据结构
## Pandas核心数据结构及其优势

优势： 
- 便捷的数据处理能力
- 读取文件方便
- 封装了 Matplotlib、Numpy 的画图和计算

核心数据结构：
- DataFrame
- Pannel
- Series
## DataFrame
**结构**

既有行索引，又有列索引的二维数组
- 行索引，表明不同行，横向索引，叫 index
- 列索引，表明不同列，纵向索引，叫 columns

**常用属性**
- shape
- index 行索引列表
- columns 列索引列表
- values 直接获取其中 array 的值
- T 行列转置

**常用方法**
head()开头几行
tail()最后几行

- 示例代码
```Python
import numpy as np
import pandas as pd
# 创建一个符合正态分布的10个股票5天的涨跌幅数据
stock_change = np.random.normal(0, 1, (10, 5))
pd.DataFrame(stock_change)
# 添加行索引
stock = ["股票{}".format(i) for i in range(10)]
pd.DataFrame(stock_change, index=stock)
# 添加列索引
date = pd.date_range(start="20200101", periods=5, freq="B")
data = pd.DataFrame(stock_change, index=stock, columns=date)

# 属性
print(data.shape)
print(data.index)
print(data.columns)
print(data.values)
data.T # 行列转置

# 方法
data.head(3) # 开头3行
data.tail(2) # 最后2行

# 索引的设置
# 修改行列索引值
# data.index[2] = "股票88" 不能单独修改索引
stock_ = ["股票_{}".format(i) for i in range(10)]
data.index = stock_

# 重设索引
data.reset_index(drop=False) # drop=True把之前的索引删除

# 设置新索引
df = pd.DataFrame({'month': [1, 4, 7, 10],
                    'year': [2012, 2014, 2013, 2014],
                    'sale':[55, 40, 84, 31]})
# 以月份设置新的索引
df.set_index("month", drop=True)
# 设置多个索引，以年和月份
new_df = df.set_index(["year", "month"])
```

## MultiIndex 与 Panel
**MultiIndex**
多级或分层索引对象
index属性：
- names: levels 的名称
- levels: 每个 level 的元组值
```Python
new_df.index
new_df.index.names
new_df.index.levels
```

**Pannel   pandas.Panel(data=None,items=None,major_axis=None,minor_axis=None,copy=False,dtype=None)**
存储 3 维数组的 Panel 结构:
- items - axis 0，每个项目对应于内部包含的数据帧(DataFrame)。
- major_axis - axis 1，它是每个数据帧(DataFrame)的索引(行)。
- minor_axis - axis 2，它是每个数据帧(DataFrame)的列。
```Python
p = pd.Panel(np.arange(24).reshape(4,3,2),
                 items=list('ABCD'),
                 major_axis=pd.date_range('20130101', periods=3),
                 minor_axis=['first', 'second'])
p["A"]
p.major_xs("2013-01-01")
p.minor_xs("first")
# 注：Pandas 从版本 0.20.0 开始弃用，推荐的用于表示 3D 数据的方法是 DataFrame 上的 MultiIndex 方法
```
**Series： 带索引的一维数组**

### 属性

- index
- values

```Python
# 创建
pd.Series(np.arange(3, 9, 2), index=["a", "b", "c"])
# 或
pd.Series({'red':100, 'blue':200, 'green': 500, 'yellow':1000})

sr = data.iloc[1, :]
sr.index # 索引
sr.values # 值
```

## 基本数据操作


```Python
# 索引操作
data = pd.read_csv("./stock_day/stock_day.csv")
data = data.drop(["ma5","ma10","ma20","v_ma5","v_ma10","v_ma20"], axis=1) # 去掉一些不要的列
data["open"]["2018-02-26"] # 直接索引，先列后行

data.loc["2018-02-26"]["open"] # 按名字索引
data.loc["2018-02-26", "open"]
data.iloc[1, 0] # 数字索引

# 组合索引
# 获取行第1天到第4天，['open', 'close', 'high', 'low']这个四个指标的结果
data.iloc[:4, ['open', 'close', 'high', 'low']] # 不能用了
data.loc[data.index[0:4], ['open', 'close', 'high', 'low']]
data.iloc[0:4, data.columns.get_indexer(['open', 'close', 'high', 'low'])]

# 赋值操作
data.open = 100
data.iloc[1, 0] = 222

# 排序操作
# 1. 内容排序  -----使用 df.sort_values(key=,ascending=)对内容进行排序

key: 单个键或者多个键进行排序，默认升序

ascending=False:降序 True:升序
# 2. 索引排序 -----使用 df.sort_index 对索引进行排序
data.sort_values(by="high", ascending=False) # DataFrame内容排序
data.sort_values(by=["high", "p_change"], ascending=False).head() # 多个列内容排序
data.sort_index().head()
sr = data["price_change"]
sr.sort_values(ascending=False).head()
sr.sort_index().head()
```

## DataFrame运算

```Python
# 1. 算术运算
data["open"].add(3).head() # open统一加3  data["open"] + 3
data.sub(100).head() # 所有统一减100 data - 100
data["close"].sub(data["open"]).head() # close减open

# 2.逻辑运算  -----query(expr) expr:查询字符串; isin(values) 判断是否为 values
data[data["p_change"] > 2].head() # p_change > 2
data[(data["p_change"] > 2) & (data["low"] > 15)].head()
data.query("p_change > 2 & low > 15").head()

# 判断'turnover'是否为4.19, 2.39
data[data["turnover"].isin([4.19, 2.39])]

# 3.统计运算 -----describe(), 综合分析：能够直接得出很多统计结果，count,mean,std,min,max 等
data.describe()
data.max(axis=0)
data.idxmax(axis=0) #最大值位置

# 4.自定义运算 -----apply(func, axis=0)
# func: 自定义函数; axis=0: 默认按列运算，axis=1 按行运算
data.apply(lambda x: x.max() - x.min())
```

|   累计统计函数   |    作用   |
| :------ | :------ |
|   cumsum    |   计算前 1/2/3/../n 个数的和     |
|   cummax    |   计算前 1/2/3/../n 个数的最大值     |
|   cummin    |   计算前 1/2/3/../n 个数的最小值     |
|   cumprod    |  计算前 1/2/3/../n 个数的积      |

## Pandas画图
**pandas.DataFrame.plot**
pd.DataFrame.plot(x=None, y=None, kind='line')
- x: label or position, default None  # 指数据列的标签或位置参数
- y: label, position or list of label, positions, default None
- kind: str # 绘图类型
  - 'line': line plot (default) # 折线图   
  - 'bar' : vertical bar plot  # 条形图
  - 'barh' : horizontal bar plot # 横向条形图
  - 'hist' : histogram # 直方图（数值频率分布）
  - 'pie' : pie plot #饼图 数值必须为正值，需指定Y轴或者subplots=True
  - 'scatter' : scatter plot # 散点图。需指定X轴Y轴 

```Python
data.plot(x="volume", y="turnover", kind="scatter")
data.plot(x="high", y="low", kind="scatter")

# Pandas.Series.plot
sr.plot(kind="line")
```

## 文件的读取与存储
### CSV

1. read_csv
- pandas.read_csv(filepath_or_buffer, sep=',')
  - filepath_or_buffer：文件路径
  - usecols：列表，指定读取的列名
```Python
# 读取文件，并指定只获取open和close这两列
data = pd.read_csv("./data/stock_day.csv", usecols=['open', 'close'])
```
2. to_csv
- DataFrame.to_csv(path_or_buf=None, sep=',', columns=None, header=True, index=True, mode='w', encoding=None)
  - path_or_buf：文件路径sep：分隔符，默认用','分开
  - columns：选择需要的列索引
  - header：bool或字符串列表，是否保存列名，默认为 True ，保存
  - index：是否保存索引，默认为 True ，保存
  - mode：'w' 重写，'a' 追加

### HDF5
read_hdf()与 to_hdf()
HDF5 文件的读取和存储需要指定一个键，值为要存储的 DataFrame
pandas.read_hdf(path_or_buf, key=None, **kwargs)

从 h5 文件当中读取数据
- path_or_buffer: 文件路径
- key: 读取的键
- mode: 打开文件的模式
- reurn: The Selected object

DataFrame.to_hdf(path_or_buf, key, **kwargs)

```Python
day_close = pd.read_hdf("./stock_data/day/day_close.h5")
day_close.to_hdf("test.h5", key="close")
```

### Json

**read_json()
pandas.read_json(path_or_buf=None,orient=None,typ="frame",lines=False)**
- 将 JSON 格式转换成默认的 Pandas DataFrame 格式
- orient: string,Indication of expected JSON string format.


 

 




      
