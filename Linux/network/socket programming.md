# Network相关的C编程
这里简单记录一下pantheon的tunnel中使用到的packet，如果还要可以继续补充。

sys/socket.h
```c++
#include <sys/socket.h>

```

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


