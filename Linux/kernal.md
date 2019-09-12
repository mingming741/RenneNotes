# Kernal
Linux Kernal(内核)，即操作系统本身，平时我们说的kernal base即操作系统内部的运行环境，而各种application通常是调用kernal中的一些method来达到目的。


## Linux File System Hierarchy
参考http://www.pathname.com/fhs/pub/fhs-2.3.html
### / directory folders
linux的根目录`/`下面有许多文件夹，每个文件夹都有自己的意义，下面列举一些：
* `/bin` - Binary，通常的Binaries executable文件，例如cat, ip, chmod, ls, mv, rm, pwd, tar等
* `/sbin` - System Binary，用于系统booting和维护等功能，例如bridge, ifconfig, insmod, route, reboot, sulogin等
* `/boot` -  Boot loader的静态文件，即Linux kernel本身和一些boot loader
* `/dev` - device，驱动，例如CD drive, hard disk, 或者其他physical device。包含了bus, cpu, disk, net等文件夹
* `/home` - home，包含所有user的home directory
* `/lib` - library，关键的share library和kernal module，`/lib64`应该是64位的。
* `/mnt` - 
* `/media` - media，在插入U盘的时候，U盘的路径会被mount到`/media/USER_NAME/USB_NAME`的路径下
* `/var` - variable data，有log，mail，cache，lib等文件夹。`/var`的文件通常是可以被不同的program read和write的，类似于一种保存cache的位置，例如
* `/tmp` - temporary，默认temporary file放置的位置，我电脑里有fcitx，sougou，atom的一些temp file
* `/usr` - user，如果说`/`是根目录的话，`/usr`就是对于user来说的二级根目录，包括了bin, sbin, lib, inclode, local, share等文件夹，这个不是对每个user都有个对应的路径，而是表示user level的文件。
* `/etc` - etcetera，(或者every thing config)，以前的意义是系统附加物，现在被用于保存system的configuration file。可以找到apt.conf.d，sysctl.d，debian_version，environment，hostname，protocal等文件夹和文件。
* `/opt` - Option，不在linux标准库的third party package被安装在这里，通常是通过deb这种形式安装的，有自己的sub directory
* `/proc` - process，这个路径不在disk中而是在ram中，保存了process运行中的变量，每个变量都用一个file来表示。
* `/srv` - service，系统service的data
* `/run` - 
* `/sys` - system


### Difference of /bin & /usr/bin等
对于bin：
* `/bin` - 通常是整个系统都需要的binary
* `/usr/bin` - 当前用户级别执行的binary，例如apt，file，gcc，git，ldd(List Dynamic Dependencies)，make，perl，python2.7，top。可以看出这个分级只是system level和user level的分级，并不是非常的明确，两个路径都在`$PATH`中。
* `/usr/share/bin` - 可以被web accesed的binary, 通常是Apache web applications

对于`/usr/local/bin`和`/usr/local/share/bin`，`local`意味着这个binary不是linux distribution中标准的软件，而是用户自己写的和安装的。通常sudo make产生的可执行文件都放在这里，并且被所有与user使用。


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


