## 一、C++基础问题

### 1、C和C++的区别

1. C++有新的语法和关键字。新增了new、delete进行内存管理；新增了引用的概念；新增了auto进行类型声明。
2. C++有重载和虚函数的概念。C++支持函数重载，因为C++函数名字修饰时将参数带在后面。C++还有虚函数实现多态。
3. C的struct和C++的class不同。
4. C是结构化语言，面向过程开发，重在算法和数据结构。C++考虑构造对象模型，能够契合对应的问题领域，通过对象的状态信息得到输出。

### 2、指针和引用的区别

指针是一个实体，引用是一个别名。

1. **实质内容**：指针内容是内存地址；引用是内存别名。
2. **能否改变和赋值**：指针包含的内容可以改变，允许拷贝和赋值；引用不能更改。
3. **const属性**：指针有const、非const的区别，引用没有。
4. **sizeof**：sizeof（指针）得到的是指针本身的大小，32位系统返回4，64位系统返回8；sizeof（引用）得到引用对象的大小。
5. **编译角度**。编译时会将引用、指针添加到符号表（表中记录变量名和它的地址）上。指针的表中地址是指针地址，引用则是引用对象的地址，相当于是同一地址，多个名称。所以引用不能修改指向的对象。
6. **参数传递**的效果不同。

### 3、值传递、指针传递、引用传递

值传递：形参是实参的拷贝，修改形参不影响实参。

指针传递：是一种值传递，**形参是指向实参地址的指针**，形参修改能影响实参。

引用传递：传入的是主函数的实参变量的地址，引用参数会通过一个间接寻址的方式找到主函数中的相关变量。

### 10、取地址&、解引用*

&可以获取变量的内存地址，*会根据地址获取变量的值，获取变量的操作就是解引用。

```cpp
float f = 1.0;
short c = *(short*)&f; 
```

这两行进行了以下操作：

- &f获得了f的首地址
- `(short*)&f`则是认为f这个地址存储的是一个short类的变量。
- 最后解引用f地址，获取了原本float的前2个字节，按照short的编码方式解释，并把解释的结果赋值给c。

解引用的特点：只会按照**指针的类型**去解释。

```cpp
char buffer[4];
...
printf("%f %x\n", *buffer, *buffer);
```

例如这里的*buffer并不能获取char数组的全部值，而是把它解释成了char类型指针，根据char类型的解释方法，只获取了第一个字节的内容，并以float的形式打印出这1个字节。

### 11、指针声明

根据出现的运算符的优先级逐次解析即可。()和[]的优先级最高，同时出现时从左到右解析。

- 数组：`int p[3]`p和[]结合，说明是一个数组，再与int结合，说明数组里存放的是int数。
- 指针数组：`int *p[3]`。p先和[]结合，说明p是一个数组，再与*结合，说明数组里是指针类型，再与int结合，说明指针所指向的内容是整型。
- 指针数组：`int (*p)[3]`。p先和*结合，说明p是一个指针，再与[]结合，说明指针指向的内容是一个数组，再与int结合，说明数组内容是整型的，所以p是一个指向大小为3的int数组的指针。
- 二级指针：`int **p`。p先和*结合，说明p是一个指针，再与\*结合，说明指针指向的内容还是指针。
- 函数声明：`int p(int)`。p先和()结合，说明p是一个函数，进入()内，说明函数参数是int，最后与外面的int结合，说明函数返回值是int。
- 函数指针：`int (*p)(int)`。p先和\*结合，说明p是一个指针，在和()结合，说明p指针指向是一个函数，进入()内，说明函数参数是int，最后与外面的int结合，说明函数返回值是int。p是一个指向一个int传入参数，返回值是int的函数的函数指针。

> 复杂声明举例：`int* (*p(int))[3];`
>
> `*p(int)`是一个传入参数是int，返回值是指针的函数。这个返回的指针再与[]结合，说明返回值的指针指向的是一个数组，最后与int*结合，说明这个数组里元素都是int指针。p 是一个参数为一个整数据且返回一个指向由整型指针变量组成的数组的指针变量的函数。

#### 11.1、指针声明阅读顺序

- 先抓住标识符：变量名、函数名。
- 从距离标识符最近的地方，按照运算符优先顺序解释。
  - 用于改变优先级的括弧
  - 用于表示数组的[]，用于表示函数的()
  - 用于表示指针的*
- 最后，补上数据类型修饰符，一般在最左边，int、double等。

注意，一般复杂声明，使用typedef简化。

### 12、指针和数组

对于数组而言，也可以用指针的方式进行访问。

```cpp
int array[10] = {10, 9, 8, 7};
printf("%d\n", *array);  // 	输出 10
printf("%d\n", array[0]);  // 输出 10

printf("%d\n", array[1]);  // 输出 9
printf("%d\n", *(array+1)); // 输出 9

int *pa = array;
printf("%d\n", *pa);  // 	输出 10
printf("%d\n", pa[0]);  // 输出 10

printf("%d\n", pa[1]);  // 输出 9
printf("%d\n", *(pa+1)); // 输出 9
```

但是指针和数组并不是一回事，在sizeof上就有差别：

```cpp
printf("%u", sizeof(array));
printf("%u", sizeof(pa));
```

数组会返回40，因为数组中有10个int，而指针返回指针本身长度，也就是4。

因为sizeof操作符会从**符号表**中查询符号的长度，在**编译期间**就会确定。

- 数组的类型由元素类型、数组长度共同构成。
- 指针类型根本不知道指向的是一个整数，还是一堆整数。

#### 12.1、二维数组

二维数组的存储其实和一维数组没有什么区别，是每一行展开成一行存储的。

访问: array[a][b]

那么被访问元素地址的计算方式就是: array + (m * a + b)

### 13、void指针

void指针表示通用指针，可以用来存放**任何数据类型**的引用。

void这种性质实现了C语言的泛型编程，任何的返回类型都可以先交给void*，再进行具体的转化。

**void* 不能解引用**：因为编译器不知道void*到底指向的是int、double还是一个结构体。

```cpp
int num;
void *pv = (void*)&num;
*pv = 4; // 错误
```

### 4、extern

extern用于修饰变量、函数，表明此变量、函数是**别处定义的，此处要引用它**，通知编译器在它处寻找定义，可以先通过编译，**链接器**会在别处寻找这个变量。可以用于在**多个源文件之间共享变量和函数**。

#### 4.1、链接

- **外部链接**：**extern的变量、没有static修饰的全局变量**通过外部链接在不同源文件之间共享符号。
- **内部链接**：**static修饰的全局变量、全局常量**用内部链接，不能被其他文件访问。
- **无链接**：只能在当前代码块使用，不能被其他函数、代码块使用，**局部变量、函数形参**属于此类。
- **外部C链接**：与C语言编写的代码交互。

#### 4.2、extern C匹配C函数

同一个函数C++编译后与C编译后不同：函数foo(int x, int y)，C++编译后名字为foo_int_int，C编译后foo。（事实上不同编译器的命名风格也不一样，总的来说C++会加上函数参数类型）C++这样做是为了**支持重载**。

所以C++程序中调用被C编译的函数，要在**函数声明前加上extern ”C“**，解决名字匹配问题。

```cpp
// C 语言代码
#include <stdio.h>

void print_message(const char* message) {
    printf("%s\n", message);
}

// C++ 代码
extern "C" {
    // 声明 C 语言函数
    void print_message(const char* message);
}

int main() {
    // 调用 C 语言函数
    print_message("Hello, world!");
    return 0;
}
```

#### 4.3、extern作用

- 声明变量或函数的存在，但不进行定义，**可以在不同源文件共享相同的变量或函数**。让编译器链接时在其他源文件查找定义。

  ```cpp
  //fileA.cpp
  int i = 1;         //声明并定义全局变量i
  
  //fileB.cpp
  extern int i;    //声明i，链接全局变量
  
  //fileC.cpp
  extern int i = 2;        //错误，多重定义
  int i;                    //错误，这是一个定义，导致多重定义
  main()
  {
      extern int i;        //正确
      int i = 5;            //正确，新的局部变量i;
  }
  ```

