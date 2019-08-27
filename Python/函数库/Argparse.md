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
```python
import argparse
parser = argparse.ArgumentParser()
parser.add_argument("-v", "-V", "--verbose", help="increase output verbosity", action="store_true")
args = parser.parse_args()
if args.verbose:
   print "verbosity turned on"
```
这样argument "--verbosity"就会成为一个可选的argument，并且store_true这个action，可以做到在打了`--verbose`的时候，让`args.verbose`的值变成True，没打的时候就是False，类似一个flag的作用。这里不能使用`type=bool`，因为bool需要一个输入值，并且这个输入值无法被读取，自己可以试试看。同时`add_argument`可以给一个参数多个引用名称，并且这些引用名称的值等价(不知道本质的object是否等价，有可能是多个指针指向同一个object)，比如上面的例子的`-v, -V, --verbose`。

'Positional argument'和'Optional arguments'也可用混合使用。下面的例子说明了如何实现不同的log level。使用`choice=[0, 1, 2]`限定log level的值。并且，下面这个script使用`python Argparse.py -v 2 3`的调用方式也是可以正常调用的。这说明，'Optional arguments'的位置可以是任意的，但是不推荐任意使用。
```python
import argparse
parser = argparse.ArgumentParser()
parser.add_argument("square", type=int, help="display a square of a given number")
parser.add_argument("-v", "--verbosity", type=int, choices=[0, 1, 2], help="increase output verbosity")
args = parser.parse_args()
answer = args.square**2
if args.verbosity == 2:
    print "the square of {} equals {}".format(args.square, answer)
elif args.verbosity == 1:
    print "{}^2 == {}".format(args.square, answer)
else:
    print answer
```
对于互相conflict的两个argument，我们可以将其添加到同一个group中，这样如果两个argument都不是NULL的话，就会报错
```python
import argparse

parser = argparse.ArgumentParser()
group = parser.add_mutually_exclusive_group() # 互斥的组
group.add_argument("-v", "--verbose", action="store_true")
group.add_argument("-q", "--quiet", action="store_true")
parser.add_argument("x", type=int, help="the base")
parser.add_argument("y", type=int, help="the exponent")
args = parser.parse_args()
answer = args.x**args.y

if args.quiet:
    print answer
elif args.verbose:
    print "{} to the power {} equals {}".format(args.x, args.y, answer)
else:
    print "{}^{} == {}".format(args.x, args.y, answer)
```
上面的example中，`python Argparse.py 4 2 -v -q`会报错。同事`--help`中会变成这样的显示：
```
   usage: Argparse.py [-h] [-v | -q] x y
```
这里`[-h]`表示`-h`是一个optional的argument，`[-v | -q]`表示这两个optional的argument最多只能选取一个，x和y表示`x`和`y`是positional的argument


### 处理sub paser
sub paser的作用是给一个脚本处理多个argument的option的能力，类似git commit和git push中，git这个脚本可以对commit和push有不同的操作。以Pantheon的test为例，这是test.py
```python
import argparse
parser = argparse.ArgumentParser(description='perform congestion control tests')

subparsers = parser.add_subparsers(dest='mode') # 添加一个subparsers的container
local = subparsers.add_parser('local', help='test schemes locally in mahimahi emulated networks') # 给这个contrainer添加一个parser，即main parser的subparser。
remote = subparsers.add_parser('remote', help='test schemes between local and remote in real-life networks') # 添加另一个subparser
args = parser.parse_args()
```
运行`python test.py -h`会得到这样的说明：
```
usage: Argparse.py [-h] {local,remote} ...
```
这样，我们的调用就变成了`python test.py local arg1 arg2...`和`python test.py remote arg1 arg2...`。而`python test.py -h`也会被保留下来，同样我们给main paser添加其他的function依旧起作用。因此我们可以讲subparsers看做main parser添加二级命令的一个container，用于延展一级命令的长度。在二级命令create之后，`local`和`remote`的模式，添加argument和一级相同。



