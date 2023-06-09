# Numpy快速入门

## 基本介绍

num - numerical 数值化的
py - python 

Numpy(Numerical Python)是一个开源的科学计算库，用于快速处理任意维度的数组，支持常见的数组和矩阵操作。
Numpy使用ndarray对象来处理多维数组，该对象是一个快速而灵活的大数据容器。

### ndarray介绍

n - 任意个
d - dimension 维度
array - 数组

### 优势

1. 存储风格
  - ndarray - 相同类型 - 通用性不强
  - list - 不同类型 - 通用性很强
2. 并行化运算
  - ndarray 支持向量化运算
3. 底层语言
  - Numpy 底层使用 C 语言编写，内部解除了 GIL（全局解释器锁），其对数组的操作速度不受 Python 解释器的限制，效率远高于纯 Python 代码。

### 属性

| 属性名字 | 属性解释 |
| :-----| :----  |
| ndarray.shape | 数组维度的元组 |
| ndarray.ndim | 数组维数 |
| ndarray.size | 数组中的元素数量 |
| ndarray.itemsize	 | 一个数组元素的长度（字节） |
| ndarray.dtype | 数组元素的类型 |

在创建 ndarray 的时候，如果没有指定类型，默认：整数 int64/int32 浮点数 float64/float32

## 应用

```Python
import numpy as np
score = np.array([[80, 89, 86, 67, 79],
[78, 97, 89, 67, 81],
[90, 94, 78, 67, 74],
[91, 91, 90, 67, 69],
[76, 87, 75, 67, 86],
[70, 79, 84, 67, 84],
[94, 92, 93, 67, 64],
[86, 85, 83, 67, 80]])
print(type(score))
print(score.shape)
print(score.dtype)

# 创建数组的时候指定类型
np.array([1.1, 2.2, 3.3], dtype="float32")
```

## 基本操作

### 一. 生成数组
```Python
# 1，生成0和1的数组
np.zeros(shape=(3, 4), dtype="float32") # 生成一组0
np.ones(shape=[2, 3], dtype=np.int32) # 生成一组1

# 2，从现有数组生成
data1 = np.array(score) # 深拷贝（常用）
data2 = np.asarray(score) # 浅拷贝
data3 = np.copy(score) # 深拷贝

# 3，生成固定范围的数组
np.linspace(0, 10, 5) # 生成[0,10]之间等距离的5个数
np.arange(0, 11, 5) # [0,11)，5为步长生成数组

# 4，生成均匀分布的一组数[low,high)
data1 = np.random.uniform(low=-1, high=1, size=1000000)

# 5，生成正态分布的一组数，loc：均值；scale：标准差
data2 = np.random.normal(loc=1.75, scale=0.1, size=1000000)

```

### 二. 对数组的基本操作

```Python
# 1.数组的索引、切片
stock_change = np.random.normal(loc=0, scale=1, size=(8, 10))
# 获取第一个股票的前3个交易日的涨跌幅数据
print(stock_change[0, :3])
a1[1, 0, 2] = 100000

# 2.形状修改
stock_change.reshape((10, 8)) # 返回新的ndarray,原始数据没有改变
stock_change.resize((10, 8)) # 没有返回值，对原始的ndarray进行了修改
stock_change.T # 转置 行变成列，列变成行

# 3.类型修改
stock_change.astype("int32")
stock_change.tostring() # ndarray序列化到本地

# 4.数组去重
temp = np.array([[1, 2, 3, 4],[3, 4, 5, 6]])
np.unique(temp)
set(temp.flatten())

```

## ndarray计算

### 1. 运算符
```Python
# 逻辑判断, 如果涨跌幅大于0.5就标记为True 否则为False
stock_change > 0.5
stock_change[stock_change > 0.5] = 1.1

# 判断stock_change[0:2, 0:5]是否全是上涨的
np.all(stock_change[0:2, 0:5] > 0)
# 判断前5只股票这段期间是否有上涨的
np.any(stock_change[:5, :] > 0)

# np.where(布尔值，True的位置的值，False的位置的值)
np.where(temp > 0, 1, 0)
# 大于0.5且小于1
np.where(np.logical_and(temp > 0.5, temp < 1), 1, 0)
# 大于0.5或小于-0.5
np.where(np.logical_or(temp > 0.5, temp < -0.5), 11, 3)

```

### 2. 统计运算(min,max,mean(均值),median(中位数),var(方差),std(标准差))

```Python
temp.max(axis=0)
np.max(temp, axis=1)

# 返回最大值、最小值的位置
np.argmax(tem,axis=)
np.argmin(tem,axis=)

```

### 3. 数组间运算

```Python
# 数组与数字间的运算
arr = np.array([[1, 2, 3, 2, 1, 4], [5, 6, 1, 2, 3, 1]])
arr / 10

# 数组与数组之间的运算（维度相等，shape（其中相对应的一个地方为 1）两个条件都得满足
np.dot(data,data1)
np.matmul(data,data1)
data @ data1

# 矩阵（matrix）必须是二维的
matrix1*matrix2

```

### 合并和分割

```Python
numpy.hstack           #水平拼接
numpy.vstack           #竖拼接
numpy.concatenate((a1,a2),axis=0)      #水平|竖拼接

numpy.split(x,3)           # 分三份
numpy.split(x,[3,4,6,10])   # 按索引分割
```

###  IO操作与数据处理

```Python
np.genfromtxt("test.csv", delimiter=",") # 会有问题，读不出字符串
```

### 处理缺失值

  - 直接删除含有缺失值的样本
  - 替换/插补 （补入平均值或中位数）

