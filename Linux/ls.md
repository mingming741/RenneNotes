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
  * \-
  * 
