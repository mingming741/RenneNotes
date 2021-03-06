## 系统库os
python的自带库os包含了许多系统操作，对应的是电脑这个操作系统的指令(而sys对应的是python这个编译器的指令)。可以理解为对于cmd的python封装，可以实现一些cmd实现的事情，路径的函数通常都是返回字符串或者字符串list
```python
import os

# 当前路径
os.getcwd() # 当前python文件的路径，返回一个字符串
# 列举路径ls
os.listdir(os.getcwd()) # 这个操作就是ls当前路径，返回字符串list
# 绝对路径
os.path.abspath('.') # 等同于当前路径，para可以写其他绝对路径的内容
# 路径是否存在
os.path.exists('D:\\pythontest\\ostest\\hello.py') # 返回Ture或者False

# 路径相加
os.path.join('D:\\pythontest', 'ostest') # 'D:\\pythontest\\ostest' 等于string相加

# 通常，python的当前路径是包含了这个py文件的文件夹，所以下面操作一般用不到
# 文件夹路径
os.path.dirname('D:\\pythontest\\ostest\\hello.py') # 'D:\\pythontest\\ostest' 包含这个路径文件的路径
# 文件名称
os.path.basename('D:\\pythontest\\ostest\\hello.py') # 'hello.py'
```

## python的路径问题
python在import包的时候，都是去sys.path这个路径文件下面去寻找路径，sys.path本质是一个str的list，存储的系统(sys)的全部路径。而库sys本身也是存储一些python系统相关的东西。可以通过给sys.path这个变量append一些内容，使得import的操作更加简单。
```python
import sys
print(sys.path)
#['/usr/lib/python36.zip', '/usr/lib/python3.6', '/usr/lib/python3.6/lib-dynload', '', '/home/SENSETIME/zhangyuming/.local/lib/python3.6/site-packages', '/usr/local/lib/python3.6/dist-packages', '/usr/local/lib/python3.6/dist-packages/parrots-0.1.0a2-py3.6-linux-x86_64.egg', '/usr/local/lib/python3.6/dist-packages/colorlog-4.0.2-py3.6.egg', '/usr/lib/python3/dist-packages', '/home/SENSETIME/zhangyuming/.local/lib/python3.6/site-packages/IPython/extensions', '/home/SENSETIME/zhangyuming/.ipython']
```
实际上，sys.path的内容是current working directory加上PYTHONPATH这个环境变量里面的内容，再加上installation-dependent的default paths组成的list。installation-dependent即对于每一个python的环境，你用pip install的那些packet的位置(不同python的环境这个位置也不一样)
```python
home_dir = os.path.expanduser("~") # 通过os构建一个path
sys.path.append(home_dir) #将这个path添加的python import路径中去
#这样操作之后，当前user的home下的路径都可以被python的解释器找到了
```

## \_\_file\_\_
\_\_file\_\_指定了你import的那个module所在的系统的位置。可以理解为每一个import Module都是系统路径中的一个py文件(或许也可以是可执行文件，cpython等计算机理解的文件，总之有对应的文件)，\_\_file\_\_就是这个文件的路径了。<br/>
需要注意的时候，仅py文件有__file__这个attribute，在jupyter里面的这个attribute是不可取的
```
import numpy
numpy.__file__ # '/usr/local/lib/python3.6/dist-packages/numpy/__init__.py'
# 即numpy位于我安装的python3.6的安装包之一中，通过init被public出来
```

## pathlib
python的pathlib提供了路径操作的一些函数，对系统的路径进行了封装，使其不仅仅是一个str，而变成了一个类。
```python
from pathlib import Path
p = Path('.')
type(p) # pathlib.PosixPath 一种pathlib的类
[x for x in p.iterdir() if x.is_dir()] #列举这path下的全部subpath

p = Path('/etc')
q = p / 'init.d' / 'reboot'
q # PosixPath('/etc/init.d/reboot')
```




