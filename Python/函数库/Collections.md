# Collections
collections是python自己的库，python除了一些默认的数据机构(list, dict...)之外，在collections中还implement了一些其他的常用数据结构，这里会说明几个常用的。

### defaultdict
和dict类似，不过defaultdict不需要你去check某个key是否存在。并且使用defaultdict可以产生dict嵌套的结构，下面是使用的例子
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
