# Network相关的C编程
这里简单记录一下pantheon的tunnel中使用到的packet，如果还要可以继续补充。

### Socket
socket类似与一个网络接口interface，Linux中定义在`sys/socket.h`中。下面的方法定义一个socket：（在socket.h中定义的函数，大都是return 0表示success，-1表示error）
```c
int sockfd = socket(domain, type, protocol)
// sys/socket.h: extern int socket (int __domain, int __type, int __protocol) __THROW; in "sys/socket.h"
// sockfd: 对应socket的file descriptor
// domain: 通常是指定值，例如 AF_INET (IPv4 protocol) , AF_INET6 (IPv6 protocol)
// type: L4的机制，SOCK_STREAM (TCP)，SOCK_DGRAM (UDP)
// protocol: L3的机制，Internet Protocol(IP)的话就是0
```
下面的方法也可以set指定socket的属性：
```c
int setsockopt(int sockfd, int level, int optname, const void *optval, socklen_t optlen);
```
因为socket本身只是一块内存的config，我们需要将其bind到一个network interface上去，使用bind函数：
```c
int bind(int sockfd, const struct sockaddr *addr, socklen_t addrlen);
// sys/socket.h: extern int bind (int __fd, sockaddr __addr, socklen_t __len) __THROW; in "sys/socket.h"
// sockfd: 要绑定的socket
// *addr: 要绑定的address，使用struct sockaddr定义
// addrlen: 对应的socket address的length
```
这里讲一下`struct sockaddr`，定义在`bits/socket.h`中，被`sys/socket.h`调用，而`sockaddr_in`是类似继承了sockaddr的一个结构
```c
struct sockaddr {
    unsigned short    sa_family;    // address family, AF_xxx
    char              sa_data[14];  // 14 bytes of protocol address
};

struct sockaddr_in {
    short            sin_family;   // e.g. AF_INET, AF_INET6
    unsigned short   sin_port;     // e.g. htons(3490)
    struct in_addr   sin_addr;     // see struct in_addr, below
    char             sin_zero[8];  // zero this if you want to
};
```
我找到的example的code，可以这样使用bind函数，即直接将sockaddr_in cast成了一个sockaddr，（没看懂，先记下来）
```c
bind(server_fd, (struct sockaddr *)&address, sizeof(address))
```
bind函数其实是给定义的socket的object(fd)赋予了ip和port，这个ip和port由object sockaddr来声明。在bind之后，如果我们跑的是server，则需要调用listen函数。listen函数大概是等待client接入。
```c
int listen(int sockfd, int backlog);
// extern int listen (int __fd, int __n) __THROW; in "sys/socket.h"
// backlog: 表示server等待client connection的最大等待数量，如果超过会return ECONNREFUSED
```
listen并不会让程序block住并且等待client接入，只是给sockfd一个listen的属性，而accept才会真正让server等待client：
```c
int new_socket= accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen);
```
accept会接受第一个connect server的client，并且create一个新的socket，并且将这个socket的fd返回。在accept成功之后，这个socket就是和指定的client交流的socket，后面的send和read都用指定的这个新的client，即类似一个connection。

对于client side，下面一些函数也是有用的，例如，`inet_pton`函数可以将address转化成binary的值，并且赋值给struct sockaddr_in的sin_addr。应该说，本来计算机中处理的address都是binary格式的。
```c
inet_pton(AF_INET, "127.0.0.1", &address.sin_addr)
```
在server listen并且accept block住的时候，client就可以使用函数connect来连接到server：
```c
int connect(int sockfd, const struct sockaddr *addr, socklen_t addrlen);
// extern int connect (int __fd, __CONST_SOCKADDR_ARG __addr, socklen_t __len); in "sys/socket.h"
```
在connection建立之后，我们可以使用send/read函数，使得两端互相发送信息：
```c
// send
char *message = "Client Message";
send(sock, message, strlen(message), 0);

// read
char buffer[1024] = {0};
valread = read(new_socket_fd, buffer, 1024);
```
read的一段会block住，等待另一端的send，因此两边一定要协调好



### 其他
```c++
#include <netinet/in.h>
// Internet Protocol family
/*
这个header定义了IP相关的typedef
in_port_t： 
in_addr_t：
sockaddr_in： used to store addresses for the Internet protocol family. Values of this type must be cast to struct sockaddr for use with the socket interfaces defined in this document
sa_family_t： 
*/
```

```c++
#include <netdb.h>
// definitions for network database operations
```


