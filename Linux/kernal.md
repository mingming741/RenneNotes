# Kernal
Linux Kernal即操作系统本身，平时我们说的kernal base即操作系统内部的运行环境，而各种application通常是调用kernal中的一些method来达到目的的，kernal的中文翻译叫“内核”

### Linux Kernal Module
在linux中，"Kernel modules are pieces of code that can be loaded and unloaded into the kernel upon demand"。意思就是，module等价于一段code，这段code不绑定在linux系统本身，而是可以根据需要，动态的被导入到linux系统中。这样的好处是，使得系统本身至于太重。并且各种module之间有可以替换性，使得os本身可以适应不同的工作环境需求。

例如，Golf的stateful和Cubic+就是作为linux的一个module被插入系统，module本身独立于系统，但是可以成为系统操作的一部分。linux在开机的时候会自动load系统中的部分module，取决于linux的implementation。对module的操作主要使用insmod和rmmod，也可用使用modprobe。
```console
# 查看linux中已有的module
lsmod
# 插入一个module
insmod modulename
# 移除一个module
rmmod modulename
```
系统在启动之后，所有module的信息都在/proc/modules文件中列出。而modprobe更加系统，包含了上面几个cmd的内容。modprobe比起insmod，可以做到载入module的dependence，但是需要module的ko文件保存在/lib/modules/这个路径下面的一个文件夹内。相反的是，insmod可以insert任何路径中的一个module。


### Shell & Terminal
Shell值得是运行的程序，而Terminal是Shell运行的载体，Linux默认运行的Shell一般是`/bin/bash`



### Shell Variables and Environment Variables (环境变量)
和windows类似，Linux同样有一些内置的环境变量。平时说的环境变量，是环境变量和shell变量的统称。环境变量(Environment Variables)定义于当前shell和child shells或者processes，通常用于向子进程pass information processes，由export定义，使用`printenv`或者`env`查看`csh/tcsh`中的环境变量。而Shell variables仅在当前shell中定义，由set定义，使用`set`查看`sh/ksh/bash`中的shell变量。通常用于记录短暂的data。例如current working directory。环境变量等级高于Shell变量。

记录Linux os中常见的环境变量，用`printenv`查看，括号中为其在我system中的值，空括号表示没有，可以看出数组类型是用`:`间隔开的。
```
printenv PWD
```
* SHELL (/bin/bash): Shell本身的binary executable。通常默认是bash,也是default的linux shell。我们平时的terminal都是通过开这个文件产生的。
* TERM (xterm-256color): Terminal，表示emulate shell的这个terminal类型。
* USER (showing): The current logged in user.
* PWD (/home/showing): Print Working Directory，当前路径.
* OLDPWD (/home/showing/Desktop): 上一个路径，等于windows的返回键，可以通过 `cd -`回到这个路径。我的电脑的记录说明我从`/home/showing/Desktop`回到了`/home/showing`
* LS_COLORS (... *.jpeg=01;35:*.gif=01;35: ...): 控制ls这个cmd print出来对应类型的file的颜色
* MAIL ( ): The path to the current user’s mailbox.
* PATH (... /usr/local/bin:/usr/sbin: ...): 在shell中打executable的时候，system回去`$PATH`中寻找这个binary executable文件。
* LANG (en_HK.UTF-8): Language，当前使用的语言设置和字符编码.
* HOME (/home/showing): The current user’s home directory.

同样的是Shell变量，用`set`查看，或者使用`echo`查看，因为这些变量都是当前terminal工作正在运行的变量，因此可以直接echo出来。
```
echo $HOSTNAME
```
* BASHOPTS (... checkwinsize:cmdhist: ...): Bash Option，bash在executed的时候附带的参数.
* BASH_VERSION ('4.3.48(1)-release'): The version of bash being executed
* BASH_VERSINFO (\[0\]="4" \[1\]="3" \[2\]="48" \[3\]="1" \[4\]="release" [5]="x86_64-pc-linux-gnu"): The version of bash, 机器语言格式。
* COLUMNS (80): The number of columns wide that are being used to draw output on the screen.
* HISTFILE (/home/showing/.bash_history): 存储history的文件
* HOSTTYPE (x86_64): 主机类型
* OSTYPE (linux-gnu): os的类型
* HOSTNAME (showing-OptiPlex-9010): The hostname of the computer
* IFS ($' \t\n'): Internal Field Separator，用于切断command line的标示符号，告诉shell如何分割command。
* SHELLOPTS (... emacs:hashall:histexpand:history: ...): Shell options，用于开启这个shell
* UID (1000): The UID of the current user.

设置Shell变量
```
VARNAME="my value"
echo $VARNAME
```
取消Shell变量：
```
unset VARNAME
echo $VARNAME
```
设置环境变量，注意，环境变量等级高于shell变量，因此环境变量在设置之后，也会加到shell变量中去
```
export VARNAME="my value"      # shorter, less portable version
printenv VARNAME
echo $VARNAME
```
取消环境变量，环境变量取消之后，被加进shell的变量不会取消，需要再取消一次。
```
export -n VARNAME
printenv VARNAME
```
上面的修改都是临时性的，如果要永久性修改这个环境变量，就需要修改configure file。如果修改当前user的环境变量，就去修改`$HOME`中的`.bashrc`（或者是.bash_profile）。如果要修改整个系统的环境变量，去到`/etc/environment`，然后这样添加或者修改变量，不要用export
```
VARNAME="my value"
```








