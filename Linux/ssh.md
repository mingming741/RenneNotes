# SSH
Secure Shell (SSH)安全外壳协议，是IETF制定的基于application layer之上的协议，主要是用于远程登录到其他的host上去，并且避免信息的泄露。（主要是用于解决ftp，PoP等不安全的问题.）通常SSH有基于口令（账号密码）的登录和基于密钥的登录方式。基于口令的方式有可能会被冒充服务器导致MITM攻击，而基于密钥的方式更加安全。

ssh最基本的使用方法就是用户名(-l或者@)加上host name(IP)
```console
ssh usename@host_name
ssh -l username host_name
例如
ssh showing@192.168.80.77 
```
当然也可以只打ip，这样的话ssh会使用你当前的用户名做登录。
```console
例如，从我的电脑上ssh到lab的.39上面
ssh 192.168.80.39
等价于
ssh showing@192.168.80.39
```
如果server和client上有可以对应的密钥，那么就不需要username，直接可以ssh过去。


# SFTP
SSH File Transfor Protocal(SFTP)，是一种安全的文件传输协议，为SSH的一部分，并且和FTP有着相同的使用方法。但是由于在传输的过程中使用了加密，因此比普通的FTP传输效率要低一些。

通常使用SFTP都是从服务器上取东西，通常port是22。因为SFTP是基于SSH的机制的，因此支持SSH的服务器，（即如果你能够ssh到那个server上去的话），就可以使用SFTP来取走server上的东西了。

sftp的使用方式类似ssh，应该说是基于ssh产生的，用起来感觉和ssh有点不一样。使用sftp登录到server上去：
```
sftp username@host_name
例如
sftp mclab@192.168.80.39
```


# SCP
Secure Copy(SCP)，用于做文件的原创拷贝，通常都是host将数据传输到服务器上去。数据传输同样使用ssh（密钥或者账户密码的方式）。
