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

### pointer & reference
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

下面总结一个各种指针引用搭配的组合table，我们可以认为在c中，所有变量名其实都是一个引用。

|  Target             | value (copy)    | reference (给了个新名字)    | pointer (另一个变量)         |   pointer_of_pointer (修改内存位置)|
|---|---|---|---|---|
| variable            | int a = 1;      |  int& ref_of_a  = a;      | int* pointer_of_a = &a;     |   |
| function parameter  | void func(int a)|  void func(int& ref_of_a) | void func(int* pointer_of_a)|  void func(int** pointer_of_pointer) |
| object              | Foo foo;        |  Foo& foo2 = foo;         | Foo* foo3 = &foo; |

这个table也揭露了等号`=`的本质，等号左右两边必须有相同的type。`int a = 1`，a自己就是对一块内存的引用，表示"这个引用所引用的内存位置的值是1"。 `int& ref_of_a  = a;`，对引用进行copy赋值。变量ref_of_a和a引用了内存相同的位置，本质是是相同变量。`int* pointer_of_a = &a;`，pointer_of_a是一个指针变量，表示内存中的一块位置中存储了引用a的地址。这也表示了指针和引用的本质区别，引用就是变量本身，或者变量的另一个名字，而指针本质是是另外一个变量，只是指针的值存储的是这个变量的地址，地址其实就是引用要引用的那一块内存。

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


### class
c++的class的实现方式很类似一个class的namespace，下面给出这个例子：
```c++
#include <iostream>
#include <string>
using namespace std;

class Car {        // The class
  public:          // Access specifier
    string brand;  // Attribute
    string model;  // Attribute
    int year;      // Attribute
    Car(string x, string y, int z); // 而这个constructor其实是个返回Car object的函数

    Car(){ // 这是默认的constructor，用申明变量的方式构造
      this->brand = "undefined";
    }
};

// Constructor definition outside the class
Car::Car(string x, string y, int z) {
  this->brand = x;
  this->model = y;
  this->year = z;
}

int main() {
  Car carObj1;
  cout << carObj1.brand << endl;

  Car carObj2("Ford", "Mustang", 1969);
  carObj2.year = 2007;
  cout << carObj2.brand << " " << carObj2.model << " " << carObj2.year << "\n";
  return 0;
}
```
这里要注意的是，class的内部等同于class自己的namespace。只要在`Car`的namespace中，修改变量使用`this->var`。而在class生成了object之后，修改变量使用`carObj1.brand`。

关于`this`，`this` is a pointer to the instance of the class。给下面的例子：
```c++
#include <iostream>

class Foo{
    public:
        Foo(){
            this->value = 0;
        }
        Foo get_copy(){ // 这个函数会构建一个object Foo，通过this这个指针的值来构建，因此调用应该是foo2 = foo.get_copy();
            return *this;
        }
        Foo& get_copy_as_reference(){ //返回一个object的引用，
            return *this;
        }
        Foo* get_pointer(){ //返回对应对象的指针
            return this;
        }
        void increment(){
            this->value++;
        }
        void print_value(){
            std::cout << this->value << std::endl;
        }
    private:
        int value;
};

int main()
{
    Foo foo;
    foo.increment();
    foo.print_value(); // 1

    Foo foo1 = foo.get_copy();
    foo1.increment();
    foo.print_value(); // 1

    Foo& foo2 = foo.get_copy_as_reference();
    //注意下面的写法，Foo foo2同样可以通过编译，但是此时foo2不再是引用，而是一个new object，这个写法和copy结果一样
    //Foo foo2 = foo.get_copy_as_reference();
    foo2.increment();
    foo.print_value(); // 2

    Foo* foo_pointer = foo.get_pointer();
    foo_pointer->increment();
    foo.print_value(); // 3
    foo2.print_value(); // 3, foo2和foo是相同的object

    return 0;
}

```
在c中，对象指针的引用使用`pointer->attribute`，而创建的object的对象`foo`其实也是自己本身的引用，因此引用的调用大部分是`ref.attribute`。如果说`this`是指向class创建的对象的指针，那么`*this`其实就是这个对象的引用了。

总结一下，所谓的指针，引用和对象的关系。
* 指针(pointer)，即存储地址的变量，对于class同样奏效，定义方式为`Foo* foo_pointer`，首位地址为`*foo_pointer`，其实就是这个对象的引用，通过指针access object或者是class的attribute使用`foo_pointer->attribute`和`foo_pointer->function`
* 引用(reference)，即一种重命名，在申明变量`Foo foo`的时候，`foo`可以看做object本身，也可用看做自己对自己的引用。如果要创建新的引用，使用`Foo& = foo`。如果引用指向了同一个instance，那么这两个引用变量其实完全等价。












