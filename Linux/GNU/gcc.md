# GCC
GNU Compiler Collection，初衷为了给GNU编写一套开源的编译软件，包括C，还要C++，Java等其他软件的编译系统。关于C的编译问题，这里总结C从代码带可执行文件的过程，希望分到linux和window的不同区别。

查看gcc的include path
```
echo | gcc -E -Wp,-v -
```
在我的ubuntu16.04中，输出结果大概是下面的几个dir：
* `/usr/lib/gcc/x86_64-linux-gnu/5/include` & `/usr/lib/gcc/x86_64-linux-gnu/5/include-fixed`这里的5表示gcc-version5，这些library应该是安装gcc的时候，顺带给我安装的，表示跟gcc有关的lib，里面的lib都是不常见的。
* `/usr/include`：这应该是系统主要的c include path的位置，包括了这些文件夹：GL(OpenGL)，net，c++，nodejs，openssl等文件夹，有点像是系统的许多支持插件调用源文件的位置，同时还有很多文件，包括一些GNU C library的文件，如：`arrest.h`, `elf.h`, `netdb.h`, `pthread.h`, `printf.h`, `regex.h`, `signal.h`, `string.h`, `time.h`等。而c++对应的库其实就是对c的一种延伸，通过object的方式。c++的Standard library在这个路径下的c++文件夹中，里面包括：`array`, `cctype`, `exception`, `fstream`, `iostream`, `list`, `vector`, `string`。同时很多这些文件都reference了某一个对应的c的文件，例如`cctype` include了`ctype.h`，`signal`include了`signal.h`。
* `/usr/include/x86_64-linux-gnu`： 包含了另外一些，里面的文件夹有：`sys`, `bits`, `sys`, `unicode`。中间也有一些和`/usr/include`有一些重复。
* `/usr/local/include`: 我电脑里面没有东西，这个路径应该是user自己可以安装的c library的dependency。


## 文件组成
c语言在gcc的编译之后，会生产不同的后缀形式的代码，而每个后缀都有自己的含义
* .c文件：源代码文件，在c++中，通常是.C, .cc和.cxx这种形式的文件
* .h文件：程序包含的头文件
* .i文件：经过预处理之后的c文件，其代码格式依旧是c的格式，在这一个文档的编译过程这个项目中，会介绍预处理，将hello.c预处理变成hello_pre.c。但是对于编译器来说，通常是变成hello.i这个文件。而.ii通常是c++的预编译完成的文件。
* .s文件：汇编语言文件，.i文件经过处理产生，表示机器的指令集。
* .o文件：二进制对象文件。

## 编译过程

### 1. 写C代码
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

### 2. 预处理
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

### 3.编译
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

### 4.组装(汇编)
组装(汇编)过程将汇编语言(hello.s)作为输入，产生一个对象文件(hello.o)。使用命令：
```bash
gcc -c -o hello.o hello.s
```
hello.o是一个二进制文件，人类不可读。如果不指定输出对象名称会产生a.out的输出文件。现在通用的是ELF格式，据说是比out文件更加复杂的编码方式。

### 5.链接
函数调用的最后一个阶段。使用命令(即gcc本身)
```
$ gcc -o hello hello.o
```
在这之前，gcc不知道printf()函数的定义(implementation)，只使用占位符来进行函数调用。在link阶段，printf()函数的实际地址被插入。所有hello文件是hello.o链接了外部调用库，即printf()的implementation之后生成的的可执行文件，`hello`不需要放到特定位置，也不需要引用库，可以直接执行，执行cmd为：
```
$ ./hello
```

## 编译细节

### include path
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

## Marco
c/c++预编译需要处理的对象
* Marco和编译器独立，可以使用编译器使用的关键词，但是不能使用Marco自己的关键词，例如`#define`。
* Marco在惯例中都是使用大写来定义，当然小写也可以，只是看code会很难受。
* 预编译的过程是in sequence的，所以Marco仅仅在定义了之后才会生效，这也是为什么大部分Marco定义在`.h`中的原因

Marco大致分为Object-like Macros和Function-like Macros。

Object-like Macros是最简单的marco，在预编译的时候会被code替换，例如：`define BUFFER_SIZE 1024`，那么对于`foo = (char *) malloc (BUFFER_SIZE);`，在预编译的过程中，`BUFFER_SIZE`就已经被替换成1024，从编译器的角度看，这个`BUFFER_SIZE`并不存在，编译器只能看到1024。这种用法很常见，通常是用于指定函数的选项，例如在`sys/socket.h`中，函数`socket(domain, type, protocol)`使用的参数，都有对应的Marco：
```c
socket(domain, type, protocol)
// domain: AF_INET (IPv4 protocol) , AF_INET6 (IPv6 protocol)
// type: SOCK_STREAM (TCP)，SOCK_DGRAM (UDP)
```
这些marco大都对应某个integer value，直接填这个integer一样奏效。这种方式是code设计的一种方法。

Marco也可以用于定义数组，例如
```c
#define NUMBERS 1, \
                2, \
                3
int x[] = { NUMBERS };
// 等价于 int x[] = { 1, 2, 3 };
```
广义上，`#define`只会读取Marco定义的一行，并且在行尾结束。而使用`\`符号等于延长了这一行。上面的些法，在实践中大都用于定义error message。可以看出，预编译同样是完全等价替换，`NUMBERS`被替换成了`1, 2, 3`这样。

同时，Marco也可以被其他Marco定义。Marco在定义的时候会被替换，并且只有在使用的时候才会展开，如果ref了其他Marco，那么preprocessor会继续寻找，直到找到一个不是Marco的定义。下面给出了一些idea，当然in practive我们最好避免这种写法。
```c
#define TABLESIZE BUFSIZE
#define BUFSIZE 1024
// TABLESIZE  → BUFSIZE  → 1024

#undef BUFSIZE
#define BUFSIZE 37
// TABLESIZE will be 37
```
Function-like Macros，因为Marco的本质是替换，所以我们可以定义长得像函数的Marco来替换函数本身，直观上类似一种别名alias。例如`#define lang_init()  c_init()`，在调用的时候这两个等价。

### Stringizing
同样Marco也可以定义一段code，附带一些argument，例如：
```c
#define WARN_IF(EXP) \
do { if (EXP) \
        fprintf (stderr, "Warning: " #EXP "\n"); } \
while (0)

WARN_IF (x == 0);
/* 等价于
     do { if (x == 0)
           fprintf (stderr, "Warning: " "x == 0" "\n"); } while (0);	   
*/
```










