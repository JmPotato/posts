#如何判断一个变量是否被定义

- tags: Work,Python

----

今天闲来无事，研究Python中，突然有一想法，能否判断一个变量是否被定义，如果被未定义，则给它赋值1，于是尝试：

```python
a = a if a is not None else 1
```

结果报错了，Python中不允许未被定义的变量进行`a=a`这样的语句，也许可以利用这一特性，用`try: except:`来捕获异常：

```python
try:
    a = a
except:
    a = 1
```

当a未被定义时，`a=a`会抛出一个异常（说明`a`未被定义），捕获异常后，赋值1

后来，whtsky又提供了另一种方法

```python
a = locals().get("a", "1")
```

`locals()`会返回所有的局部变量（一个字典），可以判断`a`是否在其中。
