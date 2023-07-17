# Python内置函数

【python3.6官方文档共68个】：https://docs.python.org/3/library/functions.html 

![img](https://leetan.oss-cn-beijing.aliyuncs.com/code/python-functions.png)

#### abs(x)

返回一个数的绝对值。参数可以是一个整数或一个浮点数。若参数是复数，返回复数的模

#### all(iterable)

若 可迭代对象中所有元素为真（或可迭代对象为空），则返回True。等价于：

```python
def all(iterable):
    for element in iterable:
        if not element:
            return False
    return True
```

#### any(iterable)

若 可迭代对象中任意元素为真，则返回True。如果可迭代对象为空，返回False。等价于：

```python
def any(iterable):
    for element in iterable:
        if element:
            return True
    return False
```

#### ascii(object)

如repr(),返回一个包含对象的可打印表示的字符串，但会用\x,\u,or \U转义pepr()返回的字符串中的非ascii字符。生成一个类似于Python 2 中repr()返回的字符串。

#### bin(x)

将整数转换为以“0b”为前缀的二进制字符串。结果是一个有效的Python表达式。若 x 不是一个int型对象，它必须定义了一个`__index __`方法去返回整数。

一些例子：

> ```python
>>>> bin(3)
> '0b11'
> >>> bin(-10)
> '-0b1010'
> ```
> 
> 前缀“0b”需要或不需要，则可以使用以下任一方法。
>
> ```python
>>>> format(14, '#b'), format(14, 'b')
> ('0b1110', '1110')
>>>> f'{14:#b}', f'{14:b}'
> ('0b1110', '1110')
>```
> 

#### class bool([x])

返回一个布尔值，True或False。x用标准的真值测试程序来转换。如果x为false或空，它返回False，否则返回True。bool类是int的一个子类。它不能被子类化。它拥有唯一两个实例True和False

#### class bytearray([source[, encoding[, errors]]])

返回一个新的字节数组。bytearray类是一个范围在0 <= x < 256之间的可变整数序列。它有可变序列大多数常规方法。

可选参数source可以用几种不同方式来初始化数组：

若 它是一个字符串，必须给出编码（可选的，错误）参数；bytearray()用str.encode()把字符串转换成字节。

若 它是一个整数，数组将具有该大小，并用空字节初始化。

若 它是一个遵循buffer接口的对象，对象的只读buffer将被用来初始化字节数组

若 它是一个可迭代对象，它必须是一个范围在0 <= x < 256中的整数可迭代对象，被用做数组的初始内容。

如果没有参数，它创建一个大小为0的数组。

#### class bytes([source[, encoding[, errors]]])

返回一个新的字节对象。一个数值在0 <= x < 256之间的不可变整数序列。bytes是byte array的不可变版本。它有相同的非修改性方法和相同的索引与切片操作。

因此，构造函数参数与bytearray()相同

Bytes对象也可以用字面值创建，详情见String and Bytes literals

#### callable(object)

若 对象可调用，返回True，否则返回False。若，返回True，依然可能调用失败，若返回False，调用绝不会成功。

注意类是可调用的（调用一个类返回一个实例），如果类有__call__()方法，实例可调用

#### chr(i)

返回整数i对应的Unicode字符字符串。比如chr(97)返回字符串‘a’，chr(8364)返回字符串'€'。它是ord()的逆操作。

参数的有效范围在0到1114111（0x10FFFF 16进制）之间。若，超出异常，将抛出ValuaError异常

#### @classmethod

将一个方法转换为类方法

类方法接受类作为隐式的第一参数，就像实例方法接受实例作为隐式的第一个参数。声明一个类方法，用如下惯例

```html
class C:
    @classmethod
    def f(cls, arg1, arg2, ...): ...
```

@classmethod形式是一个装饰器函数。它既可以在类上调用（如C.f()）也可以在实例上调用（如C().f()）。

除了实例的类，实例本身被忽略。如果一个类方法在子类上调用，那么子类对象被传递为隐式的第一个参数。

#### compile(source, filename, mode, flags=0, dont_inherit=False, optimize=-1)

将source编译成代码对象，或者AST（Abstract Syntax Tree，抽象语法树）对象。代码对象可以由exec()或eval()执行。源可以是普通字符串，字节字符串或AST对象。有关如何使用AST对象的信息，请参阅ast模块文档。

filename参数是要从中读取代码的文件名；如果它不是从文件中读取的话，需要传入一些可识别的内容（通常使用'string'）

mode 参数指定必须编译模式；如果source由语句序列组成，则它可以是'exec'；如果它是单个语句，则可以使用'eval'；如果它由单个交互式语句组成，则可以使用'single'。（在最后一种情况下，非None语句将会被打印出来）

可选参数flags和dont_inherit控制哪些未来版本的语句会应用于源编译。如果两者都不存在（或两者都为零），则使用在调用compile()的代码中生效的未来语句来编译代码。如果给出了flags参数且没有给出dont_inherit参数（或者为0），除了本该使用的future语句之外，由flags参数指明的future语句也会影响编译。如果dont_inherit是非0整数，flags参数被忽略（调用compile周围的有效的future语句被忽略）。

future语句由bit位指明，这些bit可以做或运算，以指明多个语句。可以在__future__模块中，_Feature实例的compiler_flag属性找到指明功能的bit位。

参数optimize指定编译器的优化级别；默认值-1选择由-O选项给出的解释器的优化级别。显式级别为0（无优化； __debug__为真），1（声明被删除，__debug__为假 ）或2（docstrings也被删除）。

如果源包含空字节，则此函数引发SyntaxError（如果编译的源无效）和ValueError

如果要将Python代码解析为其AST表示形式，请参阅ast.parse()。

注意

当以'single'或者'eval'模式编译多行代码字符串的时候，输入至少以一个新行结尾。这主要是便于code模块检测语句是否结束。

#### class complex([real[, imag]])

返回值形式为real + imag * 1j的复数，或将字符串或数字转换为复数。如果第一个参数是个字符串，它将被解释成复数，同时函数不能有第二个参数。第二个参数不能是字符串。每个参数必须是数值类型（包括复数）。如果省略imag，则默认为零，构造函数会像int和float一样进行转换。如果省略这两个参数，则返回0j。

注意

当从字符串转化成复数的时候，字符串中+或者-两边不能有空白。例如，complex('1+2j')是可行的，但complex('1 + 2j')会抛出ValueError异常。

#### delattr(object, name)

这个函数和setattr()有关。参数是一个对象和一个字符串。字符串必须是对象的某个属性的名字。只要对象允许，这个函数删除该名字对应的属性。例如，delattr(x, 'foobar')等同于del x.foobar。

#### class dict(**kwarg)

#### class dict(mapping, **kwarg)

#### class dict(iterable, **kwarg)

创建一个新字典。dict对象是字典类。

#### dir([object])

如果没有参数，返回当前本地作用域内的名字列表。如果有参数，尝试返回参数所指明对象的合法属性的列表。

如果对象具有名为__dir__()的方法，那么将调用此方法，并且必须返回属性列表。这允许实现自定义__getattr__()或__getattribute__()函数的对象自定义dir()报告其属性的方式。

如果对象不提供__dir__()，则函数会尽量从对象的__dict__属性（如果已定义）和其类型对象中收集信息。结果列表不一定是完整的，并且当对象具有自定义__getattr__()时，可能不准确。

默认的dir()机制对于不同类型的对象具有不同的行为，因为它尝试生成最相关，而不是完整的信息：

- 如果对象是模块对象，列表包含模块的属性名。
- 如果对象是类型或者类对象，列表包含类的属性名，及它的基类的属性名。
- 否则，列表包含对象的属性名，它的类的属性名和类的基类的属性名。

返回的列表按字母顺序排序。例如：

```python
>>> import struct
>>> dir()   # show the names in the module namespace
['__builtins__', '__name__', 'struct']
>>> dir(struct)   # show the names in the struct module 
['Struct', '__all__', '__builtins__', '__cached__', '__doc__', '__file__',
 '__initializing__', '__loader__', '__name__', '__package__',
 '_clearcache', 'calcsize', 'error', 'pack', 'pack_into',
 'unpack', 'unpack_from']
>>> class Shape:
...     def __dir__(self):
...         return ['area', 'perimeter', 'location']
>>> s = Shape()
>>> dir(s)
['area', 'location', 'perimeter']
```

注意

因为dir()主要是方便在交互式环境中使用，它尝试提供一组有用的名称，而不是试图提供完整或一致性的名称集合，具体的行为在不同的版本之间会有变化。例如，如果参数是一个类，那么元类属性就不会出现在结果中。

#### divmod(a, b)

取两个（非复数）数字作为参数，并在使用整数除法时返回由商和余数组成的一对数字。对于混合的操作数类型，应用二元算术运算符的规则。对于整数，结果与（a // b， a ％ b）相同。对于浮点数，结果为（q， a ％ b），其中q通常为math.floor(a / b)，也有可能比这个结果小1。不管怎样，q * b + a % b非常接近于a，如果a % b非0，它和b符号相同且0 <= abs(a % b) < abs(b)。

#### enumerate(iterable, start=0)

返回一个枚举对象。iterable 必须是一个序列、一个迭代器，或者其它某种支持迭代的对象。enumerate()返回的迭代器的__next__()方法返回一个元组，该元组包含一个计数（从start开始，默认为0）和迭代iterable得到的值。

```python
>>> seasons = ['Spring', 'Summer', 'Fall', 'Winter']
>>> list(enumerate(seasons))
[(0, 'Spring'), (1, 'Summer'), (2, 'Fall'), (3, 'Winter')]
>>> list(enumerate(seasons, start=1))
[(1, 'Spring'), (2, 'Summer'), (3, 'Fall'), (4, 'Winter')]
```

等同于：

```python
def enumerate(sequence, start=0):
    n = start
    for elem in sequence:
        yield n, elem
        n += 1
```

#### eval(expression, globals=None, locals=None)

参数是字符串和可选的全局变量和局部变量。如果有全局变量，globals必须是个字典。如果有局部变量，locals可以是任何映射类型对象。

expression参数被当作Python表达式来解析并演算（技术上来说，是个条件列表），使用globals和locals字典作为全局和局部的命名空间。如果globals字典存在，且缺少‘__builtins__’，在expression被解析之前，当前的全局变量被拷贝进globals。这意味着expression通常具有对标准builtins的完全访问权限，并且传播受限环境。如果locals字典被忽略，默认是globals字典。如果两个字典都省略，则在调用eval()的环境中执行表达式。返回值是被演算的表达式的结果。语法错误报告成异常。例子：

```python
>>> x = 1
>>> eval('x+1')
2
```

此函数也可用于执行任意代码对象（例如由compile()创建的代码对象）。在这种情况下，传递代码对象而不是字符串。如果代码对象已使用'exec'作为mode参数编译，则eval()的返回值将为None 。

提示：exec()函数支持语句的动态执行。globals()和locals()函数分别返回当前的全局和局部字典，可以用于传递给eval或exec()。

#### exec(object[, globals[, locals]])

这个函数支持动态执行Python代码。object必须是一个字符串或代码对象。如果它是一个字符串，该字符串被解析为一套Python语句，然后执行（除非语法错误发生）。[1]如果它是一个代码对象，只是简单地执行它。在所有情况下，执行的代码应该可以作为有效的文件输入（参见“参考手册”中的“文件输入”部分）。请注意，即使在传递给exec()函数的代码上下文中，函数定义外面的return和yield 语句可能不被执行。返回值为None。

在所有情况下，如果省略可选部分，则代码在当前作用域中执行。如果只提供globals，它必须是一个字典，它将用于全局变量和局部变量。如果提供globals和locals，它们分别用于全局变量和局部变量。如果存在，locals可以是任意的映射类型对象。记住在模块级别，全局和局部字典是同一个字典。如果exec的globals和locals是独立的两个对象，代码的执行就像它嵌入在类定义中一样。

如果globals字典的__builtins__键没有值，则会给这个赋予一个内置模块builtins字典的引用。这样，你可以在将globals传递给exec()之前插入自己的__builtins__字典，来控制执行的代码可访问的builtins。

注

内置函数globals()和locals()分别返回当前全局和局部字典，它们可以用做传递给exec()的第二和第三个参数。

注意

默认的locals的行为和下述的locals()函数一样：不应该尝试修改默认的locals字典。如果在函数exec()返回后，需要在locals上查看代码的效果，请传递一个明确的locals字典。

#### filter(function, iterable)

用iterable中传入function后返回True的元素构造一个迭代器。iterable可以是个序列，支持迭代的容器，或者一个迭代器。如果function是None，使用特性函数，即为False的iterable中的元素被移除。

注意filter(function, iterable) 如果函数不是 None等效于生成器表达式 (item for item in iterable if function(item)) 。如果函数是 None ，(item for item in iterable if item)

See itertools.filterfalse() for the complementary function that returns elements of iterable for which function returns false.

#### class float([x])

返回从数字或字符串x构造的浮点数。

如果参数是一个字符串，它应该包含一个十进制数，可选地前面有一个符号，并且可选地嵌入在空格中。可选的sign可以是'+'或'–'； '+'符号对生成的值没有影响。参数还可以是表示NaN（非数字）或正或负无穷大的字符串。更确切地说，输入必须符合以下语法，前导和尾随空白字符被删除：

```html
sign           ::=  "+" | "-"
infinity       ::=  "Infinity" | "inf"
nan            ::=  "nan"
numeric_value  ::=  floatnumber | infinity | nan
numeric_string ::=  [sign] numeric_value
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

这里floatnumber是在浮点字面值中描述的Python浮点字面值的形式。情况不重要，因此，例如，“inf”，“Inf”，“INFINITY”和“iNfINity”都是正无穷大的可接受拼写。

否则，如果参数是整数或浮点数，则返回具有相同值（在Python的浮点精度内）的浮点数。如果参数在Python浮点数的范围之外，则引发一个OverflowError。

对于一般的Python对象x，float(x)委托给x .__float__()。

如果没有给出参数，则返回0.0。

例子：

```python
>>> float('+1.23')
1.23
>>> float('   -12345\n')
-12345.0
>>> float('1e-003')
0.001
>>> float('+1E6')
1000000.0
>>> float('-Infinity')
-inf
```

float类型在数字类型 - int，float，complex中描述。

#### format(value[, format_spec])

将value转化成“格式化”的表现形式，格式由format_spec控制。对format_spec的解释依赖于value参数的类型，大多数内置类型有标准的格式化语法：Format Specification Mini-Language。

默认的format_spec是一个空字符串，通常给出与调用str（value）相同的效果。

对格式（值， format_spec）的调用将被转换为type（value）.__ format __（value， t4> format_spec）其在搜索值的__format__()方法时绕过实例字典。如果方法搜索到达object并且format_spec不为空，或者如果format_spec，则会引发TypeError t7>或返回值不是字符串。

#### class frozenset([iterable])

返回一个新的frozenset对象，如果可选参数iterable存在，frozenset的元素来自于iterable。frozenset是个内置类。

#### getattr(object, name[, default])

返回object的属性值。name必须是个字符串。如果字符串是对象某个属性的名字，则返回该属性的值。例如，getattr(x, 'foobar')等同于x.foobar。如果这个名字的属性不存在，如果提供default则返回它，否则引发AttributeError。

#### globals()

返回表示当前全局符号表的字典。它总是当前模块的字典（在函数或者方法中，它指定义的模块而不是调用的模块）。

#### hasattr(object, name)

参数是一个对象和一个字符串。如果字符串是对象的一个属性，则返回True，否则返回False。（它的实现是通过调用getattr(object, name)并查看它是否引发一个AttributeError）。

#### hash(object)

返回该对象的哈希值（如果有的话）. 哈希值应该是一个整数。哈希值用于在查找字典时快速地比较字典的键。相等数值的哈希值相同（即使它们的类型不同，比如1和1.0）.

#### help([object])

调用内置的帮助系统。（这个函数主要用于交互式使用。）如果没有参数，在解释器的控制台启动交互式帮助系统。如果参数是个字符串，该字符串被当作模块名，函数名，类名，方法名，关键字或者文档主题而被查询，在控制台上打印帮助页面。如果参数是其它某种对象，生成关于对象的帮助页面。

该函数加入内置函数的名字空间，函数收录在site 模块里.

#### hex(x)

将整数转换为以“0x”为前缀的小写十六进制字符串，例如：

```python
>>> hex(255)
'0xff'
>>> hex(-42)
'-0x2a'
```

如果x不是Python int对象，它必须定义一个__index__()方法，返回一个整数。

另请参见int()用于将十六进制字符串转换为使用16为基数的整数。

注意

要获取浮点型的十六进制字符串表示形式，请使用float.hex()方法。

#### id(object)

返回对象的“标识”。这是一个整数，它保证在该对象的生命周期内是唯一的和恒定的。具有不重叠寿命的两个对象可以具有相同的id()值。

CPython实现细节：这是内存中对象的地址。

#### input([prompt])

如果有prompt参数，则将它输出到标准输出且不带换行。该函数然后从标准输入读取一行，将它转换成一个字符串（去掉一个末尾的换行符），然后返回它。当读取到EOF时，会产生EOFError。例子：

```python
>>> s = input('--> ')  
--> Monty Python's Flying Circus
>>> s  
"Monty Python's Flying Circus"
```

如果readline模块已加载，则input()将使用它提供精细的行编辑和历史记录功能。

#### class int(x=0)

#### class int(x, base=10)

从数字或字符串（x）构造并返回一个整数对象，如果没有给出参数，则返回0。如果 x 是一个数字，返回 x.__int__()。对于浮点数，这将截断到零。

如果x不是数字，或者如果给定base，则x必须是字符串bytes bytearray实例代表基数base中的integer literal。字面量的前面可以有+或者-（中间不能有空格），周围可以有空白。以n为基数的字面量包含数字0到n-1，用a到z（或者A到Z）来表示10到35。默认的base是10。允许的值为0和2-36。Base-2, -8, and -16 literals can be optionally prefixed with 0b/0B, 0o/0O, or 0x/0X, as with integer literals in code. base为0意味着完全解释为代码字面值，使得实际基数为2,8,10或16，并且使得int（'010'， 0 ）是不合法的，而int('010')是以及int（'010'，8）。

#### isinstance(object, classinfo)

如果object是clsaainfo的一个实例（或者是classinfo的直接、间接或虚拟子类的实例），那么则返回true。如果对象不是给定类型的对象，则函数始终返回false。如果classinfo是对象类型的元组（或递归地，其他这样的元组），如果对象是任何类型的实例，则返回true。如果classinfo不是类型或类型组成的元祖和此类元组，则会引发TypeError异常。

#### issubclass(class, classinfo)

如果 class 是classinfo的子类(直接、 间接或 虚拟) 则返回 true 。一个类被认为是它自己的子类。classinfo可以是类对象的元组，这时classinfo中的每个类对象都会被检查。在任何其他情况下，会引发TypeError异常。

#### iter(object[, sentinel])

返回一个迭代器对象。根据有无第二个参数，对第一个参数的解释相差很大。没有第二个参数，object必须是一个支持迭代协议（__iter__()方法）的容器对象，或者它必须支持序列协议（从 0开始的整数参数的__getitem__() 方法）。如果它不支持这些协议任何一个，将引发TypeError。如果给出第二个参数sentinel，那么object必须是一个可调用的对象。这种情况下创建的迭代器将在每次调用时不带参数调用object的__next__()方法；如果返回的值等于sentinel，将引发StopIteration，否则返回这个值。

另见迭代器类型。

iter()第二个参数的有用的一个场景是读取文件的行直至到达某个特定的行。下面的示例读取一个文件，直至readline()方法返回一个空字符串：

```python
with open('mydata.txt') as fp:
    for line in iter(fp.readline, ''):
        process_line(line)
```

#### len(s)

返回对象的长度（元素的个数）。参数可以是序列（如字符串，字节，元组，列表或者范围）或者集合（如字典，集合或者固定集合）。

#### class list([iterable])

list不是一个函数，它实际上是一个可变的序列类型，其文档在Lists和序列类型 — list, tuple, range中。

#### locals()

更新和返回表示当前局部符号表的字典。当locals()在函数代码块中调用时会返回自由变量，但是在类代码块中不会。

注意

不应该修改这个字典的内容；因为这些变化可能不会影响解释器使用的局部变量和自由变量。

#### map(function, iterable, ...)

返回一个迭代器，对iterable的每个项应用function，并yield结果。如果传递多个iterable参数，function必须接受这么多参数，并应用到从iterables并行提取的项中。如果有多个iterable，迭代器在最短的iterable耗尽时停止。对于函数的输入已经排列成参数元组的情况，参见itertools.starmap()。

#### max(iterable, *[, key, default])

#### max(arg1, arg2, *args[, key])

返回可迭代的对象中的最大的元素，或者返回2个或多个参数中的最大的参数。

填入的位置参数应该是可迭代的（ iterable）对象.返回可迭代对象中最大的元素。如果有2个或更多的位置参数，返回最大位置参数。

有两个可选的仅关键字参数。key参数指定类似于用于list.sort()的单参数排序函数。default参数指定如果提供的iterable为空则要返回的对象。如果迭代器为空并且未提供default，则会引发ValueError。

如果多个项目是最大的，则函数返回遇到的第一个项目。这与其他排序稳定性保留工具（例如sorted（iterable， key = keyfunc， reverse = True）[0] t3 >和heapq.nlargest（1， iterable， key = keyfunc）。

#### memoryview(obj)

返回给定参数的“内存视图”。

#### min(iterable, *[, key, default])

#### min(arg1, arg2, *args[, key])

返回可迭代的对象中的最小的元素，或者返回2个或多个参数中的最小的参数。

如果提供了一个位置参数，它应该是一个可迭代对象。返回可迭代对象中最小的元素。如果有2个或更多的位置参数，返回最小的位置参数。

有两个可选的仅关键字参数。键参数指定类似于用于list.sort()的单参数排序函数。默认参数指定如果提供的iterable为空则要返回的对象。如果迭代器为空并且未提供default，则会引发ValueError。

如果多个项目是最小的，函数返回遇到的第一个。这与其他排序稳定性保留工具（例如sorted（iterable， key = keyfunc）[0]和heapq.nsmallest（1， iterable， key = keyfunc）。

#### next(iterator[, default])

通过调用__next__()方法从迭代器中检索下一个项目。如果有default参数，在迭代器迭代完所有元素之后返回该参数；否则抛出StopIteration。

#### class object

返回一个新的无特征的对象。object是所有类的基础类.它包含所有Python类实例里都会有的通用方法.该函数不接受任何的参数。

注意

object不不具有__dict__，因此您不能将任意属性分配给object类的实例。

#### oct(x)

将整数转换为八进制字符串。结果是一个合法的Python表达式。如果x不是Python int对象，则必须定义一个返回整数的__index__()方法。

#### open(file, mode='r', buffering=-1, encoding=None, errors=None, newline=None, closefd=True, opener=None)

打开 file 并返回一个相应的 文件对象.如果文件不能被打开, 抛出 OSError 异常.

参数 file 是一个字符串表示的文件名称，或者一个数组表示的文件名称。文件名称可以是相对当前目录的路径，也可以是绝对路径表示。（如果给出了一个文件描述器，则当关闭返回的I / O对象时除外，除非closefd设置为False。

参数 mode 是指明打开文件的模式。默认值是'r'，表示使用文本的方式打开文件来读取。其他常见值为'w'用于写入（如果文件已经存在则截断文件），'x'用于排他性创建，'a' （在某些 Unix系统上，意味着全部写入追加到文件的末尾，而不管当前的查找位置）。在文本模式下，如果未指定encoding，则使用的编码取决于平台：locale.getpreferredencoding（False）以获取当前语言环境编码。（对于读取和写入原始字节，使用二进制模式，不指定编码。可用的模式有：

| 字符 | 含义                                       |
| :--- | :----------------------------------------- |
| 'r'  | 打开阅读（默认）                           |
| 'w'  | 打开写入，首先截断文件                     |
| 'x'  | 打开以供独占创建，如果文件已存在则失败     |
| 'a'  | 打开以供写入，如果存在，则附加到文件的末尾 |
| 'b'  | 二进制模式                                 |
| 't'  | 文本模式（默认）                           |
| '+'  | 打开磁盘文件进行更新（读写）               |
| 'U'  | 通用换行符模式（已弃用）                   |

默认模式为'r'（打开阅读文本，'rt'的同义词）。对于二进制读写访问，模式'w b'打开并将文件截断为0字节。'r b'打开文件而不截断。

如概述中所述，Python区分二进制和文本I / O。以二进制模式打开的文件（包括模式参数中的'b'）将内容作为字节对象，而不进行任何解码。在文本模式（默认情况下，或当't'包括在模式参数中）时，文件的内容将作为str ，这些字节已经使用平台相关编码首先解码，或者如果给出则使用指定的编码。

注意

Python不依赖于底层操作系统的文本文件的概念；所有的处理都是由Python本身完成的，因此是平台无关的。

参数 buffering是用于设置缓冲策略的可选整数。通过0以关闭缓冲（仅在二进制模式下允许），1选择行缓冲（仅在文本模式下可用）和整数当未给出buffers参数时，默认缓冲策略工作如下：

- 二进制文件以固定大小的块缓冲；使用启发式尝试确定底层器件的“块大小”并回退到io.DEFAULT_BUFFER_SIZE来选择缓冲区的大小。在许多系统上，缓冲区通常为4096或8192字节长。
- “交互式”文本文件（isatty()返回True的文件）使用行缓冲。其他文本文件使用上述策略用于二进制文件。

参数 encoding是用于解码或编码文件的编码的名称。这应该只在文本模式下使用。默认编码是平台相关的（无论locale.getpreferredencoding()返回），但是可以使用Python支持的任何文本编码。有关支持的编码列表，请参阅编解码器模块。

参数 errors是一个可选字符串，指定如何处理编码和解码错误 - 这不能在二进制模式下使用。虽然使用codecs.register_error()注册的任何错误处理名称也有效，但仍提供了多种标准错误处理程序（在错误处理程序下列出）。标准名称包括：

- 'strict'引发ValueError例外，如果存在编码错误。默认值None具有相同的效果。
- 'ignore'忽略错误。请注意，忽略编码错误可能会导致数据丢失。
- 'replace'会导致替换标记（例如'？'）插入到存在格式错误的数据的位置。
- 'surrogateescape'将表示任何不正确的字节，作为从U DC80到U DCFF范围内的Unicode私人使用区域中的代码点。当写入数据时使用surrogateescape错误处理程序时，这些专用代码点将被转回相同的字节。这对于处理未知编码中的文件很有用。
- 仅当写入文件时，才支持'xmlcharrefreplace'。编码不支持的字符将替换为相应的XML字符引用
- 'backslashreplace'通过Python的反斜杠转义序列替换格式错误的数据。
- 'namereplace'（也仅在编写时支持）用\ N {...}转义序列替换不支持的字符。

参数 newline控制通用换行符模式的工作原理（仅适用于文本模式）。它可以是None、''、'\n'、'\r'、'\r\n'。它的工作原理如下：

- 从流读取输入时，如果newline为None，则启用通用换行符模式。输入中的行可以以'\n'，'\r'或'\r\n'结尾，它们在返回给调用者之前被转换成'\n'。如果它是''，则启用通用换行符模式，但行结尾将返回给调用者而不会转换。如果它具有任何其它合法值，则输入行仅由给定字符串终止，并且行结尾被返回给调用者而不会转换。
- 将输出写入流时，如果newline为None，则写入的任何'\n'字符都将转换为系统默认行分隔符os.linesep。如果newline是''或'\n'，则不会进行转换。如果newline是任何其他合法值，写入的任何'\n'字符都将转换为给定字符串。

如果closefd是False并且给出了文件描述器而不是文件名，则当文件关闭时，基本文件描述器将保持打开。如果给定文件名，则closefd必须为True（默认值），否则将产生错误。

通过传递可调用对象opener可以使用自定义开启器。然后通过调用opener（文件，标志）获取文件对象的基础文件描述器。opener必须返回一个打开的文件描述器（传递os.open为opener 结果类似的功能 None）。

新创建的文件为non-inheritable。

以下示例使用os.open()函数的dir_fd参数打开相对于给定目录的文件：

```python
>>> import os
>>> dir_fd = os.open('somedir', os.O_RDONLY)
>>> def opener(path, flags):
...     return os.open(path, flags, dir_fd=dir_fd)
...
>>> with open('spamspam.txt', 'w', opener=opener) as f:
...     print('This will be written to somedir/spamspam.txt', file=f)
...
>>> os.close(dir_fd)  # don't leak a file descriptor
```

由open()函数返回的file object的类型取决于模式。当open()用于以文本模式打开文件（'w'，'r'，'wt'，'rt'等。），它返回io.TextIOBase（具体为io.TextIOWrapper）的子类。当用于通过缓冲以二进制模式打开文件时，返回的类是io.BufferedIOBase的子类。确切的类别不同：在读取二进制模式下，它返回io.BufferedReader；在写二进制和追加二进制模式中，它返回io.BufferedWriter，并且在读/写模式下，它返回io.BufferedRandom。当禁用缓冲时，返回原始流，即io.RawIOBase，io.FileIO的子类。

#### ord(c)

给定一个表示一个Unicode字符的字符串，返回一个表示该字符的Unicode代码点的整数。例如，ord('a')返回整数97和ord('€')（欧元符号）返回8364。这是chr()的逆操作。

#### pow(x, y[, z])

返回x的y次方; 如果提供z参数， 返回x 的y次方再除以z的余数 (计算效率比pow(x, y) % z更高)。双参数形式pow(x, y)等效于使用幂操作符号：x**y 。

参数必须是数字类型的。由于操作数是混合类型的，二进制计算的原因需要一些强制的规定。对于int操作数，结果具有与操作数相同的类型（强制后），除非第二个参数为负；在这种情况下，所有参数都转换为float，并传递float结果。例如, 10**2 返回 100, 但 10**-2 返回0.01. 如果第二个参数为负数，那么第三个参数必须省略。如果存在z，则x和y必须是整数类型，且y

#### print(*objects, sep=' ', end='\n', file=sys.stdout, flush=False)

将object打印到文本流file，由sep分隔，尾部接end。sep, end 和 file，如果提供这三个参数的话，必须以关键参数的形式。

All non-keyword arguments are converted to strings like str() does and written to the stream, separated by sep and followed by end.sep和结束必须是字符串；它们也可以是None，这意味着使用默认值。如果没有打印 对象, print() 只打印一个 结束符号 end.

文件参数必须是具有write(string)方法的对象；如果不存在或None，将使用sys.stdout。由于打印的参数被转换为文本字符串，因此print()不能与二进制模式文件对象一起使用。对于这些，请改用file.write(...)。

尽管通常是由file 参数来决定输出流是否缓存，但是如果 flush 参数为ture，那么输出流将会被强制刷新。

#### class property(fget=None, fset=None, fdel=None, doc=None)

返回一个property 属性。

fget是获取属性值的函数。fset是用于设置属性值的功能。fdel是用于删除属性值的功能。并且doc为属性创建一个docstring。

典型的用法是定义一个托管属性x：

```python
class C:
    def __init__(self):
        self._x = None

    def getx(self):
        return self._x

    def setx(self, value):
        self._x = value

    def delx(self):
        del self._x

    x = property(getx, setx, delx, "I'm the 'x' property.")
```

如果c是C的实例，则c.x将调用getter，c.x = value将调用setter，del c.x将调用deleter。

如果给出doc，它将是该属性的文档字符串。否则，该属性将拷贝fget的文档字符串（如果存在）。这使得可以使用property()作为装饰器轻松创建只读属性：

```html
class Parrot:
    def __init__(self):
        self._voltage = 100000

    @property
    def voltage(self):
        """Get the current voltage."""
        return self._voltage
```

@property装饰器将voltage()方法转换为具有相同名称的只读属性的“getter”，并设置为voltage的文档字符串为“Get the current voltage.”。

Property对象具有可用作装饰器的getter、setter和deleter方法，用于创建property的副本，并将相应的访问器函数设置为装饰的功能。最好的解释就是使用一个例子：

```python
class C:
    def __init__(self):
        self._x = None

    @property
    def x(self):
        """I'm the 'x' property."""
        return self._x

    @x.setter
    def x(self, value):
        self._x = value

    @x.deleter
    def x(self):
        del self._x
```

这段代码与第一个例子完全相等。请务必给予附加函数与原始属性相同的名称（在本例中为x）。

返回的property对象还具有对应于构造函数参数的属性fget、fset和fdel。

#### range(stop)

#### range(start, stop[, step])

range实际上是Ranges和Sequence Types — list, tuple, range中描述的不可变序列类型，而不是函数。

#### repr(object)

返回某个对象可打印形式的字符串。For many types, this function makes an attempt to return a string that would yield an object with the same value when passed to eval(), otherwise the representation is a string enclosed in angle brackets that contains the name of the type of the object together with additional information often including the name and address of the object. 类可以通过定义__repr__()方法控制该函数对其实例的返回。

#### reversed(seq)

返回一个反向iterator。seq必须是一个具有__reversed__() 方法或支持序列协议的对象（整数参数从0开始的__len__()方法和__getitem__()方法）。

#### round(number[, ndigits])

返回一个浮点型 近似值，保留小数点后 ndigits 位。如果省略 ndigits,将返回最接近输入的整数.代表number.__round__(ndigits)。

对于支持round()的内建类型，值被四舍五入为功率减去ndigits的最接近的倍数10；如果两个倍数相等地接近，则对偶数选择进行舍入（因此，例如，round(0.5)和round(-0.5)都是0和round(1.5)是2）。如果使用一个参数调用，返回值是一个整数，否则类型与number相同。

注意

浮点数round()的行为可能让人惊讶，例如round(2.675, 2)给出的是2.68 而不是期望的2.67。这不是一个错误：大部分十进制小数不能用浮点数精确表示，它是因为这样的一个事实的结果。更多信息，请参阅Floating Point Arithmetic: Issues and Limitations。

#### class set([iterable])

返回一个新的set 对象，其元素可以从可选的iterable获得。set是一个内建的类。

#### setattr(object, name, value)

它与getattr()相对应。参数是一个对象、一个字符串和一个任意值。字符串可以是一个已存在属性的名字也可以是一个新属性的名字。该函数将值赋值给属性，只要对象允许。例如，setattr(x, 'foobar', 123)等同于x.foobar = 123。

#### class slice(stop)

#### class slice(start, stop[, step])

返回一个slice对象，表示由索引range(start, stop, step)指出的集合。start和step参数默认为None。切片对象具有只读属性start、stop和step，它们仅仅返回参数的值（或者它们的默认值）。他们没有其他明确的功能；但是它们被数字Python和其他第三方扩展使用。在使用扩展的索引语法时同样会生成切片对象。例如：a[start:stop:step]或者a[start:stop, i]。请参见itertools.islice()中另外一个返回迭代器的版本。

#### sorted(`iterable[, key][, reverse]`)

依据iterable中的元素返回一个新的排好序的列表。

具有两个可选参数，它们必须指明为关键字参数。

key指示一个带有一个参数的函数，它用于从列表的每个元素中提取比较的关键字：key=str.lower。默认值是None（直接比较元素）。

reverse是一个布尔值。如果设置为True，那么列表中元素反过来比较来排序。

functools.cmp_to_key()用于将老式的cmp函数转换为key函数。

内建的sorted()函数保证是稳定的。如果保证不更改比较相等的元素的相对顺序，则排序是稳定的 - 这有助于在多个通过中排序（例如，按部门排序，然后按工资级别排序）。

#### staticmethod(function)

返回function的一个静态方法。

静态方法不接受隐式的第一个参数（也就是实例名称self）。要声明静态方法，请使用下面的习惯方式：

```python
class C:
    @staticmethod
    def f(arg1, arg2, ...): ...
```

@staticmethod形式是一个函数装饰器 - 有关详细信息，请参阅函数定义中的函数定义的描述。

它可以在类上（如C.f()）或实例上（如C().f()）调用。除了它的类型，实例其他的内容都被忽略。

Python中的静态方法类似于Java或C++。另请参见classmethod()了解用于创建备用类构造函数的变体。

有关静态方法的详细信息，请参阅标准类型层次结构中标准类型层次结构的文档。

#### class str(object='')

#### class str(object=b'', encoding='utf-8', errors='strict')

返回object的str版本。有关详细信息，请参见str()。

str是内置字符串类。有关字符串的一般信息，请参阅文本序列类型 - str。

#### sum(iterable[, start])

将start以及iterable的元素从左向右相加并返回总和。start默认为0。iterable的元素通常是数字，start值不允许是一个字符串。

对于某些使用情况，有很好的替代sum()的方法。连接字符串序列的首选快速方法是调用''.join(sequence)。要以扩展精度添加浮点值，请参见math.fsum()。要连接一系列可迭代对象，请考虑使用itertools.chain()。

#### super([type[, object-or-type]])

返回一个代理对象，它委托方法给父类或者type的同级类。这对于访问类中被覆盖的继承方法很有用。除了跳过type本身之外，搜索顺序与getattr()所使用的顺序相同。

type的__mro__属性列出getattr()和super()使用的方法解析顺序。该属性是动态的，并且可以在继承层次结构更新时更改。

如果省略第二个参数，则返回的super对象是未绑定的。如果第二个参数是一个对象，则isinstance(obj, type)必须为真。如果第二个参数是类型，则issubclass(type2, type)必须为真（这对类方法很有用）。

super有两种典型的使用情况。在具有单继承的类层次结构中，可以使用super来引用父类，而不必明确命名它们，从而使代码更易于维护。这种使用非常类似于在其他编程语言中super的使用。

第二种使用情况是在动态执行环境中支持协同多继承。这种使用情况是Python独有的，在静态编译语言或仅支持单继承的语言中找不到。这使得可以实现“菱形图”，其中多个基类实现相同的方法。良好的设计指出此方法在每种情况下具有相同的调用顺序（因为调用的顺序在运行时确定，因为该顺序适应类层次结构中的更改，并且因为该顺序可以包括在运行时之前未知的兄弟类）。

对于这两种使用情况，典型的超类调用看起来像这样：

```python
class C(B):
    def method(self, arg):
        super().method(arg)    # This does the same thing as:
                               # super(C, self).method(arg)
```

注意，super()只实现显式点分属性查找的绑定过程，例如super().__getitem__(name)。它通过实现自己的__getattribute__()方法来实现这一点，以便以支持协同多继承需要的以可预测的顺序搜索类。因此，super()没有定义隐式的查找语句或操作，例如super()[name]。

还要注意，如果不是零个参数的形式，没有限制super()在方法内部使用。如果两个参数的形式指定准确的参数，就能进行正确的引用。零个参数的形式只在类定义中工作，因为编译器填充必要的细节以正确检索正在定义的类，原理类似访问当前实例的普通方法。

有关如何使用super()设计协同类的实用建议，请参阅使用super()的指南。

#### tuple([iterable])

而不是一个函数，tuple实际上是一个不变序列类型，如Tuples和Sequence Types — list, tuple, range中所述。

#### class type(object)

#### class type(name, bases, dict)

只有一个参数时，返回object的类型。返回值是一个类型对象，通常与object.__class__返回的对象相同。

建议使用isinstance()内建函数来测试对象的类型，因为它考虑了子类。

带有三个参数时，返回一个新的类型对象。它本质上是class语句的动态形式。name string是类名，并成为__name__属性； bases元组列出基类，并成为__bases__属性；并且dict字典是包含类主体的定义的命名空间，并且复制到标准字典以成为__dict__属性。例如，下面的两条语句创建完全相同的type对象：

```python
>>> class X:
...     a = 1
...
>>> X = type('X', (object,), dict(a=1))
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

#### vars([object])

返回一个模块、字典、类、实例或者其它任何一个具有__dict__属性的对象的__dict__属性。

模块和实例这样的对象的__dict__属性可以更新；但是其它对象可能对它们的__dict__属性的写操作具有限制（例如，类使用types.MappingProxyType来阻止对字典直接更新）。

如果不带参数，vars()的行为就像locals()。注意，locals字典只用于读取，因为对locals字典的更新会被忽略。

#### zip(*iterables)

创建一个迭代器，聚合来自每个迭代器的元素。

返回一个由元组构成的迭代器，其中第i个元组包含来自每一组参数序列或可迭代量的第i元素。当最短输入可迭代被耗尽时，迭代器停止。使用单个可迭代参数，它返回1元组的迭代器。没有参数，它返回一个空迭代器。等同于：

```python
def zip(*iterables):
    # zip('ABCD', 'xy') --> Ax By
    sentinel = object()
    iterators = [iter(it) for it in iterables]
    while iterators:
        result = []
        for it in iterators:
            elem = next(it, sentinel)
            if elem is sentinel:
                return
            result.append(elem)
        yield tuple(result)
```

可以保证迭代按从左向右的计算顺序。这使得使用zip(*[iter(s)]*n)将数据序列聚类为n长度组的习语成为可能。这重复了相同的迭代器n次，以使每个输出元组具有对迭代器的n调用的结果。这具有将输入划分为n个长块的效果。

zip()当迭代器元素不一致时，循环停止在较短的迭代器元素，较长的迭代器元素会被舍弃。如果这些值很重要，请改用itertools.zip_longest()。

zip() 与 * 操作符一起可以用来 unzip 一个列表：

```python
>>> x = [1, 2, 3]
>>> y = [4, 5, 6]
>>> zipped = zip(x, y)
>>> list(zipped)
[(1, 4), (2, 5), (3, 6)]
>>> x2, y2 = zip(*zip(x, y))
>>> x == list(x2) and y == list(y2)
True
```

#### `__import__`(name, globals=None, locals=None, fromlist=(), level=0)

注意

与 importlib.import_module() 不同，这是一个高级的函数，不会在日常的 Python 编程中用到。

通过 import 语句调用此函数。他能够被替代(通过导入builtins 模块，赋值给 builtins.__import__)去改变 import 语句的语义, 但是强烈 不鼓励，因为通常使用import钩子  更容易达到相同的目标，而且不会对使用了默认import实现的代码造成任何问题。也不建议直接使用`__import__()`以支持importlib.import_module()。

该函数导入模块名称，可能使用给定的globals和locals来确定如何解释包上下文中的名称。fromlist给出了应从name给出的模块导入的对象或子模块的名称。标准实现不使用其 locals 参数，仅仅使用 globals 确定 导入 语句的包的上下文。

level specifies whether to use absolute or relative imports. 0（默认值）表示仅执行绝对导入。级别的正值表示相对于调用`__import__()`的模块的目录进行搜索的父目录数

When the name variable is of the form package.module, normally, the top-level package (the name up till the first dot) is returned, not the module named by name. However, when a non-empty fromlist argument is given, the module named by name is returned.

例如，语句import spam导致字节码类似于以下代码：

```python
spam = __import__('spam', globals(), locals(), [], 0)
```

语句import spam.ham导致此调用：

```python
spam = __import__('spam.ham', globals(), locals(), [], 0)
```

Note how` __import__() `returns the toplevel module here because this is the object that is bound to a name by the import statement.

On the other hand, the statement from spam.ham import eggs, sausage as saus results in

```python
_temp = __import__('spam.ham', globals(), locals(), ['eggs', 'sausage'], 0)
eggs = _temp.eggs
saus = _temp.sausage
```

Here, the spam.ham module is returned from __import__(). From this object, the names to import are retrieved and assigned to their respective names.

如果你只是想要按名称导入模块 ，使用 importlib.import_module()。