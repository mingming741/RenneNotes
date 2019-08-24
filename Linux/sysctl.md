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

# sysctl
sysctl是在runtime调整kernal的工具。可以理解为，kernal在系统开机的时候就
