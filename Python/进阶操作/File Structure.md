# File Structure
这个文档研究python的程序，在形成多文件的project的时候，如何寻找不同路径的文件，避免file not find的一些tips

# sys.path
因为python是脚本语言，所以在执行的时候会直接去系统路径中寻找其import的file，这个动作类似c++的link，而寻找的路径在`sys.path`中。`sys.path`返回一个数组，保存了所有指定的python编译器（不同的python.exe这个路径可能并不一样）的path。

`sys.path`在py文件被调用的时候被构建，其value是环境变量`PYTHONPATH`的值，加上当前被调用的py文件的路径，因此当py文件位置固定的时候，想要看到上层路径的话需要进行一些额外的操作。

# import
无论是自己写的库还是系统预装的库(如numpy)，在被import的时候都会生成对应的pyc文件。pyc文件是binary code，在某个module被import的时候，python解释器会将其编译为pyc文件，然后在被调用。例如：
```python
print(numpy.__file__)
# /usr/local/lib/python3.5/dist-packages/numpy/__init__.py
```
如果要讲一个文件夹里面的所有文件都成为library，则需要在这个文件夹下面创建`__init__.py`，`__init__.py`在编译之后也会将这个路径下的其他文件的pyc编译出来，然后import他们的全部。
