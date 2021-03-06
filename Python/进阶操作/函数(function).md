# 1. Function 
### 1.1 函数传参 pass by assignment
理解python的最基本的函数传参，在python中，只有一种传参方式叫pass by assignment。在python中，所有的变量都是object，整数变量也是，不同的整数变量等价于不同的object(比如2333和6666就是不同的object)，这种object是不可变(immutable)的。而加减乘除的操作，其实是将一个变量名引导去了不同的object。而对于list，dict这种object就是可以变的(mutable)，大部分的类型都是可变的。不可变的objet有：
* numbers
* booleans
* strings
* tuples
* frozensets
```python
#这是一个函数的例子
my_var = 25
def my_method(v):
    v += 10
    return v
    
#这个例子等价于下面的几个句子：
my_var = 25
v = my_var # 即在传参的那个步骤，用等号赋值，assignment的意思就是等号赋值
v += 10  # This is identical to v = v + 10
```
而pass by assignment的意思来源于一个object是否是mutable的，等号在immutable的object赋值的时候，是完全等价赋值，而在mutable的object赋值的时候，是引用赋值，区别来源于' = '符号的用法。同时这种赋值的方法是不可以改变的(暂时看是这样)。

### 1.2 多类型参数 \*args and \*\*kwargs
通常情况下，python和其他语言的函数，给定的参数值是固定的，即使是有默认参数，也是需要申明的，而python是可以pass不定量的parameter进入函数的，这就要使用arg和kwarg
    * \*args (Non Keyword Arguments)
    * \*\*kwargs (Keyword Arguments)
```
def adder(x,y,z=13):
    print("sum:",x+y+z)
adder(11,12) # 输出36
```
先给Non Keyword Arguments，在申明\*num的时候，num会把所有的没有指定值的值(比如3，True，这种叫没有指定值)加入num这个list(tuple?)，在函数之内就可以用iterator来遍历这些值
```python
def adder(*num):
    sum = 0
    for n in num:
        sum = sum + n
    print("Sum:",sum)
adder(3,5,7,9) # 24
```
而Keyword Arguments就是将有申明的指定值(比如number=3，isGood=False，这些值)生成一个dictionary
```python
def intro(**data):
    print("\nData type of argument:",type(data))
    for key, value in data.items():
        print("{} is {}".format(key,value))
intro(Firstname="Sita", Lastname="Sharma", Age=22, Phone=1234567890)
```
当这两个都被使用的时候，python的函数就可以接受任意数量的parameter了。<br/>
同时，函数本身传入的list，也可以通过引用传入转化为\*arg所接受的参数，这种写法等于在list(或者是其他iterable)被传入函数然后展开的时候，避免其展开而成为参数
```
# 构建神经网络的架构的时候，因为nn.Sequential()只支持多参数输入
modulelist = [nn.Linear(3, 2), nn.Tanh(), nn.Linear(2, 3)]
nn.Sequential(*modulelist)
```

