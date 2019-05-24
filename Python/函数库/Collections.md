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

### 
