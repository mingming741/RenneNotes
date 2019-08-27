# Collections
collections是python自己的库，python除了一些默认的数据机构(list, dict...)之外，在collections中还implement了一些其他的常用数据结构，这里会说明几个常用的。

### defaultdict
和dict类似，不过defaultdict不需要你去check某个key是否存在。在reference不存在的key的时候会输出0，即不会产生KeyError。并且使用defaultdict可以产生dict嵌套的结构，下面是使用的例子
```python
from collections import defaultdict

tree = lambda: defaultdict(tree)
some_dict = tree()
some_dict['colours']['favourite'] = "yellow"
import json
print(json.dumps(some_dict))
# Output: {"colours": {"favourite": "yellow"}}
```

### OrderedDict
排序过的dict，顺序为add key的时候的顺序，并且在key的value改变的同时，key的位置不会改变。OrderedDict自带的有sort的函数，可以根据key或者是value来sort。非常的常用。
```python
from collections import OrderedDict

colours = OrderedDict([("Red", 198), ("Green", 170), ("Blue", 160)])
for key, value in colours.items():
    print(key, value)
```

### counter
用来做计数，使用的最多的就是打开文件，并且计数共有多少行的问题
```python
with open('filename', 'rb') as f:
    line_count = Counter(f)
print(line_count)
```

### deque
queue是先进先出的结构，而deque是一个双向的queue，即queue的两边都可以先进先出。deque的工作原理类似python的list，但是有pop和popleft的method在里面。
```python
d = deque(range(5))
print(len(d))  # Output: 5
d.popleft()  # Output: 0
d.pop()  # Output: 4
print(d) # Output: deque([1, 2, 3])
```
deque也有extend的method，用于给queue加元素，如果元素超过maxlength的话，另一端的元素会被丢弃
```python
d = deque([0, 1, 2, 3, 5], maxlen=5)
d.extend([6])
print(d)
#Output: deque([1, 2, 3, 5, 6], maxlen=5)
```

### namedtuple
Namedtuple makes your tuples self-document。即可以用类的方法调用tuple中的元素，而不是用index的方法。但同时namedtuple和tuple也是兼容的，即index的调用也被保留下来了。
```python
from collections import namedtuple

Animal = namedtuple('Animal', 'name age type')
perry = Animal(name="perry", age=31, type="cat")
print(perry) # Output: Animal(name='perry', age=31, type='cat')
print(perry.name) # Output: 'perry'
```

