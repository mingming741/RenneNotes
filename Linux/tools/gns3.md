# GNS3
graphic network simulater 3, this document record the command about config switch and router.

### Switch config
if you are outside config mode, all command are view the state of this switch, here is:

Enter root mode
```
enable
```
Exit to user mode
```
disable
```
check network interface 
```
show interface status
```
check neighbor switch and router
```
show cdp neighbors
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





