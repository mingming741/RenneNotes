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
| apt edit-sources	| 新cmd | Edits sources list |
有了apt的封装，apt-get依旧在被使用，并且还有些没有完全封装的功能。对于linux的用户来说，apt应该足够了
