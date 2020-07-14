# hardware command
用于记录如何在Linux机器上查看硬件的command


## 硬件知识
### bios (Basic Input/Output System)
用于启动system的framework，通常已经集成在了主板上，用于引导系统启动。




## command
### lshw
`lshw`即list hardware，就是列出了/proc files中的information。这里包括各种hardware的信息。不过这些信息比较的简略，可以用来查看电脑都有些什么硬件。
* memory
* cpu
* pci(Peripheral Component Interconnect)即主板上面的各种插口，例如插ram的，可以理解为主板的外接设备，或者主板本身
* usb
* generic: 似乎是和linux的内核有关的硬件
* serial: 似乎是usb的控制器，Universal Serial Bus Controllers
* communication: 似乎也是总线控制器
* storage：硬盘，这里看不到细节
* lsa (Industry Standard Architecture)： 总线
* multimedia: 通常是声卡
* network: 网卡
* cache: CPU里面的cache


### hwinfo
`hwinfo`比`lshw`列出的硬件信息更加详细。甚至可以检测到USB的外接，例如显示屏的model，鼠标和键盘。


### lscpu
`lscpu`用于查看电脑cpu的general information


### lspci
`lspci`用于查看主板上面的各种控制器，例如vga adapter, graphics card, network adapter, usb ports, sata controllers。pci大概就是指主板上的各种连接线，和集成在主板上面的各种东西。这个command可以看到主板上面集成的网卡，显卡，声卡等等。非集成的也是根据pci的接口插到主板上去的。

### lsblk / df / fdisk
`lsblk`会列出简单的硬盘分区，df用于查看文件系统。`df -H`查看硬盘使用情况，`pydf`是python版本的df，使用起来更加方便。`fdisk -l`也可以查看到更多信息。


### free
`free`用于查看当前ram的使用情况，常用的有`free -th`，结合watch使用可以实时监测ram的使用，使用command `watch -n 1 free -th`，每秒刷新ram的使用


### /proc的file
可以通过cat查看/proc中不同的file的当前信息，即获取当前系统的使用情况
* cpu information: `cat /proc/cpuinfo`
* memory information: `cat /proc/meminfo'
* linux version: `cat /proc/version`
* partitions: `cat /proc/partitions`


