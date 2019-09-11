# Networks 

### ifconfig
显示当前os的network interface。network interface可以是hardware的也可用是software的，最基本的功能是传递信息。

### ip link
command `ip link`是用于给ip这个程序，设定routing, network devices, interfaces和tunnels。ip link创建的是network的interface，在创建之后，你还需要对其进行config，最基本的创建是add。创建一个名为Renne的link：
```
  $ ip link add Renne
```

### routing table
routing table用于记录，如果要发送packet去对应的destination的ip，我们的system应该发给的下一个node是谁。查看linux的routing table：
```
  $ ip route
```
或者
```
  $ netstat -rn
```
或者
```
  $ route -n
```

### 创建新的network namespace
创建一个名为Renne的network namespace
```
  $ ip netns add Renne
```
查看现有的network namespace
```
  $ ip netns list
```
如果要在新的namespace中获得链接的话，首先在这个namespace中我们需要一个network interface，其次我们需要另一个链接这个interface的network interface，将我们这个namespace中的信息forward到hardware的namespace(即网线)那边去，下面的cmd可以创建两个链接的interface：
```
  $ ip link add veth0 type veth peer name veth1
```
等于创建了两个virtual的Ethernet interface。所有的network interface在创建的时候，都是在默认的namespace"defalut"中的(有些时候在global中)。这时候我们可以用`ip link list`查看所有的interface。(这里不知道为什么ifconfig看不到但是ip link list看得到)。使用下面的cmd将veth1转移到namespace Renne中。
```
  $ ip link set veth1 netns Renne
```
这时候再打`ip link list`的话，我们就看不到veth1了，因为默认`ip link list`显示的是namespace 'default'中的interface，我们可以使用下面的cmd看namespace Renne中的link list
```
  $ ip netns exec Renne ip link list
```
也可给这个namespace中的interface assign ip。最好一个设置成up表示这个interface是被active的，如果是down的话就是没激活，用ifconfig就看不到。
```
  $ ip netns exec Renne ifconfig veth1 10.1.1.1/24 up
```
这样就完成了。但是这里注意到的是，namespace中的veth1只能ping到veth0，还是ping不到我自己电脑的ip和实验室里面的任意一部机，可能是还有一些gateway之类的东西需要去调节。并且，这上面的操作，是对os本身网络环境的全局修改，mahimahi可以做到在shell中独立创建环境，然后进行操作。


# iperf
iperf为linux中网络性能的测试工具，可以报告带宽和loss。

### Server和Client
启动TCP Socket的server和client，使用下面的命令：
```
$ iperf -s
$ iperf -c "server ip"
```
Server和client可以启动在同一个机器上，server在开启之后，会持续监听client的连接信息，而client在链接server之后，server会向client进行packet传输，并且在完成之后产生类似下面的log。因为iperf最基本的功能是带宽测试，测试的机制就是发包了。这里Interval表示Server对client持续发包的时间，即第0秒开始到第10.0秒结束。Transfer表示在这段时间内一共发出去了多少data，而Bandwidth表示measure到的带宽，其和时间的乘机通常比总data要大一点。在server和client的端都有measure，server端计算会慢一些，并且两端的结果有稍微的不同。
```
[  3] local 100.64.0.2 port 38142 connected with 192.168.80.77 port 5001
[ ID] Interval       Transfer     Bandwidth
[  3]  0.0-10.0 sec   158 MBytes   132 Mbits/sec
```
上面的命令默认启用的是TCP发包，同样我们也可用使用UDP模式进行packet send，使用`-u`的option
```
$ iperf -u -s
$ iperf -u -c "server ip"
```
这样使用的就是默认的UDP。

### 通用参数
通用参数表示报告的模式。每个参数都对于了一个环境变量，如`-l`对应了`$IPERF_LEN`。通用参数client和server都可以加，从man page上列举：
  * -i, --interval n: 中途报告时间，默认的iperf只会在flow结束的时候报告一次，加了-i就会在途中每隔n时间报告一次
  * -l, --len n[KM]: 调整read/write buffer的长度(default 8 KB)。（为什么server和client都有buffer？并且据我的测试，client side的buffer对测试结果也有影响。）
  * -M, --mss n: set TCP maximum segment size (默认MTU是1500bytes)
  * -m, --print_mss: 在结束后print TCP maximum segment size(MTU)
  * -p, --port n：在server打就表示server开启哪个port来listen client的request(default 5001)，在Client打表示client要连接server的哪个port。(我试了下如果一部机开了两个iperf的server，client不指定port的话好像只能根据某种priority连接到两个中的其中一个，或者默认连接port 5001)
  * -w, --window n[KM]: TCP window size (socket buffer size)
  * -B, --bind <host>: Bind to specific interfaces (我没看懂)
  * -u, --udp: use UDP rather than TCP，这里我发现UDP就只管发，不管server那边任何的reply，不管server ip对不对，也不管对应的port是否存在。（UDP是真的流弊）
 
 例如client连接到server的7777port并且每秒都report当前的state：
 ```
$ iperf -c 192.168.80.77 -i 1 -p 7777
```

### Server的参数
包括下面
  * -s, --server: run in server mode
  * -U, --single_udp，run in single threaded UDP mode
  * -D, --daemon，run the server as a daemon

非常的直观QAQ

### Client的参数
客户端的参数变量，
 * -c, --client <host> run in client mode, connecting to <host>，即server的IP
 * -b, --bandwidth n[KM] set bandwidth to n bits/sec (default 1 Mbit/sec). 仅在UDP(-u)模式下有效
 * -d, --dualtest: 同时双向测试，似乎需要client这边也能够监听server那边的connection，即两部机都要run一个server
 * -n, --num n[KM]: number of bytes to transmit (instead of -t)
 * -t, --time ntime in seconds to transmit for (default 10 secs)。关于`-t`和`-n`，如果两个都不打，就是默认发10秒，如果两个都打了的话会产生冲突，打在后面的会完全覆盖前面的，貌似无法实现两个constrain都有的情况。in practice最好不要两个一起打。
 * -F, --fileinput <name>: input the data to be transmitted from a file。（这个选项在client的话说明了client是sender啊）
 * -P, --parallel n：number of parallel client threads to run，也可以理解为client同时有几个flow。
 * -Z, --linux-congestion <algo>: set TCP congestion control algorithm (Linux only)。说明CC还是可以改的，但是只能改linux支持的。



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



