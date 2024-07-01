# python

## 1.python基础

### 常见函数

title()以首字母大写的方式显示每个单词，即将每个单词的首字母都改为大写。

剔除字符串开头的空白，或同时剔除字符串两端的空白。为此，可分别使用方法lstrip()和strip()：

python中的/（除）保留小数，而整除是 //

```python
3 / 2  # 1.5
3.0 / 2 # 1.5
3 // 1 # 1
3.0 // 2 # 1.5
```

###  列表

列表由一系列按特定顺序排列的元素组成。你可以创建包含字母表中所有字母、数字0~9或所有家庭成员姓名的列表；也可以将任何东西加入列表中，其中的元素之间可以没有任何关系。

```python
classmates = ['Michael', 'Bob', 'Tracy']
# 在列表中添加新元素时，最简单的方式是将元素附加到列表末尾。
classmates.append('Adam')

classmates.insert(1, 'Jack')

# 要删除list末尾的元素
classmates.pop() 

print(classmates.pop(1))#弹出指定位置的元素

del classmates[0]#删除指定位置元素

classmates.remove('Bob')#根据值删除元素

classmates[1] = 'Sarah' #要把某个元素替换成别的元素，可以直接赋值给对应的索引位置

#list里面的元素的数据类型也可以不同，比如：
L = ['Apple', 123, True]
```

Python为访问最后一个列表元素提供了一种特殊语法。通过将索引指定为-1，可让Python返回最后一个列表元素： 索引-2返回倒数第二个列表元素，索引-3返回倒数第三个列表元素，以此类推。

```python
# 访问列表最后一个元素
classmates[-1]
```

使用方法insert()可在列表的任何位置添加新元素。

```python
motorcycles = ['honda', 'yamaha', 'suzuki'] 

motorcycles.insert(0, 'ducati') 
```

#### 列表排序

sort()函数永久性地修改了列表元素的排列顺序，而sorted()函数只是对序列进行临时排序。

```python
classmates.sort()#对列表进行排序

classmates.sort(reverse=True)#对列表进行反向排序

classmates.reverse()#列表反转

cars = ['bmw', 'audi', 'toyota', 'subaru'] 

print(sorted(cars)) # ['audi', 'bmw', 'subaru', 'toyota']

cars  # ['bmw', 'audi', 'toyota', 'subaru']
```

倒着打印列表

```
cars.reverse()
```

方法reverse()永久性地修改列表元素的排列顺序，但可随时恢复到原来的排列顺序，为此只需对列表再次调用reverse()即可。

#### 使用range()创建数字列表 

```python
# 函数range()从2开始数，然后不断地加2，直到达到或超过终值（11）
even_numbers = list(range(2,11,2))
```

#### 列表解析

列表解析将for循环和创建新元素的代码合并成一行，并自动附加新元素

```python
squares = [value**2 for value in range(1,11)]
print(squares)
```

#### 切片 

```python
players = ['charles', 'martina', 'michael', 'florence', 'eli']
# 打印前三个元素
print(players[0:3])
# 提取列表的第2~4个元素
print(players[1:4])
# 没有指定第一个索引，Python将自动从列表开头开始：
print(players[:4])
# Python将返回从第3个元素到列表末尾的所有元素：
print(players[2:])
# Python将返回最后3个元素
print(players[-3:])
# 要复制列表，可创建一个包含整个列表的切片，方法是同时省略起始索引和终止索引（[:]）
my_foods = ['pizza', 'falafel', 'carrot cake']
friend_foods = my_foods[:]
```

### 元组

列表非常适合用于存储在程序运行期间可能变化的数据集。列表是可以修改的，这对处理网站的用户列表或游戏中的角色列表至关重要。然而，有时候你需要创建一系列不可修改的元素，元组可以满足这种需求。Python将不能修改的值称为不可变的，而不可变的列表被称为元组。 

```python
classmates = ('Michael', 'Bob', 'Tracy')
t = (1) #表示一个数字1
t=(1,) #表示一个元组
>>> t = ('a', 'b', ['A', 'B'])
>>> t[2][0]
'A'
>>> t[2][0]='X'
>>> t
('a', 'b', ['X', 'B'])
```

虽然不能修改元组的元素，但可以给存储元组的变量赋值。因此，如果要修改前述矩形的尺寸，可重新定义整个元组：

### 循环和匹配

#### 1.input

`input()`读取用户的输入，·input()`返回的数据类型是`str`，`str`不能直接和整数比较，必须先把`str`转换成整数。Python提供了`int()函数来完成这件事情

```python
birth = input('birth: ')
birth = int(birth)
if birth < 2000:
    print('00前')
else:
    print('00后')
```

#### 2.模式匹配

当我们用`if ... elif ... elif ... else ...`判断时，会写很长一串代码，可读性较差。如果要针对某个变量匹配若干种情况，可以使用`match`语句。l类似于java中的switch语句

```python
score = input('请输入你的选择：')
match score:
    case 'A':
        print("score is A")
    case 'B':
        print("score is B")
    case _:
        print("你输入的是什么鬼???")
```

`match`语句除了可以匹配简单的单个值外，还可以匹配多个值、匹配一定范围，并且把匹配后的值绑定到变量：

```python
age = 15
match age:
    case x if x < 10:
        print(f'< 10 years old: {x}')
    case 10:
        print('10 years old.')
    case 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18:
        print('11~18 years old.')
    case 19:
        print('19 years old.')
    case _:
        print('not sure.')
```

`match`语句还可以匹配列表，功能非常强大

```python
args = ['gcc', 'hello.c', 'world.c']
# args = ['clean']
# args = ['gcc']
match args:
    # 如果仅出现gcc，报错:
    case ['gcc']:
        print('gcc: missing source file(s).')
    # 出现gcc，且至少指定了一个文件:
    case ['gcc', file1, *files]:
        print('gcc compile: ' + file1 + ', ' + ', '.join(files))
    # 仅出现clean:
    case ['clean']:
        print('clean')
    case _:
        print('invalid command.')
```

#### 3.break和continue

在循环中，`break`语句可以提前退出循环。

```python
n=1
while n<100:
    n+=1
    print(n)
    if n>30:
        print("今天只打印30个")
        break
print("结束了，回去吧")
```

在循环过程中，也可以通过`continue`语句，跳过当前的这次循环，直接开始下一次循环。

*要特别注意*，**不要滥用`break`和`continue`语句**。`break`和`continue`会造成代码执行逻辑分叉过多，容易出错。大多数循环并不需要用到`break`和`continue`语句

### 使用字典和集合

Python内置了字典：dict的支持，dict全称dictionary，在其他语言中也称为map，使用键-值（key-value）存储，具有极快的查找速度。

```python
d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}
print(d['Michael']) #95
#通过in判断key是否存在
print('tom' in d) #false
#取得key对应的value值
print(d.get('Michael'))
#删除一个key，用pop(key)方法，对应的value也会从dict中删除
print(d.pop('Bob'))
```

注意：**dict内部存放的顺序和key放入的顺序是没有关系的**。

#### 添加键—值对 

字典是一种动态结构，可随时在其中添加键—值对。要添加键—值对，可依次指定字典名、用方括号括起的键和相关联的值。 

```python
alien_0 = {'color': 'green', 'points': 5}
print(alien_0)
alien_0['x_point'] = 0
alien_0['y_point'] = 10
print(alien_0)
```

#### 删除键—值对 

对于字典中不再需要的信息，可使用del语句将相应的键—值对彻底删除。使用del语句时，必须指定字典名和要删除的键。

```python
alien_0 = {'color': 'green', 'points': 5}
print(alien_0)
del alien_0['color']
print(alien_0)
```

#### 遍历字典

```python
user_0 = 
	{ 'username': 'efermi', 
 		'first': 'enrico',
 		'last': 'fermi',
	}
# 遍历键和值    
for key, value in user_0.items():    
    print("key:"+key)    
    print("value:"+value)
# 遍历建
for k in user_0.keys():
    print(k)
# 遍历值
for value in user_0.values():
    print(value)
```

遍历字典时，会默认遍历所有的键，因此，如果将上述代码中的`for k in user_0.keys():`替换为`for k in user_0:`，输出将不变。

#### 按顺序遍历字典中的所有键 

要以特定的顺序返回元素，一种办法是在for循环中对返回的键进行排序。为此，可使用函数sorted()来获得按特定顺序排列的键列表的副本： 

```python
for key in sorted(user_0):    
    print(key)
```

#### 嵌套

有时候，需要将一系列字典存储在列表中，或将列表作为值存储在字典中，这称为嵌套。

```python
# 在列表中嵌套字典
alien_0 = {'color': 'green', 'points': 5}
alien_1 = {'color': 'yellow', 'points': 10}
alien_2 = {'color': 'red', 'points': 15}
aliens = [alien_2, alien_1, alien_0]
for alien in aliens:    
    print(alien)
# 在字典中嵌套列表    
pizza = {
 'crust': 'thick',
 'toppings': ['mushrooms', 'extra cheese'],
}

for topping in pizza['toppings']:
    print("\t" + topping) # mushrooms extra cheese    
```

注意: 列表和字典的嵌套层级不应太多,如果嵌套层级比前面的示例多得多,很可能有更简单的解决问题的方案。

```python
# 在字典中存储字典
users = {    
	'aeinstein': {        
		'first': 'albert',        
		'last': 'einstein',        
		'location': 'princeton',    
	},    
	'mcurie': {        
		'first': 'marie',        
		'last': 'curie',       
		'location': 'paris',    
	},
}
for username, userinfo in users.items():    
	print(username)    
	print(userinfo)
```

和list比较，dict有以下几个特点：

1. 查找和插入的速度极快，不会随着key的增加而变慢；
2. 需要占用大量的内存，内存浪费多。

而list相反：

1. 查找和插入的时间随着元素的增加而增加；
2. 占用空间小，浪费内存很少。

set和dict类似，也是一组key的集合，但不存储value。由于key不能重复，所以，在set中，没有重复的key。

- 要创建一个set，需要提供一个list作为输入集合：
- 重复元素在set中自动被过滤：
- `add(key)`方法可以添加元素到set中，可以重复添加，但不会有效果
- `remove(key)`方法可以删除元素：

