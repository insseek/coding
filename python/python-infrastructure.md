# Python整体架构

 Python属于 面向对象 解释型 高级动态计算机程序设计语言

了解一门语言 当先了解它的整体架构、基本语法、内置数据结构、内置函数、标准库以及运行模式、编程模式等。

### 1、Python总体架构

![img](https://leetan.oss-cn-beijing.aliyuncs.com/code/python-core.png)

Python的整体架构可以分为主要三个部分：

左边是 Python提供的内置模块 库 及用户自定义模块

右边是Python的运行环境，包括对象/类型系统、内存分配器、运行状态信息

中间是Python的核心 解释器。Python运行时的数据流 词法分析、语法分析、编译、执行

### 2、Python基本语法

Python以强制缩进来标识不同的代码块

Python的33个关键字：

```python
import keysword
keyword.kwlist
['False','None', 'True','and','as', 'assert','break',
 'class','continue', 'def','del','elif', 'else','except',
'finally', 'for', 'from','global','if','import','in','is',
'lambda', 'nonlocal','not','or','pass','raise', 'return',
'try','while','with','yield']
```

### 3、内置数据结构

字符串str、列表list、字典dict、元组tuple、集合set

collections里的namedtuple、deque、defaultdict、Counter、OrderedDict有时也挺有用

### 4、内置函数68个：

![img](https://leetan.oss-cn-beijing.aliyuncs.com/code/python-functions.png)

### 5、常用标准库![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://leetan.oss-cn-beijing.aliyuncs.com/code/library.png)