- **让全局常量可以被共享访问。**全局常量默认是内部链接的，想要共享操作必须要加extern。

  ```cpp
  //fileA.cpp
  const int i = 1;        //定义 (不用 extern 修饰)
  
  //fileB.cpp                    //声明
  extern const int i;
  ```

- 参与编译和链接过程：

  - **编译器**：extern通知编译器这个变量在其他源文件定义，编译器给这个变量在符号表中加上一个引用，具体引用的指向需要在链接过程确定。
  - **链接器：**链接将多个目标文件合并成一个可执行文件，并且在当前源文件中声明的符号，会在其他源文件找到对应的定义，并把它们链接起来。

### 5、define和const

**共同点：**二者都可以定义常量。

define宏定义在**预处理阶段处理**，**没有类型**，仅仅是将宏定义的字符串展开。**没有类型检查**。运行时系统**不为宏定义分配内存**。这是C使用的定义常量的方式，没有类型检查所以不安全。define的**ifndef、endif还起到了防止文件重复引用**的问题。

const是在**编译阶段处理**，**有类型区分**、**类型检查**，运行时系统**会为const常量分配内存**。const修饰的数据成员、函数、参数、返回值受强制保护，能提高程序健壮性。这是C++使用的定义常量的方式，更安全。

#### 5.1、既然const更好，为什么C++还有宏？

宏可以起到卫哨作用，防止文件重复引用。尤其在头文件中要使用条件编译语句。

```cpp
#ifndef aaa
#define aaa
...
#endif
```

#### 5.2、const修饰指针（const在指针后是只读指针）

**指向只读变量的指针 const int* p;**：const修饰指针：常量指针，修饰的是指针指向的值；const int* p; p = 26；会报错。

**只读指针 int* const p ：**const修饰的是指针本身，可以通过指针修改变量，但是不能修改指向关系。int* const p = p1; p = p2；会报错。

**只读指针指向只读变量 const int* const p ：**指针本身和所指向的变量都是只读的。指向关系和指向的变量都不能修改。

#### 5.3、const的功能

保证变量和函数的安全性、代码的可读性。

- 修饰值：
  - 修饰变量：变量无法被修改，保证变量无法被修改。
  - 修饰函数参数：表示函数不会修饰这个参数的值，使得代码更清晰、安全。
  - 修饰函数返回值：表示返回值是只读，保证返回值安全。
  - 修饰指针：根据const位置分为只读指针、指向只读变量的指针。
- 修饰成员函数：在成员函数后加const，表示函数不会修改任何成员变量，并且const的成员函数不能调用非const的成员函数。

### 16、define和inline的区别

inline内联函数可以避免函数调用带来的开销。

- **处理阶段不同**：define是在**预处理**阶段展开，inline是在**编译**阶段展开。
- 内联函数会进行**参数检查**：宏只是展开，inline毕竟是一个函数。

### 17、define和typedef的区别

都是定义别名的方法，二者区别也和define和inline的区别很像，typedef更多用于复杂变量的别名。

- 处理阶段不同：define是在**预处理**阶段展开，typedef是在**编译**阶段展开。
- typedef有类型检查：宏只是展开。
- typedef受到作用域限制：宏定义没有作用域限制，typedef超出了命名空间、类空间就失效。



### 6、浅拷贝、深拷贝的区别

二者的本质区别是**是否会分配新的内存空间**保存变量。

浅拷贝是默认的拷贝函数，会对成员一一复制，**数据成员没有指针时，浅拷贝可行**。但是当数据成员有指针时，两个类中两个指针指向同一个地址，析构两次，造成了野指针。

深拷贝会在**堆内存中另外申请空间来存储数据（根本区别）**。数据成员有指针时，深拷贝更加安全。

### 7、拷贝构造函数

#### 7.1、为什么拷贝构造函数必须引用传递

**为了防止递归调用。**一个对象以值方式传递时，编译器需要调用它的拷贝构造函数生成副本，这样会无限循环。

#### 7.2、调用拷贝构造函数的情况

- 对象以值传递方式进入函数。
- 对象以值方式从函数返回。
- 对象通过另一个对象初始化。

### 8、预处理、编译、汇编、链接

- 预处理：处理#开头的命令，将头文件插入到程序文本中。
- 编译：将程序文本编译成汇编语言程序，以标准的文本格式描述一条低级语言指令。
- 汇编：把汇编指令翻译成机器语言指令。也就是.o文件，只有二进制信息，之前阶段还有字符。
- 链接：汇编后的程序是多个.o文件，程序中不同函数会分成多个不同的目标文件，需要把这些模块合并在一起，形成可执行文件。

### 9、智能指针

智能指针是一个类，用来管理指针。类的作用域结束时，就会自动调用类的析构函数。可以**防止悬空指针使用、二次释放、堆内存泄漏。**

- auto_ptr：c11已经放弃。

  采用所有权模式。智能指针p2可以剥夺智能指针p1的所有权，导致访问p1时出错，存在潜在的内存崩溃问题。

  ```cpp
  auto_ptr<string> p1(new string("hello"));
  auto_ptr<string> p2;
  p2 = p1; // 不报错，导致后续使用p1出错。
  ```

- unique_ptr 替代auto_ptr

  使用严格所有权，同一时间只有一个智能指针可以指向对象。防止资源泄漏。上面`p2 = p1`的操作编译器认为非法。

- shared_ptr 共享型，强引用

  shared_ptr包含两部分：堆上对象的裸指针raw_ptr，引用计数。多个智能指针可以指向相同对象，采用计数表明变量被几个指针共享，解决了unique_ptr必须独占的缺点。

  **环形引用**：如果两个shared_ptr互相引用，这两个指针永远不会为0，资源永远不会释放。A类中有指向B类的智能指针，B类中有指向A类的智能指针。

  ```cpp
  class A; 
  class B; 
  
  class A {
  public:
  public:
  	shared_ptr<B> ptr;
  };
   
  class B {
  public:
  public:
  	shared_ptr<A> ptr;
  };
  
  int main(){
  	shared_ptr<A> pa(new A());
  	shared_ptr<B> pb(new B());
  	pa->ptr=pb;
  	pb->ptr=pa;
  	cout<<pa.use_count()<<endl;
  	cout<<pb.use_count()<<endl;
  	return 0;
  }
  ```

- weak_ptr 弱引用

  weak_ptr只是提供一种管理对象的访问手段，不会引起引用计数的变化。shared_ptr循环引用中，只要有一个改用weak_ptr，就能避免死锁。

#### 9.1、Shared_ptr原理

shared_ptr继承自_Ptr_base<_Ty>，是一个引用计数资源管理的类。

_Ptr_base是shared_ptr和weak_ptr的基类。

```cpp
template<class _Ty>
class _Ptr_base
{
private:
    element_type * _Ptr{ nullptr };      //指向资源
    _Ref_count_base * _Rep{ nullptr };   //指向资源引用计数
}
```

element_type是萃取了Ty的数据类型获得的。

Ptr_base中有两个元素：一个指向对应类型的指针，一个引用计数。因为指向相同对象，也就是Ptr相同的指针，应该共享同一引用计数。所以，引用计数是一个**指针指向引用计数类**。

计数类是一个指向C++**计数基类**_Ref_count_base 的**指针，也就是一个控制块**（注意是指针而不是整形数，这样才能实现多个指针指向同一个对象的时候，计数共享）。

在_Ref_count_base 类中，有3个参数，指向数据块的指针，引用计数use_cnt和弱引用计数weak_cnt。同时提供了**原子操作增减引用计数**，保证线程安全。

> 原子操作是怎么保证的？
>
> ![img](C:\Users\absol\Desktop\面试知识\c++的经文.assets\1936870-20200510131120081-1089007295.png)
>
> 能看到增加计数这个操作，需要三个参数，当前值、新值、期望值。只有当前的引用计数和函数开始时的计数值一样，才会进行增加，保证+1是正确修改的。

##### 9.1.1、shared_ptr是何时发生了引用计数增加的？

