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





