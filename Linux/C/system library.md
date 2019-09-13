# System library
记录一些c++平时常用的系统层面的library，以linux作为蓝本，(windows可能有点不同)。通常在include path在`/usr/include/c++`中，而`namespace std`代表了Standard library中的函数。

### Standard Stream
常见的为`stdin`，`stdout`和`stderr`，这三个都是file descriptor，其实就是指向file的一个指针。`<unistd.h>`表示POXIS中的定义，`<stdio.h>`表示在c中的定义，而` <iostream>`表示在c++中的定义
* `stdin`：fd序号为0，在c中为变量` FILE* stdin`，而在c++中为变量`std::cin`
* `stdout`: fd序号为1，在c中为变量` FILE* stdout`，而在c++中为变量`std::cout`
* `stderr`: fd序号为2，在c中为变量` FILE* stderr`，而在c++中为变量`std::cerr`和`std::clog`

我们可以认为，当cout被调用的时候，其实我们是将数据导向了`stdout`这个file descriptor指向的文件，然而shell/ternimal中会有一些机制，使得这些消息会在terminal中显示出来。在linux系统中，file descriptor都被放在`/proc/self/fd`路径下，对应的0, 1, 2就是上面说的stdin，stdout，stderr了。

### SIGNALFD
signalfd - create a file descriptor for accepting signals
```c
#include <sys/signalfd.h>
int signalfd(int fd, const sigset_t *mask, int flags);
```
