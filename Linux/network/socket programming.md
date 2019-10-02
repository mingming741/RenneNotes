# Network相关的C编程
这里简单记录一下pantheon的tunnel中使用到的packet，如果还要可以继续补充。

### Socket
socket类似与一个网络接口interface，Linux中定义在`sys/socket.h`中。下面的方法定义一个socket：（在socket.h中定义的函数，大都是return 0表示success，-1表示error）
```c
int sockfd = socket(domain, type, protocol)
// sys/socket.h: extern int socket (int __domain, int __type, int __protocol) __THROW;
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
// sys/socket.h: extern int bind (int __fd, sockaddr __addr, socklen_t __len) __THROW;
// sockfd: 要绑定的socket
// *addr: 要绑定的address，使用struct sockaddr定义
// addrlen: 对应的socket address的length
```
这里讲一下`struct sockaddr`，定义在`bits/socket.h`中，被`sys/socket.h`调用，结构大概是：
```c
struct sockaddr {
    unsigned short    sa_family;    // address family, AF_xxx
    char              sa_data[14];  // 14 bytes of protocol address
};
```
而`sockaddr_in`是类似继承了sockaddr的一个结构
```c
struct sockaddr_in {
    short            sin_family;   // e.g. AF_INET, AF_INET6
    unsigned short   sin_port;     // e.g. htons(3490)
    struct in_addr   sin_addr;     // see struct in_addr, below
    char             sin_zero[8];  // zero this if you want to
};
```
我找到的example的code，可以这样使用bind函数：
```c
bind(server_fd, (struct sockaddr *)&address, sizeof(address))
```
即直接将sockaddr_in cast成了一个sockaddr，（没看懂，先记下来）

```c++
#include <sys/socket.h>

```



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


