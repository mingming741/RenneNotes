# GNS3
graphic network simulater 3,记录各种switch and router的config操作以及意义。

### Switch config
Switch的config类似文件系统，即cd到某个路径下，所有的config都作用于该路径。这里是config进入某个switch就config这个switch，如果是某个switch的某个port，就是config这个port。

首先是管理员权限：
```shell
 # Enter root mode
enable
 # Exit to user mode
disable
```

然后是查看当前状态
```shell 
show interface status # 查看自己的network interface
show cdp neighbors # 查看与自己相连的neighbor
```

check STP detail
```
show spanning-tree 
```


You can enter config mode to config this switch:
```
configure terminal
```
then you are in config mode, to config specific interface of the switch, type:
```
interface e0/1
```
then you will config the interface `e0/1`, shutdown this interface:
```
shutdown
```
In most of command, add an `no` before the command to get reverse effect, `no shutdown` means enable this interface.

You can use `exit` to return back to one layer, and `end` to exit the config mode.

We can also config multiple interface at the same time. Use range, here is config e0/2 and e0/3 
```
interface range e0/2 - 3
```
We can modify the VLAN mode to access mode by using:
```
switchport mode access
```
The access mode is to using normal VLAN.

After all config, we need to save the config to the switch for run, use:
```
wr
```

## Switch config Knowledgement 
我们可以查看switch某个port的详细config，输入
```
show interface switchport e0/2
```
可以查看


### Host
linux pc, same as any linux, we can using any command in linux inside these host

ARP在任何host和router中都存在，switch中不存在。Check arp table:
```
arp -a
```
Remove a arp entry for 10.0.0.3:
```
arp -d 10.0.0.3
```





