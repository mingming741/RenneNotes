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
# 扫描源代码以搜寻普通的可移植性问题，比如检查编译器，库，头文件等，生成文件configure.scan,它是configure.ac的一个雏形。

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
$ autoscan
autoscan
```


