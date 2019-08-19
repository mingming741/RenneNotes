# apt
apt是用于在linux中安装依赖包的command，在ubuntu16.04之前，用的是apt-get。下面的两个command在平时使用应该都有见过
```
$ apt install package
$ apt-get install package
```
在这之后，越来越多的软件包支持变成了新版的apt而不是apt-get。可以认为apt是apt-get在Linux上的python wrapper，apt的封装上比apt-get更加user friendly一些。下面显示了apt封装的apt-get一些command。

|  apt command |  the command it replaces |  function of the command |
|---|---|---|
| apt install	| apt-get install	| Installs a package  |
| apt remove	| apt-get remove	| Removes a package  |
| apt purge	  | apt-get purge	  | Removes package with configuration  |
| apt update	| apt-get update	| Refreshes repository index |
| apt upgrade	| apt-get upgrade	| Upgrades all upgradable packages |
| apt autoremove |	apt-get autoremove	| Removes unwanted packages |
| apt full-upgrade	| apt-get dist-upgrade |	Upgrades packages with auto-handling of dependencies |
| apt search	| apt-cache search	| Searches for the program |
| apt show	| apt-cache show	| Shows package details |
| apt list	| 新cmd | Lists packages with criteria (installed, upgradable etc) |
| apt edit-sources	| 新cmd | Edits sources list | |

有了apt的封装，apt-get依旧在被使用，并且还有些没有完全封装的功能。对于linux的用户来说，apt应该足够了。

### apt update & upgrade
update的操作会更新当前安装的可用的packet的版本，但是不会真的安装这些包。update的source来源于/etc/apt/sources.list这个文件。即只检查是否有可用的更新，但不跟新，在结束之后，给出给出汇总报告。类似apple store中的刷新可以更新的软件。
```
# updates the list of available packages and their versions, but it does not install or upgrade any packages.

$ apt-get update 
```
upgrade的操作会根据update的list，安装那些你检查到了更新但是没有安装的包。同时也有dist-upgrade的操作
```
# actually installs newer versions of the packages you have. After updating the lists, the package manager knows about available updates for the software you have installed. This is why you first want to update

$ apt-get upgrade
```
所以通常是先跑update再跑upgrade，这样就可以完整的更新到系统的packet了。

### AptUrl
apt的GUI工具，也是在ubuntu中默认安装的，帮助user安装各种packet的。（不知道为什么）


