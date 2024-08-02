# Tensor
笔记总结于来自[PyTorch官方教程](https://pytorch.org/tutorials/beginner/basics/tensorqs_tutorial.html)

#### Tensor:
Tensor为pytorch中存储数据的数据结构，类似平时使用的list。`Tensor`和numpy的`ndarrays`非常相似，而区别在于`Tensor`可以在GPU或者其他加速硬件上运行。`Tensor`和`ndarrays`在内存上甚至可以share，并且可以相互转化。额外的，`Tensor`自带自动计算梯度的功能。

Tensor的构建和对应函数
```python
import torch
import numpy as np

# -------------Tensor可以通过普通的list直接构建
data = [[1, 2],[3, 4]]
x_data = torch.tensor(data)
# tensor([[1, 2],
#        [3, 4]])

# Tensor也可以通过ndarrays去构建
np_array = np.array(data)
x_np = torch.from_numpy(np_array)

# 可以使用 like()/like() 函数构建和原有的Tensor同样shape的Tensor
x_ones = torch.ones_like(x_data)
# tensor([[1, 1],
#        [1, 1]])
x_rand = torch.rand_like(x_data, dtype=torch.float) # 随机Tensor

# --------------Tensor也可以通过指定的tuple给定shape去创建
shape = (2,3,) # 创建 2 x 3的Tensor
ones_tensor = torch.ones(shape) # 全1Tensor
# tensor([[1., 1., 1.],
#       [1., 1., 1.]])
rand_tensor = torch.rand(shape) # 随机Tensor
zeros_tensor = torch.zeros(shape) # 全0Tensor

# --------------numpy存在的method，torch中也有类似的构筑-----------
# [[1, 2],[3, 4]]
# ndim：矩阵维度
print(np_array.ndim) # 2
print(x_np.ndim) # 2
# size：矩阵大小，numpy中为所有行x列，torch中为所有维度具体的值，为size()函数
print(np_array.size) # 2x2 = 4
print(x_np.size()) # torch.Size([2, 2])
# shape为每个维度的长度，torch中和size一样
print(np_array.shape) # (2, 2)
print(x_np.shape) # torch.Size([2, 2])
# dtype数据类型，默认为int64
print(np_array.dtype) # int64
print(x_np.dtype) # torch.int64
# device为Tensor的处理器，numpy中没有对应的attribute
print(x_np.device) # cpu

# --------------Tensor和ndarrays可以互相转化，但是其底层的memory是share的
t = torch.ones(3)
n = t.numpy()
print(f"t: {t}") # t: tensor([1., 1., 1.])
print(f"n: {n}") # n: [1. 1. 1.]
t.add_(1) # 给Tensor所有element都+1，对应的numpy也会+1
print(f"t: {t}") # t: tensor([2., 2., 2.])
print(f"n: {n}") # n: [2. 2. 2.]

n = np.ones(3)
t = torch.from_numpy(n)
np.add(n, 1, out=n)
print(f"t: {t}") # t: tensor([2., 2., 2.], dtype=torch.float64)
print(f"n: {n}") # n: [2. 2. 2.]
```

### Tensor的Operation
包括arithmetic, linear algebra, matrix manipulation (transposing, indexing, slicing), sampling，更多参考[PyTorch官方教程](https://pytorch.org/docs/stable/torch.html)

在有gpu的情况下，可以将tensor的计算指定到gpu上
```python
if torch.cuda.is_available():
    tensor = tensor.to("cuda")
```

Tensor的操作大部分和numpy类似
```python
data = [[11,12,13,14],
        [21,22,23,24],
        [31,32,32,34],
        [41,42,43,44]]
tensor = torch.tensor(data, dtype=torch.float32)
print(f"First row: {tensor[0]}") # tensor([11, 12, 13, 14])， Tensor的row和column和数学中的是相反的，第一个维度就是row，和numpy类似
print(f"First column: {tensor[:, 0]}") # tensor([11, 21, 31, 41])，第一个column，:表示全部，0表示第一个
print(f"Last column: {tensor[..., -1]}") # tensor([14, 24, 34, 44])，这里的...和：作用是一样的
print(tensor[0:3,1:4]) # 可以对任意dimension取值，比如这里的submatrix取了前三个row和的三个column（右上角的3x3）
#tensor([[12, 13, 14],
#        [22, 23, 24],
#        [32, 32, 34]])
tensor[:,1] = 0  # 这里可以通过全选将一个列全部设置成0
print(tensor)
# tensor([[11,  0, 13, 14],
#        [21,  0, 23, 24],
#        [31,  0, 32, 34],
#        [41,  0, 43, 44]])
print(tensor.T) # Tensor的transport
# tensor([[11., 21., 31., 41.],
#        [ 0.,  0.,  0.,  0.],
#        [13., 23., 32., 43.],
#        [14., 24., 34., 44.]])
t1 = torch.cat([tensor, tensor, tensor], dim=1) # Tensor也可以通过cat函数join到一起，dim表示join的方向，指定的方向会增加
print(t1)
# tensor([[11,  0, 13, 14, 11,  0, 13, 14, 11,  0, 13, 14],
#        [21,  0, 23, 24, 21,  0, 23, 24, 21,  0, 23, 24],
#        [31,  0, 32, 34, 31,  0, 32, 34, 31,  0, 32, 34],
#        [41,  0, 43, 44, 41,  0, 43, 44, 41,  0, 43, 44]])
```
Tensor也可以进行数学操作
```python
tensor = torch.ones(4, 4)
tensor[:,1] = 0

agg = tensor.sum() # tensor(12.)， sum()函数会把tensor所有的element相加变成一个1x1的tensor
agg_item = agg.item() # 把1x1的tensor变成scalar
print(agg_item, type(agg_item)) # 12.0 <class 'float'>

tensor.add_(5) # add_()函数表示给每个element相加一个scalar
print(tensor)
# tensor([[6., 5., 6., 6.],
#        [6., 5., 6., 6.],
#        [6., 5., 6., 6.],
#        [6., 5., 6., 6.]])
```

































##### END
