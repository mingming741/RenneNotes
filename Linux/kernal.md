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

### Environment Variables (环境变量)
和windows类似，Linux同样有一些内置的环境变量，在一些程序运行的时候following这些config。应该说，环境变量本身是一些全局变量，不同的程序在安装的时候，都会默认设置一些系统的环境变量，用于辅助这个程序的运行。

严格意义上来说，平时说的环境变量，是环境变量和shell变量的统称。环境变量(Environment Variables)定义于当前shell和child shells或者processes，通常用于向子进程pass information processes。而Shell variables仅在当前shell中定义，通常用于记录短暂的data,例如current working directory.

使用`printenv`或者`env`查看`csh/tcsh`中的环境变量，也可用查看指定的，使用`set`查看`sh/ksh/bash`中的环境变量。
```
printenv PWD （等价于pwd）
set （查看全部）
```
我们可以通过下面的cmd来修改环境变量：
* set：新增或者是修改某一个环境变量
* unset：删除一个已有的环境变量
* export: 将这个环境变量export到shell和其subprocess中发挥作用。

下面是设置环境变量的方法：
为当前运行的shell设置环境变量（其实是shell variable）
```
VARNAME="my value"
```
为当前运行的shell和其开启的sub processes设置环境变量:（export设置的是Environment Variables）
```
export VARNAME="my value"      # shorter, less portable version
```
上面的修改都是临时性的，如果要永久性修改这个环境变量，就需要修改configure file。如果修改当前user的环境变量，就去修改`$HOME`中的`.bashrc`（或者是.bash_profile）。如果要修改整个系统的环境变量，去到`/etc/environment`，然后这样添加或者修改变量，不要用export
```
VARNAME="my value"
```
Linux中常见的环境变量有：
* DISPLAY
* 
* 
* 
* 
* 
* 
* 
* 
* 
* 
* 
* 
* 
* 
* 
* 
* 