```python
s = set([1, 1, 2, 2, 3, 3])
print(s) #{1, 2, 3}
s.add(23)
print(s) #{1, 2, 3, 23}
```

set和dict的唯一区别仅在于没有存储对应的value，但是，set的原理和dict一样，所以，同样不可以放入可变对象，因为无法判断两个可变对象是否相等，也就无法保证set内部“不会有重复元素”。

#### 数据容器的特点

基于各类数据容器的特点，它们的应用场景如下：

- 列表：一批数据，可修改、可重复的存储场景


- 元组：一批数据，不可修改、可重复的存储场景


- 字符串：一串字符串的存储场景


- 集合：一批数据，去重存储场景


- 字典：一批数据，可用Key检索Value的存储场景


#### 字符串的常用方法

```python
str.title()#字符串首字母大写
language.lstrip()#删除左边空格
language.rstrip()#删除右边空格
language.strip()#删除两边空格
age=23
strs=str(age)#整数转换为字符串
```

### 函数

#### 数据类型转换

Python内置的常用函数还包括数据类型转换函数，比如`int()`函数可以把其他数据类型转换为整数

```python
str='345'
print(int(str)) #345
print(float(str)) #345.0
```

函数名其实就是指向一个函数对象的引用，完全可以把函数名赋给一个变量，相当于给这个函数起了一个"别名"

#### 定义函数

定义一个函数要使用`def`语句，依次写出函数名、括号、括号中的参数和冒号，然后，在缩进块中编写函数体，函数的返回值用`return`语句返回

```python
def my_bas(x):
    if x>=0:
        return x
    else:
        return -x
#函数调用
print(my_bas(-908)) #908
```

#### 空函数

如果想定义一个什么事也不做的空函数，可以用`pass`语句：

```python
def nop():
    pass
```

`pass`可以用来作为占位符，比如现在还没想好怎么写函数的代码，就可以先放一个`pass`，让代码能运行起来。

函数可以返回多个值,但其实这只是一种假象，Python函数返回的仍然是单一值。原来返回值是一个元组！但是，在语法上，返回一个元组可以省略括号，而多个变量可以同时接收一个元组，按位置赋给对应的值，所以，Python的函数返回多值其实就是返回一个元组，但写起来更方便。

```python
import math
def move(x, y, step, angle=0):
    nx = x + step * math.cos(angle)
    ny = y - step * math.sin(angle)
    return nx, ny
```

#### 可变参数

```python
def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
print(calc(1,23,5,5,6))
```

定义可变参数和定义一个list或元组参数相比，仅仅在参数前面加了一个`*`号。在函数内部，参数`numbers`接收到的是一个元组，因此，函数代码完全不变。但是，调用该函数时，**可以传入任意个参数，包括0个参数**：

注意： 传进的所有参数都会被args变量收集，它会根据传进参数的位置合并为一个元组，args是元组类型，这就是位置传递

#### 关键字参数

**参数是“键=值”形式的形式的情况下**, 所有的“键=值”都会被kwargs接受, 同时会根据“键=值”组成字典.

```python
def person(name,age,**kw):
    print('name:',name,'age:',age,'other:',kw)
person('Adam', 45, gender='M', job='Engineer')
extra = {'city': 'Beijing', 'job': 'Engineer'}
person('Jack', 24, city=extra['city'], job=extra['job'])
#name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}
#以上参数传递方式的简易写法
person('Jack', 24, **extra)
#name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}
```

#### 命名关键字参数

命名关键字参数需要一个特殊分隔符`*`，`*`**后面的参数被视为命名关键字参数**。命名关键字参数必须传入参数名，这和位置参数不同。如果没有传入参数名，调用将报错：

```python
def person(name, age, *args, city, job):
    print(name, age, args, city, job)
```

命名关键字参数可以有缺省值，从而简化调用，由于命名关键字参数`city`具有默认值，调用时，可不传入`city`参数：

```python
def person(name, age, *, city='Beijing', job):
    print(name, age, city, job)
def person(name, age, city, job):
    # 缺少 *，city和job被视为位置参数
    pass
"""
city 参数是一个命名关键字参数，这意味着在调用函数时必须使用关键字 city 来指定其值，而不能省略。job 是一个普通的关键字参数，可以在调用时使用关键字指定，也可以省略，如果省略了 job，则它将不会被传递给函数。
"""
person("tom", 12, city="上海", job="程序员")
```

#### 递归函数

如果一个函数在内部调用自身本身，这个函数就是递归函数。

```python
def fact(n):
    if n==1:
        return 1
    return n * fact(n - 1)
```

递归函数的优点是定义简单，逻辑清晰。理论上，所有的递归函数都可以写成循环的方式，但循环的逻辑不如递归清晰。使用递归函数需要注意防止栈溢出。

##### **尾递归**优化

尾递归是指，在函数返回的时候，调用自身本身，并且，return语句不能包含表达式。这样，编译器或者解释器就可以把尾递归做优化，使递归本身无论调用多少次，都只占用一个栈帧，不会出现栈溢出的情况。

上面的`fact(n)`函数由于`return n * fact(n - 1)`引入了乘法表达式，所以就不是尾递归了。要改成尾递归方式，需要多一点代码，主要是要把每一步的乘积传入到递归函数中：

```python
def fact_iter(num, product):
    if num == 1:
        return product
    return fact_iter(num - 1, num * product)
```

#### lambda匿名函数 

函数的定义中 

• def关键字，可以定义带有名称的函数 

• lambda关键字，可以定义匿名函数（无名称） 

​		有名称的函数，可以基于名称重复使用。 

​		无名称的匿名函数，只可临时使用一次。

语法

```python
lambal 传入参数:函数体
```

lambda 是关键字，表示定义匿名函数 ，传入参数表示匿名函数的形式参数，

如：x, y 表示接收2个形式参数 ，函数体，就是函数的执行逻辑，要注意：**只能写一行**，无法写多行代码

```python
def test_func(compute):
    result = compute(1, 2)
    print(f"结果是:{result}")
# 通过lambda匿名函数的形式，将匿名函数作为参数传入
def add(x, y):
    return x + y
test_func(add)
test_func(lambda x, y: x + y)
```

#### 将函数存储在模块中

函数的优点之一是，使用它们可将代码块与主程序分离。可以将函数存储在被称为模块的独立文件中，再将模块导入到主程序中。import语句允许在当前运行的程序文件中使用模块中的代码。

```python
import pizza

pizza.make_pizza(16, '蘑菇')

pizza.make_pizza(12, '蘑菇', '辣椒', '茄子')
```

通过import语句，可以使用pizza.py中定义的所有函数。 

还可以导入模块中的特定函数，这种导入方法的语法如下： 

```python
from module_name import function_name 

# 通过用逗号分隔函数名，可根据需要从模块中导入任意数量的函数：
from module_name import function_0, function_1, function_2
```

##### 使用as给函数指定别名

如果要导入的函数的名称可能与程序中现有的名称冲突，或者函数的名称太长，可指定简短而独一无二的别名

```python
from pizza import make_pizza as mp

mp(16, 'pepperoni')

mp(12, 'mushrooms', 'green peppers', 'extra cheese') 

# 使用星号（*）运算符可让Python导入模块中的所有函数
from pizza import * 

```

















































## 2.python进阶

### 1.高级特性

#### 1.切片

`L[0:3]`表示，从索引`0`开始取，直到索引`3`为止，但不包括索引`3`。即索引`0`，`1`，`2`，正好是3个元素。

```python
L = ['Michael', 'Sarah', 'Tracy', 'Bob', 'Jack']
print(L[0:3]) #['Michael', 'Sarah', 'Tracy']
print(L[2:4])#['Tracy', 'Bob']
#最后两个值
print(L[-2:])#['Bob', 'Jack']
#前5个数每两个去一个
print(L[:4:2])
#甚至什么都不写，只写[:]就可以原样复制一个list：
print(L[:])
```

#### 2.列表生成式

Python内置的非常简单却强大的可以用来创建list的生成式。

如果要生成`[1x1, 2x2, 3x3, ..., 10x10]`，方法一是循环：

```python
L = []
for x in range(1,11):
    L.append(x * x)
print(L) # [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

但是循环太繁琐，而列表生成式则可以用一行语句代替循环生成上面的list：

```python
print([x*x for x in range(1,11)]) # [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]、
print([m + n for m in 'abc' for n in 'xyz']) # ['ax', 'ay', 'az', 'bx', 'by', 'bz', 'cx', 'cy', 'cz']
```

不能在最后的`if`加上`else`：

```python
print([x*x for x in range(1,11) if x%2==0]) # [4, 16, 36, 64, 100]
[x for x in range(1, 11) if x % 2 == 0 else 0]
#  File "<stdin>", line 1
#    [x for x in range(1, 11) if x % 2 == 0 else 0]
```

判断变量的数据类型

```python
x ='123'
y = 123
print(isinstance(x ,str)) # True
print(isinstance(y , str)) # false
```

#### 3.生成器

创建`L`和`g`的区别仅在于最外层的`[]`和`()`，`L`是一个list，而`g`是一个generator。

```python
L = [x * x for x in range(10)] # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
g = (x * x for x in range(10)) # <generator object <genexpr> at 0x000001F790B26AC0>
```

在Python中，这种一边循环一边计算的机制，称为生成器：generator。

generator保存的是算法，每次调用`next(g)`，就计算出`g`的下一个元素的值，直到计算到最后一个元素，没有更多的元素时，抛出`StopIteration`的错误。

我们创建了一个generator后，基本上永远不会调用`next()`，而是通过`for`循环来迭代它

```python
g=(x*x for x in range(10))#定义一个生成器
for n in g:
    print(n) # # 0 1 4 9 16 25 36 49 64 81
```

generator函数和普通函数的执行流程不一样。普通函数是顺序执行，遇到`return`语句或者最后一行函数语句就返回。而变成generator的函数，在每次调用`next()`的时候执行，遇到`yield`语句返回，再次执行时从上次返回的`yield`语句处继续执行。

如果一个函数定义中包含`yield`关键字，那么这个函数就不再是一个普通函数，而是一个generator函数，调用一个generator函数将返回一个generator：

```python
def _feibo(max):
    n,a,b=0,0,1
    while n<max:
        a,b=b,a+b #相当于t=(a,a+b),a=t[0],b=t[1]
        n=n+1
        yield b 
        # print(b)
    return 'done'
