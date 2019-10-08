# Functional Programming (FP)
c++提供的一种编程方式。和python/JS类似的，将函数作为变量传入其他的函数，或者一个函数返回另一个函数这种。函数可以被data structure存储，或是被赋予一个变量名(类似python的函数object)这样。

FP只被g++11以上的版本支持，因此如果要使用FP，使用下面编译：
```
g++ -std=c++11 FP.cpp
```
参考https://www.codeproject.com/Articles/1267996/Functional-Programming-in-Cplusplus


### lambdas
lambdas即定义函数变量的方式，最基本的结构是`[] () {}`。其中`[]`表示introducer，`()`表示函数的参数，`{}`表示函数的内容，下面列举几个lambda的结构：
```c++
[] () {} // lambdas基本结构，不能写在程序主体中，否则报错
auto a = [] () {}; // a表示一个函数的变量
a(); // 在定义之后调用函数a
auto c = [] (int id, string name) {cout << id << " " << name << endl;}; //有参数有action的函数变量
c(7, "Renne"); //调用方法
[] () {cout << "auto call" << endl;}(); // 在申明函数变量之后，立即call这个函数变量。
double pi = 3.1416;
auto func = [pi] () {std::cout << "The value of pi is "<< pi << std::endl;}; // introducer调用外部变量
```
这里的`auto`类似与JS的`var`，即编译器帮我们自动选择类型，下面是代码的例子。
```c++
#include <iostream>
#include <string>
using namespace std;

int main(){
  [] () {};
  auto b = [] () {cout << "Hello from C++ Lambda!" << endl;};
  b(); // Hello from C++ Lambda!
  auto c = [] (int id, string name) {cout << id << " " << name << endl;};
  c(7, "Renne"); // 7 Renne
  [] () {cout << "auto call" << endl;}(); // auto call
  double pi = 3.1416;
  auto func = [pi] () {std::cout << "The value of pi is "<< pi << std::endl;};
  return 0;
}
```
既然函数作为变量，那么变量就可以赋值给其他变量，下面是函数变量赋值的例子。要注意这里的auto用在lambdas的parameter中，可以自动cast`A`和`B`的类型，但是这个操作只在g++14以及以上才支持。
```c++
auto sum = [] (auto A, auto B) { return A + B; };
auto add = sum;
std::cout << add(3.25, 5.65) <<std::endl;
std::cout << add(3, 5) <<std::endl;
```

### introducer
基本的introducer就是如上面的介绍：
```c++
double pi = 3.1416;
auto func = [pi] () {std::cout << "The value of pi is "<< pi << std::endl;}; 
```
写在`[]`中，类似与预装的参数，而`[]`的操作被叫做capturing，默认的capturing是pass by value的，我们也可以决定capturing的是什么，例如：
```c++
[] // nothing
[&] // all by reference
[=] // all by value
[&A, =B] //	captures A by reference, B by value
[=, &A] // captures A by reference, everything else by value
```

### Polymorphic Function Wrapper 
define在`std::function`中，是c++编译器给我们的一个封装了lambda操作的API，方便用户定义函数，基本定义为：
```c++
std::function<return_value(args)> function_name
```
上面的定义取代了前面使用的`auto`关键词，使得这个函数的定义更加明确，不会动态到处转化，例如下面的例子，就参数就一定是`double`类型而不会变成`int`
```c++
std::function<double(double, double)> sum = [](double A, double B) { return A + B; };
std::cout << sum(4.6, 5.9) << std::endl;
```


