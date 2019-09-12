# GNU
GNU是一个自由的操作系统，目的是为了制造开源的环境，允许任何创作者在遵循GPL开发规范的情况下，在其中加入新的内容。

## GPL & LGPL
GPL(GNU General Public License)，即GNU通用公共授权。目的在于，保证你共享和修改自由软件的自由。遵循GPL开发协议的软件，在release之后，开发者必须将自己的所有right共享，将源代码公开，允许其他人在你的代码基础之上，进行修改和重新发布。

LGPL(GNU Lesser General Public License)，是旧版的GPL的更新的版本，修改了某些条目。同样需要保证软件的开源性，不能将别人的开源代码私有化，并且用作商业发布。

## GCC
GNU Compiler Collection，初衷为了给GNU编写一套开源的编译软件，包括C，还要C++，Java等其他软件的编译系统。关于C的编译问题，这里总结C从代码带可执行文件的过程，希望分到linux和window的不同区别。

### 文件组成
c语言在gcc的编译之后，会生产不同的后缀形式的代码，而每个后缀都有自己的含义
* .c文件：源代码文件，在c++中，通常是.C, .cc和.cxx这种形式的文件
* .h文件：程序包含的头文件
* .i文件：经过预处理之后的c文件，其代码格式依旧是c的格式，在这一个文档的编译过程这个项目中，会介绍预处理，将hello.c预处理变成hello_pre.c。但是对于编译器来说，通常是变成hello.i这个文件。而.ii通常是c++的预编译完成的文件。
* .s文件：汇编语言文件，.i文件经过处理产生，表示机器的指令集。
* .o文件：二进制对象文件。

### 编译过程

#### 1. 写C代码
用下面的代码作为例子，hello.c
```c
#include <stdio.h>

# define SENTENCE "hello world!\n"
int main()
{
  printf(SENTENCE);
  return 0;
}
```

#### 2. 预处理
生成.i的预处理文件，gcc中使用下面的代码，会以下面的顺序执行：
```
gcc -E -o hello.i hello.c
```
这个命令会处理所有的条件编译指令，包括带#的预处理的逻辑语句，例如#ifdef #ifndef #endif等。预处理会决定编译方向，比如说程序的可执行文件是linux版还是window版，是32位还是64位等，这些需要#ifdef这类逻辑语句，配合Marco的得以完成。下面的例子中，就通过`#ifdef _WIN32`决定是否要`#include <windows.h>`
```c
#ifdef _WIN32
#include <windows.h>

#ifndef AVUTIL_CPU_H
#define AVUTIL_CPU_H
```

c的`#define`是定义了宏(Marco)，是一种批处理的称谓。宏可以是一系列命令的总结，之后只需要执行宏，即可完成批量操作。预处理会将所有#define删除，并用对应的宏(Marco)替换。在C中，Marco可以代表一个文件，也可以代表静态全局变量，在整个project中被使用。常用的写法有：（来自ffmpeg的源代码）
```c
#define VSYNC_AUTO       -1
#define DEFAULT_PASS_LOGFILENAME_PREFIX "ffmpeg2pass"

#define MATCH_PER_STREAM_OPT(name, type, outvar, fmtctx, st)\
{\
    // do something
}
```

将#include文件暴力插入对应位置。这里也揭露了include的本质，因此，include的文件是.c还是.h并不重要，include可以插入任何文件。然后删除所有注释，给code添加行号和文件标示，这些attribute可以通过__LINE__和__FILE__这些静态值获取到，辅助编译器报错。最后保留#pragma编译器指令，编译器需要它们（暂时不懂为什么）。

我们可以只执行预编译过程，产生hello.i的文件，Marco中SENTENCE会被替换成"hello world!\n"。同时我们search hello.i中的printf文件，可以找到以下定义：
```c
extern int printf (const char *__restrict __format, ...);
```
这就是头文件#include <stdio.h>中对printf的定义，extern表示这是一个外部的链接，在link时，会去标准库找对应的动态链接库，这个库是一个implement了printf.c文件编译出来的lib。这里也说明，动态链接库的header需要被调用者#include和重新编译，但是implementation并不需要，而是早就编译好并且放在系统的lib路径下，交给linker去找。

#### 3.编译
编译将预处理的hello.i作为输入（也可以是hello.c），生成hello.s，输出是汇编级指令。
```
gcc -S -o hello.s hello.i
```
下面是hello.s的部分内容，汇编语言阅读难度较大，依旧human readable，操作类似于对register层面操作。
```
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
```
编译的过程可以理解为，编译器对c style的语言(.c & .i)进行了词法语法语义分析，优化后产生了汇编语言。

#### 4.组装(汇编)
组装(汇编)过程将汇编语言(hello.s)作为输入，产生一个对象文件(hello.o)。使用命令：
```bash
gcc -c -o hello.o hello.s
```
hello.o是一个二进制文件，人类不可读。如果不指定输出对象名称会产生a.out的输出文件。现在通用的是ELF格式，据说是比out文件更加复杂的编码方式。

#### 5.链接
函数调用的最后一个阶段。使用命令(即gcc本身)
```
$ gcc -o hello hello.o
```
在这之前，gcc不知道printf()函数的定义(implementation)，只使用占位符来进行函数调用。在link阶段，printf()函数的实际地址被插入。所有hello文件是hello.o链接了外部调用库，即printf()的implementation之后生成的的可执行文件，`hello`不需要放到特定位置，也不需要引用库，可以直接执行，执行cmd为：
```
$ ./hello
```

### 编译细节

#### include path
当我们调用#include的时候，编译器会试图寻找#include文件的在系统的位置。即在#include的前面添加了某个绝对路径，有下面两种写法
```c
#include "sys/socket.h"
#include <sys/socket.h>
```
写法在寻找这个头文件的顺序上有少许不同，打引号的`#include "sys/socket.h"`，绝对路径的寻找顺序为：
1. 当前路径
2. In the directories of the currently opened include files, in the reverse order in which they were opened. The search begins in the directory of the parent include file and continues upward through the directories of any grandparent include files.（没看懂...）
3. 来自compiler的option /I
4. 来自INCLUDE的环境变量

对于打括号的格式`#include <sys/socket.h>`，顺序为：
1. 来自compiler的option /I
2. 来自INCLUDE的环境变量

这里unix类的系统和windows的区别在于INCLUDE的环境变量寻找方法有一点不同。linux环境变量为C_INCLUDE_PATH和CPLUS_INCLUDE_PATH，默认通常是： "/usr/local/include", "libdir/gcc/target/version/include", "/usr/target/include", "/usr/include"。使用下面的命令查看gcc查找lib的路径：
```
gcc -print-search-dirs
```
编译器会在按顺序查找，找到一个同名文件会立即停止。

### GCC configure
gcc相比kernal来说，是独立的program。因此gcc可以有自己的环境变量为`C_INCLUDE_PATH`和`CPLUS_INCLUDE_PATH`，不一定要follow kernal的configure，如`LD_LIBRARY_PATH`
