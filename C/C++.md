# C++
跨平台，高性能，涉及到内存操作的编程语言，c++也可用用gcc来编译，使用：
```
gcc FILENAME.cpp -o FILENAME -lstdc++
```

#### namespace
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














