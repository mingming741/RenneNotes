# scipy.stats
scipy.stats提供各种概率分布的随机变量，应该可以帮助建立模型。

### basic code
`np.linspace`用于产生坐标轴空间，`np.linspace(2.0, 3.0, num=5)`即在2和3之间产生一个一共为5个数的坐标空间。
```python
y = np.linspace(start=2.0, stop=3.0, num=5)
print(y) # [2.   2.25 2.5  2.75 3.  ]
```
区别于`np.arange()`，`np.linspace`多用于连续随机变量的x轴，而`np.arange()`多用于离散随机变量的x轴

### rv_continuous & rv_discrete
scipy.stats中大部分随机变量都继承了`rv_continuous`和`rv_discrete`这两个基类，所以这里介绍一些基类的使用方法。

`rv_continuous.ppf`的ppf表示Percent point function，即CDF的反函数，例如`norm.ppf(0.01)`和`norm.ppf(0.99)`出来的value为`-2.326`和`2.326`，即standard norm的99% confidence interval。如果ppf的输入是一个list，那么输出也是一个list对应的ppf的value。


### norm
正态分布，这里用于做参考的例子, `scipy.stats.norm`
```python
from scipy.stats import norm
fig, ax = plt.subplots(1, 1)
mean, var, skew, kurt = norm.stats(moments='mvsk') # default config
x = np.linspace(norm.ppf(0.01), norm.ppf(0.99), 100) # 产生x坐标轴
probabiliy = norm.pdf(x, loc=0.3, scale=1.2) # 直接出概率
ax.plot(x, probabiliy, 'r', lw=5, alpha=0.6, label='uniform pdf')
plt.show()
```
这里是默认参数的normal distribution，pdf这个函数会根据当前distribution的参数，在输入的x轴上生成对应的概率。如果要使用mean和standard deviation，使用`norm.pdf(x, loc, scale)`，这里loc表示mean，scale表示standard deviation。这里的parameter是在生成probability distribution的时候才产生的，或许可以用来implement我自己的class。
```
r = norm.rvs(size=1000) # rvs为generate sample的函数，会产生histogram 
ax.hist(r, density=True, histtype='stepfilled', alpha=0.2) # 可以在matplotlab中使用hist函数将其绘制。
ax.legend(loc='best', frameon=False)
plt.show()
```