print(_feibo(10))
```

### 2.函数式编程

函数本身也可以赋值给变量，即：变量可以指向函数。

```python
f=abs
print(f(-10)) #10
```

把`abs`指向`10`后，就无法通过`abs(-10)`调用该函数了！因为`abs`这个变量已经不指向求绝对值函数而是指向一个整数`10`！

```python
abs=10
print(abs(-10))
#Traceback (most recent call last):
 # File "E:\code\pythonCode\test\Test02.py", line 20, in <module>
 # print(abs(-10))
#TypeError: 'int' object is not callable
```

当然实际代码绝对不能这么写，这里是为了说明**函数名也是变量**

#### 传入函数

既然变量可以指向函数，函数的参数能接收变量，那么一个函数就可以接收另一个函数作为参数，这种函数就称之为高阶函数。

```python
def add(x, y, f):
    return f(x,y) + f(y,y)
print(add(-5, 6, abs)) # 11
```

#### map/reduce

`map()`函数接收两个参数，一个是函数，一个是`Iterable`，`map`将传入的函数依次作用到序列的每个元素，并把结果作为新的`Iterator`返回。

```python
def f(x):
    return x+x
r = map(f, [1, 2, 3, 4, 5, 6, 7, 8, 9])
print(list(r))#[2, 4, 6, 8, 10, 12, 14, 16, 18]
```

`reduce`把一个函数作用在一个序列`[x1, x2, x3, ...]`上，这个函数必须接收两个参数，`reduce`把结果继续和序列的下一个元素做累积计算，其效果就是：

```python
reduce(f, [x1, x2, x3, x4]) = f(f(f(x1, x2), x3), x4)
```

使用reduce实现一个求和案例

```python
from functools import reduce
def add(x,y):
    return x+y
print(reduce(add,[1,2,3,4,5,6,7,8])) # 36
```

#### filter

Python内建的`filter()`函数用于过滤序列。和`map()`类似，`filter()`也接收一个函数和一个序列。和`map()`不同的是，`filter()`把传入的函数依次作用于每个元素，然后根据返回值是`True`还是`False`决定保留还是丢弃该元素。

```python
def is_odd(n):
    return n % 2 == 1
list(filter(is_odd, [1, 2, 4, 5, 6, 9, 10, 15]))
# 结果: [1, 5, 9, 15]
```

#### 返回函数

高阶函数除了可以接受函数作为参数外，还可以把函数作为结果值返回。

```python
def lazy_sum(*args):
    def sum():
        ax = 0
        for n in args:
            ax = ax + n
        return ax
    return sum
print(lazy_sum(1,2,3,4)) # 返回的不是求和结果，而是求和函数sum
sums =lazy_sum(1,2,3,4)
print(sums()) # 调用函数sums时，才真正计算求和的结果：
```

在这个例子中，我们在函数`lazy_sum`中又定义了函数`sum`，并且，内部函数`sum`可以引用外部函数`lazy_sum`的参数和局部变量，当`lazy_sum`返回函数`sum`时，相关参数和变量都保存在返回的函数中，这种称为“闭包（Closure）”的程序结构拥有极大的威力

注意：当我们调用`lazy_sum()`时，每次调用都会返回一个新的函数，即使传入相同的参数：

```python
sums =lazy_sum(1,2,3,4)
sums2=lazy_sum(1,2,3)
print(sums == sums2) # false
```

#### 闭包

```python
def count():
    fs = []
    for i in range(1, 4):
        def f():
             return i*i
        fs.append(f)
    return fs
f1, f2, f3 = count()
print(f1()) # 9
print(f2()) # 9
print(f3()) # 9
```

在上面的例子中，每次循环，都创建了一个新的函数，然后，把创建的3个函数都返回了。实际结果是三个9，原因就在于返回的函数引用了变量`i`，但它并非立刻执行。等到3个函数都返回时，它们所引用的变量`i`已经变成了`3`，因此最终结果为`9`

**返回闭包时牢记一点：返回函数不要引用任何循环变量，或者后续会发生变化的变量。**

如果一定要引用循环变量怎么办？方法是再创建一个函数，用该函数的参数绑定循环变量当前的值，无论该循环变量后续如何更改，已绑定到函数参数的值不变：

```python
def count2():
    def f(j):
        def g():
            return j*j
        return g
    fs = []
    for i in range(1, 4):
        fs.append(f(i)) # f(i)立刻被执行，因此i的当前值被传入f()
    return fs
f4, f5, f6 = count2()
print(f4()) # 1
print(f5()) # 4
print(f6()) # 9
```

#### nonlocal

使用闭包，就是内层函数引用了外层函数的局部变量。如果只是读外层变量的值，我们会发现返回的闭包函数调用一切正常：

```python
def inc():
    x = 0
    def fn():
        # 仅读取x的值:
        return x + 1
    return fn
f = inc()
print(f()) # 1
print(f()) # 1
```

但是，如果修改外部变量的值会报错

```python
def inc1():
    x = 0
    def fn():
        # nonlocal x
        x = x + 1
        return x
    return fn
f = inc1()
print(f()) # 1
print(f()) # 2
"""
UnboundLocalError: local variable 'x' referenced before assignment
"""
```

原因是`x`作为局部变量并没有初始化，直接计算`x+1`是不行的。但我们其实是想引用`inc()`函数内部的`x`，所以需要在`fn()`函数内部加一个`nonlocal x`的声明。加上这个声明后，解释器把`fn()`的`x`看作外层函数的局部变量，它已经被初始化了，可以正确计算`x+1`。

**注意：使用闭包时，对外层变量赋值前，需要先使用nonlocal声明该变量不是当前函数的局部变量。**

#### 匿名函数

当我们在传入函数时，有些时候，不需要显式地定义函数，直接传入匿名函数更方便。

```python
print(list(map(lambda x: x * x, [1, 2, 3, 4, 5, 6, 7, 8, 9])))
"""
匿名函数lambda x: x * x实际上就是：
def f(x):
    return x * x
"""
```

关键字`lambda`表示匿名函数，冒号前面的`x`表示函数参数。匿名函数有个限制，就是只能有一个表达式，不用写`return`，返回值就是该表达式的结果。

用匿名函数有个好处，因为函数没有名字，不必担心函数名冲突。此外，匿名函数也是一个函数对象，也可以把匿名函数赋值给一个变量，再利用变量来调用该函数：

同样，也可以把匿名函数作为返回值返回

```python
def build(x, y):
    return lambda: x * x + y * y
```

#### 装饰器

由于函数也是一个对象，而且函数对象可以被赋值给变量，所以，通过变量也能调用该函数。

```python
def now():
    print('20240423')
f = now
print(now.__name__) # 打印函数名
print(f.__name__) # 打印函数名
```

假设我们要增强`now()`函数的功能，比如，在函数调用前后自动打印日志，但又不希望修改`now()`函数的定义，这种在代码运行期间动态增加功能的方式，称之为“装饰器”

#### 偏函数







### 3.文件操作

#### 1.文件的读取

open()打开函数

在Python，使用open函数，可以打开一个已经存在的文件，或者创建一个新文件，语法如下 

name：是要打开的目标文件名的字符串(可以包含文件所在的具体路径)。 

mode：设置打开文件的模式(访问模式)：只读、写入、追加等。 

encoding:编码格式（推荐使用UTF-8） 

```python
f = open("D:/ruanjian/LICENSE.txt", "r", encoding="UTF-8")
```

##### read()方法：

```python
f.read(num)
```

​	num表示要从文件中读取的数据的长度（单位是字节），如果没有传入num，那么就表示读取文件中所有的数据。

##### readlines()方法： 

readlines可以按照行的方式把整个文件中的内容进行一次性读取，并且返回的是一个**列表**，其中每一行的数据为一个元素。

```python
f = open('python.txt')
content = f.readlines()
# ['hello world\n', 'abcdefg\n', 'aaa\n', 'bbb\n', 'ccc']
print(content)
# 关闭文件
f.close()
```

##### readline()方法：

一次读取一行内容

```python
line=f.readline()
```

循环读取行

```python
n=1
for line in f:
    n=n+1
    print("第",n,"行是:",line)
```

**close()关闭文件对象**

最后通过close，关闭文件对象，也就是关闭对文件的占用 ， 如果不调用close,同时程序没有停止运行，那么这个文件将一直被Python程序占用。

**通过with open语法打开文件，可以自动关闭**

#### 2.文件的写入 

```python
f = open('E:\code\\test.txt', 'w')
f.write('hello world,张三')
f.flush()
```

注意： 

 直接调用write，内容并未真正写入文件，而是会积攒在程序的内存中，称之为缓冲区 

 当调用flush的时候，内容会真正写入文件 

 这样做是避免频繁的操作硬盘，导致效率下降（攒一堆，一次性写磁盘）

- 文件如果不存在，使用”w”模式，会创建新文件 
- 文件如果存在，使用”w”模式，会将原有内容清空

#### 3.文件的追加

```python
f = open('E:\code\\test.txt', 'a')
f.write('hello world,你好啊')
f.flush()
```

a模式，文件不存在会创建文件 

a模式，文件存在会在最后，追加写入文件

### 4.Python异常、模块与包

#### 1.异常处理

捕获异常的作用在于：提前假设某处会出现异常，做好提前准备，当真的出现异常的时候，可以有后续手段。

基本语法

```python
try: 
    可能发生错误的代码 
except: 
    如果出现异常执行的代码
```

##### 捕获指定异常

```python
try: 
    print(name) 
except NameError as e: 
    print('name变量名称未定义错误')
```

注意：

① 如果尝试执行的代码的异常类型和要捕获的异常类型不一致，则无法捕获异常。

② 一般try下方只放一行尝试执行的代码。

##### 捕获多个异常

当捕获多个异常时，可以把要捕获的异常类型的名字，放到except 后，并使用元组的方式进行书写。

```python
try:
    print(1/0)
except (NameError, ZeroDivisionError):
    print('ZeroDivision错误...')
```

##### 捕获异常并输出描述信息

```python
try:
    print(num)
except (NameError, ZeroDivisionError) as e:
    print(e)
```

##### 捕获所有异常

```python
try:
    print(name)
except Exception as e:
    print(e)
```

##### 异常else

else表示的是如果没有异常要执行的代码。

```python
try:
    print(1)
