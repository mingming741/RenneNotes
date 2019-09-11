# Kernal
Linux Kernal(内核)，即操作系统本身，平时我们说的kernal base即操作系统内部的运行环境，而各种application通常是调用kernal中的一些method来达到目的。


## OS


## Linux Kernal Module
在linux中，Kernel module等价于一段code，不绑定在kernal本身，而是根据需要，动态导入kernal中。好处是使得系统至于太重，并且module之间有替换性，使得kernal适应不同的工作环境需求。linux在开机时会根据configure，load系统中部分module，所有module的信息都在/proc/modules文件中列出。对module的操作主要使用insmod和rmmod。
```console
lsmod
# 查看linux中已有的module

insmod modulename
# 插入一个module

rmmod modulename
# 移除一个module
```
也可用使用更系统的modprobe，包含了上面几个cmd的内容。modprobe比起insmod，可以做到载入module的dependence，但是需要module的ko文件保存在/lib/modules/这个路径下面的一个文件夹内。相反的是，insmod可以insert任何路径中的一个module。


## Shell & Terminal
Shell是正在运行的程序，而Terminal是Shell运行的载体，Linux默认的Shell一般是`/bin/bash`。bash在开启terminal的时候，会follow一些参数，根据这些启动option读取不同的configure，生成不同的Shell Variable来控制shell的运行。

#### bash
bash即Bourne-Again Shell，最早的Bourne shell，即command-line interpreter，被研发出来用于解释user input的各种command，而bash是Bourne的一个改版，目前广泛使用。

被bash调用的脚本，也就是我们平时执行的command line，被叫做Shell script。Shell script不限定文件类型，可以是c也可用是sh等等。Shell本身也是scirpt。

#### Login Shell
即有user authentication的Shell，例如我们用putty远程登录到别的host上去，这里的terminal在我们的Host上，但是SSH的Shell则是在被我们SSH的那个Host上，这个Shell和Terminal的组合，叫一个session。这时我们用bash开启个新的shell，这个shell就是non-login shell。是否是login决定了这个shell在开启的时候，reading了哪些configure file

#### interactive shell
interactive shell就是attached了terminal的Shell，反之就是non-interactive shell。用bash运行的其他程序基本都是non-interactive, non-login shell。


### Shell Variables and Environment Variables
和windows类似，Linux也有环境变量。平时说的环境变量，是Shell Variables and Environment Variables的统称，两者有少许不同。环境变量(Environment Variables)由`export`定义，在整个Shell和其subprocess中起作用，并且在定义后会生成对应的Shell Variable。对应的，Shell variables由set定义，在当前Shell中全局存在。

记录Linux os中常见的Environment Variables，用`printenv`查看，括号中为其在我system中的值，空括号表示没有，数组类型是用`:`间隔开的。
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
* LD_LIBRARY_PATH (... /usr/local/lib ...):shared library object的path，某些程序在搜索lib的时候会找这里。

记录Linux os中常见的Shell Variables是变量，用`set`或者`echo`查看，这些变量都是当前terminal工作正在运行的变量。
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
* PS1 :基本提示符，对于root用户是#，对于普通用户是$

这些变量也可用被runtime的时候临时修改，设置Shell变量
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
取消环境变量，环境变量取消之后，被加进shell的变量不会取消。
```
export -n VARNAME
```
修改configure file可以做到每次开机都永久性修改这些变量。(Ubuntu12.04为例)Login shell开启时会读取`/etc/profile`，找对应的user的Home directory，然后读取Home的`~/.bash_profile`, `~/.bash_login`, 和`~/.profile`。Non login shell会读取`/etc/bash.bashrc`和`~/.bashrc`。Non-interactive shells会去环境变量`BASH_ENV`中寻找configure file的路径。

不过当前很多Linux的系统，都用login configuration files去source了non-login configuration files。这就意味一通操作后，configure file都变成了`~/.bashrc`这个file。我们通过给`~/.bashrc`添加variable，然后export这些变量，例如：
```
TCL_LIB="/home/showing/ns-allinone-2.35/tcl8.5.10/library"
USR_LIB="/usr/lib"
export TCL_LIBRARY="$TCL_LIB:$USR_LIB:$TCL_LIBRARY"
```
然后应用这些环境变量的configure：
```
source ~/.bashrc
```
上面的修改是对于一个user的，整个系统的修改需要修改`/etc/profile` (系统全局shell), `/etc/bash.bashrc`(interactive bash), `/etc/profile.d/`, or `/etc/environment`。这些地方可以添加新的环境变量进去。

同样的，kernal中的环境变量不一定被每一个binary executable继承。例如`LD_LIBRARY_PATH`本质是系统predefined environmental variable，用于给linker找到dynamic libraries/shared libraries，寻找的是可执行文件，为linker path。而gcc的编译需要的是c文件，所以有着自己一套不同的path，为compiler dependency。gcc的compile输出gcc本身的范畴，但是linker属于kernal的范畴，gcc的工作只是编译，而不需要care到动态链接，这就是为什么环境变量这样分的。





