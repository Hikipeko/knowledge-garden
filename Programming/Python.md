## Python Language

Python is a interpreted language: program is executed by an interpreter (python), which is another program (written in C).

[Compile language vs. interpreted language](https://stackoverflow.com/questions/441824/java-virtual-machine-vs-python-interpreter-parlance)

##### Data model

* Object are Python's abstraction for data, all data in Python are represented by objects. An object has Identity, Type, Value.
* Dynamic typing: runtime objects have a type
* Use garbage collection mechanism to deallocate objects automatically (use reference counting)



## Use Python

### Data Type

##### Immutable

Unlike mutable objects, immutable objects cannot change its state.

int, float, string, Boolean, tuple

```python
def my_abs(x: int) -> int:
    if x >= 0:
        return x
    else:
        return -x
```

### Function

Python functions are also objects.

**Input type check**

```python
# this is just type hint, not runtime checking
def my_abs(x: int) -> int:
    if x >= 0:
        return x
    else:
        return -x

def my_abs(x):
    if not isinstance(x, (int, float)):
        raise TypeError('bad operand type')
    if x >= 0:
        return x
    else:
        return -x
```

Python function can return multiple results.

Python function doesn't allow same function name for different input types.

```python
x, y = move(100, 100, 60, math.pi / 6)
```

**Multiple inputs**

```python
def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum

calc(1, 2)
calc(*nums)
```

#### 关键字参数

允许传入任意参数名的参数，这些参数在函数内部组装为一个dict。

```python
def person(name, age, **kw):
    print('name:', name, 'age:', age, 'other:', kw)

>>> person('Adam', 45)
name: Adam age: 45 other: {}

>>> person(age=45, name='Adam')
name: Adam age: 45 other: {}

>>> person('Adam', 45, gender='M', job='Engineer')
name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}
```

限制关键字参数的名字

```python
# 关键字只能是city或job
def person(name, age, *, city, job):
    print(name, age, city, job)
    
>>> person('Jack', 24, city='Beijing', job='Engineer')
Jack 24 Beijing Engineer
```



### 高级特征

#### 迭代

```python
d = {'a': 1, 'b': 2, 'c': 3}

for key in d:
    print(key)
    
# c++式
for i, value in enumerate(['A', 'B', 'C']):
    print(i, value)
    
# 多变量
for x, y in [(1, 1), (2, 4), (3, 9)]:
    print(x, y)
```

#### 列表生成式

```
>>> [x * x for x in range(1, 11) if x % 2 == 0]
[4, 16, 36, 64, 100]

>>> [m + n for m in 'ABC' for n in 'XYZ']
['AX', 'AY', 'AZ', 'BX', 'BY', 'BZ', 'CX', 'CY', 'CZ']

>>> import os # 导入os模块，模块的概念后面讲到
>>> [d for d in os.listdir('.')] # os.listdir可以列出文件和目录
['.emacs.d', '.ssh', '.Trash', 'Adlm', 'Applications', 'Desktop', 'Documents', 'Downloads', 'Library', 'Movies', 'Music', 'Pictures', 'Public', 'VirtualBox VMs', 'Workspace', 'XCode']
```

#### 生成器

节省空间

```
>>> L = [x * x for x in range(10)]
>>> g = (x * x for x in range(10))
>>> for n in g:
...     print(n)
```

如果一个函数定义中包含 `yield` 关键字，那么这个函数就不再是一个普通函数，而是一个generator函数，调用一个generator函数将返回一个generator：

```
def fib(max):
    n, a, b = 0, 0, 1
    while n < max:
        yield b
        a, b = b, a + b
        n = n + 1
    return 'done'
    
>>> f = fib(6)
```

generator的函数，在每次调用 `next()` 的时候执行，遇到 `yield` 语句返回，再次执行时从上次返回的`yield`语句处继续执行。

但是用 `for` 循环调用generator时，发现拿不到generator的 `return` 语句的返回值。如果想要拿到返回值，必须捕获 `StopIteration` 错误，返回值包含在 `StopIteration` 的 `value` 中：

```
>>> g = fib(6)
>>> while True:
...     try:
...         x = next(g)
...         print('g:', x)
...     except StopIteration as e:
...         print('Generator return value:', e.value)
...         break
```

### 函数式编程

函数式语言允许把函数本身作为参数传入另一个函数，且允许返回一个函数

**函数名是指向函数的变量**

##### map/reduce

`map()`函数接收两个参数，一个是函数，一个是`Iterable`，`map`将传入的函数依次作用到序列的每个元素，并把结果作为新的`Iterator`返回。

```python
>>> list(map(str, [1, 2, 3, 4, 5, 6, 7, 8, 9]))
['1', '2', '3', '4', '5', '6', '7', '8', '9']
```

`reduce`把一个函数作用在一个序列`[x1, x2, x3, ...]`上，这个函数必须接收两个参数，`reduce`把结果继续和序列的下一个元素做累积计算，其效果就是：

```python
>>> from functools import reduce
>>> def add(x, y):
...     return x + y
...
>>> reduce(add, [1, 3, 5, 7, 9])
25
```

```python
>>> from functools import reduce
>>> def fn(x, y):
...     return x * 10 + y
...
>>> def char2num(s):
...     digits = {'0': 0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9}
...     return digits[s]
...
>>> reduce(fn, map(char2num, '13579'))
13579
```

##### filter

`filter()`也接收一个函数和一个序列。和`map()`不同的是，`filter()`把传入的函数依次作用于每个元素，然后根据返回值是`True`还是`False`决定保留还是丢弃该元素。

```python
def is_odd(n):
    return n % 2 == 1

list(filter(is_odd, [1, 2, 4, 5, 6, 9, 10, 15]))
# 结果: [1, 5, 9, 15]
```

##### sort

现在，我们提出排序应该忽略大小写，按照字母序排序。要实现这个算法，不必对现有代码大加改动，只要我们能用一个key函数把字符串映射为忽略大小写排序即可。

```python
>>> sorted(['bob', 'about', 'Zoo', 'Credit'], key=str.lower, reverse=True)
['Zoo', 'Credit', 'bob', 'about']
```



### 模块

一个.py文件为一个模块（module）

包（package）按目录来组织模块

```ascii
mycompany
 ├─ web
 │  ├─ __init__.py
 │  ├─ utils.py
 │  └─ www.py
 ├─ __init__.py
 ├─ abc.py
 └─ utils.py
```

文件`www.py`的模块名就是`mycompany.web.www`，两个文件`utils.py`的模块名分别是`mycompany.utils`和`mycompany.web.utils`。

#### 安装第三方模块

在Python中，安装第三方模块，是通过包管理工具pip完成的。

使用anaconda， conda install xxx

import numpy as np



### OOP

#### Class

```python
# class className(FatherClassName)
# use __init__ to initialize
# __fieldName to indicate private member
class Student(object):
    def __init__(self, name, score):
        self.__name = name
        self.__score = score
        
    def get_name(self):
        return self.__name

    def get_score(self):
        return self.__score
    
bart = Student('Bart Simposon', 98)
```

#### 获取对象信息

```python
# returns the type of the instance
type() 
# returns whether the instance is an instance of the input class
isinstance(h, Dog) 
# 返回所有关于对象的属性和方法
dir('ABC') 
getattr(obj, 'x')
setattr(obj, 'x', 1)
hasattr(obj, 'x')
```

#### 类属性

类似于类静态成员

```python
class Student(object):
    name = 'Student'
```



### File I/O

```python
try:
    f = open('/path/to/file', 'r')
    print(f.read())
finally:
    if f:
        f.close()

# equivalent to 
with open('/path/to/file', 'r') as f:
    print(f.read(size))
    
# output
with open('/Users/michael/test.txt', 'w') as f:
    f.write('Hello, world!')
```


### Standard library modules

sys, os, io, math, statistics, copy, csv, re

```python
# list of command-line arguments
sys.argv
# run a shell command
os.system("ls -a")
# run a shell command, get its output, decode to string via UTF-8
subprocess.run(['ls', '-l'], capture_output=True).strout.decode('utf-8')
# perform shallow copy
copy.copy()
copy.deepcopy()
```
