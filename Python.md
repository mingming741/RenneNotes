# 常识：

#### Indentation: 
即tab, python没有tab会报错

#### Comment: 
    #comment
即#开始表示comment，适用于一行

#### Docstring(document string): 
    """  
        docstring 
    """
可以理解为多行comment，通常用于给函数添加说明

#### Data type: 
python的数据类型会在initial的时候根据被赋值的值去初始化，并且数据类型在计算中也可以改变, 常见的数据类型有
* int:
```python
    a = 1 
```
* float: 
```python
    a = 1.3 
    a=35e-3
```
* complex:
```python
    a = 1j
```
* str:
```python
    a = "hello"
```
* tuple: 
```python
    a = ()
```
* list:
```python
    a = []
```
* dict:
```python
    a = {}
```
* object:
```python
    a = Telephone()
```
* type (type本身也是一个type):
```python
    a = int
```


# 通用操作
#### String
* 字符串相加可以直接使用加号，同时不同数据类型相加会做相应的转换
```python
a = "Hello" + "" + "Renne"
```

# 通用函数：

