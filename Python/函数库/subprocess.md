# subprocess
类似于linux的fork，python的subprocess函数可以创建新的进程，并且使用这个进程exec一些code，这些code可以是外部的程序，同时subprocess也有pipe的机制，帮助进程之间互相交流。

### 基本函数
`subprocess.call()`等于fork()，会创建一个新的process，父进程等待子进程完成，返回return code之后，才会继续执行。
```python
subprocess.call(args, *, stdin=None, stdout=None, stderr=None, shell=False)
```
下面的例子中，`subprocess.call(["ls", "-l"])`这句话会在shell中直接被当做ls -l来执行，在stdout中print出ls -l的结果。这里的`retcode`表示call这个函数执行的结果，如果是0的话说明执行没有问题。(这也是为什么c++的程序很多都要return 0，以表示程序正常结束)
```python
import subprocess
retcode = subprocess.call(["ls", "-l"])
print retcode
```
`subprocess.check_call()`检查退出信息，如果returncode不为0，则举出错误subprocess.CalledProcessError，该对象包含有returncode属性，可用try…except…来检查。`subprocess.check_output()`检查退出信息，如果returncode不为0，则举出错误subprocess.CalledProcessError，该对象包含有returncode属性和output属性，output属性为标准输出的输出结果，可用try…except…来检查。

上面的三个函数都是基于Popen封装的wrapper的，表现形式都有点相似。都有shell这个参数，默认为False，在Linux下，shell=False时, Popen调用`os.execvp()`执行args指定的程序；shell=True时，Popen直接调用系统的Shell来执行这个命令。

### Popen
Popen(process open)为一个封装的class，是subprocess开启进程的完整命令，基本结构是：
```python
class Popen(args, bufsize=0, executable=None, stdin=None, stdout=None, stderr=None, preexec_fn=None, close_fds=False, shell=False, cwd=None, env=None, universal_newlines=False, startupinfo=None, creationflags=0)
```
Popen可以说是对fork()的封装，需要wait让父进程等待子进程：
```python
import subprocess
child = subprocess.Popen('ping -c 4 192.168.80.77',shell=True)
child.wait()
print 'parent process'
```
子进程在创建后，父进程中可以通过child这个指针下面的几个attribute来进行调整子进程的状态
```
child.pid # 显示子进程的process id
child.poll() # 检查子进程状态，并且set returncode
child.kill() # 终止子进程
child.send_signal() # 向子进程发送信号
child.terminate() # 终止子进程
child.communicate() # returns a tuple (stdoutdata, stderrdata).
```
子进程默认的stdin,stdout,stderr都是没有的，即None。(但是我感觉就是占用父进程的这个shell的位置，或许是hi一种继承)，而PIPE可以用于承接子进程的输出，将多个子进程的输入和输出连接在一起，下面的例子，就是讲child1的输入写到了subprocess的PIPE中去。在读入之后，可以用communicate的方法获取对于的subprocess的PIPE的值，每个child是有不同PIPE的。
```python
import subprocess
child1 = subprocess.Popen(["ls","-l"], stdout=subprocess.PIPE)
stdout, stderr = child1.communicate()
print stdout
```
下面给出几个常用的使用方法，os也有popen的方法，不过用的是shell
```python
output = check_output(["cmd", "arg"]) # 等于sh的 output=`cmd arg`
status = subprocess.call("mycmd" + " myarg", shell=True) # 等于status = os.system("mycmd" + " myarg")
pipe = Popen("cmd", shell=True, bufsize=bufsize, stdin=PIPE).stdin # 等于 pipe = os.popen("cmd", 'w', bufsize)
pipe = Popen("cmd", shell=True, bufsize=bufsize, stdout=PIPE).stdout # 等于 pipe = os.popen("cmd", 'r', bufsize)

```





