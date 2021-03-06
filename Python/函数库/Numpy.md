
# 函数库
### numpy
numpy的array其实就是对python自带的array做了封装，支持一些矩阵运算操作，并且这些矩阵运算操作涉及到了一些优化，numpy的array可以和python自己的array互相转化
```python
# numpy产生的array是一个numpy array object
x = [1, 2, 3] # x is a list
a = np.array(x) # array([1, 2, 3])
type(a) # numpy.ndarray
b = a.tolist()
type(b) # list

#因为是ndarray，所以这个数值可以是一个tensor
b = np.array([[1,2,3],[4,5,6]])    # Create a rank 2 array
print(b.shape)                     # Prints "(2, 3)"

#numpy中有一些initial矩阵的操作
np.zeros((2,2))  # 2x2的全0矩阵
b = np.ones((3,2))  # 3x2的全1矩阵，第一个数值表示#row，即(m * n)的格式输入，m=3，n=2
c = np.full((2,2), 6) # 2x2的全6矩阵，full用于初始化填充
d = np.eye(2)  # identity matrix I，即对角为1的矩阵
e = np.random.random((2,2)) # 随机矩阵
```
#### numpy Slicing
等同于list的slicing，(即array\[1:6\]的操作)，可以从ndarray中取出sub-matrix
```python
a = np.array([[1,2,3,4], [5,6,7,8], [9,10,11,12]])
b = a[:2, 1:3] # 取出a的0到2个row(不包括第二行)和1到3个column(不包括第三列) 
# b= [[2 3]
#    [6 7]]

#Slicing的原来的矩阵对应的是同一个object，即指针不一样，修改并不是copy而是引用
print(a[0, 1])   # 2
b[0, 0] = 77     # b[0, 0] is the same piece of data as a[0, 1]
print(a[0, 1])   # 77

#Slicing的时候注意不同的维度
row_r1 = a[1, :]
row_r2 = a[1:2, :]  
print(row_r1, row_r1.shape)  # Prints "[5 6 7 8] (4,)"， 相当于取出来了一个新的row，dimension变成1
print(row_r2, row_r2.shape)  # Prints "[[5 6 7 8]] (1, 4)"，相当于舍弃了之前的一些部分，矩阵还是矩阵
#对column也是一样
col_r1 = a[:, 1]
col_r2 = a[:, 1:2]
print(col_r1, col_r1.shape)  # Prints "[ 2  6 10] (3,)"
print(col_r2, col_r2.shape)  # Prints "[[ 2]
                             #          [ 6]
                             #          [10]] (3, 1)"
```
#### numpy index
index可以和slicing一同构建array
```python
a = np.array([[1,2], [3, 4], [5, 6]])
print(a[[0, 1, 2], [0, 1, 0]])  # Prints "[1 4 5]"，即a的[0,0],[1,1],[2,0]的位置构成的的row
print(np.array([a[0, 0], a[1, 1], a[2, 0]]))  # Prints "[1 4 5]"，等同于上面

# 同样也可以用一个ndarry做index去选取另一个ndarray的内容
a = np.array([[1,2,3], [4,5,6], [7,8,9], [10, 11, 12]])
b = np.array([0, 2, 0, 1])
print(a[np.arange(4), b])  # Prints "[ 1  6  7 11]"， arrage函数会遍历a的row并且取出b中对应的位置的数值
a[np.arange(4), b] += 10   # 在b指定的位置上全部做+10的操作
```

#### numpy dtype
dtype是numpy ndarray的matrix中存的数字的data type。
```python
x = np.array([1.0, 2.0])   # Let numpy choose the datatype
print(x.dtype)  # Prints "float64"
```

#### numpy array math
numpy比起本身的数组，好在其对array的操作性能，而实现这些性能我们需要用到numpy内置的函数，这些operator都是返回一个numpy的ndarray或者是一个标量，并不改变原来的操作单元，如果想改写需要额外的赋值(assignment)
```python
x = np.array([[1,2],[3,4]], dtype=np.float64)
y = np.array([[5,6],[7,8]], dtype=np.float64)
print(x + y) # 等同于 np.add(x, y)，矩阵相加
# [[ 6.0  8.0]
#  [10.0 12.0]]

print(x - y) # 等同于 np.subtract(x, y)，矩阵相减
# [[-4.0 -4.0]
#  [-4.0 -4.0]]

# 这里对比一下list的相加，list的operator加号(+)是list append，即将两个list合成一个list的操作
a = [1,3]
b = [2,4]
print(a + b) # print(a + b)
# list没有operator减号(-)

# 同样numpy还有*,/,开根号
x * y # np.multiply(x, y)
x / y # np.divide(x, y)
np.sqrt(x)

# 矩阵点乘，按照不同的类型也会有不同的操作方式，第二个数是标量的时候会做transport，是矩阵的时候不会做
x = np.array([[1,2],[3,4]])
y = np.array([[5,6],[7,8]])
v = np.array([9,10])
w = np.array([11, 12])
# Inner product of vectors
v.dot(w) # 219，等于np.dot(v, w)，即v乘以w的transport，得到一个标量
# Matrix / vector product
x.dot(v) # [29 67]， rank 1 array，x乘以v的transport，得到矩阵，等同于矩阵乘以向量
# Matrix / matrix 
x.dot(y) # product both produce the rank 2 array，x乘以y，矩阵叉乘
# [[19 22]
#  [43 50]]

# 求和函数sum，返回标量和，注意这个和add的区别
x = np.array([[1,2],[3,4]])
print(np.sum(x))  # Compute sum of all elements; prints "10"
print(np.sum(x, axis=0))  # Compute sum of each column; prints "[4 6]"

# transport，ndarray自带attribute 'T'
x = np.array([[1,2], [3,4]])
print(x.T)  

```
