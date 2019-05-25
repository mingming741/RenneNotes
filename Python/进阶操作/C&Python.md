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


### Python/C API
目前最常用的API了，要求c按照python的实现方法来写，全部的要求都写在Python.h里面。例如，我们要通过c的方式，实现一个python的module，下面的module作为例子
```python
import addList

l = [1,2,3,4,5]
print(addList.add(l)) # 15
```
这个module名为addList，其中有一个函数名为add，下面给出在c中，这个module的实现方式
```c
#include <Python.h> //Python.h中包含了预先写好的C和python的一些接口类

// AddList:将list中所有元素的加起来，返回他们的和
static PyObject* addList_add(PyObject* self, PyObject* args){ //这是python直接call的文件
    PyObject * listObj;
    if (! PyArg_ParseTuple( args, "O", &listObj)) //输入需要能转化成python的tuple
        return NULL;
    long length = PyList_Size(listObj); //获取length

    long i, sum = 0;
    for(i = 0; i < length; i++){
        PyObject * temp = PyList_GetItem(listObj, i); // 读取python list object的value
        long elem = PyInt_AsLong(temp); //将整数转化成C的long形式
        sum += elem;
    }
    return Py_BuildValue("i", sum); //返回一个将sum转化成python的integer的值
}

static char addList_docs[] = "add( ): add all elements of the list\n"; //创建AddList函数的doc string

// 下面给出四个mapping，分别表示 <function-name in python module>, <actual-function>, <type-of-args the function expects>, <docstring>

static PyMethodDef addList_funcs[] = {
    {"add", (PyCFunction)addList_add, METH_VARARGS, addList_docs},
    {NULL, NULL, 0, NULL}
};

// 然后初始化addList这个module，mapping表示 <module name>, <the-info-table>, <module's-docstring>
PyMODINIT_FUNC initaddList(void){
    Py_InitModule3("addList", addList_funcs, "Add all ze lists");
}
```
