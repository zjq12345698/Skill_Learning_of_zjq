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



      
