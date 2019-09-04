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
