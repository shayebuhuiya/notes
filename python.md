# Python入门篇

### 例：温度转换程序

~~~python
#TempConvert_loop.py
for i in range(3):
    val = input("请输入带符号的温度值：")
    if val[-1] in ['c', 'C']:
        f = 1.8 * float(val[0:-1]) + 32
        print("转换后的温度为：%.2fF"%f)
    elif val[-1] in ['f', 'F']:
        c = (float(val[0:-1]) - 32) / 1.8
        print("转换后的温度为：%.2fC"%c)
    else:
        print("输入有误")
~~~

1. 循环：for i in range(3)
2. 分支：if、elif、else
3. 输入输出：input、print
4. 变量：f、c。不用表示类型，需要指定某种类型时直接转换即可
5. 字符串：
   1. 左开右闭遍历val[0:-1]，遍历第一个到最后一个字符串前一个字符
   2. val[-1]表示最后一个字符
   3. 输入都是以字符串形式存在，使用时需要转换
   4. str in [...]表示判断str是否在后面的字符列表中
6. 注释：加#号

### 例：蟒蛇程序

~~~python
import turtle

def drawSnake(rad, angle, len, neckrad):
    for i in range(len):
        turtle.circle(rad, angle)	#指定乌龟爬行半径与角度
        turtle.circle(-rad, angle)
    turtle.circle(rad, angle / 2)
    turtle.fd(rad)					#乌龟向前爬行
    turtle.circle(neckrad + 1, 180)
    turtle.fd(rad*2/3)

def main():
    turtle.setup(1300, 800, 0, 0)
    pythonsize = 30
    turtle.pensize(pythonsize)		#路径宽度
    turtle.pencolor("blue")			#颜色
    turtle.seth(-40)
    drawSnake(40, 80, 5, pythonsize / 2)

main()
~~~

1. import：引入外部库
2. from <库名> import <函数名>:使用函数时不需要使用库名
3. from <库名> import *:使用全部该库函数不需要使用库名
4. def：定义函数，不调用不会自动运行
5. turtle库函数，通过turtle.xxx来运行，也可以在import时使用as来为turtle取别名使用turtle库

## 类型及应用

### 数字和字符串类型

* 类型是编程语言对数据的一种划分，编程语言不允许出现歧义

* python语言的类型：数字类型、字符串类型、元组类型、列表类型、文件类型、字典类型

#### 数字类型

	* 整数类型
	* 浮点数类型
	* 复数类型：负数z：z = a + bj，z.real \ z.imag

三种类型存在扩展关系：整数->浮点数->复数。不同类型进行混合运行，生成结果为最宽类型 

三种类型可以互相转换：int()、float()、complex()

例：int(4.5) = 4 , float(4) = 4.0

数字类型的推断：type(x)

例：type(4.5)		#<class 'float'>

