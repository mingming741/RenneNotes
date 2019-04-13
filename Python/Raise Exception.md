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
except Exception:
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
python常见的异常类型有这些
```python
AttributeError # 不存在属性
IoError  输入或输出异常
```
