# Linux Kernal
Linux Kernal即操作系统本身，平时我们说的kernal base即操作系统内部的运行环境，而各种application通常是调用kernal中的一些method来达到目的的，kernal的中文翻译叫“内核”

# Linux Kernal Module
在linux中，"Kernel modules are pieces of code that can be loaded and unloaded into the kernel upon demand"。意思就是，module等价于一段code，这段code不绑定在linux系统本身，而是可以根据需要，动态的被导入到linux系统中。这样的好处是，使得系统本身至于太重。并且各种module之间有可以替换性，使得os本身可以适应不同的工作环境需求。

例如，Golf的stateful和Cubic+就是作为linux的一个module被插入系统，module本身独立于系统，但是可以成为系统操作的一部分。linux在开机的时候会自动load系统中的部分module，取决于linux的implementation。对module的操作主要使用insmod和rmmod，也可用使用modprobe。
```console
# 查看linux中已有的module
lsmod
# 插入一个module
insmod modulename
# 移除一个module
rmmod modulename
```console
而modprobe更加系统，包含了上面几个cmd的内容。
```

```

