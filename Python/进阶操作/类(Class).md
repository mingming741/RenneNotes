# 2. Class
### 2.0 Introspection
函数dir()可以列举出一个Class生产的object的全部attribute和function，例如，查看python list的内部接口。在这里，不同的attribute可以兼容不同的function，比如2.1里面提及的，print调用的就是object中的__repr__的值。
```python
my_list = [1, 2, 3]
dir(my_list)
# Output: ['__add__', '__class__', '__contains__', '__delattr__', '__delitem__',
# '__delslice__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__',
# '__getitem__', '__getslice__', '__gt__', '__hash__', '__iadd__', '__imul__',
# '__init__', '__iter__', '__le__', '__len__', '__lt__', '__mul__', '__ne__',
# '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__reversed__', '__rmul__',
# '__setattr__', '__setitem__', '__setslice__', '__sizeof__', '__str__',
# '__subclasshook__', 'append', 'count', 'extend', 'index', 'insert', 'pop',
# 'remove', 'reverse', 'sort']
```
在python里面，所有的object都有独立unique的id，即便是const的value也有一个恒定的id，例如
```python
name = "Renne"
print(id(name))
# 140686421042208，在不同python环境中会有不同的值。
```
而inspect.getmembers这个method可以看到一个class或者object中全部的attribute和function。
```python
import inspect
print(inspect.getmembers(str))
```
user自己定义的class中，也会默认存在一些attribute，即使这些attribute没有定义，例如：
```python
class Demo():
    def __init__(self):
        self.name = 'Renne'
        self.pet = "Cat"

    def action(self):
        print("Make a choice")
        
a = Demo()
for attr in dir(a):
    print(attr)
    
'''result会print出来这些
__class__ ：(type)，即这个object的Class，在这里是__main__.Demo
__delattr__ ：(method-wrapper)，delete attribute，在attribute被试图delete的时候，被call
__dict__ ：(dict)，即用于调用attribute的一些信息，在这里是{'name': 'Renne', 'pet': 'Cat'}
__dir__ ：(builtin_function_or_method)，即dir函数的输出，a.__dir__()可以列举出全部attribute和function
__doc__ ：(str)， document string，写在注释里面，在正确的格式下会被赋值
__eq__ ：(method-wrapper)在==判定的时候，调用object.__eq__(self, other)这回method
__format__ ：(builtin_function_or_method)，被format函数调用的method
__ge__ ：(method-wrapper)
__getattribute__ ：()
__gt__ ：()
__hash__ ：()
__init__ ：()
__init_subclass__ ：()
__le__ ：()
__lt__ ：()
__module__ ：()
__ne__ ：()
__new__ ：()
__reduce__ ：()
__reduce_ex__ ：()
__repr__ ：()
__setattr__ ：()
__sizeof__ ：()
__str__ ：()
__subclasshook__ ：()
__weakref__ ：()
action ：()
name ：()
pet ：()
'''
# 这里面method-wrapper通常是一个operation或者是特殊字符串的重载（例如__eq__重载==）
# 而builtin_function_or_method通常是被某个function调用 （例如__format__被format()调用）
```


### 2.1 列举和print的区别
在一些编译器中，比如jupyter的解释器，直接列举某个变量可以达到print的效果
```
a = 10
a # 输出 10
```
但是这个列举和print其实是不一样的，print会print对应的对象的str转化类型，即__str__的输出，而列举调用的是类里面的__repr__函数对象
```python
class Renne():
    def __init__(self):
        self.word = "Hello Renne"
        
    def __str__(self):
         return self.word
         
    def __repr__(self):
        return "2333"

r = Renne()
print(r) # “Hello Renne”，实际为call back函数__str__的输出结果，即类Renne()对str的默认转化函数
r # 2333
```
这两个函数本身默认都是没有重载的，如果对于一个类，这两个函数都没有的话，那么就会输出内存的位置，如”<\_\_main\_\_.Renne at 0x7f76500b54e0>“

### 2.2 Alias & overload
python的等号赋值都是重名(alias)，实际传递参数是引用传递。
* 对于整数小数字符串等，等号赋值在内存上指向同一个constant的位置
* 对于类的，等号赋值等价于指针赋值，同样指向相同的类
同时，python本来的一些关键词也可以被alias，这种时候相当于你overload了这个name，比如
```python
def fun(a, b, range):
    for i in range(range):
        # do sth
# 这个函数运行的时候会产生error，因为range这个关键词在传进来的时候被重载，可能不再是一个函数的类型
```

### 2.3 Super() init
用于类的继承，即子类在初始化的时候，调用父类的constructor，super()这个函数在单一层次继承和直接Base.\_\_init\_\_意思一样，在大于两个类的继承的时候，super比直接写要更加明确，推荐使用
```python
class Base(object):
    def __init__(self):
        print "Base created"

class ChildA(Base):
    def __init__(self):
        Base.__init__(self)

class ChildB(Base):
    def __init__(self):
        #super(ChildB, self).__init__() in python 2
        super().__init__() #in python 3
        
ChildA() 
ChildB()
```

### 3.4 Class的存储
Class在python中，其attribute默认是用一个dict来存储的，这样可以做到在class生成对象之后，动态给对象添加新的attribute，例如
```python
class MyClass(object):
    def __init__(self, name, identifier):
        self.name = name
        self.identifier = identifier
        # self.set_up()
        
c = MyClass('Renne', 2)
c.Cat = 'GuoGuo'
print(c.Cat)
# 输出 GuoGuo
```
但是这种方式，在class很小，但是生成的object很多的时候，会占据许多的RAM，我们可以使用__slots__来锁定class的attribute，使得attribute的内容被绑定，例如
```python
class MyClass(object):
    __slots__ = ['name', 'identifier']
    def __init__(self, name, identifier):
        self.name = name
        self.identifier = identifier
        # self.set_up()
        
c = MyClass('Renne', 2)
c.Cat = 'GuoGuo'

# AttributeError: 'MyClass' object has no attribute 'Cat'
```
这种写法即告诉python，在创建对象的时候，不要使用dict，而是分配静态内存给这个object，这样会比较节省RAM。


