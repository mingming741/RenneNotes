# gcc 
关于C语言的编译问题，大概总结一些C从代码带可执行代码的过程，希望分到linux和window的不同区别。

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

* 将#include的文件，直接插入改行的位置。所以说在一个文件执行的时候，会暴力插入其#include的其他文件，让其先编译。反过来说，include是为了能让一个文件不至于过大，和某些通用函数能重复使用的定义规则。

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

## Makefile
Makefile(or makefile)最基本的定义是，告诉binary executable 'make'所指定的的命令。并不局限于c的编译，make指定的命令可以是任何的cmd或者是调用其他binary executable，有点类似带参数的sh执行方式。

要使用Makefile，则在对应的目录下创建Makefile文件，这样在这个目录下调用make的时候，make会检查Makefile中的option的entry，来实现不同的调用。Makefile最基本的entry格式为：
```makefile
makecmd:
	action

# 例如
say_hello:
	echo "Hello World"
```
类似于‘函数名’ + 空格TAB + 功能，上面的例子就是，输入`make`或者`make say_hello`都会输出"Hello World"。更加通用的格式是：
```makefile
target: prerequisites # 先决条件，即会先运行prerequisites
<TAB> recipe

# 或者是
final_target: sub_target final_target.c
        Recipe_to_create_final_target

sub_target: sub_target.c
        Recipe_to_create_sub_target
```
我们举几个例子看看上面的格式的使用，如果我们不想在make的时候，同时print出来make的cmd，则在前面加@。同时我们增加两个function为generate和clean。
```makefile
say_hello:
	@echo "Hello World"
	
generate:
	@echo "Creating empty text files..."
	touch file-{1..10}.txt

clean:
	@echo "Cleaning up..."
	rm *.txt
```
这时候我们`make`的话，只有`say_hello`会被执行，因为第一个function是`make`的default option。因为make本身也是binary executable，所以自己也有一些config的API，例如`.DEFAULT_GOAL := generate`可以讲`make`默认的default option修改成function`generate`。makefile有许多选项可以设置：
```makefile
.DEFAULT_GOAL := func_name # 设置默认执行的命令
.PHONY: func_name # 强制执行，即会刷新原本的，如gcc命令会重新编译
```

同时，makefile最常用的是`all`的选项，用于决定`make`的default执行中间的哪些function。需要注意的是，`all`并不是特殊的名称，你也可以将其换成`2333`，而是仅仅需要放在makefile的第一个位置，代表默认执行即可：
```makefile
all: say_hello generate

say_hello:
	@echo "Hello World"
	
generate:
	@echo "Creating empty text files..."
	touch file-{1..10}.txt

clean:
	@echo "Cleaning up..."
	rm *.txt
```

### Makefile Variable
上面介绍的都是静态的，并且没有target的make的操作。make也可用使用变量。变量在被cmd赋值之后，用`${var}`表示，例如：
```makefile
CC = gcc

hello: hello.c
	${CC} hello.c -o hello
```
这里的`${CC}`等价于`gcc`，或者`$(CC)`的小括号形式也可以。在makefile中，`=`和`:=`有微小的区别，例如：
```makefile
CC = gcc
CC = ${CC}

all:
	@echo ${CC}
```
会报错，line 2会递归的reference自己，使用`:=`可以解决这个问题。这里`:=`又被叫做simply expanded variable。

下面介绍一个更加全的例子，#开头的表示comment
```makefile
# Usage:
# make        # compile all binary
# make clean  # remove ALL binaries and objects

.PHONY = all clean  # 强制重新执行的cmd

CC = gcc                        # compiler to use
LINKERFLAG = -lm  		# defines flags to be used with gcc in a recipe

SRCS := $(wildcard *.c) 	
# $(wildcard pattern)是一个filename的函数，all files with the .c extension will be stored in SRCS， wildcard的中文意思是通配符

BINS := $(SRCS:%.c=%)		
# substitution reference. if SRCS has values 'foo.c bar.c', BINS will have 'foo bar'，类似于冲SRCS中取出来了%代表的部分

all: ${BINS}
# 即指定全部的指令，这里${BINS}类似一个数组，包含'foo bar'这些名称，即通用名称

%: %.o
	@echo "Checking.."
	${CC} ${LINKERFLAG} $< -o $@

%.o: %.c
	@echo "Creating object.."
  ${CC} -c $<

clean:
	@echo "Cleaning up..."
	rm -rvf *.o ${BINS}
```
前半部分有对于的comment解释，后半部分对应不同的Rules，下面会介绍这些rules，例如：
```makefile
%: %.o
	@echo "Checking.."
	${CC} ${LINKERFLAG} $< -o $@
```
表示一个rule，即从object到binary executable的过程。在具体执行过程中，变量会被替换，以`foo`为例子，可以写作：
```makefile
foo: foo.o
  @echo "Checking.."
  gcc -lm foo.o -o foo
```
可以看到，这里`${CC}`和`${LINKERFLAG}`作为link的操作参数被传进去。`%`用来match输入的参数，类似于input的一个位置。而`$<`是match prerequisites的patterned，`$@`是 matches target的patterned。这里的logic是，首先根据输入，target(func_name)和prerequisites(:后面的参数类似)先被赋值，然后继续向下传递，类似函数的传参。

通过理解了.o到binary的规则，也不难理解.c到.o的规则了。如果我们使用`make`，则默认会make all，加入这个folder下包含了`foo.c`和`bar.c`两个文件的话，就等价于`all: foo bar`，即会执行`foo`和`bar`这两个make。

对于任意的make，Makefile中的Rules会进行match，例如`foo`，会被match到`foo`这个function中，因为foo有prerequisites(即dependence)的`foo.o`，这样又会跳转到`foo.o`这边去，先将c文件转化成object，然后在执行`foo`。

所以如果只有一个file `foo.c`的话，上面的script等价于：
```makefile
# Usage:
# make        # compile all binary
# make clean  # remove ALL binaries and objects

.PHONY = all clean

CC = gcc                        # compiler to use

LINKERFLAG = -lm

SRCS := foo.c
BINS := foo

all: foo

foo: foo.o
	@echo "Checking.."
	gcc -lm foo.o -o foo

foo.o: foo.c
	@echo "Creating object.."
	gcc -c foo.c

clean:
	@echo "Cleaning up..."
	rm -rvf foo.o foo
```
到这里，我们可以认为target在function的output是一个文件的情况下，会生成对应的文件，prerequisites则是执行这个function需要的文件，如果不存在的话，就去尝试生成这个文件。

需要注意的是，make这个command在linux的kernal中似乎本来就有定义，因此有的路径下即使没有makefile，`make hello.o`和`make hello`依旧可以执行（我本地测试出现了这种情况，并且自己写的Rule无法替换default的rule）。目前还不知道怎么解决



