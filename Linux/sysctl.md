# procfs
Process configue file system. Unix like system的特殊文件系统，用于保存process的信息。会在启动的时候动态生成，并且被mount到/proc的路径下。这个路径虽然会在硬盘中显示，但是其实并不占硬盘而是占内存。idea是“process as file”。

如果打开linux的/proc这个路径，会看到当前允许的process的PID的文件夹。并且有着下面的attribute(表现形式就是一个file)
* cmdline, 启动该进程的命令行.
* cwd, 当前工作目录的符号链接.
* environ 影响进程的环境变量的名字和值.
* exe, 最初的可执行文件的符号链接, 如果它还存在的话。
* fd, 一个目录，包含每个打开的文件描述符的符号链接.
* fdinfo, 一个目录，包含每个打开的文件描述符的位置和标记
* maps, 一个文本文件包含内存映射文件与块的信息。
* mem, 一个二进制图像(image)表示进程的虚拟内存, 只能通过ptrace化进程访问.
* root, 该进程所能看到的根路径的符号链接。如果没有chroot监狱，那么进程的根路径是/.
* status包含了进程的基本信息，包括运行状态、内存使用。
* task, 一个目录包含了硬链接到该进程启动的任何任务

实际上，这些attribute都是对这个process的描述。

# systemd
system daemon，系统后台进程（或者叫守护进程。）貌似目前systemd有取代sysctl的趋势，但是两者似乎都有在使用。

# sysctl
System control(sysctl)是在runtime调整kernal的工具，修改核心参数。可以理解为，kernal在系统开机的时候就已经启动，而sysctl提供了开机之后modify kernal的各种函数，用于改变kernal运行的参数和调用的packet。sysctl在开机的时候会load一个默认的conf file，（大概是/etc/sysctl.d/99-sysctl.conf这个file，有些系统不一样），作为默认的system的config。当然作为control的工具，sysctl也可以动态修改，通常修改包括下面几块：
* dev
* fs
* kernal
* net
* vm (vurtial memory)
输入下面命令查看当前系统设置的默认值
```console
sysctl -a
```

### sysctl fs (file system)
fs attribute下面储存了对当前系统的file system的各种config，例如（我从我电脑中截取的）：
```console
fs.file-max = 384595
fs.lease-break-time = 45
fs.overflowuid = 65534
fs.pipe-max-size = 1048576
```
对于kernal来说这些似乎都是parameter，可以理解为c语言写的linux kernal在某些条件下会检查这些config，如果出现溢出等问题会发生相应的报告

### sysctl kernal
kernal attribute下存储的是kernal的一些参数，例如不同cpu的信息，随机函数的seed，最多几个thread这样的信息。
```console
kernel.keys.maxbytes = 20000
kernel.ngroups_max = 65536
kernel.overflowuid = 65534
kernel.random.poolsize = 4096
kernel.sched_domain.cpu0.domain1.idle_idx = 0
kernel.sched_domain.cpu7.domain0.flags = 4783
kernel.threads-max = 30175
kernel.watchdog_thresh = 10
```

### sysctl net
net attribute存储的是对网络的设置：
```console


```