except Exception as e:
    print(e)
else:
    print('我是else，是没有异常的时候执行的代码')
```

##### 异常finally

finally表示的是无论是否异常都要执行的代码，例如关闭文件。 

```python
try:
    f = open('test.txt', 'r')
except Exception as e:
    f = open('test.txt', 'w')
else:
    print('没有异常，真开心')
finally:
    f.close()
```

#### 2.异常的传递

异常是具有传递性的 

当函数func01中发生异常, 并且没有捕获处理这个异常的时候, 异常会传递到函数func02, 当func02也没有捕获处理这个异常的时候 main函数会捕获这个异常, 这就是异常的传递性. 

提示: **当所有函数都没有捕获异常的时候, 程序就会报错**

#### 3.Python模块

以内建的`sys`模块为例，编写一个`hello`的模块：

```python
#!/usr/bin/env python3		 # 可以让这个hello.py文件直接在Unix/Linux/Mac上运行
# -*- coding: utf-8 -*-		 # .py文件本身使用标准UTF-8编码
' a test module '		     # 模块的文档注释，任何模块代码的第一个字符串都被视为模块的文档注释；
__author__ = 'Michael Liao'	  # __author__变量把作者写进去

import sys

def test():
    args = sys.argv
    if len(args)==1:
        print('Hello, world!')
    elif len(args)==2:
        print('Hello, %s!' % args[1])
    else:
        print('Too many arguments!')

if __name__=='__main__':
    test()
```





模块就是一个Python文件，里面有类、函数、变量等，我们可以 拿过来用（导入模块去使用）导入模块的语法

```python
from 模块名 import 模块|类|函数|变量|*  as 别名
```

常用的组合形式如： 

- import 模块名 

- from 模块名 import 类、变量、方法等 

- from 模块名 import * 

- import 模块名 as 别名 

- from 模块名 import 功能名 as 别名


```python
# 导入时间模块
import time
print("开始")
# 让程序睡眠1秒(阻塞)
time.sleep(1)
print("结束")
```

from 模块名 import 功能名

```python
from time import sleep
print("开始")
sleep(10)
print("结束")
```

from 模块名 import *

```python
from time import *
print("开始")
sleep(3)
print("结束")
```

as定义别名

```python
# 模块定义别名
import 模块名 as 别名
# 功能定义别名
from 模块名 import 功能 as 别名
```

```python
# 功能别名
from time import sleep as sl
print("开始")
sl(10)
print("结束")
# 模块别名
import time as tt
tt.sleep(2)
print('hello')
```

##### 自定义模块

 每个Python文件都可以作为一个模块，模块的名字就是文件的名字. 也就是说自定义模块名必须要符合标识符命名规则

定义一个module模块	MyModule.py

```python
def test(a, b):
    print(a + b)
```

```python
#在其他模块里面引用
import MyModule
MyModule.test(10,23)
```

注意事项：当导入多个模块的时候，且模块内有同名功能. 当调用这个同名功能的时候，调用到的是后面导入的模块的功能

自定义模块

```python
__all__=['testA']
def testA():
    print('这是testa')

def testB():
    print("this is test b")
```

测试模块

```python
from MyModule import *
testA()
testB()#不能调用b
#NameError: name 'testB' is not defined
```

#### 4.Python包

基于Python模块，我们可以在编写代码的时候，导入许多外部代码来丰富功能。 但是，如果Python的模块太多了，就可能造成一定的混乱

- 从物理上看，包就是一个文件夹，在该文件夹下包含了一个 __init__.py 文件，该文件夹可用于包含多个模块文件 
- 从逻辑上看，包的本质依然是模块

快速入门

① 新建包`my_package` 

② 新建包内模块：`my_module1` 和 `my_module2` 

③ 模块内代码如下

```python
#模块一 my_module1.py
def testModuleOne(a, b):
    print(a + b)
    print("我是模块1")
#模块2 my_module2.py
def testModuleTwo(a, b):
    print(a - b)
    print("我是模块2")
```

在其他python文件中引入

```python
from my_package import my_module1
from my_package import my_module2
my_module1.testModuleOne(12,23)
my_module2.testModuleTwo(12,234)
```

引入方式二： 

注意：必须在`__init__.py`文件中添加`__all__ = []`，控制允许导入的模块列表

init文件

```python
__all__=['my_module2']
```

测试文件

```python
from my_package import *
my_module2.testModuleTwo(12,23) #因为此处只引入了module2，所以module1不能使用
```

##### 第三方包

在Python程序的生态中，有许多非常多的第三方包（非Python官方），可以极大的帮助我们提高开发效率，如： 

​		• 科学计算中常用的：numpy包 

​		• 数据分析中常用的：pandas包 

​		• 大数据计算中常用的：pyspark、apache-flink包 

​		• 图形可视化常用的：matplotlib、pyecharts 

​		• 人工智能常用的：tensorflow • 等 这些第三方的包，极大的丰富了Python的生态，提高了开发效率。 

但是由于是第三方，所以Python没有内置，所以我们需要安装它们才可以导入使用哦

连接国内的网站进行包的安装： 

https://pypi.tuna.tsinghua.edu.cn/simple 是清华大学提供的一个网站，可供pip程序下载第三方包

```cmd
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple 包名称
```

### 5.数据可视化

Python数据和Json数据的相互转化

```python
# 导入json模块
import json
# 准备符合格式json格式要求的python数据
data = [{"name": "老王", "age": 16}, {"name": "张三", "age": 20}]
# 通过 json.dumps(data) 方法把python数据转化为了 json数据
data = json.dumps(data)
# 通过 json.loads(data) 方法把json数据转化为了 python数据
data = json.loads(data)
```

#### pyecharts模块

Echarts 是个由百度开源的数据可视化，凭借着良好的交互性，精巧的图表设计，得到了众多开发者的认可. 而 Python 是门富有 表达力的语言，很适合用于数据处理. 当数据分析遇上数据可视化时pyecharts 诞生了.

使用在前面学过的pip命令即可快速安装PyEcharts模块

```cmd
pip install pyecharts
```

绘制一个折线图

```python
from pyecharts.charts import Line
line=Line()
#添加x轴坐标
line.add_xaxis(['中国','英国','美国','加拿大'])
line.add_yaxis("GDP",[100,45,67,47])
#生成图标
line.render()
```

程序运行之后会在文件夹下生成一个render.html为网页文件

pyecharts模块中有很多的配置选项, 常用到2个类别的选项: 

- 全局配置选项 
- 系列配置选项

全局配置选项可以通过set_global_opts方法来进行配置

```python
from pyecharts.charts import Line
from pyecharts.options import TitleOpts, LegendOpts, ToolboxOpts, VisualMapOpts
line=Line()
#添加x轴坐标
line.add_xaxis(['中国','英国','美国','加拿大'])
line.add_yaxis("GDP",[100,45,67,47])
line.set_global_opts(
    title_opts=TitleOpts(title="GDP展示", pos_left="center", pos_bottom="1%"),
    legend_opts=LegendOpts(is_show=True),
    toolbox_opts=ToolboxOpts(is_show=True),
    visualmap_opts=VisualMapOpts(is_show=True),
)
#生成图标
line.render()
```

### 6.面向对象

#### 类的定义和使用

```python
class 类名称:
    成员变量
    成员方法
```

#### 成员方法的定义

在类中定义成员方法和定义函数基本一致，但仍有细微区别： 

在方法定义的参数列表中，有一个：self关键字 self关键字是成员方法定义的时候，必须填写的。 

- 它用来表示类对象自身的意思 
- 当我们使用类对象调用方法的是，self会自动被python传入 
- 在方法内部，想要访问类的成员变量，必须使用self

```python
# 定义一个带有成员方法的类
class Student:
    name = None     # 学生的姓名
    def say_hi(self):
        print(f"大家好呀，我是{self.name}，欢迎大家多多关照")
    def say_hi2(self, msg):
        print(f"大家好，我是：{self.name}，{msg}")
stu = Student()
stu.name = "周杰轮"
stu.say_hi2("哎哟不错哟")

stu2 = Student()
stu2.name = "林俊节"
stu2.say_hi2("小伙子我看好你")
```

self关键字，尽管在参数列表中，但是传参的时候可以忽略它。

```python
class Student:
    name = None     # 学生的姓名
    def say_hiNoParam(self):
        print("无参数打印出来结果")
stu =Student()
stu.say_hi()#无参数打印出来结果
```

#### 类和对象

面向对象编程： 设计类，基于类创建对象，由对象做具体的工作

```python
class Clock:
    id = None       # 序列化
    price = None    # 价格
    def ring(self):
        import winsound
        winsound.Beep(2000, 3000)
# 构建2个闹钟对象并让其工作
clock1 = Clock()
clock1.id = "003032"
clock1.price = 19.99
print(f"闹钟ID：{clock1.id}，价格：{clock1.price}")
clock1.ring()
clock2 = Clock()
clock2.id = "003033"
clock2.price = 21.99
print(f"闹钟ID：{clock2.id}，价格：{clock2.price}")
clock2.ring()
```

#### 构造方法

Python类可以使用：__init__()方法，称之为构造方法。

- 在创建类对象（构造类）的时候，会自动执行
- 在创建类对象（构造类）的时候，将传入参数自动传递给__init__方法使用。

```python
class Student:
    def __init__(self, name, age ,tel):
        self.name = name
        self.age = age
        self.tel = tel
        print("Student类创建了一个类对象")
stu = Student("周杰轮", 31, "18500006666")
print(stu.name)
print(stu.age)
print(stu.tel)
```

注意:

- **千万不要忘记init前后都有2个下划线**
- 构造方法也是成员方法，不要忘记在参数列表中提供：self 
- 在构造方法内定义成员变量，需要使用self关键字，变量是定义在构造方法内部，如果要成为成员变量，需要用self来表示。

#### 魔术方法

**str 字符串方法**

我们可以通过__str__方法，控制类转换为字符串的行为。

```python
def __str__(self):
        return f"student类对象，name={self.name},age={self.age},tel={self.tel}"
stu = Student("周杰轮", 31, "18500006666")
print(str(stu))
```

**lt 小于符号**

在类中实现__lt__方法，即可同时完成：小于符号 和 大于符号 2种比较

```python
def __lt__(self, other):
    return self.age < other.age
