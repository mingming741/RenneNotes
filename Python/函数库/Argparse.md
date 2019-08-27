# Argparse
python常用对应command line的工具，用于处理输入的argument的格式，等于是python处理argument的便携库

### Argparse python 2.7
因为是做pantheon用到的Argparse，因此这里介绍python2.7版本的。创建Argparse.py，并且在里面写：
```python
import argparse

parser = argparse.ArgumentParser() 
# 创建ArgumentParser的类

parser.parse_args() 
# 将ArgumentParser的类apply到command line上去，这句话不打的话，python不会对argument产生反应
```
这样操作之后，Argparse.py就会开始监听cmd输入的argument，并且默认的`-h`会被载入，显示帮助信息。

如果要给这个py脚本添加别的argument，需要给`parser`变量添加属性，使用函数add_argument
```python
import argparse
parser = argparse.ArgumentParser()
parser.add_argument("echo", help="echo the string you use here")
args = parser.parse_args()
print args.echo
```
运行python Argparse.py 2333，会得到输出'2333'，add_argument会给这个parser添加一个echo的属性，之后可被parser.parse_args()读取出来，要知道这个属性并不是参数类型的，而是暴力读取第0个参数，即'2333'，作为args.echo的值。这样的argument被认为是必须要输入的argument，被叫做'Positional argument'。这个py脚本类似一个复读机。

我们可以用同样的方式，创建一个脚本函数：
```python
import argparse
parser = argparse.ArgumentParser()
parser.add_argument("square", help="display a square of a given number", type=int) # 避免类型错误
args = parser.parse_args()
print args.square**2
```
同样，除去必要的'Positional argument'，我们也可用添加可选的'Optional arguments'，同样使用add_argument。optional的argument需要以单横杠`-`开头，通常人们对于简写会用`-`，对于全名会用双横杠`--`开头来表示。
```
import argparse
parser = argparse.ArgumentParser()
parser.add_argument("-v", "-V", "--verbose", help="increase output verbosity", action="store_true")
args = parser.parse_args()
if args.verbose:
   print "verbosity turned on"
```
这样argument "--verbosity"就会成为一个可选的argument，并且store_true这个action，可以做到在打了`--verbose`的时候，让`args.verbose`的值变成True，没打的时候就是False，类似一个flag的作用。这里不能使用`type=bool`，因为bool需要一个输入值，并且这个输入值无法被读取，自己可以试试看。

同时`add_argument`可以给一个参数多个引用名称，并且这些引用名称的值等价(不知道本质的object是否等价，有可能是多个指针指向同一个object)，比如上面的例子的`-v, -V, --verbose`。




