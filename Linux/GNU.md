# GNU
GNU是一个自由的操作系统，参与者在参与开发的时候，只要遵循其中的GPL协议做软件开发即可达到目的。目的是为了制造开源的环境，类似一个linux的开源天堂，允许任何创作者在遵循GPL开发规范的情况下，在其中加入新的内容。

### GPL & LGPL
GPL， GNU General Public License的缩写，即GNU通用公共授权。目的在于，试图保证你共享和修改自由软件的自由，保证自由的软件对于所有的用户来说是自由的。比方说，遵循GPL开发协议的软件，在release之后，开发者必须将自己的所有right共享，即需要将源代码公开。并且允许其他人在你的代码基础之上，进行修改，和重新发布。

LGPL， GNU Lesser General Public License，是旧版的GPL的更新的版本，修改了某些条目。同样需要保证软件的开源性，不能将别人的开源代码私有化，并且用作商业发布。

### GCC
GNU Compiler Collection，即GNU编译器套件，其初衷是为了给GNU编写一套开源的编译软件，包括了不至于C，还要C++，Java等其他软件的编译系统。

### Autoconf
Autoconf是一个用于生成shell脚本的工具，可以自动配置软件源代码以适应多种类似POSIX的系统。为了让你的软件包在所有的不同系统上都可以进行编译。类似于有的code代码在mac和window上面会有不同的生成文件这样子。

这里有一个教程，告诉我们怎么使用autoconf和automake生成conf文件。首先，需要os安装了以下binary：
```console
autoscan 

aclocal 
# 根据已经安装的宏，用户定义宏和acinclude.m4文件中的宏将configure.ac文件所需要的宏集中定义到文件 aclocal.m4中。aclocal是一个perl 脚本程序，它的定义是：“aclocal - create aclocal.m4 by scanning configure.ac”

automake 
# 将Makefile.am中定义的结构建立Makefile.in，然后configure脚本将生成的Makefile.in文件转换 为Makefile。如果在configure.ac中定义了一些特殊的宏，比如AC_PROG_LIBTOOL，它会调用libtoolize，否则它 会自己产生config.guess和config.sub

autoconf 
# 将configure.ac中的宏展开，生成configure脚本。这个过程可能要用到aclocal.m4中定义的宏。
```
可以直接type `autoscan`看看能不能找到这个cmd即可验证安装。以c++为例，我们有hello.cpp和hello.h文件：
```c++
/*hello.cpp*/
#include <iostream>
#include "hello.h"

using namespace std;

int main()
{
    CHello a;
    return 0;
}
```
```c++
/*hello.h*/
#ifndef __HELLO_H__
#define __HELLO_H__

#include<iostream>
using namespace std;

class CHello
{
public:
    CHello(){ cout<<"Hello!"<<endl;}
    ~CHello(){ cout<<"Bye!"<<endl;}
};

#endif
```
然后跑autoscan：
```console
autoscan
```
autoscan会生成`autoscan.log`和`configure.scan`这两个文件，他会检查代码的可移植问题，比如检查库，头文件，编译器等。要知道linux和window中有些标准库是不同的，这个时候可以用一些宏Marco来定义这些区别。

我们重命名`configure.scan`，并且打开`configure.ac`：
```console
mv configure.scan configure.ac
```
`configure.ac`中包含了一些发行的基本信息，前几行大概是这样
```
#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

AC_PREREQ([2.69])
AC_INIT([FULL-PACKAGE-NAME], [VERSION], [BUG-REPORT-ADDRESS])
AC_CONFIG_SRCDIR([hello.h])
AC_CONFIG_HEADERS([config.h])

# Checks for programs.
AC_PROG_CXX
AC_PROG_CC
# Checks for libraries.
# Checks for header files.
# Checks for typedefs, structures, and compiler characteristics.
# Checks for library functions.
AC_OUTPUT
```
包括`[FULL-PACKAGE-NAME], [VERSION], [BUG-REPORT-ADDRESS]`，我们将其替换为我们自己的信息：
```
#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.
AC_PREREQ([2.69])
AC_INIT(hello, 1.0, admin@qq.com)
AM_INIT_AUTOMAKE(hello, 1.0)
AC_CONFIG_SRCDIR([hello.h])
AC_CONFIG_HEADERS([config.h])

# Checks for programs.
AC_PROG_CXX
AC_PROG_CC
# Checks for libraries.
# Checks for header files.
# Checks for typedefs, structures, and compiler characteristics.
# Checks for library functions.
AC_OUTPUT(Makefile)
```
这里解释一下上面的Marco的意义：
* AC_PREREQ: 本文件要求的autoconf版本，由autoscan自己生成，不用修改
* AC_INIT： 定义这个packet的mata data，由packet name，版本和作者邮箱组成，修改成我们自己的信息
* AM_INIT_AUTOMAKE
* AC_CONFIG_SRCDIR： 侦测所指定的源码文件是否存在，来确定源码目录的有效性。
* AC_CONFIG_HEADER： 用于生成config.h文件，以便autoheader使用
* AC_PROG_CC： 指定编译器，默认gcc
* AC_OUTPUT： configure产生的文件，如果是makefile，configure会把它检查出来的结果带入makefile.in文件产生合适的makefile。使用Automake时，还需要一些其他的参数，这些额外的宏用aclocal工具产生

然后我们跑下面的指令：
```concole
aclocal
```