stu = Student("周杰轮", 31, "18500006666")
stu1=Student("张安",12,"12344546")
print(stu<stu1) #false
```

**le 小于等于比较符号方法**

__le__可用于：<=、>=两种比较运算符上。

```python
def __le__(self, other):
     return self.age <= other.age
stu = Student("周杰轮", 31, "18500006666")
stu1=Student("张安",12,"12344546")
print(stu<stu1) #false
```

**eq，比较运算符实现方法**

- 不实现__eq__方法，对象之间可以比较，但是是比较内存地址，也即是：不同对象==比较一定是False结果。  
- 实现了__eq__方法，就可以按照自己的想法来决定2个对象是否相等了。

```python
def __eq__(self, other):
    return self.age == other.age
stu1 = Student("周杰轮", 31)
stu2 = Student("林俊节", 36)
print(stu1 == stu2)
```

#### 封装 

面向对象包含3大主要特性：  **封装 、 继承 、 多态**

##### 私有成员

类中提供了私有成员的形式来支持。 

​		• 私有成员变量 

​		• 私有成员方法 

定义私有成员的方式非常简单，只需要： 

​		• 私有成员变量：变量名以__开头（2个下划线） __

​		• 私有成员方法：方法名以__开头（2个下划线） 即可完成私有成员的设置

在类中提供仅供内部使用的属性和方法，而不 对外开放（类对象无法使用）

```python
class Phone:
    __current_voltage = 0.5        # 当前手机运行电压
    def __keep_single_core(self):
        print("让CPU以单核模式运行")
    def call_by_5g(self):
        if self.__current_voltage >= 1:
            print("5g通话已开启")
        else:
            self.__keep_single_core()
            print("电量不足，无法使用5g通话，并已设置为单核运行进行省电。")
phone = Phone()
phone.call_by_5g()
```

#### 继承

##### 单继承

继承分为：单继承和多继承 。 

继承表示：将从父类那里继承（复制）来成员变量和成员方法（不含私有）

```python
class Phone:
    Item=None
    product=None
    def call_by_4g(self):
        print("可以进行4g通话")
phone1=Phone()
phone1.call_by_4g()
#单继承机制
class PhonePlus(Phone):
    face_id=None,
    def call_by_5g(self):
        print("可以进行5g通话")
phone2=PhonePlus()
phone2.call_by_4g()#可以进行4g通话
phone2.call_by_5g()#可以进行5g通话
```

##### 多继承 

Python的类之间也支持多继承，即一个类，可以继承多个父类

```python
# 演示多继承
class NFCReader:
    nfc_type = "第五代"
    producer = "HM"
    def read_card(self):
        print("NFC读卡")
    def write_card(self):
        print("NFC写卡")
class RemoteControl:
    rc_type = "红外遥控"
    def control(self):
        print("红外遥控开启了")
class MyPhone(Phone, NFCReader, RemoteControl):
    pass
phone = MyPhone()
phone.call_by_4g()
phone.read_card()
phone.write_card()
phone.control()
print(phone.producer)
```

多个父类中，如果有同名的成员，那么默认以继承顺序（从左到右）为优先级。 即：**先继承的保留，后继承的被覆盖**

 **pass关键字**

作用是什么 pass是占位语句，用来保证函数（方法）或类定义的完整性，表示无内容，空的意思

##### 复写 

子类继承父类的成员属性和成员方法后，如果对其“不满意”，那么可以进行复写。 即：在子类中重新定义同名的属性或方法即可

```python
class Phone:
    Item=None
    product=None
    def call_by_4g(self):
        print("可以进行4g通话")
    def call_by5g(self):
        print("父类的5g")
class PhonePlus(Phone):
    face_id=None,
    def call_by_5g(self):
        # 使用被复写的父类的成员
        super().call_by5g()#父类的5g
        print("子类的5g，可以进行5g通话")
phone2=PhonePlus()
phone2.call_by_5g()#可以进行5g通话
```

如果需要使用被复写的父类的成员，需要特殊的调用方式: 

方式1： • 

调用父类成员 使用成员变量：

​		父类名.成员变量 使用成员方法：

​		父类名.成员方法(self)

```python
print(f"父类的厂商是：{Phone.producer}")
Phone.call_by_5g(self)
```

 • 使用super()调用父类成员 使用成员变量：

​		super().成员变量 使用成员方法：

​		super().成员方法()

```python
def call_by_5g(self):
    # 使用被复写的父类的成员
    super().call_by5g()#父类的5g
    print("子类的5g，可以进行5g通话")
```

#### 类型注解 

Python在3.5版本的时候引入了类型注解，以方便静态类型检查工具，IDE等第三方工具。 

类型注解：在代码中涉及数据交互的地方，提供数据类型的注解（显式的说明）。 

主要功能： 

​		• 帮助第三方IDE工具（如PyCharm）对代码进行类型推断，协助做代码提示 

​		• 帮助开发者自身对变量进行类型注释 支持： 

​		• 变量的类型注解 • 函数（方法）形参列表和返回值的类型注解

##### 类型注解的语法

为变量设置类型注解 

基础语法： 变量: 类型

```python
var_int:int=10
var_float:float=10.0
var_str:str="你好"
#类对象注解
cat:Cat=Cat()
cat.spark()
# 基础容器类型注解
my_list: list = [1, 2, 3]
my_tuple: tuple = (1, 2, 3)
my_dict: dict = {"itheima": 666}
my_list: list[int] = [1, 2, 3]
my_tuple: tuple[int, str, bool] = (1, "itheima", True)
my_dict: dict[str, int] = {"itheima": 666}
```

注意： 

• 元组类型设置类型详细注解，需要将每一个元素都标记出来 

• 字典类型设置类型详细注解，需要2个类型，第一个是key第二个是value

##### 函数（方法）的类型注解

函数和方法的形参类型注解语法：

```python
def 函数名(形参:类型,形参:类型...):
	pass
```

示例：

```python
# 对形参进行类型注解
def add(x: int, y: int):
    return x + y
```

函数（方法）的类型注解 - 返回值注解

```python
def 函数名(形参:类型,形参:类型...)->返回值类型:
	pass
```

示例：

```python
# 对返回值进行类型注解
def func(data: list) -> list:
    return data
```

##### Union类型 

Union联合类型注解，在变量注解、函数（方法）形参和返回值注解中，均可使用。

```python
# 使用Union类型，必须先导包
from typing import Union
my_list: list[Union[int, str]] = [1, 2, "itheima", "itcast"]
def func(data: Union[int, str]) -> Union[int, str]:
    pass
```

#### 多态

同样的行为（函数），传入不同的对象，得到不同的状态

```python
class Animal:
    def spark(self):
        pass
class Dog(Animal):
    def spark(self):
        print("汪汪汪")
class Cat(Animal):
    def spark(self):
        print("喵喵喵")
cat = Cat()
cat.spark()
```

##### 抽象类（接口）

 包含抽象方法的类，称之为抽象类。抽象方法是指：没有具体实现的方法（pass） 称之为抽象方法

### 7.Python&MySQL

#### 操作mysql

Python中使用第三方库来操作MySQL

```cmd
pip install pymysql
```

获取链接对象？

1. ```from pymysql import Connection ```导包
2. ```Connection(主机,端口,账户,密码)```即可得到链接对象
3. ```链接对象.close()``` 关闭和MySQL数据库的连接

```python
from pymysql import Connection
conn=Connection(
    host="localhost",   # 主机名（IP）
    port=3306,          # 端口
    user="root",        # 账户
    password="root",  # 密码
    autocommit=True     # 设置自动提交
)
# 获取游标对象
course=conn.cursor()
#选择数据库
conn.select_db("dormitorys")
#使用游标对象执行sql
course.execute("select * from student")
#获取查询结果
result=course.fetchall()
for r in result:
    print(r)
# 打印数据库信息
print(conn.get_server_info())
conn.close()
```

#### 插入语句

pymysql在执行数据插入或其它产生数据更改的SQL语句时，默认是需要提交更改的，即，需要通过代码“确认”这种更改行为。通过链接对象.commit() 即可确认此行为。

```python
flag=course.execute("insert into student values(64,'020','张三','男',5,'迁出','2023-10-09')")
if(flag):
    print("插入成功")
else:
    print("检查一下，多少有点问题")
```

### 8.函数高阶技巧

#### 闭包

不使用闭包，需要定义一个全局变量，然后再函数内部使用全部变量，尽管功能实现是ok的，但是仍有问题：

- 代码在命名空间上（变量定义）不够干净、整洁
- 全局变量有被修改的风险

```python
account_sum=0
def atm(sum,isSave=True):
    global account_sum
    if isSave:
        account_sum +=sum
        print(f"存货增加{sum}，余额{account_sum}")
    else:
        account_sum-=sum
        print(f"花费{sum}，余额{account_sum}")
atm(356,True)
atm(34,False)
```

在函数嵌套的前提下，内部函数使用了外部函数的变量，并且外部函数返回了内部函数，我们把这个使用外部函数变量的内部函数称为闭包。

```python
def account_create(init_total=0):
    def atm(num,isSave=True):
        nonlocal init_total
        if isSave:
            init_total+=num
            print(f"存货增加{num}，余额{init_total}")
        else:
            init_total-=num
            print(f"花费{num}，余额{init_total}")
    return atm
fn=account_create()
fn(300)#存货增加300，余额300
fn(100,False)#花费100，余额200
```

修改外部函数变量的值

需要使用```nonlocal```关键字修饰外部函数的变量才可在内部函数中修改它

**闭包注意事项**

优点，使用闭包可以让我们得到：

- 无需定义全局变量即可实现通过函数，持续的访问、修改某个值
- 闭包使用的变量的所用于在函数内，难以被错误的调用修改

缺点：

- 由于内部函数持续引用外部函数的值，所以会导致这一部分内存空间不被释放，一直占用内存

#### 装饰器

装饰器其实也是一种闭包， 其功能就是**在不破坏目标函数原有的代码和功能的前提下，为目标函数增加新功能**。

**装饰器的一般写法（闭包写法）**

定义一个闭包函数， 在闭包函数内部：

- 执行目标函数
- 并完成功能的添加

```python
# 装饰器的一般写法（闭包）
def outer(func):
    def inner():
        print("我要休息了")
        func
        print("我起来了")
    return inner
