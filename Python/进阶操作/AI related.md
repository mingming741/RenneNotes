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
* numpy的矩阵方向和matrix是相反的，是一个row然后再是一个column，不是column base的矩阵。例如`a[1,2]`表示a的第2个row的第3个element。并且array没有绝对的方向，一个行或者说一个列都可以是一个list。
* index: `a[[0, 2, 3]]`可以取出矩阵a的第0，2， 3和row，和`[-1:1]`这个操作类似，因为a的第一维是row，同理`a[1:3]`可以取出a的第2到第4行。
* operator：numpy中的operator都是element wise的operation。 例如两个list的`a * b`得到的还是一个list，而矩阵乘法的operator是`@`。
* boardcasting: matrix + array的操作，会把array的值boardcast到matrix的每一个row上面。对于column同样apply。


### numpy.linalg
为numpy中追加的用于线性代数操作的函数库
* linalg.det(a): 计算矩阵的determine。
* np.dot(a, b): 计算矩阵乘法。和operator`@`相同，本质都是matrix product
* linalg.inv(a): 计算矩阵a的inverse。
* linalg.solve(A, b)：solve x，with Ax = b。
* linalg.eig()，获取eigenvalue和eigenvector。


# matplotlib
用于可视化的工具包
* line format：可以通过字符同时调整整个line的格式，例如`o--r`表示point为实心`o`，线为虚线`--`，颜色为红色`r`的line plot。
* bar chart是可以叠加在一起的，本质上是控制width和bar的position，将不同颜色的bar拼在一起。
* pie chart: 用于可视化不同part的比率，可以用label把不同的bar都展示出来。
* image： 本质上3位的tensor，表示rgb的三个channel的值，可以通过plt.imshow(img)展示出每个channel的值。









