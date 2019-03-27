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
```python
# int:
a = 1 
# float: 
a = 1.3 
a= 35e-3
# complex:
a = 3+5j
# str: python中没有char这个概念，string的index位置依旧是一个string
a = "hello"
type(a[1]) #return "<class 'str'>"
# tuple: 
a = ()
# list:
a = []
# dict:
a = {}
thisdict =	dict(brand="Ford", model="Mustang", year=1964)
# set: #set中value不重复
a = {"a", "b"}
# function
a = lambda para : para * 2
# object:
a = Telephone()
# type (type本身也是一个type):
a = int
```
python中的iterable可以存储不同的数据类型，并且其constructor可以互相转化

#### lambda
python中的lambda可以认为是函数简单定义的方式，fun_name = lambda parameter_1, parameter_2 : return_value，这种定义的好处是可以使得function变成inline，在某些只需要定义一次的function中非常的好用。lambda定义的变量名类型是一个函数，（似乎和装饰器有关）
```python
a = lambda para : para * 2 #type(a) is function
def myfunc(n):
  return lambda a : a * n
mydoubler = myfunc(2) #mydoubler 是一个接受一个参数的函数，会返回这个参数的翻倍值
```


# 通用操作
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

#### 操作符
```python
x ** y #Exponentiation
x // y #Floor division, 在小数的时候，取小的那个interger，10 // 3.0 = 3.0
not(x < 5 and x < 10) # not 操作仅在not equal的时候用！=，其他情况都用not
x is y #Identity，在x和y是同一个object的时候return true
x in y #Membership， 在y这个container(list, dict....)中存在x这个member的时候return true
#python中也有bit wise和shift operation, &, |, << 等

#python中的Identity，可以看出python的数据存储和C类似
a = 1
b = 1
print(a is b) #True
a = utility.Neko()
b = utility.Neko()
print(a is b) #False
```
# 通用函数：
### String
字符串相加可以直接使用加号，同时不同数据类型相加会做相应的转换
```python
a = "Hello" + " " + "Renne"
```
字符串也可以使用函数
* 去除空格
    ``` a.strip() ```
* 字符替换
    ``` a.replace("A", "B") ```
    
### List
python的list变量名存储的是list的指针
```python
thislist = ["apple", "banana", "cherry"]
thislist.insert(1, "orange") #Insert到指定位置
thislist.remove("banana")   #Remove对应值的element
del thislist[0] #Remove指定位置的element
thislist.clear() #清除所有element
thislist = list(("apple", "banana", "cherry")) #用tuple构建list
otherlist = thislist.copy() #内存复制一个list，直接等号赋值会导致两个list变量其实指向同一个list
thislist.extend(otherlist) #通过其他的iterable变量延伸list，otherlist也可以是一个tuple或者其他
thislist.count("apple") #数一下list中有几个叫apple的变量
thislist.index("apple") #返回第一个叫apple变量的index位置
thislist.reverse() #返回反向list的一个新的list
    
```
### Dict
dictionary可用来表示各种object
```python
fromkeys() #返回指定的keys的sub dictionary
items() #返回key&value的tuple
values() #返回所有的value
```

# 函数库
### numpy
numpy的array其实就是对python自带的array做了封装，支持一些矩阵运算操作，并且这些矩阵运算操作涉及到了一些优化，numpy的array可以和python自己的array互相转化



