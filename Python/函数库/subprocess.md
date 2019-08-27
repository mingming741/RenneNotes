# subprocess
类似于linux的fork，python的subprocess函数可以创建新的进程，并且使用这个进程exec一些code，这些code可以是外部的程序，同时subprocess也有pipe的机制，帮助进程之间互相交流。

### 基本函数
```python
  subprocess.call(args, *, stdin=None, stdout=None, stderr=None, shell=False)
```
`subprocess.call()`等于fork()，会创建一个新的process，父进程等待子进程完成。下面的例子中，`subprocess.call(["ls", "-l"])`这句话会在shell中直接被当做ls -l来执行，在stdout中print出ls -l的结果。这里的`retcode`表示call这个函数执行的结果，如果是0的话说明执行没有问题。(这也是为什么c++的程序很多都要return 0，以表示程序正常结束)
```python
import subprocess
retcode = subprocess.call(["ls", "-l"])
print retcode
```
`subprocess.check_call()`检查退出信息，如果returncode不为0，则举出错误subprocess.CalledProcessError，该对象包含有returncode属性，可用try…except…来检查。

`subprocess.check_output()`检查退出信息，如果returncode不为0，则举出错误subprocess.CalledProcessError，该对象包含有returncode属性和output属性，output属性为标准输出的输出结果，可用try…except…来检查。

### shell=True
所有subprocess的函数都有shell这个参数，
