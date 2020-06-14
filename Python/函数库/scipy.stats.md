# scipy.stats
scipy.stats提供各种概率分布的随机变量，应该可以帮助建立模型。

### basic code
`np.linspace`用于产生坐标轴空间，`np.linspace(2.0, 3.0, num=5)`即在2和3之间产生一个一共为5个数的坐标空间，有点类似np.arange()，但是多用于科学计算的基底。
```python
y = np.linspace(start=2.0, stop=3.0, num=5)
print(y) # [2.   2.25 2.5  2.75 3.  ]
```

### rv_continuous & rv_discrete
scipy.stats中大部分随机变量都继承了`rv_continuous`和`rv_discrete`这两个基类，所以这里介绍一些基类的使用方法。

`rv_continuous.ppf`的ppf表示Percent point function，即CDF的反函数，例如`norm.ppf(0.01)`和`norm.ppf(0.99)`出来的value为`-2.326`和`2.326`，即standard norm的99% confidence interval。


### norm
正态分布，这里用于做参考的例子, `scipy.stats.norm`
```python
from scipy.stats import norm
fig, ax = plt.subplots(1, 1)
mean, var, skew, kurt = norm.stats(moments='mvsk')
x = np.linspace(norm.ppf(0.01), norm.ppf(0.99), 100)
probabiliy = norm.pdf(x)
ax.plot(x, probabiliy, 'r', lw=5, alpha=0.6, label='uniform pdf')
plt.show()
```
这里是默认参数的normal distribution，pdf这个函数会根据当前distribution的参数，在输入的x轴上生成对应的概率。


