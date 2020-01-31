# GNS3
graphic network simulater 3,记录各种switch and router的config操作以及意义。

### Switch config


首先是管理员权限：
```shell
enable  # 进入root模式
disable  # 进入user模式
```

然后是查看当前状态
```shell
show interface status  # 查看自己的network interface
show cdp neighbors  # 查看与自己相连的neighbor
show spanning-tree # 查看STP的细节
```

Switch的config类似文件系统，即cd到某个路径下，所有的config都作用于该路径。这里是config进入某个switch就config这个switch，如果是某个switch的某个port，就是config这个port。
```shell
configure terminal  # 进入switch的config模式
```
在config模式下，可以进入switch的任何一个port的config
```shell
interface e0/1  # 进入e0/1的config，等价于cd到这个interface的config路径下。
interface range e0/2 - 3   # 同时config这个switch的0/2和0/3两个port
```
然后可以使用下面的config命令。大部分情况下，前面加一个`no`都是某个命令的反命令。
```shell
shutdown  # 关掉这个port
no shutdown  # 启用这个port
exit # 等于cd ..
end  # 退出config模式
switchport mode access  # 将这个port的VLAN设置为access 模式
switchport access vlan 1  # access模式设置其access的那个VLAN
switchport trunk encapsulation  dot1q  # 将VLAN封装模式设置为IEEE标准模式

wr  # 保存对switch的设置，在设置完之后一定要打
```

## Switch config Knowledgement 
我们可以查看switch某个port的详细config，输入
```shell
show interface switchport e0/2  # 查看switch的port e0/2的详细信息
```
可以查看到许多具体的信息，下面解释一下每个field的意义是什么。L2的switch的模式大部分都是VLAN相关，支持VTP的话会有协商模式。VLAN的port模式包括access，trunk，dynamic desirable， dynamic auto。而Trunking Encapsulation的封装标准也是可以协商的。
* Switchport：enable/disable，表明这个port是否在运作。
* Administrative Mode：switch预设的VLAN模式，只能config这个switch才能够改。
* Operational Mode：实际上正在运行的VLAN模式，这里只有access和trunk
* Administrative Trunking Encapsulation：switch预设的VLAN封装标准，cisco有自己的标准，而通用的是IETF的802.1q标准
* Operational Trunking Encapsulation：正在运作的VLAN封装标准。
* Negotiation of Trunking：cisco的Dynamic Trunking Protocol，用于自动决定使用cisco的ISL封装，还是使用802.1q
* Access Mode VLAN： ？？？
* Trunking Native Mode VLAN： ？？？
* Administrative Native VLAN tagging
* Operational private-vlan：
* Trunking VLANs Enabled: 这个trunk让哪些VLAN过
* Pruning VLANs Enabled


### Host
ARP在任何host和router中都存在，switch中不存在。Check arp table:
```
arp -a
```
Remove a arp entry for 10.0.0.3:
```
arp -d 10.0.0.3
```





