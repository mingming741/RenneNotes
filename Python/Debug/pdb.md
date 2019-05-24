# pdb
pdb是python自带的debug工具，通过下面的cmd可以使得你在运行一个py文件的时候，进入debug模式
```
$ python -m pdb my_script.py
```
debug模式在cmd的kernal中运行，可以通过输入下面的字符来trace一些debug的操作：

### Commands:
* c: continue execution
* w: shows the context of the current line it is executing.
* a: print the argument list of the current function
* s: Execute the current line and stop at the first possible occasion.
* n: Continue execution until the next line in the current function is reached or it returns.