def sleep():
    import time
    print("睡眠中，需要等5秒")
    time.sleep(5)
fn=outer(sleep())
fn()
#睡眠中，需要等5秒
#我要休息了
#我起来了
```

**装饰器的语法糖写法**

使用```@outer```

定义在目标函数sleep之上

```python
def outer(func):
    def inner():
        print("我睡觉了")
        func()
        print("我起床了")

    return inner
@outer
def sleep():
    import random
    import time
    print("睡眠中......")
    time.sleep(random.randint(1, 5))
sleep()
```

## 3.利用python进行数据分析

### 1.准备工作

#### 重要的Python库

**NumPy**

NumPy（Numerical Python的简称）是Python科学计算的基础包。它提供了以下功能（不限于此）：

- 快速高效的多维数组对象ndarray。
- 用于对数组执行元素级计算以及直接对数组执行数学运算的函数。
- 用于读写硬盘上基于数组的数据集的工具。
- 线性代数运算、傅里叶变换，以及随机数生成。成熟的C API， 用于Python插件和原生C、C++、Fortran代码访问NumPy的数据结构和计算工具。

**pandas**

pandas提供了快速便捷处理结构化数据的大量数据结构和函数。

pandas兼具NumPy高性能的数组计算功能以及电子表格和关系型数据库（如SQL）灵活的数据处理功能。它提供了复杂精细的索引功能，能更加便捷地完成重塑、切片和切块、聚合以及选取数据子集等操作。因为数据操作、准备、清洗是数据分析最重要的技能，pandas是本书的重点。

**matplotlib**

matplotlib是最流行的用于绘制图表和其它二维数据可视化的Python库。它最初由John D.Hunter（JDH）创建，目前由一个庞大的开发团队维护。它非常适合创建出版物上用的图表。虽然还有其它的Python可视化库，matplotlib却是使用最广泛的，并且它和其它生态工具配合也非常完美。我认为，可以使用它作为默认的可视化工具。

**IPython和Jupyter**

IPython项目起初是Fernando Pérez在2001年的一个用以加强和Python交互的子项目。在随后的16年中，它成为了Python数据栈最重要的工具之一。虽然IPython本身没有提供计算和数据分析的工具，它却可以大大提高交互式计算和软件开发的生产率。IPython鼓励“执行-探索”的工作流，区别于其它编程软件的“编辑-编译-运行”的工作流。它还可以方便地访问系统的shell和文件系统。因为大部分的数据分析代码包括探索、试错和重复，IPython可以使工作更快

**SciPy**

SciPy是一组专门解决科学计算中各种标准问题域的包的集合，主要包括下面这些包：

- scipy.integrate：数值积分例程和微分方程求解器。
- scipy.linalg：扩展了由numpy.linalg提供的线性代数例程和矩阵分解功能。
- scipy.optimize：函数优化器（最小化器）以及根查找算法。
- scipy.signal：信号处理工具。
- scipy.sparse：稀疏矩阵和稀疏线性系统求解器。
- scipy.special：SPECFUN（这是一个实现了许多常用数学函数（如伽玛函数）的Fortran库）的包装器。
- scipy.stats：标准连续和离散概率分布（如密度函数、采样器、连续分布函数等）、各种统计检验方法，以及更好的描述统计法。

**NumPy和SciPy结合使用，便形成了一个相当完备和成熟的计算平台，可以处理多种传统的科学计算问题。**

**scikit-learn**

2010年诞生以来，scikit-learn成为了Python的通用机器学习工具包。它的子模块包括：

- 分类：SVM、近邻、随机森林、逻辑回归等等。
- 回归：Lasso、岭回归等等。
- 聚类：k-均值、谱聚类等等。
- 降维：PCA、特征选择、矩阵分解等等。
- 选型：网格搜索、交叉验证、度量。
- 预处理：特征提取、标准化。

与pandas、statsmodels和IPython一起，scikit-learn对于Python成为高效数据科学编程语言起到了关键作用。

**statsmodels**

statsmodels是一个统计分析包，起源于斯坦福大学统计学教授Jonathan Taylor，他设计了多种流行于R语言的回归分析模型。它提供了statsmodels的公式或模型的规范框架。与scikit-learn比较，statsmodels包含**经典统计学和经济计量学**的算法。包括如下子模块：

- 回归模型：线性回归，广义线性模型，健壮线性模型，线性混合效应模型等等。
- 方差分析（ANOVA）。
- 时间序列分析：AR，ARMA，ARIMA，VAR和其它模型。
- 非参数方法： 核密度估计，核回归。
- 统计模型结果可视化。

statsmodels更关注与统计推断，提供不确定估计和参数p-值。相反的，scikit-learn注重预测。

#### 相关工具

**运行IPython Shell**

你可以用`ipython`在命令行打开IPython Shell，就像打开普通的Python解释器：

**运行Jupyter Notebook**

notebook是Jupyter项目的重要组件之一，它是一个代码、文本（有标记或无标记）、数据可视化或其它输出的交互式文档。Jupyter Notebook需要与内核互动，内核是Jupyter与其它编程语言的交互编程协议。Python的Jupyter内核是使用IPython。要启动Jupyter，在命令行中输入`jupyter notebook`:

tab补全

从外观上，IPython shell和标准的Python解释器只是看起来不同。IPython shell的进步之一是具备其它IDE和交互计算分析环境都有的tab补全功能。在shell中输入表达式，按下Tab，会搜索已输入变量（对象、函数等等）的命名空间：

**自省**

- 在变量前后使用问号？，可以显示对象的信息

```python
In [8]: b = [1, 2, 3]
In [9]: b?
Type:       list
String Form:[1, 2, 3]
Length:     3
Docstring:
list() -> new empty list
list(iterable) -> new list initialized from iterable's items
```

- 使用??会显示函数的源码：

- ?还有一个用途，就是像Unix或Windows命令行一样搜索IPython的命名空间。字符与通配符结合可以匹配所有的名字。例如，我们可以获得所有包含load的顶级NumPy命名空间：

```python
In [13]: np.*load*?
	np.__loader__
	np.load
	np.loads
	np.loadtxt
	np.pkgload
```

**%run命令**

你可以用`%run`命令运行所有的Python程序。假设有一个文件`ipython_script_test.py`：

```python
def f(x, y, z):
    return (x + y) / z
a = 5
b = 6
c = 7.5
result = f(a, b, c)
```

### 2.NumPy基础：数组和矢量计算

NumPy（Numerical Python的简称）是Python数值计算最重要的基础包。大多数提供科学计算的包都是用NumPy的数组作为构建基础。

NumPy的部分功能如下：

- ndarray，一个具有矢量算术运算和复杂广播能力的快速且节省空间的多维数组。
- 用于对整组数据进行快速运算的标准数学函数（无需编写循环）。
- 用于读写磁盘数据的工具以及用于操作内存映射文件的工具。
- 线性代数、随机数生成以及傅里叶变换功能。
- 用于集成由C、C++、Fortran等语言编写的代码的A C API。

#### NumPy的ndarray

NumPy最重要的一个特点就是其N维数组对象（即ndarray），该对象是一个快速而灵活的大数据集容器。你可以利用这种数组对整块数据执行一些数学运算，其语法跟标量元素之间的运算一样。

```python
data = np.random.randn(2,3)
data
'''
打印出来的结果，生成了一个二行三列的数组
array([[-0.68320053,  1.29637756,  2.55664198],
       [ 1.05399535,  0.54989989,  0.68095503]])
ndarray是一个通用的同构数据多维容器，也就是说，其中的所有元素必须是相同类型的。每个数组都有一个shape（一个表示各维度大小的元组）和一个dtype（一个用于说明数组数据类型的对象）：
'''
```

**创建ndarray**

创建数组最简单的办法就是使用array函数。它接受一切序列型的对象（包括其他数组），然后产生一个新的含有传入数据的NumPy数组。以一个列表的转换为例：

```python
data1=[1,3,5,6,7]
arr1=np.array(data1)
arr1
#生成一个二维数组
data2 = [[1, 2, 3, 4], [5, 6, 7, 8]]
arr2=np.array(data2)
arr2
```

除np.array之外，还有一些函数也可以新建数组。比如，zeros和ones分别可以创建指定长度或形状的全0或全1数组。empty可以创建一个没有任何具体值的数组。要用这些方法创建多维数组，只需传入一个表示形状的元组即可：

```ipython
In [29]: np.zeros(10)
Out[29]: array([ 0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.])
In [30]: np.zeros((3, 6))
Out[30]: 
array([[ 0.,  0.,  0.,  0.,  0.,  0.],
       [ 0.,  0.,  0.,  0.,  0.,  0.],
       [ 0.,  0.,  0.,  0.,  0.,  0.]])
In [31]: np.empty((2, 3, 2))
Out[31]: 
array([[[ 0.,  0.],
        [ 0.,  0.],
        [ 0.,  0.]],
       [[ 0.,  0.],
        [ 0.,  0.],
        [ 0.,  0.]]])
```

**ndarray的数据类型**

dtype（数据类型）是一个特殊的对象，它含有ndarray将一块内存解释为特定数据类型所需的信息：

```python
arr1 = np.array([1, 2, 3], dtype=np.float64)
arr2 = np.array([1, 2, 3], dtype=np.int32)
```

#### NumPy数组的运算

数组很重要，因为它使你不用编写循环即可对数据执行批量运算。NumPy用户称其为矢量化（vectorization）。大小相等的数组之间的任何算术运算都会将运算应用到元素级：

```python
In [51]: arr = np.array([[1., 2., 3.], [4., 5., 6.]])
In [52]: arr
Out[52]: 
array([[ 1.,  2.,  3.],
       [ 4.,  5.,  6.]])
In [53]: arr * arr
Out[53]: 
array([[  1.,   4.,   9.],
       [ 16.,  25.,  36.]])
In [54]: arr - arr
Out[54]: 
array([[ 0.,  0.,  0.],
       [ 0.,  0.,  0.]])
```

大小相同的数组之间的比较会生成布尔值数组：

```python
In [57]: arr2 = np.array([[0., 4., 1.], [7., 2., 12.]])
In [58]: arr2
Out[58]: 
array([[  0.,   4.,   1.],
       [  7.,   2.,  12.]])
