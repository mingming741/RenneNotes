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
