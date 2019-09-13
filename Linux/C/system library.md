# system library
记录一些c++平时常用的系统层面的library，以linux作为蓝本，(windows可能有点不同)

### SIGNALFD
signalfd - create a file descriptor for accepting signals
```c
#include <sys/signalfd.h>
int signalfd(int fd, const sigset_t *mask, int flags);
```