在使用一个新的shared_ptr指向同一个资源时，会使用到拷贝构造，或者是赋值的方式，ptr_base中提供了拷贝构造函数，会在原本的数据块计数上+1。

#### 9.2、智能指针是线程安全的吗？

**线程安全**：多线程操作一个共享数据时，能保证所有线程的行为是符合预期的。

根据上面的分析，Shared_ptr的引用计数增减通过原子操作保证了线程安全。

但是智能指针中的指针并不安全，多个线程读写同一个shared_ptr就可能会出错。比如下面的例子，线程一还没来得及赋值，共享指针可能就被改变了，这就是**多线程公用一个智能指针**导致的。

```cpp
// global
std::shared_ptr<Foo> g_ptr;

// 线程1
std::shared_ptr<Foo> x;
x = g_ptr;

// 线程2
std::shared_ptr<Foo> y;
g_ptr = y
```



#### 9.3、shared_ptr创建方式

**make_shared 只需要分配一次内存，而直接创建 shared_ptr 需要分配两次内存**。

### 10、struct、class

#### 10.1、C、C++中struct的区别

**本质区别**：C++的struct就和类一样，可以**使用成员函数**、构造、析构函数，有**权限控制**和**继承关系**。

- C结构体不能有函数，C++结构体内部允许有函数。
- C的结构体内部成员变量访问权限只能是public，C++可以设置为public、private、protected。
- C的结构体不能继承，C++可以继承。
- C++的struct可以使用构造和析构函数。

#### 10.2、C++中struct、class的区别

C++的struct和class基本类似，可以使用类的操作：构造析构、权限管理。struct、class都能通过虚函数实现多态，都有权限控制，都能定义构造、析构函数。区别是：

- **默认权限不同**：struct默认访问权限是public，class默认是private。
- **语义不同**：struct语义上是**数据结构**的实现体，class是**对象**的实现体。

### 11、static

控制**变量的存储方式**和**可见性**。

修饰变量：

- 修饰全局变量：让变量的**作用域限定在声明它的文件内**。否则，普通的全局变量是所有源文件都可以访问，static全局变量能让访问更安全，也能避免多个源文件中多个同名全局变量导致的冲突。**出于安全性，可以优先考虑使用静态全局变量**。在程序启动时，main函数执行前就被初始化。
- 修饰局部变量：**生命周期延长到整个程序运行期间，下次调用时保持上一次的数值，可以持久化数据。**但是**作用域还是在声明它的函数内部**。

- 修饰类成员变量：**所有对象只维持一个实例，生命周期延长到整个程序运行期间，下次调用时保持上一次的数值**。静态类成员变量与类相关，不和类的实例相关联。在类加载时初始化，在整个程序的执行期间不变。

  因为这个static变量是**先于对象存在**的，**只能在类外初始化**，不能在构造函数内初始化，静态成员变量也必须先初始化再使用。


修饰函数：

- 修饰普通函数：**改变函数的可见性，除了声明它的源文件，其他文件不能使用这个函数；避免函数名冲突，其他文件也可以定义同名函数**。默认的函数是可以被其他文件调用的，静态函数限定在声明它的文件可见。
- 修饰成员函数：**使函数不依赖于实例**，可以在没有创建类的实例，也就是**没有创建对象的情况下直接调用**。一些不依赖于具体对象的功能性函数，可以封装成静态成员函数。
  - 静态成员函数**只能访问静态成员变量和其他静态成员函数**，不能直接访问非静态成员变量和函数，因为静态成员函数不依附于对象存在。
  - 无法使用this指针，不能访问实例的成员变量。

### 12、野指针、悬空指针

**野指针：**没有被初始化过的指针，**不确定具体指向**哪里，因此可能会损坏正常的数据。解决：正确初始化指针变量。

**悬空指针：**最初指向的**内存已经被释放的指针**，指针却没有置空。解决：释放指针指向内存后，将指针置空。

### 13、头文件<>和“”的区别

对于#include<a.h>编译器从**标准库路径**开始搜索。#include”a.h”从**用户的工作路径**开始搜索。

### 14、函数指针

是一个**指向函数的指针变量**，指针指向函数的入口地址。

**声明函数指针：**要声明返回结果，传入参数类型

```cpp
void (*pf)(int);
```

**使用函数指针：**创建函数指针变量，并正确指向某个函数，就能通过函数指针使用函数。

```cpp
void foo(int v) {...}
int main() {
    pf p = foo;
    f(42);
}
```

### 15、友元函数

友元函数指的是**可以访问类的私有成员的**，非成员函数。给函数加上friend关键字即可。可以访问私有成员，让数据访问更灵活。

**友元函数和成员函数的区别：**

- 成员函数有this指针，友元函数没有this指针。
- 友元函数不能被继承。

友元类的功能是一样的，友元类的所有成员都能访问对方的私有成员。

### 17、左值右值，move的实现原理？

左值是可以放在赋值号左边的值，必须在内存中有实体；在**表达式结束后仍然存在**的持久对象。

右值是在赋值号右边的值，**表达式结束后就不再存在**的临时对象。

### 18、C/C++读取二维数组的方式

1. 使用两个坐标：m\[i][j]；
2. 当成一维数组，使用一维下表访问：m[i * n + j]；
3. 通过指针访问，指针也可以进行基本运算后访问：int *p = m;

### 19、常成员函数

常成员函数的**声明形式**：返回类型 函数名（参数表）const。

需要注意的是const如果放在头部，则代表返回结果是const的，而不是在强调成员函数是const的。常成员函数有几个特点：

- 不能修改成员变量；
- 不能调用类中其他没有被const修饰的成员函数，**只能调用常成员函数**。

### 20、回调函数callback

回调函数是一种编程方式，并不局限于某种语言。简单来说，就是A模块被定义，在B模块被调用。

回调函数也是一个函数，区别仅仅是将本函数编写的代码，交给了调用函数去使用，具体的函数编写还是在本函数处。

- 普通的设计模式：调用完S服务后，我执行X任务。
- 回调模式：调用完S服务后，告诉S服务，你接着去做X。

#### 20.1、针对高并发场景

在高并发场景下，可以避免等待调用函数带来的阻塞，而是直接将调用结束后要做的事情交给调用函数，本函数可以快速处理。

### 21、拷贝构造函数

#### 21.1、实现拷贝构造函数的注意点

##### 21.1.1、在类内有指针的情况下，一定要自己实现，不能用默认构造函数，并先做相等检测

**不能用默认构造函数**的原因是：默认构造函数只是针对成员变量一个个拷贝，这是浅拷贝。

例如String这个类内有char *的情况下： 需要注意首先进行相等检测。因为String拷贝时，需要三个步骤：

- delete原字符串；
- 拓展新字符串空间到旧字符串的大小。
- 拷贝旧字符串的内容到新字符串中。

进行相等检测，能避免两个String指向同一个字符串时，删除了同一个字符串。

21.1.2、

### 22、output函数

#### 22.1、重载 cout << 运算符时，一定是全局函数

因为如果是类的成员函数的话，使用方式就变成变量右边cout，不符合使用习惯。

#### 22.2、重载 << 需要交给ostream 对象可以直接处理的基本数据类型

例如ostream不能直接 cout String， 但是ostream提供了输出char *的函数，可以在自己重载的String 输出函数中，交给ostream String类中的char *，就能 cout << String 了。

#### 22.3、重载返回的类型是ostream& 

因为这样就可以进行cout 连续的输出操作了，处理完一个之后返回的仍然是ostream对象。

### 23、this指针-成员函数的隐式形参

每个对象都有一个this指针，记录**当前对象的内存地址**，可以在进入对象的成员函数后，还能看到实例本身。

每种面向对象的编程语言都有这种this指针机制，python中叫self。

#### 23.1、为什么const、static不能同时修饰成员函数？

因为const修饰的函数其实是修饰对应的this指针，**const this***，表示this指针成为了一个**常量指针**，不会修改这个对象的变量。而static成员函数没有this指针。

#### 23.2、this指针注意点