| 运算符       | 含义                     |
| ------------ | ------------------------ |
| x + y        | x与y之和                 |
| x - y        | x与y之差                 |
| x * y        | x与y之积                 |
| x / y        | x与y之商                 |
| x // y       | 不大于x与y之商的最大整数 |
| x % y        | x与y之商的余数           |
| +x           | x                        |
| -x           | x的负值                  |
| x ** y       | x的y次幂                 |
| abs(x)       | x的绝对值                |
| divmod(x, y) | (x // y, x % y)          |
| pow(x, y)    | x的y次幂                 |

复合运算符

+=, -= , *= , /= , //=

#### 字符串类型

* 字符串是用双引号或者单引号括起来的一个或多个字符

  * str1 = "Hello"
  * str2 = 'John'
  * str3 = """xxxxx"""。和多行注释的写法一样，支持换行

* 字符串拼接

  * +号拼接：str1 + "xxxx" + str2 + .....
  * %占位拼接：message = "xxx%sxx %s" % (str1, str2)

  %s表示将数字变成字符串插入位置，常用占位类型有%d, %f, %s

* 精度控制

  使用m.n来控制数据的宽度与精度

  * m控制宽度，要求数字，设置的宽度小于数字自身时不会生效
  * .n控制小数点精度，要求是数字，会进行小数的四舍五入

* 字符串格式化快速写法

  f"xxxxx{str1}, xxxxx{str2}{str3}...."，不做精度控制

* 表达式的格式化：

  例：print("1 * 1 的结果是： %d" % (1 * 1))

  print("字符串在Python中的类型是：%s" % type("字符串"))

## 输入输出

### 输入

str = input("xxx")

不管输入什么，输入都会将其转换成字符串类型

可以通过类型转换将其转换成其他类型，如：

num = int(num)

### 输出

print(str)

可以通过前面的字符串控制来控制输出

**print输出内容默认换行，但是可以通过print(str, end = '')来实现不换行**

## 判断语句

* 布尔类型：True(1)、False(0) <class 'bool'>

* if：通过缩进来区分语句范围

  ~~~py
  if True:
      语句1
      语句2
  elif xxx:
      语句4
  else:
      语句5
  语句3
  ~~~

  if, elif, ... , elif, else是从前往后逐次判断的，判断对了就退出语句

## 循环语句

~~~py
while 条件:
    xxx
    xxx
    xxx
~~~

循环嵌套：同样是通过缩进来体现的

~~~py
for 临时变量 in 序列类型:
    xxx
~~~

理论上讲，Python中的for循环无法构建无限循环，因为数据集不可能无限大。

range(num1 = 0, num2, step = 1)语句：

* range(num): 0->num - 1

* range(num1, num2): num1->num2 - 1 

* range(num1, num2, step): num1 -> num1 + step -> ... -> xxx(num1 + step < num2)

~~~python
#range(5) = [0, 1, 2, 3, 4]
#range(5, 10) = [5, 6, 7 ,8, 9]
#range(5, 10, 2) = [5, 7, 9]
for n in range(num)
~~~

变量作用域：for循环中的临时变量规范上是不能在循环外获取的，但是实际上可以（不建议）

continue:跳过本次循环

break:结束循环

## 函数

函数是组织好的，可重复使用的，实现特定功能的代码段

len(str)  #str的长度

~~~py
def fun(arg...):
    """
    fun描述
    :param 1: ...
    :param 2: ...
    :return: ...
    """
	xxx
    xxx
    xxx
    return 返回值
~~~

提高复用性，减少重复代码，提升开发效率

None类型：类型是<class 'NoneType'>，不返回值时返回值是None

if not None == true

变量定义 name = None

函数嵌套调用：函数里面调用其他函数

变量作用域：在一个语句或函数中定义的变量只在语句或者函数体中有效

**在修改全局变量的时候，必须要先用global声明全局变量**

# Python特色篇

## 数据容器

定义：数据容器是可以容纳多份数据的数据类型，容纳的每一份数据称之为一个元素，每一个元素，可以是任意类型的数据，如字符串，数字，布尔等。

数据容器根据：

* 是否支持重复元素
* 是否可以修改
* 是否有序等

分为5类，分别是：

列表（list）、元组（tuple）、字符串（str）、集合（set）、字典（dict）

### list

**基本语法：**

~~~py
#字面量
[元素1, 元素2, 元素3, 元素4, ...]

#定义变量
变量名称 = [元素1, 元素2, 元素3, 元素4, ...]

#定义空列表
变量名称 = []
变量名称 = list()

#类型
type(list) #<class 'list'>
~~~

注意：列表可以一次存储多个数据，且可以为不同的数据类型，支持嵌套（列表元素为列表）

**取数据**

使用下标索引

下标索引从0开始，依次递增

**特色：也可以反向索引，也就是从后向前，从-1开始，依次递减**

~~~py
my_list = ["Tom", "Lily", "Rose"]

print(my_list[-1])	#Rose
print(my_list[-2])	#Lily
print(my_list[-3])	#Tom
~~~

**列表的常用操作**

* 查找元素
* 插入元素
* 删除元素
* 清空元素
* 修改元素
* 统计元素个数

~~~python
my_list = ["Tom", "Lily", "Rose"]

#index方法：查找元素
index = my_list.index("Lily")	#index = 1
index = my_list.index("xiao")	#ValueError: 'xiao' is not in list

#[]方法：读写方式访问
my_list[2] = "xiao"
print(f"列表修改为: {my_list}") #["Tom", "Lily", "xiao"]

#insert方法：指定位置插入
my_list.insert(3, "zhong")
print(f"列表修改为: {my_list}") #["Tom", "Lily", "xiao", "zhong"]

#append方法：尾部插入
my_list.append("fang")
print(f"列表修改为: {my_list}") #["Tom", "Lily", "xiao", "zhong", "fang"]

#extend方法：将数据从其他容器取出，然后查到列表尾部
my_list2 = ["zhao", "qian"]
my_list.extend(my_list2)
print(f"列表修改为: {my_list}") #["Tom", "Lily", "xiao", "zhong", "fang", "zhao", "qian"]

#del方法
del my_list[2]
print(f"列表修改为: {my_list}") #["Tom", "Lily", "zhong", "fang", "zhao", "qian"]

#pop方法
elem = my_list.pop(2)
print(f"列表修改为: {my_list}") #["Tom", "Lily", "fang", "zhao", "qian"]
print(f"取出内容为{elem}") #"zhong"

#remove方法：删除找到的第一个匹配元素
my_list.remove("fang")
print(f"列表修改为: {my_list}") #["Tom", "Lily", "zhao", "qian"]

#clear方法
my_list.clear()
print(f"列表修改为: {my_list}") #[]

#count方法
count = my_list.count("xiao")
print(f"列表中xiao的个数为{count}。")	#0

#len函数
count = len(my_list)
print(f"列表中有{count}个元素")	#0
      
~~~

**特点**

* 可以容纳多个元素（上限为2\*\*63-1、9223372036854775807个、10\*\*18）
* 可以容纳不同类型的元素（混装）
* 数据是有序存储的（有下标序号）
* 允许重复数据存在
* 可以修改（增加或删除元素等）

**遍历**

while遍历

~~~py
while index < len(my_list)
~~~

for遍历

~~~py
for i in my_list
~~~

while循环和for循环的区别：

* 在循环控制上：
  * while循环可以自定义循环条件，并自行控制
  * for循环不可以自定义循环条件，只可以一个一个的从容器中取出元素
* 在无限循环上：
  * while循环可以通过条件控制做到无限循环
  * for循环理论上不可以，因为被遍历的容器容量不是无限的
* 在使用场景上：
  * while循环适用于任何想要循环的场景
  * for循环适用于，遍历数据容器的场景或简单的固定次数循环场景

### tuple 

**定义格式**

~~~py
#定义元组字面量
(elem1, elem2, ...)

#定义元组变量
t1 = (elem1, elem2, elem3)

#定义空元组对象
t2 = ()
t3 = tuple()

#元组嵌套
t4 = ((2, 4),)
~~~

**注意，定义单个元素的元组时，要在元素后面加逗号**

**特点**

元组一旦定义完成，就不可以再修改

若修改元组中的元素就会报错

但是修改元组中的list不会报错

**相关操作**

~~~py
tup = ('a', 'b', 'c', 'd', 'e')
#index方法
cnt = tup.index('a')	#0

#count方法
cnt = tup.count('a')	#1

#len方法
cnt = len(tup)	#5

#[]方法
temp = tup[3]	#'d'
~~~

循环

~~~py
while index < len(tup)
	...

for i in tup
	...
~~~

### str

字符串是字符的容器，一个字符串可以存放任意数量的字符

**操作**

~~~py
name = "xiaolinfeng"
#[]方法
print(name[3])	#o

#index方法
cnt = name.index('x') #0
cnt = name.index("lin")	#4

#修改
name[0] = 'c'	#TypeError: 'name' object does not support item assignment

#replace方法
"""
语法：str.repalce(str1, str2)
功能：将str中的全部str1替换成str2
注意：原字符串并没有变
"""
new_name = name.replace('lin', ' super lin ')	
print(name)		#"xiaolinfeng"
print(new_name)	#"xiao super lin feng"

#split方法
"""
语法：字符串.split(分割字符串)
功能：按照指定的分割字符串，将字符串划分为多个字符串，并存入列表对象中
注意：字符串本身不变，而是得到了一个列表对象
"""
name_list = new_name.split(" ")
print(name_list)	#['xiao', 'super', 'lin', 'feng']

#strip方法
"""
语法：str.strip(str1 = " ")
功能：将str中的最前面和最后面的str1去除，默认为空格
注意：传入的字符串被分解成长度为1的字符串，只要满足其中的字符串，就可以去除
"""
name2 = new_name.strip()
print(name2)	#"xiao super lin feng"

#统计字符串的出现次数
count = name.count("xiao")	
print(count)	#1

#统计字符串的长度
lenth = len(name)
print(length)	#10

# *运算符
print("Hello~" * 3)  # "Hello~Hello~Hello~"
~~~

**特点**

同元组一样，字符串是一个无法修改的数据容器

所以：

* 修改指定下标的字符		（name[0] = "a")
* 移除特定下标的字符	       (del name[0], remove(), name.pop()等)
* 追加字符等                          (name.append())