In [59]: arr2 > arr
Out[59]:
array([[False,  True, False],
[ True, False,  True]], dtype=bool)
```

**花式索引**

花式索引（Fancy indexing）是一个NumPy术语，它指的是利用整数数组进行索引。假设我们有一个8×4数组：

```python
In [117]: arr = np.empty((8, 4))

In [118]: for i in range(8):
   .....:     arr[i] = i

In [119]: arr
Out[119]: 
array([[ 0.,  0.,  0.,  0.],
       [ 1.,  1.,  1.,  1.],
       [ 2.,  2.,  2.,  2.],
       [ 3.,  3.,  3.,  3.],
       [ 4.,  4.,  4.,  4.],
       [ 5.,  5.,  5.,  5.],
       [ 6.,  6.,  6.,  6.],
       [ 7.,  7.,  7.,  7.]])
```

为了以特定顺序选取行子集，只需传入一个用于指定顺序的整数列表或ndarray即可：

```python
In [120]: arr[[4, 3, 0, 6]]
Out[120]: 
array([[ 4.,  4.,  4.,  4.],
       [ 3.,  3.,  3.,  3.],
       [ 0.,  0.,  0.,  0.],
       [ 6.,  6.,  6.,  6.]])
```

**数组转置和轴对换**

转置是重塑的一种特殊形式，它返回的是源数据的视图（不会进行任何复制操作）。数组不仅有transpose方法，还有一个特殊的T属性：

```python
In [126]: arr = np.arange(15).reshape((3, 5))
In [127]: arr
Out[127]: 
array([[ 0,  1,  2,  3,  4],
       [ 5,  6,  7,  8,  9],
       [10, 11, 12, 13, 14]])
In [128]: arr.T
Out[128]: 
array([[ 0,  5, 10],
       [ 1,  6, 11],
       [ 2,  7, 12],
       [ 3,  8, 13],
       [ 4,  9, 14]])
```

在进行矩阵计算时，经常需要用到该操作，比如利用np.dot计算矩阵内积：

```python
In [129]: arr = np.random.randn(6, 3)
In [130]: arr
Out[130]: 
array([[-0.8608,  0.5601, -1.2659],
       [ 0.1198, -1.0635,  0.3329],
       [-2.3594, -0.1995, -1.542 ],
       [-0.9707, -1.307 ,  0.2863],
       [ 0.378 , -0.7539,  0.3313],
       [ 1.3497,  0.0699,  0.2467]])
In [131]: np.dot(arr.T, arr)
Out[131]:
array([[ 9.2291,  0.9394,  4.948 ],
       [ 0.9394,  3.7662, -1.3622],
       [ 4.948 , -1.3622,  4.3437]])
```

对于高维数组，transpose需要得到一个由轴编号组成的元组才能对这些轴进行转置（比较费脑子）：

```python
In [132]: arr = np.arange(16).reshape((2, 2, 4))
In [133]: arr
Out[133]: 
array([[[ 0,  1,  2,  3],
        [ 4,  5,  6,  7]],
       [[ 8,  9, 10, 11],
        [12, 13, 14, 15]]])
In [134]: arr.transpose((1, 0, 2))
Out[134]: 
 #这里，第一个轴被换成了第二个，第二个轴被换成了第一个，最后一个轴不变。
array([[[ 0,  1,  2,  3],
        [ 8,  9, 10, 11]],
       [[ 4,  5,  6,  7],
        [12, 13, 14, 15]]])
```

**通用函数：快速的元素级数组函数**

通用函数（即ufunc）是一种对ndarray中的数据执行元素级运算的函数。你可以将其看做简单函数（接受一个或多个标量值，并产生一个或多个标量值）的矢量化包装器。

```python
In [137]: arr = np.arange(10)
In [138]: arr
Out[138]: array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
In [139]: np.sqrt(arr)
Out[139]: 
array([ 0.    ,  1.    ,  1.4142,  1.7321,  2.    ,  2.2361,  2.4495,
        2.6458,  2.8284,  3.    ])
In [140]: np.exp(arr)
Out[140]: 
array([    1.    ,     2.7183,     7.3891,    20.0855,    54.5982,
         148.4132,   403.4288,  1096.6332,  2980.958 ,  8103.0839])
```

**利用数组进行数据处理**

NumPy数组使你可以将许多种数据处理任务表述为简洁的数组表达式（否则需要编写循环）。用数组表达式代替循环的做法，通常被称为矢量化。一般来说，矢量化数组运算要比等价的纯Python方式快上一两个数量级（甚至更多），尤其是各种数值计算。

**将条件逻辑表述为数组运算**

numpy.where函数是三元表达式```x if condition else y```的矢量化版本。假设我们有一个布尔数组和两个值数组：

```python
xarr = np.array([1.1, 1.2, 1.3, 1.4, 1.5])
yarr = np.array([2.1, 2.2, 2.3, 2.4, 2.5])
cond = np.array([True, False, True, True, False])
result=[(x if c else y) for x,y,c in zip(xarr,yarr,cond)]
result
'''
最后的结果
[1.1, 2.2, 1.3, 1.4, 2.5]
'''
```

这有几个问题。第一，它对大数组的处理速度不是很快（因为所有工作都是由纯Python完成的）。第二，无法用于多维数组。若使用np.where，则可以将该功能写得非常简洁：

```python
In [170]: result = np.where(cond, xarr, yarr)
In [171]: result
Out[171]: array([ 1.1,  2.2,  1.3,  1.4,  2.5])
```

**数学和统计方法**

可以通过数组上的一组数学函数对整个数组或某个轴向的数据进行统计计算。sum、mean以及标准差std等聚合计算（aggregation，通常叫做约简（reduction））既可以当做数组的实例方法调用，也可以当做顶级NumPy函数使用。

```python
arr = np.random.randn(5, 4)
arr.mean()#求平均值
arr.sum()
```

**用于布尔型数组的方法**

在上面这些方法中，布尔值会被强制转换为1（True）和0（False）。因此，sum经常被用来对布尔型数组中的True值计数：

```python
In [190]: arr = np.random.randn(100)
In [191]: (arr > 0).sum() # Number of positive values
Out[191]: 42
```

另外还有两个方法any和all，它们对布尔型数组非常有用。any用于测试数组中是否存在一个或多个True，而all则检查数组中所有值是否都是True：

```python
In [192]: bools = np.array([False, False, True, False])
In [193]: bools.any()
Out[193]: True
In [194]: bools.all()
Out[194]: False
```

这两个方法也能用于非布尔型数组，所有非0元素将会被当做True。

#### 排序

跟Python内置的列表类型一样，NumPy数组也可以通过sort方法就地排序：

```python
In [195]: arr = np.random.randn(6)
In [196]: arr
Out[196]: array([ 0.6095, -0.4938,  1.24  , -0.1357,  1.43  , -0.8469])
In [197]: arr.sort()
In [198]: arr
Out[198]: array([-0.8469, -0.4938, -0.1357,  0.6095,  1.24  ,  1.43  ])
```

多维数组可以在任何一个轴向上进行排序，只需将轴编号传给sort即可：

```python
In [199]: arr = np.random.randn(5, 3)
In [200]: arr
Out[200]: 
array([[ 0.6033,  1.2636, -0.2555],
       [-0.4457,  0.4684, -0.9616],
       [-1.8245,  0.6254,  1.0229],
       [ 1.1074,  0.0909, -0.3501],
       [ 0.218 , -0.8948, -1.7415]])
In [201]: arr.sort(1)
In [202]: arr
Out[202]: 
array([[-0.2555,  0.6033,  1.2636],
       [-0.9616, -0.4457,  0.4684],
       [-1.8245,  0.6254,  1.0229],
       [-0.3501,  0.0909,  1.1074],
       [-1.7415, -0.8948,  0.218 ]])
```

#### 线性代数

线性代数（如矩阵乘法、矩阵分解、行列式以及其他方阵数学等）是任何数组库的重要组成部分。不像某些语言（如MATLAB），通过*对两个二维数组相乘得到的是一个元素级的积，而不是一个矩阵点积。因此，NumPy提供了一个用于矩阵乘法的dot函数（既是一个数组方法也是numpy命名空间中的一个函数）：

```python
x = np.array([[1., 2., 3.], [4., 5., 6.]])
y = np.array([[6., 23.], [-1, 7], [8, 9]])
x.dot(y)#内积
#x.dot(y)等价于np.dot(x, y)：
```

#### 伪随机数生成

numpy.random模块对Python内置的random进行了补充，增加了一些用于高效生成多种概率分布的样本值的函数。例如，你可以用normal来得到一个标准正态分布的4×4样本数组：

```python
samples = np.random.normal(size=(4, 4))
samples
Out[239]: 
array([[ 0.5732,  0.1933,  0.4429,  1.2796],
       [ 0.575 ,  0.4339, -0.7658, -1.237 ],
       [-0.5367,  1.8545, -0.92  , -0.1082],
       [ 0.1525,  0.9435, -1.0953, -0.144 ]])
