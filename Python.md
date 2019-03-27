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
    a= 35e-3
```
* complex:
```python
    a = 3+5j
```
* str: python中没有char这个概念，string的index位置依旧是一个string
```python
    a = "hello"
    type(a[1]) #return "<class 'str'>"
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
字符串相加可以直接使用加号，同时不同数据类型相加会做相应的转换
```python
a = "Hello" + "" + "Renne"
```

#### 数据转化
python可以使用 "\[type\](var)"实现数据转化，数据转化仅限于经典数据类型，默认不支持object（能否让object也可以转化？）并且只能在类型有办法转化的时候实现转化。
```python
    a = "123"
    int(a) # return 123
    b = Neko() #Neko is an object
    str(b) # <utility.Neko object at 0x7f406023a4a8>
    int(b) # run time error, no method
```

####数组操作
python使用array\[index_a:index_b\]表示数组中的一部分，对于string来说，这个操作为string的sub string,负数表示倒着数，
```python
    a = "Hello world"
    a[:4] # a的最开始到第四位
    a[1:-3] # a的第一位开始到倒数第三位
    a[-4:] # a的倒数第四位到最后
```

# 通用函数：