### 1.3 装饰器Decorators
装饰器是"functions which modify the functionality of other functions"，python的函数也是object，而函数本身可以赋值给其他函数
```python
def hi(name="Renne"):
    return "hi " + name
print(hi()) # hi Renne
greet = hi # 区别与greet = hi()，这不是赋值返回值，而是赋值变量
print(greet()) # hi Renne
del hi
try:
    print(hi())
except NameError:
    print(2333) # 2333
print(greet())  # hi Renne， 这里还是会有输出，删除hi这个名称并不会删除这个函数的object
```
同时，python的函数是可以nest操作的，并且函数可以返回函数本身。 函数也可以作为其他函数的parameter，成为input。而装饰器可以理解为一个装饰器函数，装饰器函数会接受被装饰函数作为input，modify函数本身之后，再返回被装饰的函数，例如
```python
def a_new_decorator(a_func): # 函数作为input
    def wrapTheFunction():
        print("Deco1")
        a_func()
        print("Deco2")
    return wrapTheFunction # 返回wrapTheFunction，而这个wrapTheFunction其实又是装饰过a_func的一个函数

def a_function_requiring_decoration():
    print("Func")

a_function_requiring_decoration() # Func
a_function_requiring_decoration = a_new_decorator(a_function_requiring_decoration)
a_function_requiring_decoration() # Deco1 Func Deco2, 这个函数被装饰器改变了
```
但是这个操作会override原来的函数的``__name__`` attribute和docstring，所以通常使用外部的库functools.wraps来替我们做装饰，
```python
from functools import wraps
def a_new_decorator(a_func):
    @wraps(a_func) # 这句话用来取出override的部分
    def wrapTheFunction():
        print("Deco1")
        a_func()
        print("Deco2")
    return wrapTheFunction

@a_new_decorator # 这里申明decorator的名称，即上面的那个函数
def a_function_requiring_decoration():
    print("Func")

a_function_requiring_decoration = a_new_decorator(a_function_requiring_decoration)
print(a_function_requiring_decoration.__name__) # a_function_requiring_decoration, 否则会输出wrapTheFunction
```
最后提一下Decorators的use case，因为Decorators本质是在原函数基础上添加的操作，所以用作Authorization， Logging的时候，非常的有效。同时，decorator还可以带参数，这里给出log的一个例子，这个例子在原来的基础上又封装了一层，用于包含参数名称
```python
from functools import wraps

def logit(logfile='out.log'):
    def logging_decorator(func):
        @wraps(func)
        def wrapped_function(*args, **kwargs):
            log_string = func.__name__ + " was called"
            print(log_string)
            # Open the logfile and append
            with open(logfile, 'a') as opened_file:
                # Now we log to the specified logfile
                opened_file.write(log_string + '\n')
        return wrapped_function
    return logging_decorator

@logit()
def myfunc1():
    pass

myfunc1()
# Output: myfunc1 was called
# A file called out.log now exists, with the above string

@logit(logfile='func2.log')
def myfunc2():
    pass

myfunc2()
# Output: myfunc2 was called
# A file called func2.log now exists, with the above string
```
Decorator也可以有decorator的class，这个参考本文最上面给出的链接，里面有详细步骤，这里是我自己的链接<a href="https://github.com/mingming741/RenneCode/blob/master/Python/Learn%20Note%20Code/Decorator.ipynb">Renne Decorator</a>

### 1.4 Generator
首先介绍Iterable，Iterator，Iteration这三个概念，这是用于数据结构Iterator的编程通用概念，在python中有自己的定义<br/>
* Iterable：通常指可以迭代的object，在python中，iterable有``__iter__``或者是``__getitem__``这个定义的attribute，而这个method会返回一个Iterator(迭代器)，用于遍历这个object
* Iterator：迭代器，即上班iterable的object返回的遍历自己的迭代器，通常有``__next__``这个method，用来返回下一个值
* Iteration: 迭代操作，即我们在使用for循环遍历一个可迭代的object的时候的操作
* Generator: Generator也是Iterator，但是只能够迭代一次，因为generator是不会在内存中保存可迭代的东西的值的。最简单的generator函数由yield产生
```python
def generator_function():
    for i in range(10):
        yield i  # 每次调用next的时候，就会调用出下一个值，可以理解为自动打断和调用，在内存中，i的值是被保留的

a = generator_function() # generator function返回值是一个generator的object
print(type(a)) # <class 'generator'>
for i in range(0, 10): 
    print(next(a))     
for item in generator_function(): # 和上面的等价，每次调用generator_function返回的是不同的generator，但是一个generator只能iteration一次
    print(item)
```
使用generator会避免生成很大的数组，比如斐波那契数列：
```python
# generator version
def fibon(n):
    a = b = 1
    for i in range(n):
        yield a
        a, b = b, a + b
        
for x in fibon(10):
    print(x)
```
Generator在yield完成之后会throw ``StopIteration``的exception，而for循环会自己catch这个exception并且停止``next()``的操作。通常情况，许多本身不是iterator的object都可以通过iter()函数返回他的iterator的attribute，但是通常不能直接调用``__iter__``attribute，这个attribute被封装过，就如图``next()``这个method也封装过一样，只能接受iterator，并且不能直接调用``__next__``这个attribute
```python
my_string = "Renne"
print(next(iter(my_string))) # R
list0 = [1, 2]
it = list0.__iter__
print(next(it)) # TypeError: 'method-wrapper' object is not an iterator
```
我自己的代码链接<a href="https://github.com/mingming741/RenneCode/blob/master/Python/Learn%20Note%20Code/Generator.ipynb"> Generator</a>

