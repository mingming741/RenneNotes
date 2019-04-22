# 2. Class
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
