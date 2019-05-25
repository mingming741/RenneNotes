# 2. Class
### 2.1 Introspection
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

```
上面的result会print出来这些
```python
__class__ ：(type)，即这个object的Class，在这里是__main__.Demo
__delattr__ ：(method-wrapper)，delete attribute，在attribute被试图delete的时候，被call
__dict__ ：(dict)，即用于调用attribute的一些信息，在这里是{'name': 'Renne', 'pet': 'Cat'}
__dir__ ：(builtin_function_or_method)，即dir函数的输出，a.__dir__()可以列举出全部attribute和function
__doc__ ：(str)， document string，写在注释里面，在正确的格式下会被赋值
__eq__ ：(method-wrapper)在==判定的时候，调用object.__eq__(self, other)这回method
__format__ ：(builtin_function_or_method)，被format函数调用的method
__ge__ ：(method-wrapper) great or equal，>=判定的warpper
__getattribute__ ：(method-wrapper)，操作x.name的使用调用这个method，a.__getattribute__('name')等价于a.name
__gt__ ：(method-wrapper) great，> 判定的warpper
__hash__ ：(method-wrapper)，被hash()函数调用，会返回这个object在hash之后的值
__init__ ：(method)，在__new__之后被调用，用于给新的object initialize一些static的value，object(instance)本身是new创建的
__init_subclass__ ：(builtin_function_or_method)，在这个类被作为基类继承的时候，派生类在init的时候会call这个method
__le__ ：(method-wrapper) less or equal，<= 判定的warpper
__lt__ ：(method-wrapper) less，< 判定的warpper
__module__ ：(str)，表示定义这个函数/类的module，在上面的例子输出__main__
__ne__ ：(method-wrapper) not equal，!=判定的wrapper
__new__ ：(builtin_function_or_method)用于create class的instance，__new__会在object生成的时候被调用，生产dir的静态的method
__repr__ ：(method-wrapper)，“official” string representation of an object，在jupyter列举的时候被调用
__setattr__ ：(method-wrapper)，等价于x.name = '233'这个操作，a.__setattr__('name','Kalu')等价于a.name = 'Kalu'
__sizeof__ ：(builtin_function_or_method)，generator才有的method，返回其内存的大小(number of bytes)
__str__ ：(method-wrapper)，被print()和format调用，通常表示informal的representation
action ：() # user-define method
name ：() # user-defined-attribute
pet ：() # user-defined-attribute
# 这里面method-wrapper通常是一个operation或者是特殊字符串的重载（例如__eq__重载==）
# 而builtin_function_or_method通常是被某个function调用 （例如__format__被format()调用）
```
除了这些之外，class常用的Introspection还有（这些class常用的Introspection还有可能要自己定义）
```python
# General的attribute
__del__： 在删除的时候做一些操作，del x会间接的call __del__
__bool__： bool()，被if(x) call，在没有__bool__的时候会call __len__。
__slots__：即使用__slots__方式create object，3.4有说，会让 __dict__ 和 __weakref__ 不create
__call__：function需要有的method，即x()等价于x.__call__()。

# 对于container，下列
__class_getitem__：Container应该有的method，似乎是看这个item有没有存在于这个container
__len__：被函数len()调用，返回长度
__length_hint__：被函数operator.length_hint()调用，返回预估的length
__getitem__： list type应该有的method，x[key]等价于x.__class_getitem__(key) （__setitem__和__delitem__类似）
__missing__：用于check item是否存在，（在dict中）
__iter__： 返回一个iterator的object，并且iterate整个container
__reversed__： 做reverse iteration

# 对于数字的class，下列
__add__(self, other): 重载 +
__sub__(self, other): 重载 -
__mul__(self, other): 重载 *
__matmul__(self, other): 重载 @
__truediv__(self, other): 重载 /
__floordiv__(self, other): 重载 //
__mod__(self, other): 重载 %
__divmod__(self, other): 重载 divmod()
__pow__(self, other[, modulo]): 重载 **
__lshift__(self, other): 重载 <<
__rshift__(self, other): 重载 >>
__and__(self, other): 重载 &
__xor__(self, other): 重载 ^
__or__(self, other): 重载 |
__neg__(self): 重载 -
__pos__(self): 重载 +
__abs__(self): 重载 abs()
__invert__(self): 重载 ~
```
使用inspect的函数可以看到全部的attribute和其内容
```python
import inspect
print(inspect.getmembers(a))
```


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


