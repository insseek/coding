# Python的33个关键字

###  一、Python所有关键字

```python
import keysword  
keyword.kwlist  
['False','None', 'True','and','as', 'assert','break',
 'class','continue', 'def','del','elif', 'else','except',
'finally', 'for', 'from','global','if','import','in','is',
'lambda', 'nonlocal','not','or','pass','raise', 'return',
'try','while','with','yield']
```

### 二、Python关键字详解

#### 1、内置常量

False、None、True

```python
>>> False == 0 
True
>>> True == 1
True
>>> type(False)
<class 'bool'>
>>> type(None) 
<class 'NoneType'>
```

#### 2、逻辑 与 或 非 and or not

优先级：not、and、or

 x and y 如果 x 为 False 、空、0，返 回 x，否则返回 y

 x or y   如果 x 为 False、 空、0，返回 y，否则返回x

 not x   如果 x 为 False、 空、0，返回 True，否则返回False

#### 3、判断 与 循环

 if、elif、else 

is in 

while、for  .  in ...、break、continue 

####  4、函数

定义：def、lambda

中断：pass、return、yied 

####  5、异常处理

 try、except、finally、raise、assert

####  6、导入模块、包

 import、from

####  7、重命名

 as

####  8、变量

 global、nonlocal 

####  9、类

 class

####  10、删除

 del

####  11、上下文管理

 with