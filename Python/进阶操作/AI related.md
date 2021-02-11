# numpy

### ndarray
numpy中的所有class都是ndarray，并且数据类型默认是int64。所有的function都是对应这个class的method。

* ndarray.ndim: 表示matrix的维度，1表示array，2表示matrix
* ndarray.size：表示number of element，即所有维度的相乘
* ndarray.shape：每个维度的长度
* ndarray.dtype： 数据类型，在创建的时候可以选择，可以使用astype('float32')来转化，numpy中的数据类型和python的数据类型是不同的。


### numpy的函数
创建ndaray的函数
* np.empty(size)：创建未初始化的ndarray，里面的值是计算机上次的随机值。
* np.diag(list): 创建对角矩阵
* np.one, np.zero: 创建初始是1/0的矩阵。
* np.arange(start, end, step_size): 创建从start到end的坐标轴，不包含end这个点，默认类型是int
* np.linspace(strat, end, n_point): 创建从start到end的坐标轴吗，包含end这个点，默认类型是浮点数，常用于数学的计算。
* np.squeeze(): 自动把维度中只有一个column的维度去掉，用于矩阵的降维。
* np.ravel(): 压缩全部维度，将一个ndarray变成一维的list(还是ndarray)


### numpy的基本操作
* 
