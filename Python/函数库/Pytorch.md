# Pytorth
pytorch相当于用GPU来处理numpy的ndarray，在torch中即tensor，在torch中，tensor是一个类，每个tensor都自带一个datatype

### 基础知识
```python
#torch中有自己定义的一些datatype，
x = torch.randn_like(x, dtype=torch.float)    # override dtype!
print(x)   
#tensor([[ 0.3928, -0.4377, -0.6426],
#        [ 0.6000,  0.1942, -0.9790],
#        [-3.0629,  0.2410, -1.5378],
#        [ 0.0219,  0.5899,  0.8386],
#        [-0.1540,  0.2724,  0.3881]])

#x和y是两个tensor的时候，torch有function对其进行操作
torch.add(x, y, out=result) #讲输出赋值给result
x.size() #tensor的size
y.add_(x) #同样加法操作

#tensor size和resize，使用torch.view
x = torch.randn(4, 4)
y = x.view(16)
z = x.view(-1, 8)  # the size -1 is inferred from other dimensions，即自动转化
print(x.size(), y.size(), z.size())
# torch.Size([4, 4]) torch.Size([16]) torch.Size([2, 8])

#numpy的ndarray和torch tensor可以互相转化
a = np.ones(5)
b = torch.from_numpy(a)
np.add(a, 1, out=a)
print(a) # [2. 2. 2. 2. 2.]
print(b) # tensor([2., 2., 2., 2., 2.], dtype=torch.float64)

#to()函数可以将tensor赋给一个device
# We will use ``torch.device`` objects to move tensors in and out of GPU
if torch.cuda.is_available():
    device = torch.device("cuda")          # a CUDA device object
    y = torch.ones_like(x, device=device)  # directly create a tensor on GPU
    x = x.to(device)                       # or just use strings ``.to("cuda")``
    z = x + y
    print(z)  # tensor([-0.7816], device='cuda:0')
    print(z.to("cpu", torch.double))       # tensor([-0.7816], dtype=torch.float64)，to也可以改变类型
```


### Tensor和Backward
```python
requires_grad = True # track all operations on tensor
.backward() # 用于计算全部的gradient，输出在.grad的attribute中
```
torch中，Function也是一个类，并且和tensor通过.grad_fn找到create他的Function。.backward()可以被tensor调用来看到他的训练过程，在这里tensor即训练data
```python
x = torch.ones(2, 2, requires_grad=True)
y = x + 2
print(y) # tensor([[3., 3.], [3., 3.]], grad_fn=<AddBackward0>)
#在这里，y是通过加法操作被创建的，所有y有自己的grad_fn值，tensor默认print会print所有他有的attribute。这里AddBackword表示加法
z = y * y * 3
out = z.mean()
print(z, out)
# tensor([[27., 27.], [27., 27.]], grad_fn=<MulBackward0>)   tensor(27., grad_fn=<MeanBackward0>)
# 不同的operation创建的tensor会有不同的grad_fn，记录backward的内容，这就是autograd

out.backward() # 计算out的backward path这里会计算所有被grad_fn关联起来的，和out有关的tensor的".grad"的attribute 
print(x.grad)  # tensor([[4.5000, 4.5000], [4.5000, 4.5000]]) 经过out的backward，x.grad已经被计算出来了。
```
在计算gradient的时候，如果x个y都是vector，那么y对x的倒数会变成一个matrix，(Jacobian matrix)，下面的例子给出torch的处理方法
```python
x = torch.randn(3, requires_grad=True)
y = x * 2
while y.data.norm() < 1000: # 对y进行多次翻倍操作直到平均值大于1000
    y = y * 2 
print(y) # tensor([-333.7958, -287.0981, 1274.5677], grad_fn=<MulBackward0>)
v = torch.tensor([0.1, 1.0, 0.0001], dtype=torch.float)
y.backward(v) # 在y不是scaler的时候，需要pass一个同样的vector进去 （这里我没看懂）
print(x.grad) # tensor([2.0480e+02, 2.0480e+03, 2.0480e-01])

# 通常在最后prediction的时候，不需要再计算gradient，只要forward path，这样写可以避免一些问题
with torch.no_grad():
    print((x ** 2).requires_grad)
```

### CNN
torch的cnn基于torch.nn，下面记录一下nn中的实现
####  Module ``torch.nn.Module``
通常表示一个层，因为Module中也可以包括其他Module，所以也可以表示一个网络，这里给一个torch官网上面的的简单的例子
```python
import torch.nn as nn
import torch.nn.functional as F # F可以提供一些操作，可以理解为将一些Layer变成函数的表达方式

class Model(nn.Module):
    def __init__(self):
        super(Model, self).__init__()
        self.conv1 = nn.Conv2d(1, 20, 5)
        self.conv2 = nn.Conv2d(20, 20, 5)

    def forward(self, x):
       x = F.relu(self.conv1(x)) #一个层即是一个函数，调用这一层的函数可以得到输出
       return F.relu(self.conv2(x)) 
```


### MLP (Multi-layer Perceptron)多层感知机
多层感知器即多个单层感知器链接而成，下图表示单层感知机(PLA: Perceptron Learning Algorithm)的的结构，其实就是一个线性分类器的算法。更准确的说，PLA是一个线性的二分类器，但不能对非线性的数据并不能进行有效的分类，所以在这里引用多层的结构，理论上，多层的网络可以模拟复杂的函数。
<br/><img src = "https://github.com/mingming741/RenneNotes/blob/master/Resource/Image/多层感知器_单层.png" width = 375/><br/>
而多层的分类器本质上是全链接层和激活层的嵌套链接，最后可以达到输出多个结果的效果
<br/><img src = "https://github.com/mingming741/RenneNotes/blob/master/Resource/Image/多层感知器_多层.png" width = 375/><br/>


### 训练



### 实战操作
```python
#给神经网络添加initial weight
def init_weights(m):
    if type(m) == nn.Linear:
    m.weight.data.fill_(1.0)
    print(m.weight)
>>> net = nn.Sequential(nn.Linear(2, 2), nn.Linear(2, 2))
>>> net.apply(init_weights)


```
