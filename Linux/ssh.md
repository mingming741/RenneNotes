# SSH
Secure Shell (SSH)安全外壳协议，是IETF制定的基于application layer之上的协议，主要是用于远程登录到其他的host上去，并且避免信息的泄露。（主要是用于解决ftp，PoP等不安全的问题）

通常SSH有基于口令（账号密码）的登录和基于密钥的登录方式。基于口令的方式有可能会被冒充服务器导致MITM攻击，而基于密钥的方式更加安全。

# SFTP
SSH File Transfor Protocal(SFTP)，是一种安全的文件传输协议，为SSH的一部分，并且和FTP有着相同的使用方法。但是由于在传输的过程中使用了加密，因此比普通的FTP传输效率要低一些。

通常使用SFTP都是从服务器上取东西，通常port是22。因为SFTP是基于SSH的机制的，因此支持SSH的服务器，（即如果你能够ssh到那个server上去的话），就可以使用SFTP来取走server上的东西了。


# SCP
Secure Copy(SCP)，用于做文件的原创拷贝，通常都是host将数据传输到服务器上去。数据传输同样使用ssh（密钥或者账户密码的方式）。