```

这些都是伪随机数，是因为它们都是通过算法基于随机数生成器种子，在确定性的条件下生成的。你可以用NumPy的```np.random.seed```更改随机数生成种子

### 3.pandas入门

pandas含有使数据清洗和分析工作变得更快更简单的数据结构和操作工具。pandas经常和其它工具一同使用，如数值计算工具NumPy和SciPy，分析库statsmodels和scikit-learn，和数据可视化库matplotlib。pandas是基于NumPy数组构建的，特别是基于数组的函数和不使用for循环的数据处理。

#### 1.pandas的数据结构介绍

Series和DataFrame是pandas的两个主要数据结构，虽然它们并不能解决所有问题，但它们为大多数应用提供了一种可靠的、易于使用的基础。

##### Series

Series是一种类似于一维数组的对象，它由一组数据（各种NumPy数据类型）以及一组与之相关的数据标签（即索引）组成。仅由一组数据即可产生最简单的Series：

```python
In [11]: obj = pd.Series([4, 7, -5, 3])
In [12]: obj
Out[12]: 
0    4
1    7
2   -5
3    3
dtype: int64
obj.values	#值
obj.index	#索引值
obj2 = pd.Series([4, 7, -5, 3], index=['d', 'b', 'a', 'c'])
obj2[['c', 'a', 'd']]
#使用NumPy函数或类似NumPy的运算（如根据布尔型数组进行过滤、标量乘法、应用数学函数等）都会保留索引值的链接
obj2[obj2 > 0]
obj2 * 2
np.exp(obj2)
'a' in obj2 # True
```

对于许多应用而言，Series最重要的一个功能是，它会根据运算的索引标签自动对齐数据：

```python
In [35]: obj3
Out[35]: 
Ohio      35000
Oregon    16000
Texas     71000
Utah       5000
dtype: int64
In [36]: obj4
Out[36]: 
California        NaN
Ohio          35000.0
Oregon        16000.0
Texas         71000.0
dtype: float64
In [37]: obj3 + obj4
Out[37]: 
California         NaN
Ohio           70000.0
Oregon         32000.0
Texas         142000.0
Utah               NaN
dtype: float64
```

##### DataFrame

DataFrame是一个表格型的数据结构，它含有一组有序的列，每列可以是不同的值类型（数值、字符串、布尔值等）。DataFrame既有行索引也有列索引，它可以被看做由Series组成的字典（共用同一个索引）。DataFrame中的数据是以一个或多个二维块存放的（而不是列表、字典或别的一维数据结构）。有关DataFrame内部的技术细节远远超出了本书所讨论的范围。

```python
data = {'state': ['Ohio', 'Ohio', 'Ohio', 'Nevada', 'Nevada', 'Nevada'],
        'year': [2000, 2001, 2002, 2001, 2002, 2003],
        'pop': [1.5, 1.7, 3.6, 2.4, 2.9, 3.2]}
frame=pd.DataFrame(data)
frame
'''
打印出来的效果
	state	year	pop
0	Ohio	2000	1.5
1	Ohio	2001	1.7
2	Ohio	2002	3.6
3	Nevada	2001	2.4
4	Nevada	2002	2.9
5	Nevada	2003	3.2
'''
```

##### 索引对象

pandas的索引对象负责管理轴标签和其他元数据（比如轴名称等）。构建Series或DataFrame时，所用到的任何数组或其他序列的标签都会被转换成一个Index：

```python
In [76]: obj = pd.Series(range(3), index=['a', 'b', 'c'])
In [77]: index = obj.index
In [78]: index
Out[78]: Index(['a', 'b', 'c'], dtype='object')
In [79]: index[1:]
Out[79]: Index(['b', 'c'], dtype='object')
```

Index对象是不可变的，因此用户不能对其进行修改：

```python
index[1] = 'd'  # TypeError
```

与python的集合不同，pandas的Index可以包含重复的标签：

```python
dup_labels = pd.Index(['foo', 'foo', 'bar', 'bar'])
```

#### 2.基本功能

##### 重新索引

pandas对象的一个重要方法是reindex，其作用是创建一个新对象，它的数据符合新的索引。看下面的例子：

```python
obj=pd.Series([4.5, 7.2, -5.3, 3.6], index=['d', 'b', 'a', 'c'])
obj
```

用该Series的reindex将会根据新索引进行重排。如果某个索引值当前不存在，就引入缺失值：

```python
obj=obj.reindex(['a','b','c','d','e'])
obj
'''
a   -5.3
b    7.2
c    3.6
d    4.5
e    NaN
dtype: float64
'''
```

##### 丢弃指定轴上的项

丢弃某条轴上的一个或多个项很简单，只要有一个索引数组或列表即可。由于需要执行一些数据整理和集合逻辑，所以drop方法返回的是一个在指定轴上删除了指定值的新对象：

```python
In [105]: obj = pd.Series(np.arange(5.), index=['a', 'b', 'c', 'd', 'e'])
In [107]: new_obj = obj.drop('c')
In [109]: obj.drop(['d', 'c'])
#对于DataFrame，可以删除任意轴上的索引值。
In [110]: data = pd.DataFrame(np.arange(16).reshape((4, 4)),
   .....:                     index=['Ohio', 'Colorado', 'Utah', 'New York'],
   .....:                     columns=['one', 'two', 'three', 'four'])
In [111]: data
Out[111]: 
          one  two  three  four
Ohio        0    1      2     3
Colorado    4    5      6     7
Utah        8    9     10    11
New York   12   13     14    15
#用标签序列调用drop会从行标签（axis 0）删除值：
In [112]: data.drop(['Colorado', 'Ohio'])
Out[112]: 
         one  two  three  four
Utah        8    9     10    11
New York   12   13     14    15
#通过传递axis=1或axis='columns'可以删除列的值：
In [113]: data.drop('two', axis=1)
Out[113]: 
          one  three  four
Ohio        0      2     3
Colorado    4      6     7
Utah        8     10    11
New York   12     14    15
In [114]: data.drop(['two', 'four'], axis='columns')
Out[114]: 
          one  three
Ohio        0      2
Colorado    4      6
Utah        8     10
New York   12     14
```

##### 用loc和iloc进行选取

对于DataFrame的行的标签索引，我引入了特殊的标签运算符loc和iloc。它们可以让你用类似NumPy的标记，使用轴标签（loc）或整数索引（iloc），从DataFrame选择行和列的子集。

```python
import numpy as np
data = pd.DataFrame(np.arange(16).reshape((4, 4)),
                      index=['Ohio', 'Colorado', 'Utah', 'New York'],
                      columns=['one', 'two', 'three', 'four'])
data.loc['Colorado',['two','three']]
'''
   		   one  two  three  four
Ohio        0    1      2     3
Colorado    4    5      6     7
Utah        8    9     10    11
New York   12   13     14    15
'''
'''
two      5
three    6
Name: Colorado, dtype: int64
'''
#用iloc和整数进行选取：
data.iloc[2, [3, 0, 1]]
'''
four    11
one      8
two      9
Name: Utah, dtype: int32
'''
```

##### 算术运算和数据对齐

pandas最重要的一个功能是，它可以对不同索引的对象进行算术运算。在将对象相加时，如果存在不同的索引对，则结果的索引就是该索引对的并集。对于有数据库经验的用户，这就像在索引标签上进行自动外连接。看一个简单的例子：

```python
s1 = pd.Series([7.3, -2.5, 3.4, 1.5], index=['a', 'c', 'd', 'e'])
s2 = pd.Series([-2.1, 3.6, -1.5, 4, 3.1],index=['a', 'c', 'e', 'f', 'g'])
s1+s2
'''
a    5.2
c    1.1
d    NaN
e    0.0
f    NaN
g    NaN
dtype: float64
'''
```

自动的数据对齐操作在不重叠的索引处引入了NA值。缺失值会在算术运算过程中传播。对于DataFrame，对齐操作会同时发生在行和列上：

##### 在算术方法中填充值

在对不同索引的对象进行算术运算时，你可能希望当一个对象中某个轴标签在另一个对象中找不到时填充一个特殊值（比如0）：

```python
In [165]: df1 = pd.DataFrame(np.arange(12.).reshape((3, 4)),
   .....:                    columns=list('abcd'))
In [166]: df2 = pd.DataFrame(np.arange(20.).reshape((4, 5)),
   .....:                    columns=list('abcde'))
In [167]: df2.loc[1, 'b'] = np.nan
In [168]: df1
Out[168]: 
    a    b     c     d
0  0.0  1.0   2.0   3.0
1  4.0  5.0   6.0   7.0
2  8.0  9.0  10.0  11.0
In [169]: df2
'''
Out[169]: 
     a     b     c     d     e
0   0.0   1.0   2.0   3.0   4.0
1   5.0   NaN   7.0   8.0   9.0
2  10.0  11.0  12.0  13.0  14.0
3  15.0  16.0  17.0  18.0  19.0
'''
```

##### DataFrame和Series之间的运算

默认情况下，DataFrame和Series之间的算术运算会将Series的索引匹配到DataFrame的列，然后沿着行一直向下广播：

```python
In [179]: frame = pd.DataFrame(np.arange(12.).reshape((4, 3)),
   .....:                      columns=list('bde'),
   .....:                      index=['Utah', 'Ohio', 'Texas', 'Oregon'])
In [180]: series = frame.iloc[0]
In [181]: frame
    '''
    Out[181]: 
          b     d     e
Utah    0.0   1.0   2.0
Ohio    3.0   4.0   5.0
Texas   6.0   7.0   8.0
Oregon  9.0  10.0  11.0
    '''
In [182]: series
Out[182]: 
b    0.0
d    1.0
e    2.0
Name: Utah, dtype: float64
```

##### 函数应用和映射

NumPy的ufuncs（元素级数组方法）也可用于操作pandas对象：

##### 排序和排名

根据条件对数据集排序（sorting）也是一种重要的内置运算。要对行或列索引进行排序（按字典顺序），可使用sort_index方法，它将返回一个已排序的新对象：

```python
obj = pd.Series(range(4), index=['d', 'a', 'b', 'c'])
obj.sort_index()
'''
a    1
b    2
c    3
d    0
dtype: int64
'''
```

#### 3.汇总和计算描述统计

pandas对象拥有一组常用的数学和统计方法。它们大部分都属于约简和汇总统计，用于从Series中提取单个值（如sum或mean）或从DataFrame的行或列中提取一个Series。跟对应的NumPy数组方法相比，它们都是基于没有缺失数据的假设而构建的。

```python
In [230]: df = pd.DataFrame([[1.4, np.nan], [7.1, -4.5],
   .....:                    [np.nan, np.nan], [0.75, -1.3]],
   .....:                   index=['a', 'b', 'c', 'd'],
   .....:                   columns=['one', 'two'])

In [231]: df
Out[231]: 
    one  two
a  1.40  NaN
b  7.10 -4.5
c   NaN  NaN
d  0.75 -1.3
#调用DataFrame的sum方法将会返回一个含有列的和的Series：
In [232]: df.sum()
Out[232]: 
one    9.25
two   -5.80
dtype: float64
#传入axis='columns'或axis=1将会按行进行求和运算：
In [233]: df.sum(axis=1)
Out[233]:
a    1.40
b    2.60
c     NaN
d   -0.55
#NA值会自动被排除，除非整个切片（这里指的是行或列）都是NA。通过skipna选项可以禁用该功能：
In [234]: df.mean(axis='columns', skipna=False)
Out[234]: 
a      NaN
b    1.300
c      NaN
d   -0.275
dtype: float64
```

