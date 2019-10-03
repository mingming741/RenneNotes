# wireshark
用于网络抓包的工具

### 常用功能
Statistic中，有各种protocal对应的常规可视化，例如，TCP stream graph中有throughput和RTT等东西。

### command
使用tshark将wireshark保存的pcap文件转化成txt做分析：
```
tshark -r test.pcapng -t ad > 2333.txt // 全部field
tshark -r test.pcapng -T fields -e frame.time_relative -e frame.cap_len -e tcp.srcport -e tcp.dstport > 2333.txt // 指定field
```
