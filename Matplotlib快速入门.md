# Matplotlib快速入门
## 什么是matplotlib？
- mat - matrix（黑客帝国）矩阵
- plot - 画图
- lib - library 库
主要功能：数据可视化
## 实现一个简单的Matplotlib画图
```
import matplotlib.pyplot as plt  # 导包
plt.figure()                     # 创建一块画布用来添加数据
plt.plot([1, 0, 9],[4, 5, 6])    # 按x，y传入三个点的坐标（1，4），（0，5），（9，6）
plt.show()                       # 数据可视化
```
![image](https://user-images.githubusercontent.com/129270106/228537672-9baa4e97-f987-4540-bed4-5dbe487c2a0e.png)
## 认识Matplotlib图像结构
![image](https://user-images.githubusercontent.com/129270106/228537797-ebfaba6a-3d65-44f4-b8fb-325023abfe26.png)
# Matplotlib三层结构
1. 容器层：由画板层，画布层，坐标系组成。

  画板层Canvas

  画布层Figure
  ```
  plt.figuer()
  ```
  绘图区/坐标系(axis)
  ```
  plt.subplots()
  ```
  2. 辅助显示层: 除了通过数据绘制出的图像以外的内容.包括坐标系外观(faceclolr)、边框线(spines)、坐标轴(axis)、坐标轴名称(axis label)、坐标轴刻度(tick)、刻度标签(tick label)、网格线(grid)、图例(legend)、标题(title)等内容
      
  3. 图像层: 通过plot, scatter, bar, histogram, pie等函数绘制出的图像。
