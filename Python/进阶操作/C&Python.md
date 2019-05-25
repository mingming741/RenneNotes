# Cpython

python很多源都是C编译生成的，c在linux通过gcc编译了之后会生成.so的文件，而这个文件可以被python给import，这里给出例子，创建add.c
```c
//sample C file to add 2 numbers - int and floats
#include <stdio.h>

int add_int(int, int);
float add_float(float, float);

int add_int(int num1, int num2){
    return num1 + num2;
}

float add_float(float num1, float num2){
    return num1 + num2;
}
```
然后我们通过gcc来编译这个c文件，使用
```
#For Linux
$  gcc -shared -Wl,-soname,adder -o adder.so -fPIC add.c
```
会得到add.so，我们放在python同级的目录下，使用下面的code。即我们用python的code去调用到了c编译出来的动态链接库(.so)文件。这也使得python和C关联起来。
```python
from ctypes import *

adder = CDLL('./adder.so') #CDLL是ctypes里面的一个用于load动态链接库的文件
res_int = adder.add_int(4,5)
print(res_int) # 9

a = c_float(5.5) # c_float也是ctypes中的一个attribute
b = c_float(4.1)

add_float = adder.add_float
add_float.restype = c_float
print(add_float(a, b)) # 9.600000381469727
```
