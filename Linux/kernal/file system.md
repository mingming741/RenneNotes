# Linux File System
描述了从底层开始和FS相关的一些implementation

### Linux File System Hierarchy
参考http://www.pathname.com/fhs/pub/fhs-2.3.html
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


### File descriptor (Unix like系统才有)
File descriptor(文件描述符)，其值是一个非负的integer，指向内存中的某一个文件。当process打开一个file的时候，kernal会给这个process一个File descriptor，来指向这个文件。通常所有的process都有标准的三个文件描述符：
* Standard input (STDIN_FILENO/stdin)
* Standard output (STDOUT_FILENO/stdout)
* Standard error (STDERR_FILENO/stderr)

例如，system call `open`返回的就是一个file descriptor。同时，`socket()`，`pipe()`返回的也是一个fd
```c
char path[] = "file";
fd = open(path, O_CREAT | O_RDONLY, 0644);
``` 
linux的设计就是将所有东西视为文件。