均无法完成。如果必须要做，只是得到当前字符串的一个副本而已

**遍历**

~~~py
for i in str
	xxx
    
index = 0
while index < len(str)
	xxx
~~~

### 数据容器切片

**序列概念：**内容连续、有序，可使用下标索引的一类数据容器

列表、元组、字符串，均可以视为序列

**切片**：从一个一个序列中，取出一个子序列

**语法：序列[起始下标 : 结束下标 : 步长]**

表示从序列中，从指定位置开始，依次取出元素，到指定位置（不含）结束，得到一个新序列

* 起始下标表示从何处开始，可以留空，留空视作从头开始
* 结束下标表示到何处结束（不含），可以留空，留空视作到结尾
* 步长表示依次取元素的间隔（可以用负数，但起始下标和结束下标也要反向标记）

*对序列进行切片操作不会影响到序列本身，而是会创建一个新的序列*

### set

应用场景：

* 包含关系比较
* 数据去重

**定义格式**

~~~py
#基础集合
{'a', 'b', 'c'}

#集合变量
my_set = {'a', 'b', 'c'}

#空集合
my_set = {}
my_set = set()
~~~



**特点**

不支持重复元素，自动去重

无序，不支持下标索引访问

**常用操作**

~~~py
# 集合操作符
S = {}
T = {}
# 返回交集
S|T
# 返回差集
S - T
# 返回并集
S & T
# 返回异或集
S ^ T
# 判断子集关系
S <= T
S < T
S >= T
S > T

# 增强操作符
S |= T
S &= T
S ^= T
S -= T

my_set = set()

#添加新元素
my_set.add("Python")

#移除元素
# 元素不在会报错
my_set.remove("Python")

# 元素不在不会报错
my_set.discard(x)

#随机取出元素
elem = my_set.pop()

# 清除元素
my_set.clear()

# 返回S的一个副本
my_set.copy()

# 集合大小
len(my_set)

# 返回x是否在S中
x in S

# 返回x是否不在S中
x not in S

#将其它类型变量转换为set
set(x)
~~~



### dict

**定义**

~~~py
#空字典
my_dict = {}
my_dict = dict()

#字典变量
my_dict = {key:value, key:value,...}
~~~

**操作**

~~~py
value = my_dict[key]

#key存在则覆盖value，key不存在则添加新键值对
my_dict[key] = value

#取出key对应的value并在字典内删除键值对
my_dict.pop(key)

#清空字典
my_dict.clear()

#计数
len(my_dict)
~~~

**特点**

* 可以容纳多个数据
* 可以容纳不同类型的数据
* 每一份数据是键值对
* 可以通过key取得value，key不可以重复
* 支持下标索引
* 可以修改
* 支持for循环

### 通用操作

~~~py
max()		#找最大元素
min()		#找最小元素
len()		#计算长度
list()		#转换为list（dict丢失value）
tuple()		#转换为tuple（dict丢失value）
str()		#转换为str
set()		#转换为集合
sorted(容器, reversed = false)	#排序，默认reversed = false表示从小到大
~~~

# Python进阶篇

## 函数

### 函数多返回值

~~~py
def func():
    return 1, 2

x, y = func()	#用逗号隔开就可以多返回值和多接受
~~~

### 函数多种传参方式

* 位置参数： 调用时根据位置确定传入的是哪个参数

* 关键字参数：调用的时候根据键=值的方式传递参数，可以不按照位置传参，函数调用时，如果关键字参数和位置参数混用，那么位置参数必须在关键字参数的前面，但是关键字参数之间不存在位置关系

* 不定长参数：不确定有多少个参数的时候使用不定长参数，关键字是*args，传进的所有参数都会被args变量收集，它会根据传进参数的位置合并为一个元组(tuple)，args是元组类型，这就是位置传递。或者可以按照**kwargs传递键值对，传入的键值对会作为字典存在

* 缺省参数：在定义的时候加上默认值，掉用函数的时候可不加入该参数

~~~py
#位置参数：
def func(arg1, arg2, arg3):
	print(f"{arg1}, {arg2}, {arg3}")
    
#关键字参数
func(arg2 = xxx, arg1 = xxx, arg3 = xxx)

#两者混用
func(xxx, arg1 = xxx, arg3 = xxx)

#不定长参数形式1
def func2(*args):
	print(args)

#不定长参数形式2
def func2(**kwargs):
	print(kwargs)
    
#缺省参数
def func3(arg1 = xxx, arg2 = xxx, arg3):
	print(f"{arg1}, {arg2}, {arg3}")
func3(xxx)
~~~

### 匿名函数

#### 函数作为参数传递

~~~py
def func1(func2):	#func2作为func1的参数
    print(type(func2))		#<class 'function'>
    result = func2(1, 2)
    print(result)
def func2(x, y):
    reutrn x + y
func1()
~~~

作用：传入计算逻辑，而非传入数据，可以使代码更加抽象

#### lambda匿名函数

函数的定义中

* def关键字，可以定义带有名称的函数
* lambda关键字，可以定义匿名函数

有名称的函数，可以基于名称重复使用

无名称的函数，只可临时使用一次

匿名函数定义语法：

lambda 参数: 函数体（一行）

* lambda是关键字，表示定义匿名函数
* 传入参数表示匿名函数的形式参数，如：x，y表示接收两个形式参数
* 函数体，就是函数的执行逻辑，要注意：只能写一行

~~~py
def test_func(compute):
    result = compute(1, 2)
    print(result)
    
test_func(lambda x, y:x + y)	#3
~~~

## 文件操作

### 文件编码

编码就是一种规则集合，记录了内容和二进制间相互转换的逻辑

常见的编码：UTF-8（通用）、GBK（中文）、Big5（繁体中文）等

### 文件读取

文件：将数据存到硬盘等保存

操作：打开，关闭，读，写

