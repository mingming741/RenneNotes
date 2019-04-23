# Matplotlib
通常用于机器学习的数据集绘图和结果展示，plot接受的object为np.array
```python
import matplotlib.pyplot as plt
```
下面给出最简单的例子，在matplotlib中，figure指的是一张完整的图片，一个figure可以包含多个Axe，每个Axe包含一个坐标系，可以理解为一个subplot。Axes contains two (or three in the case of 3D) Axis objects。可以使用set_xlim() and set_ylim()改变对应的Axe的x和yAxis(坐标轴)的最大值。类``Artist``表示figure中的所有的object的基类，
```python
import numpy as np

fig = plt.figure()  # 初始化一个figure，所有的data都需要在一个figure上显示
fig.suptitle('No axes on this figure')  # 改变一下这回figure的显示名称

fig, ax_lst = plt.subplots(3, 2)  # ax_list是一个numpy.ndarray
print(ax_lst.shape) # (3, 2)，可以理解为Axe是一个figure里面的sub figure的集合
plt.show() # 显示这个figure，确切的来说，应该是显示所有的plot
#注意，plt.show()会将之前放入plt中的config全部清楚掉，这个函数调用两次第二次是不会有东西的，如果不调用show，在jupyter里面也会显示出来这个数据，并且数据会得以保存
```
下面介绍怎么话一个最简单的图
```python
fig = plt.figure()  # an empty figure with no axes
x = np.linspace(0, 2, 100) # 位于[0,2]之间的100个数据组成的linear line
plt.plot(x, x, label='linear') # 添加一条线
plt.plot(x, x**2, label='quadratic') # 添加第二条线
plt.plot(x, x**3, label='cubic') # # 添加第三条
plt.xlabel('x label') # 改变这个figure的x的label
plt.ylabel('y label') # 改变y
plt.title("Simple Plot") # 改变标题
plt.legend() # 是否显示legend，即每条线的label和颜色
plt.show() #显示这张图
```
接下来介绍plt.plot函数，这个函数可以接受不同的值，并且plot出不一样的效果，
```python
import numpy as np
t = np.arange(0., 5., 0.2) # t表示横坐标

# red dashes, blue squares and green triangles
plt.plot(t, t, 'r--', t, t**2, 'bs', t, t**3, 'g^')
#这句话会plot出三条线，每三个参数合成一个函数，r--表示红色的

plt.show()
```
<img src="https://matplotlib.org/_images/sphx_glr_pyplot_004.png" height="400" width="640">
