# Kernal
Linux Kernal(内核)，即操作系统本身，平时我们说的kernal base即操作系统内部的运行环境，而各种application通常是调用kernal中的一些method来达到目的。

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