~~~py
"""
open()打开函数
在Python中，使用open函数，可以打开一个已经存在的文件，或者创建一个新的文件，语法如下
open(name,mode,encoding)
name: 是要打开文件的文件名的字符串（可以包含文件所在的具体的路径）
mode: 设置打开文件的模式（访问模式）: 只读、写入、追加等
encoding: 编码格式（推荐使用UTF-8）
"""
f = open('python.txt', 'r', encoding = "UTF-8")
#encoding顺序不是第三位，所以不能用位置参数，要用关键字参数直接指定

#读取
f.read(n)
f.read()
f.readline()
f.readlines()	#有换行符
for line in f:
    print(f"每一行数据是：{line}")
    
#关闭
f.close()

#自动关闭
with open() as f:
    print(f.readline())
~~~

注意，经过上述操作后，'f'是'open'函数的文件对象

| 模式 | 描述                                                         |
| ---- | ------------------------------------------------------------ |
| r    | 只读，文件的指针会放在文件的开头（默认）                     |
| w    | 只写，文件已存在则打开文件，并在开头开始编辑，原有内容会被删除。如果该文件不存在，创建新文件 |
| a    | 打开一个文件用于追加。如果文件存在，新的内容将被写到已有内容之后。如果文件不存在，创建新文件 |

### 文件写入

~~~py
#1.打开文件
f = open('python.txt', 'w')

#2.文件写入
f.write('hello world')

#3.内容刷新
f.flush()
~~~

注意：

* 直接调用write，内容并未真正写入文件，而是会积攒在程序的内存中，称之为缓冲区
* 当调用flush时，内容会真正的写入文件
* 这样做是避免频繁的操作硬盘，导致效率下降

### 文件追加

~~~py
#1.打开文件，通过a模式
f = open('python.txt', 'a')

#2.文件写入
f.write("hello world!!!")

#3.内容刷新
f.close()
~~~

注意：

* 文件不存在会创建文件
* 文件存在的话是在文件的最后面开始写入
* 可以使用"\n"等符号

## 异常、模块与包

### 异常

**异常**：当检测到一个错误的时候，Python解释器就无法继续执行了，反而出现了一些错误的提示，这就是所谓的异常，也就是bug

在程序遇到了BUG的时候，可以有两种处理方式：

1. 后面的程序需要用到这里的数据，整个程序停止运行。
2. 对bug进行提醒，整个程序继续运行。

**异常捕获**：在大型程序中，不能因为一个小的bug就停止运行，所以希望可以对异常进行捕获。捕获异常的作用在于，提前假设某处会出现异常，并提前做好准备，当异常真正出现时，使用指定异常处理方法

~~~py
#全部异常
try:
    xxx
except:
    xxx
    
try:
    xxx
except Exception as e:		#Exception是所有异常的父类
    xxx

#特定异常    
try:
    xxx
except NameError as e:
    xxx
#多个异常    
try:
    xxx
except (NameError, ValueError):
    xxx
    
#后处理finally
try:
    xxx
except xxx as x:	#出现xxx异常
    xxx
else:				#未出现XXX异常
    xxx
finally:			#无论如何都会执行
    xxx
~~~

**异常的传递**

异常是具有传递性的

当函数1调用函数2，并且在运行过程中，函数2出现了异常，并且异常没有被捕获的时候，异常会传递到函数1，这就是异常的传递性。

当出现了异常，并且全部函数都没有捕获异常的时候，程序就会报错

### 模块

模块：Module，是一个python文件，以.py结尾。模块能定义函数，类和变量，模块里也能包含可执行的代码。

模块的作用：python中有很多各种不同的模块，每一个模块都可以帮助我们快速的实现一些功能，比如实现和时间相关的功能就可以使用time模块，我们可以认为一个模块就是一个工具包，每一个工具包中都有各种不同的工具供我们使用进而实现不同的功能。

**模块的导入方式**

~~~py
import 模块名
from 模块名 import 类、变量、方法等	#可以直接使用，不用加模块名
from 模块名 import *	#全部
import 模块名 as 别名
from 模块名 import 功能名 as 别名
~~~

#### 自定义模块

在模块被导入的时候会自动运行，若不想运行其它文件，可以利用内置变量名\_\_name__

在其它模块内部加入:

~~~py
if __name__ == '__main__':
    xxx
~~~

在运行其它模块的时候就不会自动运行该模块了

如果一个模块文件里有_\_all__变量，当使用from xxx import * 的时候，只能导入列表内的元素

~~~py
__all__ = ['test_a']
def test_a():
    print('a')
def test_b():
    print('b')
~~~

注意：不同模块，同样的功能，如果都被导入，那么后导入的会覆盖先导入的

### python包

从物理上看，包就是一个文件夹，在该文件夹下包含了一个\__init__.py文件，该文件夹可用于包含多个模块文件。从逻辑上看，包的本质依然是模块

作用：当模块文件越来越多的时候，包可以帮助我们管理这些模块，包的作用就是包含多个模块，但包的本质依旧是模块

导入包

~~~py
import 包名.模块名
~~~

控制*行为

~~~python
#在__init__.py中定义
__all__ = ['xx', 'xxx']
~~~

安装第三方包

~~~py
pip install xxx
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple xxx
~~~

# 案例

## json文件

json是一种轻量级的数据交互格式，可以按照jsion指定的格式去组织和封装数据

json本质上就是一个带有特定格式的字符串

json文件有严格的格式要求，从python角度来看，就是一个字典，或者是列表嵌套字典

### 数据转换

python数据和json数据的相互转换

~~~py
#导入json模块
import json

#准备符合格式json格式要求的python数据
data = [{"name":"老王","age":16},{"name":"张三","age":20}]

#通过json.dumps(data)方法把python数据转化为了json数据
data = json.dumps(data)
data = json.dumps(data, ensure_ascii = 'False')		#使用参数使中文正确转换

#通过json.loads(data)方法把json数据转化为了python数据
data = json.loads(data)
~~~

注意：需要将文件字符串先读取到字符串里，然后再使用函数对格式进行相互转换

## Pyecharts模块

Echarts是由百度开源的数据可视化框架，凭借着良好的交互性，精巧的图标设计，得到了众多开发者的认可。而python是门富有表达力的语言，很适合用于数据处理。

gallery.pyecharts.org

