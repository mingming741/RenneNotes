# Event Loop
这里介绍下C的event loop的实现方式。

#### Poll
`pool.h`中包括了`poll()`函数，poll和select一样，会轮流监视其参数fds中所有fd改变的状态，并且在产生改变的时候返回。其实也是轮流检车每个file descriptor所指向的file有没有发生变化，没有最大的file限制。

对于其监视的所有file，如果被读写的话，都会产生对应的event，而每次循环检查，poll都会返回尝试的对应的event的file的数目。
```c
#include <poll.h>

int poll(struct pollfd *fds, nfds_t nfds, int timeout);
struct pollfd{
	int fd;			//文件描述符
	short events;	//等待的事件
	short revents;	//实际发生的事件
};
```
即 wait for some event on a file descriptor。

本质上，polling的意思是轮寻，`pool(x)`的意思，类似“在x为timeout时间内，检查条件，若成立，执行callback函数”
