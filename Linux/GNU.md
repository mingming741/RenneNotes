# GNU
GNU是一个自由的操作系统，参与者在参与开发的时候，只要遵循其中的GPL协议做软件开发即可达到目的。目的是为了制造开源的环境，类似一个linux的开源天堂，允许任何创作者在遵循GPL开发规范的情况下，在其中加入新的内容。

## GPL & LGPL
GPL， GNU General Public License的缩写，即GNU通用公共授权。目的在于，试图保证你共享和修改自由软件的自由，保证自由的软件对于所有的用户来说是自由的。比方说，遵循GPL开发协议的软件，在release之后，开发者必须将自己的所有right共享，即需要将源代码公开。并且允许其他人在你的代码基础之上，进行修改，和重新发布。

LGPL， GNU Lesser General Public License，是旧版的GPL的更新的版本，修改了某些条目。同样需要保证软件的开源性，不能将别人的开源代码私有化，并且用作商业发布。

# GCC
GNU Compiler Collection，即GNU编译器套件，其初衷是为了给GNU编写一套开源的编译软件，包括了不至于C，还要C++，Java等其他软件的编译系统。关于C语言的编译问题，大概总结一些C从代码带可执行代码的过程，希望分到linux和window的不同区别。

## 文件组成
c语言在gcc的编译之后，会生产不同的后缀形式的代码，而每个后缀都有自己的含义
* .c文件：源代码文件，在c++中，通常是.C, .cc和.cxx这种形式的文件
* .h文件：程序包含的头文件
* .i文件：经过预处理之后的c文件，其代码格式依旧是c的格式，在这一个文档的编译过程这个项目中，会介绍预处理，将hello.c预处理变成hello_pre.c。但是对于编译器来说，通常是变成hello.i这个文件。而.ii通常是c++的预编译完成的文件。
* .a文件：
* .o文件：二进制对象文件。
* .s文件：汇编语言文件，.i文件经过处理产生，表示机器的指令集。

## 编译过程

#### 1. 获取代码
说白了，就是完成你的代码，用下面的代码作为例子，hello.c
```c
#include <stdio.h>

# define SENTENCE "hello world!\n"
int main()
{
  printf(SENTENCE);
  return 0;
}
```

### 2. 预处理
预处理，即生成.i的预处理文件，gcc中使用下面的代码：
```
$ gcc -E -o hello.i hello.c
```
* 处理所有的条件编译指令，#ifdef #ifndef #endif等，就是带#的那些，这些语句相当于预处理的逻辑语句，而普通的if else是代码执行的逻辑语句。预处理在编译过程已经执行，并且会决定编译的方向，比如说编译完成之后，程序的可执行文件是linux版本还是window版本，是32为系统还是64为系统等等，这些的实现都需要#ifdef这类的逻辑语句，配合一些Marco的支持得以完成。
```cgcc
#ifdef _WIN32
#include <windows.h>

#ifndef AVUTIL_CPU_H
#define AVUTIL_CPU_H
```

* 将所有的#define删除，并且#define定义的宏(Marco)进行替换，而宏(Marco)表示一种批处理的称谓，或者叫语法替换。比如说，可以是用一个命令代表一系列命令的统称，定义这个命令的Marco为这一系列命令，之后只需要执行这一个命令即可完成批量操作。而在C中，Marco可以代表一个文件，也可以代表一下静态的全局变量，在整个project中被使用。常用的写法有：（来自ffmpeg的源代码）
```c
#define VSYNC_AUTO       -1
#define DEFAULT_PASS_LOGFILENAME_PREFIX "ffmpeg2pass"

#define MATCH_PER_STREAM_OPT(name, type, outvar, fmtctx, st)\
{\
    int i, ret;\
    for (i = 0; i < o->nb_ ## name; i++) {\
        char *spec = o->name[i].specifier;\
        if ((ret = check_stream_specifier(fmtctx, st, spec)) > 0)\
            outvar = o->name[i].u.type;\
        else if (ret < 0)\
            exit_program(1);\
    }\
}
```

* 将#include的文件，直接插入改行的位置。所以说在一个文件执行的时候，会暴力插入其#include的其他文件，让其先编译。反过来说，include是为了能让一个文件不至于过大，和某些通用函数能重复使用的定义规则。同时，这里也揭露了include的本质，即将另一个文件插入到include的位置，因此，include的文件是.c还是.h其实并不重要，include可以插入任何的文件。

* 删除所有注释。

* 给code添加行号和文件标示，这些attribute可以通过__LINE__和__FILE__这些静态值获取到。并且这些标示会在编译出错的时候，给编译器报告出来。

* 保留#pragma编译器指令，因为编译器需要使用它们（暂时不懂为什么）。