* 基础折线图

  ~~~py
  from pyecharts.charts import Line
  line = Line()
  line.add_xaxis({"中国", "美国", "英国"})
  line.add_yaxis("GDP", [30, 20, 10])
  line.render()
  ~~~

# 类与对象

~~~py
#设计类
class Student:
    name = None
    gender = None
    nationality = None
    
    def hi(self):
        print(f"Hi! My name is{self.name}")
    
#创建对象
stu = Student()

#赋值
stu.name = xxx
~~~

* 成员方法的定义

  ~~~py
  def 方法名(self, 形参1,..., 形参n):
      方法体
  ~~~

  在定义的时候必须加上self关键字

  * 表示类对象自身的意思
  * 使用类对象调用方法时，self会自动被传入
  * 在方法内部，想要访问类的成员变量，必须使用self

* 构造方法

  Python类可以使用：\__init__()方法，称之为构造方法

  可以实现：

  * 在创建类对象（构造类）的时候自动执行
  * 在创建类对象（构造类）的时候，将传入参数自动传递给\__init__()方法使用

  ~~~py
  class Student:
      name = None
      age = None
      tel = None
      
      def __init__(self, name, age, tel):
          self.name = name
          self.age = age
          self.tel = tel
          print("Student类创建了一个对象")
          
  stu = Student("xx",32, "1231423")
  ~~~

* 魔术方法

  上面的\__init__构造方法，是Python类内置的方法之一

  这些内置的类方法，各自有各种特殊的功能，这些内置方法我们称之为：魔术方法

  ~~~py
  #构造方法
  __init__(self)
  #控制类转换成字符串的方法
  __str__(self)
  #控制类比较小于或者大于的方法
  __lt__(self, other)
  #控制类比较小于等于或者大于等于的方法
  __le__(self, other)
  #控制类比较等于的方法
  __eq__(self, other)
  ~~~

## 封装

概念：将现实世界事物在类中描述为属性和方法

* 私有成员

  定义私有成员变量：变量名以__开头

  定义私有成员方法：变量名以__开头

## 继承

语法：

~~~py
"""
单继承
class 类名(父类名):
	类内容体
"""

class Phone:
    IMEI = None
    producer = None
    
    def call_by_4g(self):
        print("4g通话")
        
class Phone2022(Phone):
    face_id = True
    
    def call_by_5g(self):
        print("2022最新5g通话")
        
"""
多继承
class 类名(父类1, 父类2, 父类3):
	xxx
"""
class NFC:
    xxx
    
class RemoteControl:
    xxx
    
class MyPhone(Phone, NFC, RemoteControl):
    pass	#指不用定义
~~~

如果多个父类有同名变量，则继承的类名从左往右优先级逐渐降低

### 复写和使用父类成员

子类在继承了父类的方法后，还可以将其重新定义，称之为复写

一旦复写父类对象，那么类对象调用成员的时候，就会调用复写后的成员

如果需要使用被复写的父类成员，需要特殊的调用方式：

* 使用父类名调用
  * 使用成员变量：父类名.成员变量
  * 使用成员方法：父类名.成员方法(self)
* 使用super()调用父类成员
  * 使用成员变量：super().成员变量
  * 使用成员方法：super().成员方法()

~~~py
class Phone:
    IMEI = None
    producer = "ITCAST"
    
    def call_by_5g(self):
        xxx
        
class MyPhone(Phone):
    producer = "ITHEIMA"
    
    def call_by_5g(self):
		print(f"父类的品牌是：{Phone.producer}")
        Phone.call_by_5g(self)
        
        print(f"父类的品牌是：{super().producer}")
        super().call_by_5g()
~~~

## 类型注解

* 变量的类型注解

  变量直接冒号后面加类型

  ver1: int

  容器直接中括号里面加类型

  ver2: List[int]

* 函数（方法）的类型注解

* Union类型

~~~py
"""
num: List[int]		#num是List类型，且List里面存的是int
target: int			#target是int类型
def twoSum(...) -> List[int]:	#返回值是List类型，且List里面存的是int
写注释同样可以定义类型
"""
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        
var1			# type: int

from typing import Union
#两种类型二选一
#使用Union[类型, 类型, ...]
my_list: list[Union[str, int]] = [1, 2, "itheima", "itcast"]
my_dict: dict[str, Union[str, int]] = {"name": "xxx", "age": 21}
~~~

## 多态

多态：多种状态，即完成某个行为的时候，使用不同的对象会得到不同的状态

多态通常作用在继承关系之上，比如，函数形参声明接受父类对象，实际传入父类的子类对象进行工作

即：以父类做声明，以子类做实际工作，用以获得同一行为，不同状态

~~~py
class Animal():
    def speak(self):
        pass
        
        
class Dog(Animal):
    def speak(self):
        print("汪汪汪")
        
        
class Cat(Animal):
    def speak(self):
        print("喵喵喵")
        
def make_noise(animal: Animal):
	animal.speak()
    
cat = Cat()
dog = Dog()

make_noise(cat)		#喵喵喵
maek_noise(dog)		#汪汪汪
~~~

在这个例子中，父类主要是用来确定有哪些方法，可以称之为抽象类，抽象方法就是抽象方法的空实现

这种设计的含义是：父类确定具体该有哪些方法，子类来确定具体实现

# Numpy

优点：

* 底层使用c语言数组实现，针对运算进行优化，**运行效率高**
* 单条命令，执行多个数据运算（SIMD）
* 提供方便的语法完成运算（矩阵运算/矩阵形状拓展，传播）
* 为非常多的其它包（如：深度学习PyTorch/机器学习Scipy）提供科学数值运算的基础

使用：

## 数组创建/维度

### 数组创建

~~~python
import numpy as np

# 列表转换为array
arr = np.array([1, 2, 3])
print(arr.shape); print(arr)

# 创建整数数列
# 类比：list(range(10))
arr = np.arange(10)
print(arr.shape); print(arr)
~~~

```
(3,)
[1 2 3]
(10,)
[0 1 2 3 4 5 6 7 8 9]
```

**np.arrange：返回等差数列构成数组**

*  一个参数时，参数值为终点，起点取默认值0，步长取默认值1。
* 两个参数时，第一个参数为起点，第二个参数为终点，步长取默认值1。包前不包后。
* 三个参数时，第一个参数为起点，第二个参数为终点，第三个参数为步长；步长支持小数。

~~~python
# 创建整数数列
# 类比：list(range(10))
arr = np.arange(10)
print(arr.shape); print(arr)

