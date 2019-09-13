# Event Loop
这里介绍下C的event loop的实现方式。

### Poll
```c
#include <poll.h>
```
`pool.h`中包括了`poll()`函数，即 wait for some event on a file descriptor。
