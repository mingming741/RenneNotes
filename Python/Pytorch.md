# Pytorth
pytorch相当于用GPU来处理numpy的ndarray，在torch中即tensor，在torch中，tensor是一个类
```
x = torch.randn_like(x, dtype=torch.float)    # override dtype!
print(x)   
#tensor([[ 0.3928, -0.4377, -0.6426],
#        [ 0.6000,  0.1942, -0.9790],
#        [-3.0629,  0.2410, -1.5378],
#        [ 0.0219,  0.5899,  0.8386],
#        [-0.1540,  0.2724,  0.3881]])
```
