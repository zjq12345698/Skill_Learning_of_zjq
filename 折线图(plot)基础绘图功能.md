## 一. Matplotlib.pyplot模块

Matplotlib.pyplot包含了一系列类似于matlab的画图函数，作用于当前图形（Figure）的当前坐标系（axes）

```
import matplotlib.pyplot as plt
```

## 二. 线图（plot）的基本绘制和展示

```
plt.figure()    # 创建画布
plt.plot(数据，color=" ", linestyle=" ") # 绘图
plt.show()      # 显示图像 
```

## plot基本绘图功能
### 1. 设置画布属性 plt.figure(figsize=( ， )dpi=)

figsize: 画布长、宽
dpi: 图像的清晰度

### 2. 自定义x、y轴刻度 plt.xticks(x， 刻度说明),  plt.yticks(y， 刻度说明)

x, y: 要显示的刻度值

标签应用中文：plt.rc('font',family='Simhei',size=13)

### 3. 添加网格显示 plt.grid(linestyle=' ', alpha= ，color='')

linestyle: 网格线格式       -  --  -.  :  ''
alpha: 透明度（0~1）
color: 红色r、绿色g、 蓝色b、 白色w、 青色c、 洋红m、 黄色y、 黑色k、 

### 4. 添加x、y轴描述信息及标题

```
plt.xlabel()
plt.ylabel()
plt.title()
```

### 5. 显示图例 plt.legend(loc=)

loc: 图例显示位置（0~6），默认0，在右上角


## 三. 多个坐标系显示plt.sublpots()

```
matplotlib.pylot.subplots(nrows= , ncols=  )

# 几行几列，创建一个含有多个axes（坐标轴）的figure
# returns：
      fig ：图对象
fig, axes = plt.subplots(nrows=1, ncols=2)
```

设置标题等方法： set_xticks, set_xlabel




