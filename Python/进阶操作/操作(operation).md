# 3. Operations
### 3.1 String前缀
python的string底层的编码可以理解为都是16进制的编码（而16进制其实又是0101），用'\x00'来表示，不同的编码方式会影响某些函数的结果（比如print函数）。python的string定义前可以添加prefix前缀，（如s = u'abc'），不同前缀有不同的意义。
1.  u前缀表示unicode，以Unicode字符来存储字符串。在python3中，字符串的存储方式中，不管前缀带不带u，默认方式都是unicode编码的字符串。
2.  r前缀表示raw string，不识别转义，即不能识别像'\n'这样的特殊字符，而是成为纯粹的字符串，如 r"test\ntest"的输出就是 r"test\ntest"，不会有换行出现
```python
b = r"test\ntest"     
print(b) # test\ntest
```
3.  b前缀表示bytearray，生成字节序列对象，类型为bytes，可以理解为用bytes编码的字符串，本身是bytes，而print函数会将其字符串的部分显示出来，在socket中使用的比较多，并且bytes类型只可以是ASCII的字符或者16进制字符。
```
s = b"\x63\x67" #等价于 b"cg"，十六进制用ASCII解码。
```
4.  f前缀表示format，用来格式化字符串。即可以实现变量名对字符串的插入方式，类似string的加法
```python
age = 38
name = "Annie"
print(f'his name is {name},{age} years old') #his name is Annie,38 years old
```
而这些前缀之间某些可以用encode来转化
1. encode将一个string用bytes的方式来编码，通常使用的是utf-8和gbk，utf-8全称为”8-bit Unicode Transformation Format“
``` python
s0 = "好".encode("utf-8") #三个十六进制数表示一个字符
s1 = "好".encode("gbk") #两个十六进制数表示一个字符
print(s0) # b'\xe5\xa5\xbd'
print(s1) # b'\xba\xc3'
test = u'\u50c0' #把一个unicode编码的字符串赋值给一个unicode的string
print(test) # 输出'僀'，得到一个对应的奇怪的文字
```

### 3.2 对一个list中所有元素apply某个函数
```python
list1 = [x.fun() for x in list0]
```

### 3.3 Map和Filter
Map函数是用于将一个函数的output map到一个list里面去，即循环赋值的简便写法，blueprint为
```python
map(function_to_apply, list_of_inputs)
```
下面给出一个等价的例子
```python
items = [1, 2, 3, 4, 5]
squared = []
for i in items:
    squared.append(i**2)
    
# 等价于
squared = list(map(lambda x: x**2, items))
# 这里lambda返回一个函数，map的参数就是一个函数和一个list
# 而map函数会返回一个map的object，应该室友iterable的属性
```
同时，map还可以用作apply许多的函数，下面的例子
```python
def multiply(x):
    return (x*x)
def add(x):
    return (x+x)

funcs = [multiply, add]
for i in range(5):
    value = list(map(lambda x: x(i), funcs))
    print(value) # 这里的value是一个2个元素的数组，即[0, 0]， [1, 2]， [4, 4]， [9, 6]， [16, 8]
```
Filter函数，会返回list中满足function条件的sublist，blueprint为
```python
map(function_to_apply, list_of_inputs)
# 这里的function一定要是return bool值的函数。
```
例子，注意这里的lambda，也是返回一个函数，但是这样写的lambda返回的函数就是x < 0的时候返回True的函数。
```python
number_list = range(-5, 5)
less_than_zero = list(filter(lambda x: x < 0, number_list))
print(less_than_zero)

# Output: [-5, -4, -3, -2, -1]
```






