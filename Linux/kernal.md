# Kernal
Linux Kernal即操作系统本身，平时我们说的kernal base即操作系统内部的运行环境，而各种application通常是调用kernal中的一些method来达到目的的，kernal的中文翻译叫“内核”

### Linux Kernal Module
在linux中，"Kernel modules are pieces of code that can be loaded and unloaded into the kernel upon demand"。意思就是，module等价于一段code，这段code不绑定在linux系统本身，而是可以根据需要，动态的被导入到linux系统中。这样的好处是，使得系统本身至于太重。并且各种module之间有可以替换性，使得os本身可以适应不同的工作环境需求。

例如，Golf的stateful和Cubic+就是作为linux的一个module被插入系统，module本身独立于系统，但是可以成为系统操作的一部分。linux在开机的时候会自动load系统中的部分module，取决于linux的implementation。对module的操作主要使用insmod和rmmod，也可用使用modprobe。
```console
# 查看linux中已有的module
lsmod
# 插入一个module
insmod modulename
# 移除一个module
rmmod modulename
```
系统在启动之后，所有module的信息都在/proc/modules文件中列出。而modprobe更加系统，包含了上面几个cmd的内容。modprobe比起insmod，可以做到载入module的dependence，但是需要module的ko文件保存在/lib/modules/这个路径下面的一个文件夹内。相反的是，insmod可以insert任何路径中的一个module。