- this关键字是一个指向对象自己的常量指针，不能给this赋值。
- 只有成员函数才有this指针，友元函数没有、静态函数没有。
- 在类外无法获取this指针，作用域仅仅在成语函数内部。
- this指针不是对象的一部分，占用的内存不反应在sizeof上。

#### 23.3、同一个类的不同对象是怎么区分的？

一个类的不同对象调用非静态成员函数时，调用的其实是同一段代码，**通过this指针区分不同的对象**。

类的成员函数中访问类的成员变量、调用成员函数时，编译器会隐式地将**当前对象地this指针传给成员函数**。

#### 23.4、为什么static不能访问成员变量

static是一种静态成员函数，与类本身相关，与对象无关，静态函数因此无法获得this指针，不能访问任何非静态成员变量。

### 24、sizeof和strlen（涉及字节对齐问题）

#### 24.1、sizeof计算大小

sizeof本质是在编译阶段就完成的，因此sizeof括号内的赋值是无效的。例如sizeof(a=4)实际上看到的是sizeof(int)。

- 作用于类型，返回类型大小。作用于变量，取决于变量的类型。
- 作用于函数，取决于函数的返回值。作用域结构体是基本类型相加。

##### 24.1.1、指针本身的大小是固定的

32位是4字节，64位是8字节。

##### 24.1.2、数组作为函数参数会退化

C++中数组作为函数参数时，实际上传递的为指向首元素的指针。

```cpp
int func(char array[]) {
    printf("sizeof=%d\n", sizeof(array)); // 12
    printf("strlen=%d\n", strlen(array)); // 11
}

int main() {
    char array[] = "Hello World";
    printf("sizeof=%d\n", sizeof(array)); // 8
    printf("strlen=%d\n", strlen(array)); // 11
    func(array);
}
```

当数组直接作为sizeof的参数时，会得出正确的数组大小。

##### 24.1.3、字节对齐

struct和class需要注意字节对齐问题

##### 24.1.4、字符串数组算上末尾的**'\0'**。

但是strlen不会算上。

##### 24.1.5、如果是空class、struct，还会有1字节长度

C++规定，凡是一个独立的对象都必须有非0大小，这样可以确保**两个不同对象的地址不同**。

#### 24.2、strlen

strlen是一个C库函数，用于获得一个char*类型的字符串长度。与sizeof的区别：

- sizeof是一个编译期间的操作符，strlen是一个函数。
- sizeof能获取各种数据类型和对象的字节数，strlen只能获取C风格字符串的长度。
- sizeof面对char*会计入末尾的'\0'，strlen不会。

### 25、explicit-防止隐式类型转化

当表达式需要另外一个类型时，就会发生隐式类型转换，例如int与long比较大小时就会发生转换。

explicit会要求编译器不做隐式类型转换，只能显式转化。

```cpp
class MyInt {
public:
    MyInt(int n) : num(n) {}
private:
    int num;
};
```

如果使用MyInt a = 10，会成功创建一个MyInt对象。在编译器没有进行复制省略操作时，这个赋值会进行两步：

- int类型的10先**隐式转换**为MyInt。
- 隐式转换后的临时对象再复制构造生成a。

但是有些情况可能不希望这种隐式转换的出现：

```cpp
void f(MyInt n){}
f(10);
```

这个传入的int也能隐式转换成MyInt，成功被函数调用。

如果让这种隐式转换不发生，可以加explicit，这样只能通过显式类型转换才能成功调用：

```cpp
class MyInt {
public:
    explicit MyInt(int n) : num(n) {}
private:
    int num;
};

f(MyInt(10));
```

如果是构造函数参数只有一种类型的，最好加上explicit。

### 26、mutable

让一个成员变量即使是const的成员方法也能修改。

## 二、C++内存管理

### 1、C++内存分配方式

- **栈**：编译器管理，存放局部变量、函数参数。

  函数调用时，首先主函数下一条指令入栈，然后是函数各个参数，之后局部变量入栈。这样函数调用结束后就会回到主函数继续执行，没有碎片。

  栈是高地址向低地址拓展，栈低地址高，空间较小。

- **堆**：程序员管理，手动new、delete。实际上是内存中有一个空闲链表，需要时系统遍历找到第一个空闲内存块给程序，会导致内存碎片产生。

  堆是低地址向高地址拓展，空间较大。

  这样设计的好处是栈、堆地址反向增长，不必可以划分栈、堆的边界，两边碰到一起就说明内存满了。

- **静态存储区**：存储初始化和未初始化的**全局变量、静态变量。**被初始化过的存储在Data区，未初始化过的存储在BSS区，这部分会在运行时分配内存，BSS区不占据可执行文件大小

- **常量存储区**：存储一些文字常量，不允许修改。

- **代码区。**

> 为什么代码、数据分开？
>
> - CPU中指令、数据分开缓存，有利于**提高缓存命中率**。
> - 一段代码可能很多程序复用，分开存储方便实现代码复用。

**局部常量**存放在栈区，**全局常量**存放在符号表中，**字面值常量**（字符串等）存放在常量区。

#### 1.1、为什么函数调用要从右往左压栈

可以更方便的使用C++的变长参数列表特性。

在**模板参数、函数参数**中都可以使用省略号表示不确定数量的参数。

### 2、new、delete

对于非内部数据结构而言，**对象创建时还希望自动执行构造函数**，对象消亡时自动执行析构函数。malloc、free是库函数，无法做到这一点，就有了new、delete操作符。

- new有三个过程：
  - 使用malloc分配空间，malloc返回void*（无类型指针），指向一段内存的首地址；

  - 必须对指针进行强转，void*转成需要构造对象的类型。

  - 使用对象的**构造函数**进行空间初始化，**并返回空间首地址**。

- delete有两个过程：使用**析构函数**析构对象；使用free回收空间。

#### 2.1、new、delete和malloc、free的区别

相同点是：都是在**堆**上分配内存，都必须手动释放。

- new、delete是**操作符**，malloc、free是**库函数**。
- new跟的是一个类型，因为new得到的是初始化后的空间；malloc后面则是申请多少字节长度。
- malloc的返回值是一个void *，使用时必须**强转**，new不需要。
- malloc**申请空间失败**时返回NULL，**new默认会抛出bad_alloc异常**，但是一些编译器vc++6.0不会抛异常。
- malloc申请空间失败返回NULL，必须进行判空，new的对象则不必要进行处理，而是应该在new的过程就捕获异常。
- new会调用构造函数。

### 3、内存泄漏

**申请了一块空间，使用完毕后没有释放掉**。

#### 3.1、导致内存泄露的情况

- **指针重新赋值**。原来指向的内存变成了孤立的内存，无法释放。
- **错误内存释放**。一个指针p指向的变量是数个指针，free(p)会导致这几个指针都变无效，无法释放他们指向的内存。

#### 3.2、避免内存泄漏

- 修改指针前，确保不会导致孤立内存。
- 释放结构化的元素（其中有其他指针时），遍历子内存位置，挨个释放。
- 每个内存分配操作，都有一个释放动作与之对应。

使用valgrind检测内存泄漏。

### 4、类、结构体的内存布局

#### 4.1、空类

声明一个空类，这个类会**占据1个字节**。空类也能被实例化，实例化就必须在内存占据内存地址，因此编译器给空类加上一个字节。

#### 4.2、虚函数类

虚函数类中包含一个**虚函数表指针**，这就会包含一个指针的大小，即使不包含其他任何变量，只是声明了一个虚函数，也就带来指针开销。32位系统一般是4字节，64位系统一般是8字节。

存在继承情况时，有几个父类存在虚函数，子类就需要有多少个指针。

#### 4.3、字节对齐原则

32位系统下，char通常1字节，short2字节，int4字节，double8字节。

对齐会按照变量声明顺序进行，默认对齐大小是**类内最大基础类型**的大小。

##### 4.3.1数组成员

数组成员相当于直接在当前位置定义数组大小个变量。具体的对齐规则还是按照每个变量进行的，对齐长度不会因为声明的是数组而改变。

