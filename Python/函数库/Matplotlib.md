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
plt.show()
```
这句话会plot出三条线，每三个参数合成一个函数，r--表示红色的横线，bs表示蓝色正方形，g^表示绿色三角形，即颜色加形状的组合
<img src="https://matplotlib.org/_images/sphx_glr_pyplot_004.png" height="400" width="640">

如果要在一个figure中显示多个plot，就要用到subplot，下面是例子。当指定了figure之后，所有的操作都是对这个figure，如果指定了subplot，那么操作就是对指定的subplot使用，包括标题啊，什么的
```python
def f(t):
    return np.exp(-t) * np.cos(2*np.pi*t)

t1 = np.arange(0.0, 5.0, 0.1)
t2 = np.arange(0.0, 5.0, 0.02)

plt.figure(1) #这个参数1表示figure的id，在多个figure的时候不冲突，而这句话指定了我们下面的plot会对figure(1)进行操作，默认就是figure(1)
plt.subplot(211) # 纵向分成两个图，表示指定上面的图
plt.plot(t1, f(t1), 'bo', t2, f(t2), 'k') # 在指定的图上面plot东西
plt.subplot(212) # 指定下面的图
plt.plot(t2, np.cos(2*np.pi*t2), 'r--') # plot另一些东西
plt.show()
```
subplot接受的参数一定是一个三位的数字，第一位表示figure纵向分成几份，第二位表示figure横向分成几份，第三位表示在前两位分割的情况下，这个subplot的index。比如322就表示，最多6个subplot，这个subplot位于6个的第二位，如果在一个figure下多个subplot的位置有重叠，那么后面的会override前面的，所以建议所有的subplot的前两位要一样。
<img src="https://matplotlib.org/_images/sphx_glr_pyplot_007.png" height="400" width="640">
