# iperf
iperf为linux中网络性能的测试工具，可以报告带宽和loss。

### Server和Client
启动TCP Socket的server和client，使用下面的命令：
```
$ iperf -s
$ iperf -c "server ip"
```
Server和client可以启动在同一个机器上，server在开启之后，会持续监听client的连接信息，而client在链接server之后，server会向client进行packet传输，并且在完成之后产生类似下面的log。因为iperf最基本的功能是带宽测试，测试的机制就是发包了。这里Interval表示Server对client持续发包的时间，Transfer表示在这段时间内一共发出去了多少data，而Bandwidth表示measure到的带宽，其和时间的乘机通常比总data要大一点。在server和client的端都有measure，并且两端的结果有稍微的不同。
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

### 参数
通常，iperf的参数可以实现各种组合，下面分别介绍每一个参数的作用。

发送时间`-t`，启用在client side，下面表示flow持续60秒
```
$ iperf -c "server ip" -t 60
```