##### 4.3.2、非基础类型成员

在成员变量中有另一个struct时，相当于是直接展开这个struct，把struct的变量填充到当前class中，因此成员中struct里的数据会影响对齐大小、最终占用字节数。

#### 4.4、静态数据成员

静态数据成员虽然属于类，但是不占据类对象的内存，因为static修饰的成员变量声明周期和程序周期一样，不会随着对象创建。

#### 4.5、继承情况

##### 4.5.1、没有虚函数的继承

子类直接把父类的内存放在自己最前面，内存布局情况也继承父类。

### 5、debug、release版本的区别

- debug版本表示**调试**版本，包含调试信息，**容量更大，不会进行任何优化**，避免调试复杂化。debug模式除了exe文件，还有一个pdb文件，记录断点等调试信息。
- release版本表示**发布**版本，编译时会对程序速度进行优化，代码大小、运行速度都是最优。

### 6、作用域、生命周期

#### 6.1、stack 对象、heap对象的生命周期

stack 对象在超出了自己的作用域后，编译器自动释放。

heap 对象的生命周期必须在delete执行才结束。

- 裸指针在作用域结束后直接死亡，如果没有正确执行delete，指针消失了，指向的内存还没释放，导致**内存泄漏**。这里的指针本身是一个栈对象，指向的内存是一个堆对象。

  ```cpp
  Complex *c = new Complex(1, 2);
  ```

- 智能指针也是一个类，里面包裹了裸指针，智能指针因为作用域结束而死亡时，会执行自己的析构函数，在析构函数中能正确执行包裹指针的析构。

### 6.2、static对象、globla对象的生命周期

static对象的生命周期被延长到了和程序一样，超出作用域后仍然存在，直到程序结束再释放。

global对象的生命周期，在整个程序结束后才消失。

### 7、字节对齐

8个bit组成一个byte，是内存寻址的最小单位。

字节对齐指的是：基本数据类型并不会从任意地址开始存放，而是**从特定的对齐地址开始存放**，这有利于提高存取效率，但是造成了空间浪费。

> 例如，一些平台每次读取都是从偶数地址开始，如果一个int从奇数地址开始存放，就需要两次读取才能取出。

字节对齐规则：

- 基本数据类型的自然对齐边界就是数据大小。char是1字节，short是2字节，int和float是4字节，double是8字节。

- 结构体本身的总大小按照**最大对齐边界的成员**进行对齐，比如最长的成员是int，就按照4的倍数对齐。

  其次，结构体内的成员还可能插入填充字节，按照结构体长度对齐。

#### 7.1、控制对齐规则

使用编译器指令（ `#pragma pack`）更改默认的对齐规则

### 8、字节序

字节序指的是：多字节数据类型中，字节在内存中的存储顺序。

#### 8.1、大端字节序-符合阅读习惯

高位字节存储在低地址处，低字节存储在高地址。

一个4字节的整数0x12345678，在大端字节序的系统中，内存布局是：0x12 | 0x34 | 0x56 | 0x78。

网络传输多使用大端字节序，便于读取有效位。

#### 8.2、小端字节序-便于计算机处理，使用更多

低字节在低地址，一个4字节的整数0x12345678，在大端字节序的系统中，内存布局是：0x78 | 0x56 | 0x34 | 0x12。

因为计算都是低位开始的，电路会先处理低位字节。

### 9、C++RAII思想

Resource Acquistion Is Initialization，资源获取即初始化，把**资源的生命周期和某个对象的生命周期绑定在一起**。

功能是确保控制对象生命周期结束时，可以按照资源获取顺序正确释放所有资源。资源获取失败时，也能按照初始化相反顺序将已经获取的资源正确释放。

#### 9.1、RAII原理

利用**栈上的局部变量的自动析构机制**。资源可能因为程序员设计而忘记释放，或者因为碰到了异常，导致后面的代码不能正确执行，但是变量的析构函数一定会被执行。

#### 9.2、RAII的实现步骤

- 设计一个类封装资源，可以是内存、文件、socket等。
- 构造函数执行资源的初始化，比如申请内存、打开文件、申请锁。
- 析构函数执行销毁操作，比如释放内存、关闭文件、释放锁。
- 使用资源时，声明一个对象的类。

这样程序退出时，或者超出变量作用域时，就会自动调用析构函数，从而自动关闭文件。

#### 9.3、RALL思想包装mutex

使用RALL思想包装mutex，确保多线程编程能安全锁定和解锁互斥量。

```cpp
#include <iostream>
#include <mutex>
#include <thread>

class LockGuard {
public:
    explicit LockGuard(std::mutex &mtx) : mutex_(mtx) {
        mutex_.lock();
    }

    ~LockGuard() {
        mutex_.unlock();
    }

    // 禁止复制
    LockGuard(const LockGuard &) = delete;
    LockGuard &operator=(const LockGuard &) = delete;

private:
    std::mutex &mutex_;
};

// 互斥量
std::mutex mtx;
// 多线程操作的变量
int shared_data = 0;

void increment() {
    for (int i = 0; i < 10000; ++i) {
        // 申请锁
        LockGuard lock(mtx);
        ++shared_data;
        // 作用域结束后会析构 然后释放锁
    }
}

int main() {
    std::thread t1(increment);
    std::thread t2(increment);
    t1.join();
    t2.join();
    std::cout << "Shared data: " << shared_data << std::endl;
    return 0;
}
```





## 三、C++特性

### 1、动态特性

大多数程序功能在编译器就能确定，在运行时才能确定的是动态特性。C++中的**虚函数、抽象基类、多态**都属于动态特性。

### 2、面向对象编程

#### 2.1面向对象的三个特点

封装、继承、多态。

- 封装：把客观事物封装成抽象的类，隐藏类的内部实现细节，只暴露必要的几口给外部，通过封装还能控制类成员的访问级别。

- 继承：让某个类获得另一个类的属性、方法。可以构建类层次结构，减少重复代码。

- 多态：不同类的对象使用相同的接口名字，但是具有不同实现。多态通过**继承+虚函数**实现。基类和派生类使用同样的函数名，完成不同的操作。


#### 2.2、面向对象程序设计理念

**数据**和**操作**被组织在一起，形成对象。

### 3、C++的类型转换

C语言使用(type_name)expression的方式强转，C++推荐使用四种转换操作符实现。

#### 3.1、static_cast

性能和C语言的括号强转很像。

- 基本类型之间的转换。

  ```cpp
  int a = 42;
  double b = static_cast<double>(a); // 将整数a转换为双精度浮点数b
  ```

- **指针类型转换**，可以将基类转换为派生类，但是**没有类型检查**。

  ```cpp
  class Base {};
  class Derived : public Base {};
  
  Base* base_ptr = new Derived();
  Derived* derived_ptr = static_cast<Derived*>(base_ptr); // 将基类指针base_ptr转换为派生类指针derived_ptr
  ```

- **引用类型转换**，类似于指针的转换，**没有类型检查，不保证安全性**。

  ```cpp
  Derived derived_obj;
  Base& base_ref = derived_obj;
  Derived& derived_ref = static_cast<Derived&>(base_ref); // 将基类引用base_ref转换为派生类引用derived_ref
  ```

#### 3.2、dynamic_cast

可以更安全的进行父子类层次结构的转换，**必须基类有虚函数表**的情况才能将基类指针转化为子类。（因为底层实现依赖的**RTTI和虚函数表关联**）

如果将基类指针转换为子类时失败，会返回空指针。引用父子类转换则是抛出异常。

##### 3.2.1、多态类型检查

利用的是父子类指针转换失败返回空指针的特性：

```cpp
class Animal { public: virtual ~Animal() {} };
class Dog : public Animal { public: void bark() { /* ... */ } };
class Cat : public Animal { public: void meow() { /* ... */ } };

Animal* animal_ptr = /* ... */;

// 尝试将Animal指针转换为Dog指针
Dog* dog_ptr = dynamic_cast<Dog*>(animal_ptr);
if (dog_ptr) {
    dog_ptr->bark();
}

// 尝试将Animal指针转换为Cat指针
Cat* cat_ptr = dynamic_cast<Cat*>(animal_ptr);
if (cat_ptr) {
    cat_ptr->meow();
}
```

