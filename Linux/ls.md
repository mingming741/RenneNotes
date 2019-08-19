# ls
list的command基本信息是list directory contents。例如，看隐藏文件使用`$ ls -ah`。

### ls Format
这里以`ls -al`为例，格式为以下：
```
权限 层数 拥有者 创建者 文件大小 最后修改时间 文件名
e.g: -rw-rw-r--  1 showing showing 1932239 Aug 14 17:25 pantheon_report.pdf
```
在这里，权限分为read write和exec，层数表示这个目录还有几个子目录，对于普通文件就是1，文件夹最小是2(文件夹里面子文件越深，这个数值越大)。据我观察，改变拥有者并不会改变执行权限。应该说，拥有者决定了这些file的mate data可以被谁修改，即普通的user不能对root所有的file进行chmod的操作，也不能把自己所有的文件改成root权限。如果出现权限问题，还是直接sudo改权限比较好。

### 文件类型
linux中的文件类型和window不同，不是后缀名决定的，而是分为下面几种：
  * \-: regular file
  * d: directory
  * c: character device file
  * b: block device file
  * s: local socket file
  * p: name pipe
  * l: symbolic link
用的比较多的就是\-代表的普通的file，d代表的路径，和l代表的快捷方式。

#### Regular file
linux大部分的file都是这个format，而是不是一个可执行文件取决于权限的exec是否enable，文件的图标很多也都取决于打开方式（很多linux的可执行文件打开方式都是AptUrl，就是那个菱形的方块，但是不代表不是菱形方块的文件就是不可执行的）。普通的file可以通过`touch`创建，删除这种文件使用简单的`rm`即可。

#### Directory
通过`mkdir`创建，删除使用`rmdir`，对于不空路径的删除使用`rm -r dirname`删除。代表一个文件夹。文件夹默认都是可执行的，而执行本身就是打开这个文件夹进入下一个目录。（这里可以理解为打文件夹的路径即执行了这个文件夹的内容，即将当前路径跳转到文件夹指定的路径中去）。

这里，我尝试讲一个Directory改成不可执行的，然后cd这个directory，发现cd没权限了，但是使用root还是可以进去。使用GUI进到这个文件夹，发现里面的图片不能preview，然后也打不开。这应该就是文件夹需要可执行的意义吧。

#### Character & block device
这两个都是允许用户和程序于hardware的peripheral device做交互的，类似于分配硬盘内存这种程序的存在，硬件驱动。

#### Local domain sockets & Named Pipes
用于processes之间的交互，Named Pipes主要是local process的交互

#### Symbolic Links
快捷方式。分为hard和soft link。





