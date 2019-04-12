### 异常处理
python的raise和c的throw类似，Python的异常也是一个对象，由异常的事件(event)产生，可以理解为python在遇到无法处理的情况，我们会人为产生异常的，通过对异常对象的捕获，我们可以进行一些异常中的handle。异常Exception分为产生和捕获，如何产生异常呢，最通用的写法就是
```python
raise Exception("抛出一个异常")
# 异常会打断python脚本的执行，当Exception被raise时，后面所有的code都会被block
```