##### 3.2.2、dynamic_cast底层原理-依赖RTTI

dynamic_cast底层依赖于运行时类型信息RTTI实现，包含了类的类型信息和类层次结构。

RTTI通常与虚函数表vtable关联在一起，每个多态对象都有一个虚函数指针vptr，指向vtable。

检查RTTI的信息判断能否进行转化。

#### 3.3、const_cast

new_type必须是一个指针、引用。

- 修改const对象，可以通过const_cast删除const属性，使得可以修改const的值。

  ```cpp
  const int a = 42;
  int* mutable_ptr = const_cast<int*>(&a); // 删除const属性，使得可以修改a的值
  *mutable_ptr = 43; // 修改a的值
  ```

- const对象调用非const函数。

  ```cpp
  class MyClass {
  public:
      void non_const_function() { /* ... */ }
  };
  
  const MyClass my_const_obj;
  MyClass* mutable_obj_ptr = const_cast<MyClass*>(&my_const_obj); // 删除const属性，使得可以调用非const成员函数
  mutable_obj_ptr->non_const_function(); // 调用非const成员函数
  ```

但是这两种操作都不是很安全。

#### 3.4、reinterpret_cast

转型，重新解释底层的比特位，不进行任何类型检查，不安全。

例如有时会进行指针类型转换：

```cpp
int a = 42;
int* int_ptr = &a;
char* char_ptr = reinterpret_cast<char*>(int_ptr); // 将int指针转换为char指针
```

### 4、C++类成员访问权限

通过访问修饰符控制类的访问权限。

- public：任何调用方都可以直接访问和修改。一般用于类的外部接口，一般不将成员变量设置为public，这不符合封装原则。
- private：只能在类的内部访问，类的成员函数可以访问。
- protected：类似于私有成员，可以被派生类访问，主要提供给子类访问父类对象。

> 友元函数、友元类：可以使外部类访问当前类的public、protected、private全部属性的成员函数、成员变量。

### 5、重载、重写、隐藏

#### 5.1、重载overloading

在**同一个作用域**内拥有**相同方法名**，但是**参数类型、数量不同**的方法。重载方法的**返回类型可以不相同**。

#### 5.2、重写overriding

派生类重新定义基类的方法，主要在继承关系的类之间发生。方法必须**名称相同、参数类型数量相同、返回类型相同、被重写函数在基类要有virtual修饰。**

重载与重写的不同：

- 作用域不同：重写的函数一定在不同的类，重载一定在同一作用域。
- 参数不同：重写的函数参数列表一定相同，重载和被重载的函数参数列表一定不同。
- virtual不同：重写必须在基类用virtual修饰，重载可有可无。

#### 5.3、隐藏hiding

派生类的函数屏蔽了与其同名的基类函数。只要是同名函数，不管参数列表是否相同，基类函数都会被隐藏。

隐藏与重载、重写的区别：

- 参数要求不同：隐藏的函数名一定与基类相同。当参数列表不同时，无论基类函数是否被virtual修饰，基类函数都是被隐藏，无法被重写。

### 6、类对象的初始化和析构顺序

类的初始化遵守几个规则：

- 当前类继承自多个基类，按照**声明顺序初始化**。但是存在虚继承时，优先虚继承。

  比如虚继承：class MyClass : public Base1, public virtual Base2，此时应当先调用 Base2 的构造函数，再调用 Base1 的构造函数。

- 成员变量初始化顺序只和声明顺序有关。

- 构造函数在基类、成员变量初始化完成后才进行。

而析构顺序恰好完全相反。

```cpp
#include <iostream>

class Base {
public:
    Base() { std::cout << "Base constructor" << std::endl; }
    ~Base() {
        std::cout << "Base destructor" << std::endl;
    }
};

class Base1 {
public:
    Base1() { std::cout << "Base1 constructor" << std::endl; }
    ~Base1() {
        std::cout << "Base1 destructor" << std::endl;
    }
};

class Base2 {
public:
    Base2() { std::cout << "Base2 constructor" << std::endl; }
    ~Base2() {
        std::cout << "Base2 destructor" << std::endl;
    }
};

class Base3 {
public:
    Base3() { std::cout << "Base3 constructor" << std::endl; }
    ~Base3() {
        std::cout << "Base3 destructor" << std::endl;
    }
};

class MyClass : public virtual Base3, public Base1, public virtual Base2 {
public:
    MyClass() : num1(1), num2(2) {
        std::cout << "MyClass constructor" << std::endl;
    }
    ~MyClass() {
        std::cout << "MyClass destructor" << std::endl;
    }

private:
    int num1;
    int num2;
    Base base;
};

int main() {
    MyClass obj;
    return 0;
}
```

输出结果是：

```cpp
Base3 constructor
Base2 constructor
Base1 constructor
Base constructor
MyClass constructor
MyClass destructor
Base destructor
Base1 destructor
Base2 destructor
Base3 destructor
```

### 7、析构函数

#### 7.1、析构函数可以抛出异常吗？

语法上可以抛出异常，但是这样做很危险，可能导致两个问题：

- 资源泄露。对象的析构函数一般负责释放该对象持有的资源，如果析构函数抛出异常，这个过程可能会中断，导致资源泄露。
- 叠加异常。析构函数在处理另一个异常时抛出异常，导致异常叠加。程序无法处理两个异常。

一般在析构函数中使用try-catch块捕捉潜在的问题，确保资源正确释放，而不在析构函数中抛出异常。



## 四、C++多线程

### 1、多线程基础

进程是资源分配的基本单位，线程是调度的基本单位。C++中可以加入头文件thread，调用thread的功能。

### 2、多线程设计方法

- 尽量将操作函数设计成**原子操作**，尽量**不访问全局变量**。
- **功能函数**、**线程入口函数**尽量分开。以符合**高内聚，低耦合**的设计理念。
- 在功能完成后，注意销毁线程对象。
- 能不用锁就不用锁，避免效率降低。

### 3、线程池

