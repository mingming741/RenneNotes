# GNU
GNU是一个自由的操作系统，参与者在参与开发的时候，只要遵循其中的GPL协议做软件开发即可达到目的。目的是为了制造开源的环境，类似一个linux的开源天堂，允许任何创作者在遵循GPL开发规范的情况下，在其中加入新的内容。

### GPL & LGPL
GPL， GNU General Public License的缩写，即GNU通用公共授权。目的在于，试图保证你共享和修改自由软件的自由，保证自由的软件对于所有的用户来说是自由的。比方说，遵循GPL开发协议的软件，在release之后，开发者必须将自己的所有right共享，即需要将源代码公开。并且允许其他人在你的代码基础之上，进行修改，和重新发布。

LGPL， GNU Lesser General Public License，是旧版的GPL的更新的版本，修改了某些条目。同样需要保证软件的开源性，不能将别人的开源代码私有化，并且用作商业发布。

### GCC
GNU Compiler Collection，即GNU编译器套件，其初衷是为了给GNU编写一套开源的编译软件，包括了不至于C，还要C++，Java等其他软件的编译系统。

### Autoconf & Automake
Autoconf是一个用于生成shell脚本的工具，可以自动配置软件源代码以适应多种类似POSIX的系统。为了让你的软件包在所有的不同系统上都可以进行编译。类似于有的code代码在mac和window上面会有不同的生成文件这样子。

<img src = "https://github.com/mingming741/RenneNotes/blob/master/Resource/Image/automake.jpg"/>

这里有一个教程，告诉我们怎么使用autoconf和automake生成conf文件。首先，需要os安装了以下binary：
```console
autoscan 
aclocal 
automake 
autoconf 
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
`configure.ac`中包含了一些发行的基本信息，我们将其替换为我们自己的信息：
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
```console
aclocal
```
这个指令会生成aclocal.m4,aclocal会根据configure.ac中定义的Macro，辅助生成configure文件。然后我们跑autoconf：
```console
autoconf
```
生成`configure`文件，如果`AC_CONFIG_HEADER`在`configure.ac`中定义了，我们还需要autoheader生成configure.h.in
```console
autoheader
```
然后就是automake，我们自己创建一个Makefile.am的文件，在中间写:
```makefile
AUTOMAKE_OPTIONS=foreign 
bin_PROGRAMS=hello 
hello_SOURCES=hello.cpp hello.h
```
* AUTOMAKE_OPTIONS:设置Automake的选项。Automake提供了3种软件等级：foreign、gnu和gnits，供用户选择，默认等级为gnu。本例使需用foreign等级，它只检测必须的文件。
* bin_PROGRAMS:定义要产生的执行文件名。如果要产生多个执行文件，每个文件名用空格隔开。
* hello_SOURCES 定义”hello”这个执行程序所需要的原始文件。如果”hello”这个程序是由多个原始文件所产生的，则必须把它所用到的所有原始文件都列出来，并用空格隔开。例如：若目标体”hello”需要”hello.c”、”hello.h”两个依赖文件，则定义hello_SOURCES=hello.c hello.h。

这样我们就凑齐了生成Makefile的全部零件，使用automake来生成Makefile，参数`--add-missing`会自动安装缺少的文件
```console
automake --add-missing
```
完成之后，生成了Makefile.in，我们apply config，会生成Makefile
```
./configure
```
然后就可以make并且运行hello啦。

总结一下这个过程中所有的操作和文件：
* autoscan: 在project路径下，收取基本信息，用于构建configure.ac，这样用户只需要修改一点点信息，而不需要重写整个configure.ac
* configure.ac： autoconf需要的mata data，即用户设置的基本configure，之后autocof会根据平台，讲configure.ac的configure设置成不同的脚本。
* aclocal: aclocal.m4包含了一些autoconf需要的Marco，而aclocal就是根据configure.ac，将这些Marco配置好的程序
* autoheader: 从configure.ac中生成的帮助生成Makefile的文件。
* autoconf: 根据configure.ac和aclocal.m4的信息，生成configure。这个configure是对应了平台的configure，集成了project binary的工作环境。
* Makefile.am: 用户手写的，Makefile的mata data，告诉automake你需要make那些file
* automake: 根据Makefile.am生成Makefile.in，Makefile.in在对应的configure下，可以生成最终的Makefile

可以看到，automake和autoconf最后生成的是Makefile.in和configure文件，而他们对应的mata文件分别是Makefile.am和configure.ac，即我们只需要在这两个文件中修改参数，后面全部可以自动化。最后一步是通过configure文件，生成makefile。这样整个project就可以work了。


