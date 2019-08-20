# iperf
iperf为linux中网络性能的测试工具，可以报告带宽和loss。

### Server和Client
启动TCP Socket的server和client，使用下面的命令：
```
$ iperf -s
$ iperf -c "server ip"
```
Server和client可以启动在同一个机器上，server在开启之后，会持续监听client的连接信息，而client在链接server之后，server会向client进行packet传输，并且在完成之后产生类似下面的log。因为iperf最基本的功能是带宽测试，测试的机制就是发包了。这里Interval表示Server对client持续发包的时间，Transfer表示在这段时间内一共发出去了多少data，而Bandwidth表示measure到的带宽，其和时间的乘机通常比总data要大一点。在server和client的端都有measure，server端计算会慢一些，并且两端的结果有稍微的不同。
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
  * -h, --help： 帮助
  * -i, --interval n: 中途报告时间，默认的iperf只会在flow结束的时候报告一次，加了-i就会在途中每隔n时间报告一次
  * -l, --len n[KM]: 调整read/write buffer的长度(default 8 KB)。（为什么server和client都有buffer？并且据我的测试，client side的buffer对测试结果也有影响。）
  * -M, --mss n: set TCP maximum segment size (默认MTU是1500bytes)
  * -m, --print_mss: 在结束后print TCP maximum segment size(MTU)
  * -p, --port n：在server打就表示server开启哪个port来listen client的request(default 5001)，在Client打表示client要连接server的哪个port。(我试了下如果一部机开了两个iperf的server，client不指定port的话好像只能根据某种priority连接到两个中的其中一个)
  * -w, --window n[KM]: TCP window size (socket buffer size)
  * -B, --bind <host>: Bind to specific interfaces (我没看懂)

### Server和client的参数
通常，iperf的参数可以实现各种组合，下面分别介绍每一个参数的作用。使用我lab电脑的server ip：192.168.80.77

发送时间`-t`，启用在client side，下面表示flow持续60秒
```
$ iperf -c 192.168.80.77 -t 60
```
多线程`-P`，即多少个flow同时connect到server。下面表示同时开启10个thread，10个flow连接到server
```
$ iperf -s
$ iperf -c 192.168.80.77 -P 10
```