# 类比：list(range(0, 10, 2.5))
# (开始， 截至， 步长)
# [开始， 截至）范围内步长为间隔的更新
arr = np.arange(0, 10, 2.5)
print(arr.shape); print(arr)
~~~

```
(10,)
[0 1 2 3 4 5 6 7 8 9]
(4,)
[0.  2.5 5.  7.5]
```

**np.linspace/ logspace: 指定范围内返回‘等间矩’数组**

~~~python
# 在一个区间内返回等距数组
arr1 = np.linspace(1, 101, 5)
print(arr1.shape); print(arr1)

# 不包括右区间的值
arr2 = np.linspace(1, 101, 5, endpoint=False)
print(arr2.shape); print(arr2)

# 取对数后的等距插值
# 10 ^ 0 = 1, 10 ^ 2 = 100
arr3 = np.logspace(0, 2, 5, endpoint=True)
print(arr3.shape); print(arr3)
~~~

```
(5,)
[  1.  26.  51.  76. 101.]
(5,)
[ 1. 21. 41. 61. 81.]
(5,)
[  1.           3.16227766  10.          31.6227766  100.        ]
```

### 数组维度

**创建矩阵：从一维到多维**

~~~python
# 将列表转化为矩阵
mat = np.array([1,2,3,4,5,6,7,8,9])
print(mat.shape); print(mat)
# 默认：数轴越靠前，变化越快
mat = mat.reshape(3,3)
print(mat.shape); print(mat)
# 增加第三个维度
mat = mat.reshape(1,3,3)
print(mat.shape); print(mat)
~~~

```
(9,)
[1 2 3 4 5 6 7 8 9]

(3, 3)
[[1 2 3]
 [4 5 6]
 [7 8 9]]
 
(1, 3, 3)
[[[1 2 3]
  [4 5 6]
  [7 8 9]]]
```

​	变换规则：一般默认c中的数组order：最后一个维度，变化最快，第一个维度变化最慢；

（排列数字中，后面维度比前面的变化快）

**矩阵的初始化：填充矩阵**

~~~python
arr1 = np.zeros((2,2))
arr2 = np.ones((2,4))
print(arr1.shape); print(arr1)
print(arr2.shape); print(arr2)

arr3 = np.full((2, 2), -np.inf)
arr4 = np.full((2, 2), [1, 2])
print(arr3.shape); print(arr3)
print(arr4.shape); print(arr4)
~~~

```
(2, 2)
[[0. 0.]
 [0. 0.]]
(2, 4)
[[1. 1. 1. 1.]
 [1. 1. 1. 1.]]
(2, 2)
[[-inf -inf]
 [-inf -inf]]
(2, 2)
[[1 2]
 [1 2]]
```

注意，用矩阵填充矩阵，会先填充列的维度，再填充行

np.ones_like() vs np.ones()

~~~python
x = np.array([[1,2,3]])
# 识别数组形状并将其值转换为1
ones_1 = np.ones_like(x)
onts_2 = np.ones(x.shape)
print(x)
print(ones_1)
print(ones_2)
~~~

```
[[1 2 3]]
[[1 1 1]]
[[1. 1. 1.]]
```

**更多特殊矩阵的生成: 对角矩阵**

~~~python
# identity matrix
I = np.identity(3)
print(I)

I_rect = np.eye(3, M=5)
print(I_rect)
~~~

```
[[1. 0. 0.]
 [0. 1. 0.]
 [0. 0. 1.]]
[[1. 0. 0. 0. 0.]
 [0. 1. 0. 0. 0.]
 [0. 0. 1. 0. 0.]]
```

这些矩阵再矩阵运算中很有用，类似于自然数中的1

**np.triu/ np.tril: 选取上/ 下三角矩阵**

~~~python
mat2 = np.array([0,1,2,3,4,5,6,7,8])
mat2 = mat.reshape(3, 3)
print(mat2)
# 上三角矩阵
print(np.triu(mat2))
# 下三角矩阵
print(np.tril(mat2))
# 下三角矩阵（对角线下移1）
print(np.tril(mat2, k=-1))
~~~

```
[[0 1 2]
 [3 4 5]
 [6 7 8]]
 
[[0 1 2]
 [0 4 5]
 [0 0 8]]
 
[[0 0 0]
 [3 4 0]
 [6 7 8]]
 
[[0 0 0]
 [3 0 0]
 [6 7 0]]
```

**合并/ 简化矩阵维度（np.flatten/ np.ravel/ np.squeeze）**

~~~python
mat3 = mat2.reshape(1, 3, 3)
print(mat3.shape); print(mat3)

# 合并所有维度
# mat4 = mat3.ravel()
mat4 = mat3.flatten()
print(mat4.shape); print(mat4)

# 去掉冗余维度
mat5 = np.squeeze(mat3)
print(mat5.shape); print(mat5)
~~~

```
(1, 3, 3)
[[[0 1 2]
  [3 4 5]
  [6 7 8]]]
  
(9,)
[0 1 2 3 4 5 6 7 8]

(3, 3)
[[0 1 2]
 [3 4 5]
 [6 7 8]]
```

**交换数轴的先后顺序（np.swapaxes）**

~~~python
x = np.array([[1,2,3]])
print(x)
print(x.shape)

# 将行向量变为列向量
x1 = np.swapaxes(x, 0, 1)
print(x1)
print(x1.shape)
~~~

```
[[1 2 3]]
(1, 3)

交换了第0维和第1维
[[1]
 [2]
 [3]]
(3, 1)
```

**np.swapaxes vs. np.transpose**

* 无参数：矩阵转置
* 有参数：交换i轴与参数所在位序轴

~~~python
mat2 = arange(1, 10).reshape(3,3)
print(mat2.shape)
print(mat2)
# 交换0轴和1轴
mat3 = np.swapaxes(mat2, 0, 1)
print(mat3.shape)
print(mat3)

# 这里等价于矩阵的转置操作
mat3 = mat2.T
print(mat3)
mat3 = mat3.transpose()

# 1*2*3三维矩阵
mat4 = np.zeros((1,2,3))
print(mat4.shape)
mat4 = mat4.transpose(1,0,2)
print(mat4.shape)
mat4 = mat4.transpose()
print(mat4.shape)
~~~