我们可以通过gcc只执行预编译过程，执行知乎生产hello.i的文件，Marco中SENTENCE会被替换成"hello world!\n"。同时我们search hello.i中的printf文件，可以找到以下定义：
```c
extern int printf (const char *__restrict __format, ...);
```
这就是头文件#include <stdio.h>中对printf的定义，extern表示这是一个外部的链接，之后在链接的时候，会找到执行这个头文件的动态链接库，即一个implement了printf的.c文件编译出来的可执行文件。下面就是生成的hello.i的最后几行
```c
# 5 "hello.c"
int main()
{
  printf("hello world!\n");
  return 0;
}
```

### 3.编译
编译，即将高级语言转化成机器语言，将预处理的hello.i作为输入（输入也可以是hello.c），生成hello.s，而中的输出是汇编级指令。
```
$ gcc -S -o hello.s hello.i
```
下面是hello.s的内容，可以发现，汇编语言依旧human readable，虽然阅读难度较大。操作类似于对register层面上的操作。
```
	.file	"hello.c"
	.section	.rodata
.LC0:
	.string	"hello world!"
	.text
	.globl	main
	.type	main, @function
main:
.LFB2:
	.cfi_startproc
	pushq	%rbp
	.cfi_def_cfa_offset 16
	.cfi_offset 6, -16
	movq	%rsp, %rbp
	.cfi_def_cfa_register 6
	movl	$.LC0, %edi
	call	puts
	movl	$0, %eax
	popq	%rbp
	.cfi_def_cfa 7, 8
	ret
	.cfi_endproc
.LFE2:
	.size	main, .-main
	.ident	"GCC: (Ubuntu 5.4.0-6ubuntu1~16.04.11) 5.4.0 20160609"
	.section	.note.GNU-stack,"",@progbits

```
编译的过程可以理解为，编译器对c style的语言(.c & .i)进行了词法语法语义分析，在优化之后，产生了机器语言汇编。

### 4.组装(汇编)
将汇编语言的作为输入，产生一个对象文件的过程叫组装，或者叫汇编。这一步将hello.s的汇编文件作为输入，产生了hello.o的对象文件，使用命令：
```bash
$ gcc -c -o hello.o hello.s
```
会生成机器语言文件hello.o。这是一个二进制文件，通常打开会直接乱码。如果不指定输出对象名称的话，会产生a.out的输出文件，而现在通用的是ELF格式，据说是比out文件更加复杂的编码方式。

### 5.链接
函数调用的最后一个阶段。使用命令(即gcc本身)
```
$ gcc -o hello hello.o
```
直到现在，gcc并不知道printf（）函数的定义。在编译器确切地知道所有这些函数的实现之前，它只是使用占位符来进行函数调用。在这个阶段，printf（）的定义被解析，并且printf（）函数的实际地址被插入。链接器在此阶段开始执行此任务。所有可以理解hello文件是hello.o链接了外部调用库的可执行结果文件。这个文件不需要放到某个特定的位置，也不需要引用某些库，直接就可以执行，（大概是标准了全局引用library的位置）执行cmd为：
```
$ ./hello
```

## 编译细节

### include path
当我们调用#include的时候，编译器会试图寻找include的这个文件的路径。可以理解为在include的前面添加了某个绝对路径（因为include允许子路径的存在）。对于include来说，可以有下面两种写法
```c
#include "sys/socket.h"
#include <sys/socket.h>
```
写法在寻找这个头文件的顺序上有少许不同，对于window和unix系统，也有少许出入。对于windows，打引号的`#include "sys/socket.h"`，绝对路径的寻找顺序为：
1. 当前路径，即调用#include文件现在的directory （In the same directory as the file that contains the #include statement.）
2. In the directories of the currently opened include files, in the reverse order in which they were opened. The search begins in the directory of the parent include file and continues upward through the directories of any grandparent include files.（没看懂...）
3. 来自compiler的option /I
4. 来自INCLUDE的环境变量

对于打括号的格式`#include <sys/socket.h>`，顺序为：
1. 来自compiler的option /I
2. 来自INCLUDE的环境变量

对于unix类的系统，基本和windows类似，只是最后一步的环境变量会替换为下面几个系统路径中寻找： "/usr/local/include", "libdir/gcc/target/version/include", "/usr/target/include", "/usr/include"。用户也可以通过在环境变量C_INCLUDE_PATH和CPLUS_INCLUDE_PATH中添加路径，给默认的lib路径中添加新的item

编译器会在按照顺序查找，在找到一个符合的就会立即停止，多的不会覆盖。使用下面的命令查看gcc查找lib的路径：
```
gcc -print-search-dirs
```
因为gcc相比kernal来说，是独立的program，因此gcc可以有自己的configure，不一定要follow kernal的configure，如`LD_LIBRARY_PATH`



# Autoconf & Automake
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