### Environment Variables (环境变量)
和windows类似，Linux同样有一些内置的环境变量，在一些程序运行的时候following这些config，使用`printenv`查看全部的环境变量
```
XDG_VTNR=7
XDG_SESSION_ID=c2
CLUTTER_IM_MODULE=xim
XDG_GREETER_DATA_DIR=/var/lib/lightdm-data/showing
SESSION=ubuntu
GPG_AGENT_INFO=/home/showing/.gnupg/S.gpg-agent:0:1
TERM=xterm-256color
SHELL=/bin/bash
XDG_MENU_PREFIX=gnome-
VTE_VERSION=4205
QT_LINUX_ACCESSIBILITY_ALWAYS_ON=1
WINDOWID=86000677
UPSTART_SESSION=unix:abstract=/com/ubuntu/upstart-session/1000/1622
GNOME_KEYRING_CONTROL=
GTK_MODULES=gail:atk-bridge:unity-gtk-module
USER=showing
LD_LIBRARY_PATH=/home/showing/ns-allinone-2.35/otcl-1.14:/home/showing/ns-allinone-2.35/lib:/usr/local/lib:
LS_COLORS=rs=0:di=01;34:ln=01;36:mh=00:pi=40;33:so=01;35:do=01;35:bd=40;33;01:cd=40;33;01:or=40;31;01:mi=00:su=37;41:sg=30;43:ca=30;41:tw=30;42:ow=34;42:st=37;44:ex=01;32:*.tar=01;31:*.tgz=01;31:*.arc=01;31:*.arj=01;31:*.taz=01;31:*.lha=01;31:*.lz4=01;31:*.lzh=01;31:*.lzma=01;31:*.tlz=01;31:*.txz=01;31:*.tzo=01;31:*.t7z=01;31:*.zip=01;31:*.z=01;31:*.Z=01;31:*.dz=01;31:*.gz=01;31:*.lrz=01;31:*.lz=01;31:*.lzo=01;31:*.xz=01;31:*.bz2=01;31:*.bz=01;31:*.tbz=01;31:*.tbz2=01;31:*.tz=01;31:*.deb=01;31:*.rpm=01;31:*.jar=01;31:*.war=01;31:*.ear=01;31:*.sar=01;31:*.rar=01;31:*.alz=01;31:*.ace=01;31:*.zoo=01;31:*.cpio=01;31:*.7z=01;31:*.rz=01;31:*.cab=01;31:*.jpg=01;35:*.jpeg=01;35:*.gif=01;35:*.bmp=01;35:*.pbm=01;35:*.pgm=01;35:*.ppm=01;35:*.tga=01;35:*.xbm=01;35:*.xpm=01;35:*.tif=01;35:*.tiff=01;35:*.png=01;35:*.svg=01;35:*.svgz=01;35:*.mng=01;35:*.pcx=01;35:*.mov=01;35:*.mpg=01;35:*.mpeg=01;35:*.m2v=01;35:*.mkv=01;35:*.webm=01;35:*.ogm=01;35:*.mp4=01;35:*.m4v=01;35:*.mp4v=01;35:*.vob=01;35:*.qt=01;35:*.nuv=01;35:*.wmv=01;35:*.asf=01;35:*.rm=01;35:*.rmvb=01;35:*.flc=01;35:*.avi=01;35:*.fli=01;35:*.flv=01;35:*.gl=01;35:*.dl=01;35:*.xcf=01;35:*.xwd=01;35:*.yuv=01;35:*.cgm=01;35:*.emf=01;35:*.ogv=01;35:*.ogx=01;35:*.aac=00;36:*.au=00;36:*.flac=00;36:*.m4a=00;36:*.mid=00;36:*.midi=00;36:*.mka=00;36:*.mp3=00;36:*.mpc=00;36:*.ogg=00;36:*.ra=00;36:*.wav=00;36:*.oga=00;36:*.opus=00;36:*.spx=00;36:*.xspf=00;36:
QT_ACCESSIBILITY=1
XDG_SESSION_PATH=/org/freedesktop/DisplayManager/Session0
TCL_LIBRARY=/home/showing/ns-allinone-2.35/tcl8.5.10/library:/usr/lib:
XDG_SEAT_PATH=/org/freedesktop/DisplayManager/Seat0
SSH_AUTH_SOCK=/run/user/1000/keyring/ssh
SESSION_MANAGER=local/showing-OptiPlex-9010:@/tmp/.ICE-unix/1904,unix/showing-OptiPlex-9010:/tmp/.ICE-unix/1904
DEFAULTS_PATH=/usr/share/gconf/ubuntu.default.path
XDG_CONFIG_DIRS=/etc/xdg/xdg-ubuntu:/usr/share/upstart/xdg:/etc/xdg
PATH=/home/showing/ns-allinone-2.35/bin:/home/showing/ns-allinone-2.35/tcl8.5.10/unix:/home/showing/ns-allinone-2.35/tk8.5.10/unix:/home/showing/ns-allinone-2.35/ns-2.35:/home/showing/ns-allinone-2.35/nam-1.15:/home/showing/bin:/home/showing/.local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
DESKTOP_SESSION=ubuntu
QT_IM_MODULE=fcitx
QT_QPA_PLATFORMTHEME=appmenu-qt5
XDG_SESSION_TYPE=x11
JOB=dbus
PWD=/home/showing
XMODIFIERS=@im=fcitx
GNOME_KEYRING_PID=
LANG=en_HK.UTF-8
GDM_LANG=en_US
MANDATORY_PATH=/usr/share/gconf/ubuntu.mandatory.path
IM_CONFIG_PHASE=1
COMPIZ_CONFIG_PROFILE=ubuntu
GDMSESSION=ubuntu
SESSIONTYPE=gnome-session
GTK2_MODULES=overlay-scrollbar
XDG_SEAT=seat0
HOME=/home/showing
SHLVL=1
LANGUAGE=en_HK:en
GNOME_DESKTOP_SESSION_ID=this-is-deprecated
XDG_SESSION_DESKTOP=ubuntu
LOGNAME=showing
QT4_IM_MODULE=fcitx
XDG_DATA_DIRS=/usr/share/ubuntu:/usr/share/gnome:/usr/local/share:/usr/share:/var/lib/snapd/desktop
DBUS_SESSION_BUS_ADDRESS=unix:abstract=/tmp/dbus-nJ7nqYiAU2
LESSOPEN=| /usr/bin/lesspipe %s
INSTANCE=
XDG_RUNTIME_DIR=/run/user/1000
DISPLAY=:0
XDG_CURRENT_DESKTOP=Unity
GTK_IM_MODULE=fcitx
LESSCLOSE=/usr/bin/lesspipe %s %s
XAUTHORITY=/home/showing/.Xauthority
_=/usr/bin/printenv
OLDPWD=/home/showing
```
这里列举了我电脑里的一些环境变量：
```

```


