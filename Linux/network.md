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





