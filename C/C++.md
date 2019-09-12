# C++
跨平台，高性能，涉及到内存操作的编程语言，c++也可用用gcc来编译，使用：
```
gcc FILENAME.cpp -o FILENAME -lstdc++
```

### tips
1. `&v`表示变量v的地址，如果v是kernal space中的变量，这个会seg fault
2. `int* p; p++;`，p在内存中会移动一个int的大小，即4位


### type
和c一样，c++的最基本的类型还是只有：
```c++
int    // 4 bytes	
float  // 4 bytes	
double // 8 bytes	
bool   // 1 byte	
char   // 1 byte
```

### pointer
A pointer is a variable whose value is the address of another variable。这里记录c(不是c++)的pointer的一些常用方法。

1. 指针赋值 + 内存赋值
```c
char a[10];
printf("%d\n",a); // 得到一个32位的地址，即a指向的位置，这个位置下有char a[10]的初始值
printf("%d\n",*a); // 得到这个地址下第0位的value，在没有赋值的情况下为0
```
a是一个指针变量，并且在申明这个变量的时候，已经指向了某个位置，并且对这个位置分配了10个char大小的内存，这个内存被分配在stack中。

2. 仅仅指针赋值
```c
char* a;
printf("%d\n",a); // 得到一个32位的地址，即a指向的位置，这个位置通常无效
printf("%d\n",*a); // segmentation falut，因为引用了未分配的地址。
```
a是一个指针变量，但是a仅仅是被申明，而没有被赋值。如要使用这个地址，使用malloc，这时候`a[10]`的值在heap中。

### function parameter
这里记录c的传参

1. copy
```c++
#include <iostream>
#include <cstdlib>
using namespace std;

void func(int value){
  vlaue = 2;
}

int main(){
  int b = 1;
  func(b);
  cout << b << "\n";
}
```
复制传参，即b在func调用的时候，值被复制给了func中的a，func修改a也不会影响外面的b，因为是不同namespace。

2. 指针传参
```c++
#include <iostream>
#include <cstdlib>
using namespace std;

void func(int* pointer){
  *pointer = 2; // *a表示对指针对应的位置赋值
}

int main(){
  int value = 1;
  func(&value); // &b表示b的地址
  cout << value << "\n"; // 2
}
```
上面的例子指针传参，可以修改到b的值。

3. 指针的地址
```c++
#include <iostream>
#include <cstdlib>
using namespace std;

void func(int ** addressOFpointer){
  int* anotherpointer = (int*) malloc(1*sizeof(int)); //新的指针分配内存
  *anotherpointer = 2; //给刚刚分配的内存赋值
  *addressOFpointer = anotherpointer; // 将传入的pointer的地址修改成刚刚新分配的内存地址
}
ma
int main(){
  int* pointer = (int*) malloc(1*sizeof(int));
  *pointer = 1;
  func(&pointer); // &pointer表示pointer的地址
  cout << *pointer << "\n"; // 2
}
```
上面的例子，pointer的地址被传入函数，然后这个地址被修改成了另外一个地址。这个方法比起2，除了可以修改数组本身，还可以修改数组在内存中的位置，因为是重新malloc了一个内存，给这个地址，然后传出来。

### namespace
namespace(命名空间)，定义了一系列variable和function的作用域。通过namespace，可以在同一个文件中使用相同的变量名或函数名，只要它们属于不同的namespace即可。namespance可以自己定义，平时的一个函数和一个类也会自动定义namespace。类似于文件路径，这里是变量的函数的路径。
```c++
#include <iostream>

namespace plane { // 名空间的定义
  int model;
  namespace size // 名空间的嵌套
  {
    int length;
    int width;
  }
}

namespace plane {// 添加名空间的成员
  char * name;
}

int Time; // 外部变量属于全局名空间

int main()
{
  plane::size::length=70;
  int Time = 1996; // 临时变量，应区别于全局变量
  ::Time = 1997; // 对应全局变量定义的int Time
  std::cout << "Time main() is " << Time <<", Time in global is "<< ::Time << std::endl; // 这个Time是全局空间的Time

  using namespace plane; // 使用关键字using
  model = 202;
  size::length = 93;
  std::cout << "Time main() is " << model << " " << size::length << std::endl;

  return 0;
}
```
对于常用的`cout`和`endl`，其实都是`iostream`下的namespace `std`中的，正确的full path应该是`std::cout`和`std::endl`