```cpp
#include <iostream>
#include <thread>
#include <mutex> 
#include <unordered_map>
#include <vector>
#include <queue>
#include<condition_variable>
using namespace std;

class Task {
public:
	template<typename FUNC_T, typename ...ARGS>
	Task(FUNC_T func, ARGS ...args) {
		this -> func = bind(func, forward<ARGS>(args)...);
	} 
	void run() {
		this -> func();
		return ;
	}
private:
	function<void()> func;
};

class ThreadPool {
public:
    ThreadPool(int n) : thread_Start_Flag(false), thread_Size(n), threads(n) {
        this->start();
        return;
    }
    void start() {
        if (thread_Start_Flag == true) return;
        for (int i = 0; i < thread_Size; i++) {
            threads[i] = new thread(&ThreadPool::worker, this);
        }
        thread_Start_Flag = true;
        return;
    }	
	//取任务，执行任务 
	void worker() {
		auto id = this_thread::get_id(); 
		while (running_Map[id]) {
			Task* t = get_One_Task();
            t->run();
            delete t;
		} 
	}
	
    void stop_task() {
        for (int i = 0; i < thread_Size; i++) {
            this->add_task(&ThreadPool::stop_running, this);
        }
    }

    void stop() {
        if (thread_Start_Flag == false) return;
        for (int i = 0; i < thread_Size; i++) {
            this->add_task(&ThreadPool::stop_running, this);
        }
        for (int i = 0; i < thread_Size; i++) {
            if (threads[i]->joinable()) {
                threads[i]->join();
            }
        }
        for (int i = 0; i < thread_Size; i++) {
            delete threads[i];
            threads[i] = nullptr;
        }
        thread_Start_Flag = false;
        return;
    }
	
	template<typename FUNC_T, typename ...ARGS>
	void add_task(FUNC_T func, ARGS ...args) {
		unique_lock<mutex> lock(m_mutex);
		tasks_Queue.push(new Task(func, forward<ARGS>(args)...));
		m_cond.notify_one();
        //通知一次等待信号量m_cond的线程，可以继续进行了。
		lock.unlock();
		return; 
	} 
    
    ~ThreadPool() {
        this->stop();
        while (!tasks_Queue.empty()) {
            delete tasks_Queue.front();
            tasks_Queue.pop();
        }
    }
	
private:
	vector<thread*> threads;
	// decltype 返回任意值的类型, 根据表达式推出类型 
	unordered_map<decltype(this_thread::get_id()), bool> running_Map; //保存所有线程的工作状态 
	queue<Task*> tasks_Queue; // 任务队列
    int thread_Size; // 线程池内线程数量
    bool thread_Start_Flag; // 线程工作开始标志位
    //条件信号量，控制任务队列可以被取走的任务数量
    condition_variable m_cond; 
    std::mutex m_mutex; // 一个互斥锁，确保临界资源任务队列只有一个线程在访问。 
    
	//取到这个Task的线程就会被停掉 
	void stop_running() {
		auto id = this_thread::get_id();
		running_Map[id] = false;
		return ;
	}
	//从任务队列中取出一个来做
    Task* get_One_Task() {
        unique_lock<mutex> lock(m_mutex);
        //队列为空时，等待m_cond信号量，并且将这个信号量释放掉。
        //while循环防止虚假唤醒，避免多个被唤醒的任务争抢资源。
        while (tasks_Queue.empty()) { m_cond.wait(lock); }
        Task* t = tasks_Queue.front();
        tasks_Queue.pop();
        lock.unlock();
        return t;
    }

};

void func(int a, int b) {
	cout << "function " << a << " " << b << endl;
	return ; 
}


// <---------------工作函数------------------> 
bool is_prime(int x) {
	for (int i = 2;i * i <= x; i++) {
		if(x % i == 0){
			return false;
		}
	}
	return true;
}

int prime_count(int l, int r) {
	int ans = 0;
	for (int i = l; i <= r; i++) {
		ans += is_prime(i);
	}
	return ans;
}

void worker(int l, int r, int &ret) {
	ret = prime_count(l, r);
	cout << this_thread::get_id() << "done" << endl;
	return ;
}

int main() { 
	#define batch 5000000
	ThreadPool tp(5);
	int ret[10];
	for (int i = 0, j = 1; i < 10; i++, j+=batch) {
		tp.add_task(worker, j, j + batch -1, ref(ret[i]));
	}
	tp.stop();
	int ans = 0;
	for(auto x : ret) {
		ans += x;
	}
	cout << ans << endl;
	return 0;
}
```

#### 3.1、线程池组成部分

##### 3.1.1、Task类

Task类，**只有一个成员变量function<void()> func**，将工作函数封装起来，转换为统一的任务类型，方便交给线程操作。

- function是C++11 中的一个**函数包装器**，是一个进阶版的**函数指针**。和函数指针一样，声明传入类型与返回结果类型，可以把函数指针、普通函数、模板对象函数都包装进里面。

- 为了使线程池支持不同的任务类型、不同的参数，Task使用模板对象函数来描述任务。
- 使用了bind作为函数适配器，因为Task的function作为一个成员变量，不能让成员变量在运行时才确定，而需要确定的类型，就使用bind接收不同的函数、函数参数，返回统一的可调用对象给Task类。