```
(3, 3)
[[1 2 3]
 [4 5 6]
 [7 8 9]]
 
(3, 3)
[[1 4 7]
 [2 5 8]
 [3 6 9]]
 
[[1 4 7]
 [2 5 8]
 [3 6 9]]
 
(1, 2, 3)
(2, 1, 3)
(3, 1, 2)
```

**矩阵的拼接(np.vstack/ np.hstack)**

~~~python
a = np.array((1,2,3))
b = np.array((4,5,6))
print(a)
print(b)

print(a.shape, b.shape)
print(np.hstack((a,b)))		# 水平复习拼接
print(np.vstack((a,b)))		# 竖直方向拼接
~~~

```
[1 2 3]
[4 5 6]
(3,) (3,)
[1 2 3 4 5 6]
[[1 2 3]
 [4 5 6]]
```

**矩阵的拼接(np.concatenate)**

~~~python
a = np.arange(1,4)
print(a.shape)
a = a.reshape(1, -1)
b = b.reshape(1, -1)
print(a.shape)
print(np.vstack((a,b,b)))
print(np.hstack((a,b,b)))
# 拼接到指定轴上
print(np.concatenate((a,b,b), axis=0))
print(np.concatenate((a,b,b), axis=1))
~~~

```
(3,)
(1, 3)

[[1 2 3]
 [4 5 6]
 [4 5 6]]
 
[[1 2 3 4 5 6 4 5 6]]

[[1 2 3]
 [4 5 6]
 [4 5 6]]
 
[[1 2 3 4 5 6 4 5 6]]
```

## 矩阵选取/运算

### 矩阵选取

**矩阵下标选取**

1. 整数索引
   * arr[i]：获取第i行的数据，对于二维数组，返回第一行的一维数组
   * arr[i, j]：获取第i行、第j列的元素
   * arr[:, j]：获取第j列的数据
2. 切片索引
   * arr[start : stop]：获取从索引 start 到 stop-1 的元素，不包括 stop 本身。
   * arr[start:]：获取从索引 start 开始到数组末尾的所有元素。
   * arr[:stop]：获取从数组开头到索引 stop-1 的所有元素。
   * arr[start:stop:step]：带有步长的切片索引。
3. 布尔索引
   * arr[condition]：根据条件 condition 过滤数组，返回满足条件的元素。
4. 整数数组索引
   * arr[[i, j, k]]：传入整数数组作为索引，返回指定索引位置的元素。
5. 布尔数组索引
   * arr[boolean_array]：传入布尔数组作为索引，返回布尔数组中为 True 的位置的元素。
6. 使用'ix_'函数进行高级索引
   * np.ix_(row_indices, column_indices)：使用 `ix_` 函数实现高级索引，返回一个由所有可能的 (i, j) 组合构成的二维数组。

使用以上方式对矩阵进行修改都是修改矩阵本身。

**矩阵的选取**

~~~python
mat = np.arange(9).reshape(3, 3)
print(mat)
# 选择左上角2*2矩阵
print(mat[:2,:2])

# 选择第一列
print(mat[:,0])
print(mat[:,0].shape)
# 选择第一列和第三列中的所有内容
print(mat[:,[0,2]])
~~~

```
[[0 1 2]
 [3 4 5]
 [6 7 8]]
 
[[0 1]
 [3 4]]
 
[0 3 6]
(3,)

[[0 2]
 [3 5]
 [6 8]]
```

**np.diag()：选择对角线；slicing：选择内容（可类比列表选择）**

~~~python
mat = np.arange(9).reshape(3, 3)
print(mat)
# 选择对角线内容
print(np.diag(mat))
print(np.diag(mat).shape)

# 选择第二行第三列
print(mat[1,2])
print(mat[1][2])
~~~

```
[[0 1 2]
 [3 4 5]
 [6 7 8]]
[0 4 8]
(3,)
5
5
```

**选择内容：按位置；分布选择**

~~~python
print(mat)
# 选取第1行第1列；第3行第3列
# 传入两个list，则将俩个list作为0轴列表和1轴列表访问下标
print(mat[[0,2], [0,2]])

# 选取第1/3行，第1/3列
print(mat[[0,2],:][:,[0,2]])
~~~

```
[[0 1 2]
 [3 4 5]
 [6 7 8]]
[0 8]
[[0 2]
 [6 8]] 
 
 mat[[0,2],:]指取mat的第0行和第二行的全部元素构成的新array
 mat[[0,2],:][:,[0,2]]等价于array[:,[0,2]]，即取新array的第0列和第2列的全部元素
 所以mat[0,2,:][:,[0,2]]指取1/3行的1/3列
```

**np.where：两种使用方法（替换值vs.返回索引）**

~~~python
print(mat)
# np.where用法1：只保留所有大于4的值，<4的值替换成0
print(np.where(mat > 4, mat, 0))

# np.where用法2：根据条件选取特定的值
inds_gt4 = np.where(mat > 4)
print(inds_gt4)
print(mat[inds_gt4])
print(mat[mat > 4])
~~~

```
[[0 1 2]
 [3 4 5]
 [6 7 8]]
[[0 0 0]
 [0 0 5]
 [6 7 8]]
 
(array([1, 2, 2, 2], dtype=int64), array([2, 0, 1, 2], dtype=int64))
[5 6 7 8]
[5 6 7 8]
```

**np.where vs. np.argwhere 返回索引 vs.返回每对数值 **

~~~python
print(mat)
# 返回大于4的位置
inds_gt4 = np.where(mat > 4)
print(inds_gt4)

# 返回每对符合条件的数值
inds_gt4 = np.argwhere(mat > 4)
print(inds_gt4)
~~~

```
[[0 1 2]
 [3 4 5]
 [6 7 8]]
(array([1, 2, 2, 2], dtype=int64), array([2, 0, 1, 2], dtype=int64))

[[1 2]
 [2 0]
 [2 1]
 [2 2]]
```

**np.isnan vs. np.isinf**

~~~python
arr = np.array([-1,0,1])
logarr = np.log(arr)
print(arr)
print(logarr)
print(np.isnan(logarr))
print(np.isinf(logarr))

# 不成立(np.inf != np.inf)
print(np.where(logarr==np.inf))
# 成立
print(np.where(np.isinf(logarr)))
~~~

```
[-1  0  1]
[ nan -inf   0.]
[ True False False]
[False  True False]
(array([], dtype=int64),)
(array([1], dtype=int64),)
```

