# 斐波那契数列的Python实现

Python实现斐波那契数列（Fibonacci sequence）的三种方案：列表 递归 生成器

```python
# -*- coding: utf-8 -*-
import itertools
from functools import lru_cache

# 列表实现。生成一个斐波那契数列列表
def fibo(num):
    fibo_list = []
    if num <= 0:
        return fibo_list
    else:
        x, y = 0, 1
        for i in range(num):
            fibo_list.append(y)
            x, y = y, x + y
    return fibo_list

# 生成器实现。迭代时一次生成一个值
def fibo_genetator():
    x, y = 0, 1
    while True:
        yield y
        x, y = y, x + y

# 递归实现。[添加缓存装饰器，提高效率]
@lru_cache(maxsize=None)
def fibo_recursive(num):
    if num < 0:
        return 0
    if num <= 1:
        return num

    return fibo_recursive(num - 1) + fibo_recursive(num - 2)


print(fibo(10))
print(list(itertools.islice(fibo_genetator(), 10)))
print([fibo_recursive(i) for i in range(1, 11)])
```
