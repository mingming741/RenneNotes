# 基础知识
pytorch相当于用GPU来处理numpy的ndarray，在torch中即tensor，在torch中，tensor是一个类，每个tensor都自带一个datatype
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


# Tensor和Backward
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

# NN
torch的cnn基于torch.nn，下面记录一下nn中的实现。
####  Module 
```torch.nn.Module```
Module的基类，所有的nn都继承了Module。可以理解为一个函数，对给定的input做了这个output的操作。通常表示一个层。也可以表示一个container，这时候用``nn.Sequential``表示。因为Module中也可以包括其他Module，所以也可以表示一个网络，这里给一个torch官网上面的的简单的例子
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
使用``add_module``函数给一个Module添加子的Module，``modules()``得到这个module的全部sub module，``cuda(device=None)``将内容放到gpu上去跑。``apply(fn)``将一个torch的函数使用到全部children中去。

```torch.nn.Sequential```
Sequential也是继承了Module，专门用来做container的结构。

```torch.nn.ModuleList```
ModuleList和Sequential类似，不过method比Sequential要多一些，有insert，append，extend等方法，而Sequantial的方法是forward
```python
class MyModule(nn.Module):
    def __init__(self):
        super(MyModule, self).__init__()
        self.linears = nn.ModuleList([nn.Linear(10, 10) for i in range(10)])

    def forward(self, x):
        # ModuleList can act as an iterable, or be indexed using ints
        for i, l in enumerate(self.linears):
            x = self.linears[i // 2](x) + l(x)
        return x
```
和ModuleList类似，torch.nn.ModuleDict提供了类似的构建Module的方法，不过使用的参数是Dictionary

#### Convolution Layer
```torch.nn.Conv1d```
表示一个卷积层，1D的convolution。即输入是一个1D的vertor。in_channels对应的每个kernal有几个层次，out_channels表示有几个kernal，kernel_size是每个kernal的大小，stride表示kernal移动的step size。对应的shape用下面的方法计算。一般情况下卷积操作都不会修改input的shape。
<img src = 'https://github.com/mingming741/RenneNotes/blob/master/Resource/Image/Torch_NN_Conv1d.png'/>
Lout表示这一层输出之后的vertor length，Lin是输入的vertor length
```torch.nn.Conv2d```
通常用作对图像的卷积层，输入是一个二维矩阵，shape计算方法相似，W和H表示图片的宽度和高度。
<img src = 'https://github.com/mingming741/RenneNotes/blob/master/Resource/Image/Torch_NN_Conv2d.png'/>
Torch最多支持Conv3d，没有多的了。同样也支持transposed convolutional(Deconvolution)的操作，这操作可以看做convolution的逆操作，用``ConvTranspose1d``等层实现。

### Unfold 和Fold
```torch.nn.Fold & torch.nn.Unfold```
unfold是从一个batch取出一个小的block，比如用于取出一个sample。fold是将很多sample合成一个打的tensor，用来将sample合并成batch。

### Pooling Layer
```torch.nn.MaxPool1d```
表示一个最大池化层，通常stride取kernal的大小，这样就可以缩小图片大小。Pooling因为有stride所有会改变input的size，计算方法和Conv1d一样。同理``MaxPool2d``用于做二维矩阵的pooling，output计算方法和Conv2d一样。并且Max Pooling会保留indices(即value来自原来的哪个block)。MaxUnpool1d可以用来做反操作，将不是最大值的位置全部设置成0。Pooling中还有AvgPool1d，LPPool1d，AdaptiveMaxPool1d，AdaptiveAvgPool1d类似。



