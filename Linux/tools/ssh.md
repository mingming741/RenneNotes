# SSH
Secure Shell (SSH)安全外壳协议，是IETF制定的基于application layer之上的协议，主要是用于远程登录到其他的host上去，并且避免信息的泄露。（主要是用于解决ftp，PoP等不安全的问题.）通常SSH有基于口令（账号密码）的登录和基于密钥的登录方式。基于口令的方式有可能会被冒充服务器导致MITM攻击，而基于密钥的方式更加安全。

ssh最基本的使用方法就是用户名(-l或者@)加上host name(IP)
```console
ssh usename@host_name
ssh -l username host_name
# 例如
ssh showing@192.168.80.77 
```
当然也可以只打ip，这样的话ssh会使用你当前的用户名做登录。
```console
# 例如，从我的电脑上ssh到lab的.39上面
ssh 192.168.80.39
# 等价于
ssh showing@192.168.80.39
```
如果server和client上有可以对应的密钥，那么就不需要username，直接可以ssh过去。


# SFTP
SSH File Transfor Protocal(SFTP)，是一种安全的文件传输协议，为SSH的一部分，并且和FTP有着相同的使用方法。但是由于在传输的过程中使用了加密，因此比普通的FTP传输效率要低一些。

通常使用SFTP都是从服务器上取东西，通常port是22。因为SFTP是基于SSH的机制的，因此支持SSH的服务器，（即如果你能够ssh到那个server上去的话），就可以使用SFTP来取走server上的东西了。

sftp的使用方式类似ssh，应该说是基于ssh产生的，用起来感觉和ssh有点不一样。使用sftp登录到server上去：
```console
sftp username@host_name
# 例如
sftp mclab@192.168.80.39
```
进到host的路径上去之后，可以使用help查看sftp的cmd。基本的linux command如ls，cd都可以直接使用。如果要从server上取file的话，使用get
```console
get filename
```
这样filename这个file会被host传输到本地路径，路径等于你terminal打开sftp的路径下去。


# SCP
Secure Copy(SCP)，用于做文件的原创拷贝，通常都是host将数据传输到服务器上去。数据传输同样使用ssh（密钥或者账户密码的方式）。SCP可以使用upload也可用使用download的方式，首先介绍upload
```console
# upload: local -> remote
scp local_file user@remote_host:remote_file
```
upload会把指定local file上传到host对应的指定路径下，如果没有给出绝对路径，则会传到对应user的home下面。因为upload不需要指定本地路径，因此相对也比较方便，因为download需要指定remote的路径，因此upload相对方便。比download用的要多，download可以用sftp来解决。

download使用命令如下：
```console
# download: remote -> local
scp user@remote_host:remote_file local_file 
```
remote_file需要指定绝对路径，并且每次都需要输入密码，非常的不好用，local_file会被保存在terminal当前的pwd位置。