### 数值运算

**numpy中常用的常量**

~~~python
print(np.nan)	# 空值
print(np.inf)	# 无限
print(np.e)		# e常数
print(np.pi)	# pi常数
print(np.newaxis)	# 空数轴

x = np.arange(3)
print(x); print(x.shape)
x = x[np.newaxis, np.newaxis, :]
print(x.shape)
# 等价于
x = x.reshape(1,1,-1)
print(x.shape)
~~~

```
nan
inf
2.718281828459045
3.141592653589793
None
[0 1 2]
(3,)

reshape中出现-1表示该维由其它维推断
reshape(1,1,-1)表示大小不变，新增前两维，与np.newaxis等价
(1, 1, 3)
(1, 1, 3)
```

**numpy的按位函数运算：加减乘除**

~~~python
mat = np.arange(0,4).reshape((2,2))
print(mat)
# 矩阵与数字的加减乘除
print(mat + 1) # np.add(mat, 1)
print(mat - 1)

print(mat * 2) # np.multiply(mat, 2)
print(mat / 2)
~~~

```
[[0 1]
 [2 3]]
[[1 2]
 [3 4]]
[[-1  0]
 [ 1  2]]
[[0 2]
 [4 6]]
[[0.  0.5]
 [1.  1.5]]
```

**更多按位运算**

~~~python
print(np.exp(mat))
print(np.sin(mat))

print(np.power(mat, 2))
~~~

```
[[ 1.          2.71828183]
 [ 7.3890561  20.08553692]]
[[0.         0.84147098]
 [0.90929743 0.14112001]]
[[0 1]
 [4 9]]
```

**numpy单个数组不同位置间的函数运算**

本质上是reduce运算

~~~python
mat = np.array([0,1,2,3]).reshape((2,2))
print(mat)
print(np.sum(mat,axis=0))
print(np.sum(mat,axis=1))
print(np.sum(mat))

# 累积和
print(np.cumsum(mat, axis=0))
print(np.cumsum(mat)) 
~~~

```
[[0 1]
 [2 3]]
[2 4]
[1 5]
6

[[0 1]
 [2 4]]
[0 1 3 6]
```

### 向量运算

**数组（向量）之间的运算：按位相乘/向量内积/向量外积**

~~~python
vec1 = np.array([1,2])
vec2 = np.array([2,1])
# 按位置相乘
print(vec1 * vec2)
# 矩阵的内积
print(np.dot(vec1, vec2))
# 矩阵的外积
print(np.outer(vec1, vec2))
~~~

```
[2 2]
内积：4
外积：
[[2 1]
 [4 2]] 
```

**数组之间的矩阵的运算：精确定义的乘法运算**

~~~python
vec1 = np.array([1,2])
vec2 = np.array([2,1])
print(np.matmul(vec1, vec2))
print(vec1 @ vec2)

print(np.matmul(vec1.reshape(-1,1), vec2.reshape(1,-1)))
print(vec1.reshape(-1,1) @ vec2.reshape(1, -1))
~~~

```
4
4
[[2 1]
 [4 2]]
[[2 1]
 [4 2]]
```

**Broadcasting：运算传播机制**

~~~python
arr1 = np.array([[1,2],[3,4]])
# (1,2)：一行两列
vec2 = np.array([0,2]).reshape(1,-1)
# (2,1)：两行一列
vec1 = np.array([0,2]).reshape(-1,1)
# 点乘
print(arr1*vec1)
print(arr1*vec2)
~~~

```
[[0 0]
 [6 8]]
[[0 4]
 [0 8]]
```

自动补齐缺失维度（在维度为1的地方复制数组）

与R语言中的循环机制（recycling）不同，在numpy中只有维度为1的地方才能被补齐

~~~python
# 4行两列
arr1 = np.arange(1,9).reshape(4,-1)
# 2行两列
vec1 = np.arange(4).reshape(2,2)
print(arr1 + vec1) # 报错
~~~

## 数值类型

**numpy的数值类型：自动推导**

~~~python
# 不同数据格式/类型
mat = np.arange(4).reshape(2,2)
print(mat); print(mat.dtype)

mat = mat - 1.1
print(mat); print(mat.dtype)

mat = mat + (1 + 1j)
print(mat); print(mat.dtype)
~~~

```
[[0 1]
 [2 3]]
int32

[[-1.1 -0.1]
 [ 0.9  1.9]]
float64

[[-0.1+1.j  0.9+1.j]
 [ 1.9+1.j  2.9+1.j]]
complex128
```

![image-20230804143714614](D:\Xiao\学习笔记\image\image-20230804143714614.png)

int32,即储存每个int整数花费32个0/1数，即8个字节

字节：现代计算机储存信息的最小单位（一字节=8个二进制数字串）

# Pandas

**从python字典生成DataFrame表格**

~~~python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
# 创建表格
workout_dict = {
    "calories": [420, 380, 390, 390],
    "duration": [50, 40, 45, 45],
    "type": ['run', 'walk', 'walk', 'run']
}
workout = pd.DataFrame(workout_dict)
display(workout)
~~~

|      | calories | duration | type |
| ---: | -------: | -------: | ---: |
|    0 |      420 |       50 |  run |
|    1 |      380 |       40 | walk |
|    2 |      390 |       45 | walk |
|    3 |      390 |       45 |  run |

**DataFrame单独指定/改变索引**

~~~python
# 直接重新赋值新索引
workout.index=["day1", "day2", "day3", "day4"]
# 直接重新赋值列列名/索引
workout.columns=["calories", "duration", "type"]
# 单独改变某一列列名/索引
workout=workout.rename(columns={"calories":"Cal"})
display(workout)
# 转换回dict字典
workout_dict=workout.to_dict()
~~~

|      |  Cal | duration | type |
| ---: | ---: | -------: | ---: |
| day1 |  420 |       50 |  run |
| day2 |  380 |       40 | walk |
| day3 |  390 |       45 | walk |
| day4 |  390 |       45 |  run |

或者

~~~python
workout=pd.DataFrame(workout_dict,index=['day1','day2','day3','day4'])
workout # 最后一行默认输出
~~~

|      |  Cal | duration | type |
| ---: | ---: | -------: | ---: |
| day1 |  420 |       50 |  run |
| day2 |  380 |       40 | walk |
| day3 |  390 |       45 | walk |
| day4 |  390 |       45 |  run |
