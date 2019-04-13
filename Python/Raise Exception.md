### 异常处理
python的raise和c的throw类似，Python的异常也是一个对象，由异常的事件(event)产生，可以理解为python在遇到无法处理的情况，我们会人为产生异常的。python自己也会产生很多异常(比如调用了没有定义的变量)，通过对异常对象的捕获，我们可以进行一些异常中的handle。异常Exception分为产生和捕获，一般是“try...except...finally...else”机制。如何产生异常呢，最通用的写法就是
```python
raise Exception("抛出一个异常")
# 异常会打断python脚本的执行，返货Exception object，当Exception被raise时，如果没有try except，后面所有的code都会被block
```
异常通常是因为我们自己的程序无法处理某些情况，举个栗子，比如你要open一个file，当你给的file路径不存在的时候，python自己也不知道怎么处理这个问题，因此我们可以在这种情况raise异常，并且通过捕获这个异常，选择其他的方法，比如给一个默认的file去代替你找不到的那个file，通常用try去handle异常。
```python
try:
    raise Exception("抛出一个异常")
except Exception as error: # 可以将这个Exception的object pass过来
    print(2333) 
else:
    print(2334)
# 执行结果是2333被print出来，并且程序可以执行下去不会被block，因为这个异常在程序内部被handle了。

# Python Exception的基类是Exception，Exception可以被任何except的类型捕获，如果捕获Exception，即可捕获任何抛出的异常，比如
try:
    raise Exception("抛出一个异常")
except (IOError,IndexError):: # 无法catch，因为Exception是基类，但是我们可以用tuple()来表示catch多个Exception
    print(2333)

try:
    raise IOError("抛出一个异常")
except Exception: # 可以catch，这句话也等同于 "except: "，不申明也等于捕获全部
    print(2333)
```
异常处理的步骤是这样的，try的block表示这中间可能会发生异常，等于监听tryblock的异常，在没有try或者try没有except指定类型的异常情况下，脚本都会停止运行。在except到指定类型异常的时候，程序可以继续运行，而final内的内容就是无论有没有发生异常，或者是没有catch到对应的异常，都要执行的code(即在try结束后一定会执行的block)<br/>
使用raise来自己提出异常，下面的函数表示在返回异常的时候，也附带一些log(level的值)，用于更好的debug
```python
def functionName( level ):
    if level < 1:
        raise Exception("Invalid level!", level)
```
同时用户也可以通过集成基本Exception的类型，定义自己的Exception
```python
class Networkerror(RuntimeError):
    def __init__(self, arg):
        self.args = arg
#这里只要重载__init__(self,)即可，这个函数用于raise Exception，也可以将其重载为多个input参数的形式
```
python常见的异常类型有这些，括号里表示的是Exception的基类，这里给出链接<a href="https://docs.python.org/3.4/library/exceptions.html">Built-in Exceptions</a>
```python
# Level 0
BaseException #除了Exception之外还包括SystemExit，KeyboardInterrupt，GeneratorExit，表示程序运行之外的Exception

# Level 1
Exception # 所有Exception的基类，user define的Exception都应该override这个，或者他的派生类

# Level 2
ArithmeticError # 数学运算上的错误，如OverflowError, ZeroDivisionError, FloatingPointError
AttributeError # 不存在属性(应该指的是class的属性)ref或者被赋值的时候产生
AssertionError # assert返回false的时候触发，如pytest中就是用这个
BufferError # 关系到buffer的error，如buffer overflow的情况
ImportError # import的包找不到的时候触发，fail to find module
LookupError # 表示找不到对应的key或者index给的那个target，如IndexError, KeyError
    IndexError(LookupError) # 在index out of range的时候触发，对应list
    KeyError(LookupError) # 在dictionary的key不存在的时候触发，对应dict
NameError # 在local或者global定义的变量名不存在，或者没有赋值就被调用的情况触发
OSError # 操作系统相关的error，包括IOError，FileExistsError，FileNotFoundError，PermissionError，TimeoutError
    IOError(OSError) # 输入或输出异常
    ConnectionError(OSError) # Socket中会出现
RuntimeError # 在error(exception)被raise并且不符合任何其他error的类型时候被触发
SyntaxError # 语法错误，在python中即使语法错误，前面的code依旧会执行
    IndentationError(SyntaxError) # 缩进错误，可能少打了tab之类的
TypeError # 操作类型不匹配，并且无法实现自动转化时触发
ValueError # 在类型正确但是数值不合适的时候被触发，如Unicode Error，在得到无法解码的时候触发
```
最后再介绍一下python的warning，Warning也是继承了Exception，但是Warning不会阻止脚本的运行，有些warning在默认的时候是会被忽略的，有些会log出来，我们自己在写代码的时候也可以适当的raise一些warning来提醒用户的使用，暂时介绍下面几个warning
```python
# Level 2
Warning # 所有warning的基类，Exception的派生类

# Level 3
UserWarning # 用户用warn函数产生，标记用户的提醒
DeprecationWarning # 表示使用了某些版本后可能不再使用的fun或者功能
SyntaxWarning # 表示使用了歧义的(dubious)语法
ImportWarning # import可能出现的问题
```