> 调用bind的一般形式：auto newCallable = bind(callable,arg_list);`
>
> 其中，newCallable本身是一个可调用对象，arg_list是一个逗号分隔的参数列表，对应给定的callable的参数。即，当我们调用newCallable时，newCallable会调用callable,并传给它arg_list中的参数。

Task类的构造函数使用模板对象函数，分别传入**工作函数、可变参数列表**。

```cpp
template<typename FUNC_T, typename ...ARGS>
Task(FUNC_T func, ARGS ...args) {
	this -> func = bind(func, forward<ARGS>(args)...);
} 
```

只有一个成员函数，就是调用工作函数并执行。

```cpp
void run() {
	this -> func();
	return ;
}
```

##### 3.1.2、线程池的工作变量

- 任务队列：将Task保存在队列中，供线程取用。

  ```cpp
  queue<Task*> tasks_Queue;
  ```

- 线程列表：保存所有线程对象。

  ```cpp
  vector<thread*> threads;
  ```

- 线程工作状态记录map

  ```cpp
  unordered_map<decltype(this_thread::get_id()), bool>running_Map; //保存所有线程的工作状态 
  ```

- 线程数量，方便控制使用几个线程。

##### 3.1.3、工作函数

- 设计一个函数停止线程。

  ```cpp
  void stop_running() {
  	auto id = this_thread::get_id();
  	running_Map[id] = false;
      // 取得线程ID并将线程工作状态置为false。
  	return ;
  }
  ```

- 设计一个从任务队列取任务的函数。

  ```cpp
  Task* get_One_Task() {
      unique_lock<mutex> lock(m_mutex);
      // 队列为空时，等待m_cond信号量，并且将这个信号量释放掉。
      // while循环防止虚假唤醒，避免多个被唤醒的任务争抢资源。
      while (tasks_Queue.empty()) { m_cond.wait(lock); }
      Task* t = tasks_Queue.front();
      tasks_Queue.pop();
      lock.unlock();
      return t;
  }
  ```

##### 3.1.4、线程池类

- 初始化线程池，用c++的thread创建要求数量的线程。

  ```cpp
  void start() {
     if (thread_Start_Flag == true) return;
     for (int i = 0; i < thread_Size; i++) {
        threads[i] = new thread(&ThreadPool::worker, this);
     }
     thread_Start_Flag = true;
     return;
  }	
  ```

- 取一个任务来做的函数，使用从任务队列取任务的工作函数。

  ```cpp
  //取任务，执行任务 
  void worker() {
  	auto id = this_thread::get_id(); 
  	while (running_Map[id]) {
  		Task* t = get_One_Task();
          t->run();
          delete t;
  	} 
  }
  ```

- 给线程池的任务队列添加任务。

  ```cpp
  template<typename FUNC_T, typename ...ARGS>
  void add_task(FUNC_T func, ARGS ...args) {
  	unique_lock<mutex> lock(m_mutex);
  	tasks_Queue.push(new Task(func,forward<ARGS(args)...));
      //通知一次等待信号量m_cond的线程，可以继续进行了。
      m_cond.notify_one();
  	lock.unlock();
  	return; 
  } 
  ```

- 结束线程：套用任务队列这个模式，就是给线程池加入终止任务，刚好是线程数量个，每个线程领取一个，就会释放自身资源。全部结束后就能退出线程池。

  ```cpp
  void stop() {
     if (thread_Start_Flag == false) return;
     for (int i = 0; i < thread_Size; i++) {
        this->add_task(&ThreadPool::stop_running, this);
     }
     for (int i = 0; i < thread_Size; i++) {
        if (threads[i]->joinable()) {
            threads[i]->join();
        }
     }
     for (int i = 0; i < thread_Size; i++) {
        delete threads[i];
        threads[i] = nullptr;
     }
     thread_Start_Flag = false;
     return;
  }
  ```

##### 3.1.5、互斥锁

只有一个互斥资源——任务队列，因此也只在插入任务、取任务的时候加锁。**为了防止竞争，C++的条件变量condition_variable 总是和一个互斥量结合在一起。**条件变量是为了线程通信，互斥锁是为了互斥资源被正确使用。

```cpp
condition_variable m_cond; // 条件变量，这是线程可以拿的任务数量
std::mutex m_mutex; // 一个互斥锁，确保临界资源任务队列只有一个线程在访问。 
```

- 这里的任务队列是**生产-消费者模式**，加入任务是生产者，取出任务是消费者。

- 为了防止竞争，加入、取出的操作都要在互斥锁.lock，互斥锁.unlock的操作中间。
- 任务队列为空时，需要取任务的线程就要等待条件变量m_cond，并且释放互斥锁。
- 插入任务时，就使用条件变量的notify_one，唤醒等待这个变量的一个线程。

## 五、虚函数相关问题

### 1、多态的实现有哪几种？

多态分为静态多态和动态多态。

- **静态多态**是通过**函数重载和模板技术**实现的，在**编译期间**确定；
- **动态多态**是通过**虚函数和继承关系**实现的，执⾏动态绑定，在运⾏期间确定，通过子类重写父类的虚函数来实现。

### 8、C++多态的实现方式

#### 8.1、动态多态-虚函数、纯虚函数

所调用的具体方法是在**运行**期间确定的。

使用虚函数实现的多态必须满足两个条件：

- 必须通过**基类的指针、引用**调用虚函数。
- 被调用的函数是虚函数，必须完成对**基类虚函数的重写**。

> **静态绑定**：使用派生类的指针访问、调用虚函数时，编译器就能确定对象是派生类型，直接调用派生类的虚函数。也就是上面条件的第一点不满足。
>
> **动态绑定**：通过基类的指针和引用才能调用虚函数，运行期间才能确定。

#### 8.2、静态多态-函数模板、函数重载

**编译**阶段就能确定下来的多态就是静态多态。

模板函数可以根据传入参数的类型不同，自动生成相应类型的函数代码。

函数重载可以实现名称相同、参数列表不同的不同功能的函数。

### 2、虚函数如何实现、动态绑定如何实现？

虚函数表象是在**基类的函数前加上virtual关键字**，在派生类中重写该函数。运行时根据对象的实际类型调用相应的函数。

**虚函数的实现原理：**编译器会在类中有虚函数时，创建一张**虚函数表vtable**，派生类继承基类，派生类自己的虚函数会写在第一个基类的虚表最后。

**虚函数表的内容**：存储着这个类的虚函数指针，这些指针指向实现这个虚函数的代码地址。

#### 2.1、虚函数指针的存储

在对象中增加一个**虚函数指针vptr**，指向类的虚函数表，一般都在**对象的内存布局的头部存放指针**，一个类的不同对象的指针共同指向同一块虚函数表。

#### 2.2、继承情况下的虚函数表

- 派生类拷贝基类的虚函数表，如果是**多继承**，就拷贝**每个基类的虚函数表**，派生类内存空间会有多个基类的虚函数指针；

  - **子类没有自己的虚函数**：只要基类有虚函数，就有虚函数表。
  - **子类有自己的虚函数**：子类会把自己的虚函数写在第一个基类的虚函数表的后面。

- **有一个基类和派生类自身共用一个虚函数表**，派生类自己的虚函数会写在派生类的虚函数指针指向的**基类虚函数表**中（多继承时，就会写在第一个基类虚函数表中）。

- 如果子类重写了父类的虚函数，就会覆盖子类的虚函数表中的这个函数。

- 注意不存在虚函数表间接继承的情况，B、C继承自A，D继承自B、C，D中就只有B、C的虚函数表，而不会有A的。

#### 2.3、虚函数的工作流程

- 创建对象时，编译器为对象分配内存，将**虚函数指针指向正确的虚函数表**（实例化之后只有一个指针，类中的虚函数表是所有对象共用）；
- **调用虚函数时**，编译器根据对象的虚函数指针访问对应的虚函数表；
- 根据函数在虚函数表中的偏移量确认要调用的**函数地址**。
- 根据函数地址**调用对应的虚函数**。

#### 2.4、虚函数表生成时机

在**编译阶段**生成，每个类仅有一个，供所有对象共享，放在全局数据区。

### 3、纯虚函数作用

在虚函数声明后加上=0就是纯虚函数。C++没有接口功能，纯虚函数可以提供一个类似的功能。

使得一个类称为**抽象类**，**强制派生类实现接口**。有纯虚函数的类就是**抽象类，不能被实例化**，抽象类的派生类必须实现纯虚函数才能实例化，否则也是抽象类。

### 4、虚函数表占用字节

只要在类中定义了虚函数，就会**存储虚函数指针，占据一个指针的字节**。

派生类继承基类时，会继承所有有虚函数基类的虚函数指针，有几个指针，就多占据几个指针的空间。

### 5、为什么要用虚函数定义析构函数

为了**降低内存泄漏**的可能。当使用基类指针指向派生类对象时，如果基类的析构函数不是虚析构函数，编译器根据指针类型，会认为当前对象是基类，将只会调用基类的析构函数，而不会调用派生类的析构函数。这可能导致资源泄漏或行为不正确。

通过将基类的析构函数声明为虚析构函数，可以确保在销毁对象时，根据实际对象，先调用派生类的析构函数，再调用基类的析构函数。

> 那为什么不默认把析构函数设置为虚函数？
>
> 类中有虚成员函数时，会进行一些额外工作，生成虚函数表、虚函数指针指向虚函数表，会占用额外的内存，如果基类不被继承时，就会带来内存开销。

### 6、哪些函数是虚函数，哪些不能？

可以是虚函数的：

- 类的普通成员函数。
- 析构函数。

不能是虚函数的：

- 内联函数：内联函数将函数内容替换到函数调用处，是静态编译的，动态调用的虚函数。
- 静态函数：静态函数只和类有关系，不依赖于对象调用，不能是虚函数。
- 构造函数：可以，但是一般不是虚函数。
- 友元函数：友元函数不属于类的成员函数，不能被继承，不能被继承的函数就没有虚函数的概念。
- 普通函数：因为不是成员函数。普通函数只能被重载，不能被重写，声明为虚函数没有意义。

### 7、为什么构造函数一般不能是虚函数？

- 从**语法**层面来说，**多态性在对象创建前不适用，每个类都应该有自己的构造函数**：派生类中，基类的构造函数会被派生类自动调用，构造函数没有被覆盖的必要。
- 从**虚函数表**的机制来说，**执行构造函数前对象尚未创建，虚函数表不存在：**对象需要实例化之后，才会在对象中设置一个虚函数指针，指向虚函数表。但是实例化的工作是构造函数来完成的，不该让构造函数是虚函数。

### 8、构造、析构函数中能否调用虚函数？

可以实现，但是最好不要，无法正确实现多态。

```cpp
#include <iostream>

class Base {
public:
    Base() {
        foo();  // 在构造函数中调用虚函数
    }

    virtual void foo() {
        std::cout << "Base::foo()" << std::endl;
    }
};

class Derived : public Base {
public:
    void foo() override {
        std::cout << "Derived::foo()" << std::endl;
    }
};

int main() {
    Derived d;  // 创建派生类对象
    return 0;
}

```

- 构造函数中调用虚函数，因为派生类对象还没构造完成，因此调用的虚函数指向的是基类的函数。

  正如上面例子，原本期望能使用派生类的foo函数，但是由于构造函数的调用发生在对象创建期间，动态类型还没确定，**使用的是构造函数处的类型。**

- 析构函数使用虚函数。

### 9、虚继承

解决多重继承的问题，如果有多个类继承自同一个基类，又有新的类继承自这多个类，**虚继承可以让间接继承共同基类时只保留一份成员。**

```cpp
class A    // 声明基类A
{
    // 代码
};
class B: virtual public A    // 声明类 B 是类 A 的公有派生类，A 是 B 的虚基类
{
    // 代码
};
class C: virtual public A    // 声明类 C 是类 A 的公有派生类，A 是 C 的虚基类
{
    // 代码
};
class D: public B, public C    // 类 D 中只有一份 A 的数据
{
    // 代码
};
```

C++编译器在初始化时，只执行最后的派生类对虚基类的构造函数调用，忽略其他虚基类的派生类。





## 六、使用技术

### 1、如果不使用auto，怎么写出迭代器的全称？

```cpp
unordered_map<string, vector<string>> strs_map;
for(unordered_map<string, vector<string>>::iterator it = strs_map.begin(); it != strs_map.end(); it++) {
```

写全容器中的数据类型、名称，再用::加上iterator表明是迭代器。

## 七、STL相关问题

### 1、std::find和容器的find效率比较

std::find表示接受一个范围的迭代器作为参数，以及要查找的值，查找效率很低，是O（N）。

特定的一些容器可能会有额外的优化。
