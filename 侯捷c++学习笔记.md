---
typora-root-url: E:\Typora_imageUsed
---

## 面向对象编程

### 类创建

* private：只有在类内部的函数才能使用这里的数据
* public：任意地方的函数都可以调用
* friendly：类内部的所有东西互为友元，可以相互调用
* protected：

* 类函数可以定义在类里面，也可以定义在类外面，如果定义在类外面，可以通过加inline方式使该全局函数变成类的内联函数，而且也可以加引用&使得函数只传递地址。

* 操作符重载: 

  1. 返回类型
  2. 是否为全局函数

  示例：

  ~~~c
  complex& operator << (ostream& os, const complex& cp){
  	return os << "real: " << cp.real() << ",\n" << "image: " 
  				<< cp.img() << endl;
  }
  ~~~

* const：参数不会被修改

* this：表示调用函数者
* inline：内联函数
* &：返回值不是临时对象的可以加&，不能用完就消亡了，没有意义

### 三大函数

以string为例

#### 构造函数

无指针的情况

~~~c++
inline complex::complex()
:real(0), img(0)
{
	//real = 0;
	//img = 0;
}
~~~

有指针的情况

~~~c++
inline String::String(const char* cstr = 0){
    if(cstr){
        m_data = new char[strlen(cstr) + 1];
        strcpy(m_data, cstr);		//未使用该函数会出错
    }else{
        m_data = new char[1];
        *m_data = '\0'
    }
}
~~~



* 没写的话会自动使用系统的默认构造函数

* 如果类里带指针，就需要自己写构造函数，不然只能浅拷贝，在赋值之后甚至可能造成内存泄漏（memory leakage, 东西还在，但是没有指针指着了）、别名（alias，两个指针同时读写）
* 有指针的话，就要分配空间，那么在结束的时候就需要**析构函数**去释放空间

#### 拷贝复制

* **拷贝赋值一定要关注是否是自我赋值**
* 传返回值不必在乎是以什么样的形式传出去的

~~~c++
String& operator=(const String& str){
    //检测自我赋值(self assignment)
    if(this == &str)		//这里检查的是地址是否一样
        return *this;		
    //如果是自我赋值，这步会出错(删了自己),导致不确定行为(undefined behaviour)
    delete[] m_data;		
    m_data = new char[strlen(str.m_data) + 1];
    strcpy(m_data, str.m_data);
    return *this;
}
~~~



#### 析构函数

~~~cpp
inline				//定义在类之外
String::~String(){
    delete[] m_data;
}
~~~

### 堆，栈与内存管理

#### stack

存在作用域内的一块内存空间。当调用函数时，函数本身会形成一个stack来放置所接受的参数，以及返回地址，在函数本体内声明的任何变量，所使用的内存块都取自上述stack

~~~c
{
	Complex c1(1, 2)
}
~~~

这里是local object，其生命会在作用域结束之后结束，并由析构函数删除

~~~  c++
{
	static Complex c2(1, 2);
}
~~~

这里是static object，生命在作用域结束之后仍然存在，直到整个程序结束

~~~c++
class Complex {...};
...
Complex c3(1, 2);
int main(){
    ...
}
~~~

这里是global object，其生命在整个程序结束之后才会结束。可以视为一种static object，作用域是整个程序

* 注意new和delete成对出现，否则会出现内存泄漏

#### heap

system heap，指由操作系统提供的一块global内存空间，程序可能自动分配从其中获得若干区域

~~~cpp
class Complex{...};
...
{
	Complex c1(1,2);	//c1所占用的空间来自stack
	Complex* p = new Complex(3);	
	//Complex(3)是个临时对象，所占用的空间以new自heap自动分配而得，并由p指向
}
~~~

#### new

* 先分配memory，再调用ctor

~~~c++
Complex* pc = new Complex(1, 2);
~~~

编译器转化为

~~~~c++
Complex *pc;
void* mem = operator new( sizeof(Complex) );	//分配内存,内部调用malloc
pc = static_cast<Complex*>(mem);				//转型
pc->Complex::Complex(1, 2);						//构造函数
~~~~

在内存中调用new之后，会申请一片空间，其中结构如图：

<img src="C:\Users\Xiao Linfeng\AppData\Roaming\Typora\typora-user-images\image-20221108212154144.png" alt="image-20221108212154144" style="zoom: 33%;" />

其中包括了：

1. cookie头尾：4 * 2，如果是要表示这块区域被使用了，则会再最后一位加一；
2. debug部分：32 + 4；
3. 数据区：申请的大小；
4. pad：填补到16的倍数，对齐；

#### delete

* 先调用dtor，再释放memory

~~~c++
delete ps;
~~~

编译器转化为

~~~c++
String::~String(ps);		//析构函数
operator delete(ps);		//释放内存，内部调用free
~~~

* array new  , 		m_data = new char[strlen(cstr) + 1];
* array delete, 		delete[] m_data;
* array new 和 array delete要搭配使用，否则会出错（只唤起一次dtor，只删除0位）

### 补充

#### 静态函数与成员函数

static就是静态的数据或者函数部分，在内存中单独的占有一份空间，与成员函数的区别在于：

* 成员函数的调用是通过this指针来调用的，成员函数的参数中隐含了this参数
* 成员函数的数据可能有多份（不同的this），而静态函数保存的数据应当只有一份，适用于大家看到的都是一样的数据的情况，类似银行利率，而成员函数可以理解为银行不同的客户的存款情况
* 静态函数如果要处理数据，只能处理静态的数据，因为它没有this指针
* **静态数据在类内声明，在类外定义（两步必须完整）**
* 静态函数不能调用非静态函数（不存在对象的情况下），因为**静态函数是分配了内存的**，而非静态函数是未分配内存的，而非静态函数可以直接访问静态成员
* **静态成员在每一个类中都有一份拷贝**（不管有多少个对象，在内存中只分配一块空间），解决了同一个类中的不同对象之间的数据和函数共享的问题
* 非静态成员的生命周期在类生命周期结束后就结束了，而静态成员不存在生存期的概念
* 构造函数也可以分为静态构造函数和非静态构造函数
* 可以用两种方式调用成员
  1. 非静态成员：通过object调用
  2. 静态成员：通过class name调用

~~~ c++
class Account{
public:
	static double m_rate;
	static void set_rate(const double& x){m_rate = x};
}
double Account::m_rate = 8.0;		//注意在类外写，可以不给初值

int main(){
    Account::set_rate(5.0);		//通过class name调用
    Account a;
    a.set_rate(7.0);			//通过对象调用
}
~~~

#### 将构造函数放在private区

* Singleton：对象只能有一个，外界不能创建，只在private区用静态函数的方式创建一个，可以通过public区的函数来得到该对象
* 更好的写法：在public区的函数中创建并返回，当不再使用时（离开作用域），析构函数会自动将其回收

#### cout

* 继承自ostream
* 在ostream中对<<操作符进行了重载，使其适用于基础类型

#### class template 类模板

~~~c++
template<typename T>
class complex
{
public:
	complex (T r = 0, T i = 0)
        :
    {}
    complex& operator += (const complex&);
    T real() const {return re;}		//值不能被改变，加const
    T imag() const {return im;}
private:
    T re, im;
    
    friend complex& __doapl (complex*, const complex&);
};

...

{
    complex<double> c1(2.5, 1.5);
    complex<int> c2(2, 6);
    ...
}
~~~

#### function template 函数模板

~~~c++
stone r1(2, 3), r2(3, 3), r3;
r3 = min(r1, r2);
~~~

到

~~~c++
template <class T> 
inline
const T& min(const T& a, const T& b){
	return a < b ? a : b;
}
~~~

到

~~~c++
class stone{
	...
	bool operator< (const stone& rhs);
	...
}
~~~

#### namespace

* using directive

~~~c++
#include <iostream>
namespace std;
...
{
	...
	cout << endl
	cin >> a;
}
~~~

* using declaration

~~~c++
#include <iostream>
using std::cout
...
{
	...
	cout << endl;
	std::cin >> a;
}
~~~

* no using

~~~c++
#include <iostream>
...
{
	...
	std::cout << endl;
	std::cin >> a;
}
~~~

### 面向对象编程

#### 继承

有三种继承方式：public\ private\ protected

在原来的东西上附加上新的东西，表示的是函数的调用权

~~~c++
struct _List_node_base
{
    _List_node_base* _M_next;
    _List_node_base* _M_prev;
};

template<typename _Tp>
struct _List_node
    :public _List_node_base
{
    _Tp _M_data;    
};
~~~

 <img src="C:\Users\Xiao Linfeng\AppData\Roaming\Typora\typora-user-images\image-20221109163019304.png" alt="image-20221109163019304" style="zoom: 50%;" />

* base class的class必须是virtual，否则会出现undefined behavior

* 构造由内而外

~~~c++
Derived::Derived(...): Base() {...};
~~~

* 析构由外而内

~~~
Derived::~Derived(...){... ~Base();};
~~~

##### 虚函数继承

非虚函数：不希望derive class重新定义（override，重写）

虚函数按可不可以重写可以分成两种：

纯虚函数：只声明，子函数必须要重写

非纯虚函数：内容为空或者不完整，子函数按需求重写，也可以不重写

~~~c++
class Shape{
public:
	virtual void draw() const = 0; //纯虚函数
    virtual void error(const std::string& msg); //非纯虚函数
    int objectID() const; //非虚函数
    ...
}
class Rectangle: public Shape{...};
class Ellipse: public Shape{...};
~~~

#### 复合Composition

* 表现为类里有什么东西（比如queue类的底层容器为deque)

~~~c++
//queue就是deque的adapter
template <class T>
class queue{
    ...
protected:
 	deque<T> c;		//底层容器
public:
    //以下完全利用c的操作函数完成
    bool empty() const {return c.empty;}
    size_type size() const {return c.size();}
    reference front() {return c.front();}
    reference back() {return c.back();}
    //
    void push(const value_type& x){c.push_back(x);}
    void pop() {c.pop_front();}
}
~~~

* 构造由内而外：

​			Container的构造函数首先调用Component的默认构造函数，然后才执行自己

~~~ c++
Container::Container(...): Component() {...};
~~~

* 析构由外而内：

​			Container的析构函数首先执行自己，然后才调用Component的析构函数

~~~c++
Container::~Container(...){...~Component()};
~~~

#### 委托Delegation. (Composition by reference)

在这里，用户只能看到String类，而真正实现功能的类为StringRep类，这种思想叫做pointer to implementation。也叫编译防火墙、Handle/Body.

而在将来，StringRep也可以指向不同的实现类，具有一种弹性。

~~~c++
//file String.hpp
class stringRep;
class String{
public:
	String();
    String(const char* s);
    String(const String& s);
    String &operator=(const String& s);
    ~String();
    ...
private:
    StringRap* rep;	//pimpl
}
~~~

如果某个指针要修改字符串，则它不能让其他指针看到的内容发生改变，所以会复制一份副本供修改

~~~c++
//file String.cpp
//实现
#include "String.hpp"
namespace{
class StringRep{
friend class String;
	StringRep(const char* s);	//构造函数
    ~StringRep();	//析构函数
    int count;	//统计还有几个类在指向该数据
    char* rep;	//数据
}
}

String::String(){...}
~~~

#### 一些表示类关系的符号

#function：protected

-function：private

+function：public（可不写）

<u>function</u>:typename：静态函数

derived function---▷base function：继承

class1 ◇--->class2：委托

class1 ◆--->class2：复合

#### 经典案例

##### Composite

 <img src="/C:/Users/Xiao Linfeng/AppData/Roaming/Typora/typora-user-images/image-20221112195748995.png" alt="image-20221112195748995" style="zoom:50%;" />

类与类之间的组合可以描述一种问题的解法，这里描述的是一种处理类似文件系统的模式：文件夹里可以放文件夹和文件，也就是两种不一样的指针。

其中Primitive代表file文件，Composite代表文件夹。

文件夹类有些特别，它即要包含文件，又要包含文件夹，需要一个抽象的类来表示文件和文件夹的同宗，即两者都可以用一种东西来概括，也就是Component，所以Composite里可以委托Component来实现文件夹的存储。

而容器内的物体大小应该要一致，所以存储的是指针Component*。add同理。

**那么问题来了，如何去判断Component*是哪个子类的指针？**

实际上不需要判断，typeid可以精确知道子类，且一个类指针可以使用dynamic_cast正确的转换为其基类或者子类的指针，使用*去访问的时候也可以得到正确的值，他们在内存中的结构是这样的：

![image-20221112210812928](/C:/Users/Xiao Linfeng/AppData/Roaming/Typora/typora-user-images/image-20221112210812928.png)

实际上**由派生类指针指向基类指针就叫做up-cast**

~~~c++
//用基类指针指向派生类对象up-cast
Base1* ptr = new Derived1;
~~~

实现：

~~~c++
//Component
class Component{
private:
    int value;
public:
    Component(int val) {value = val;}
    //这里不能是纯虚函数，因为文件是不需要add操作的，继承无操作的add而不用重写才合理
    virtual void add(Component*) {}
};

//file
class Primitive: public Component
{
public:
	Primitive(int val):Component(val){}    
};

//dirent
class Composite:public Component
{
protected:
	vector<Component*> c;
public:
    Composite(int val):Component(val){};
    void add(Component* elem){
        c.push_back(elem);
    }
    ...
}
~~~



##### Prototype

<img src="/C:/Users/Xiao Linfeng/AppData/Roaming/Typora/typora-user-images/image-20221112152610721.png" alt="image-20221112152610721" style="zoom:150%;" />

prototype是为了生成未来会出现的类的问题的一种解决方案。

显然开发者不会知道以后会有什么类型出现，所以可以设计一个抽象类Image，而当以后设计出一个新的类的时候，可以使用构造函数去生成一个原型，然后将其存到Image里的prototype里面。当有需要的时候，Image就可以从prototype里得到子类，从而去复制(自己写一个clone，否则没办法复制)、序列化等。

详解：

~~~c++
#include <iostream.h>
//基类
enum imageType
{
    LSAT,SPOT
};
class Image
{
public:
    virtual void draw() = 0;
    //没有类名的情况下要生成派生类，所以是静态函数
    //从prototype中找到相同type然后复制
    static Image *findAndClone(imageType);
protected:
    //子类必须告诉要能返回自己是什么类型，否则无法clone
    virtual imageType returnType() = 0;
    //子类必须要有clone
    virtual Image *clone() = 0;
    //要能通过类名生成子类
   	static void addPrototype(Image *image)
    {
        _prototypes[_nextSlot++] = image;
    }
private:
    //指向子类原型的指针和原型个数
    static Image *_prototypes[10];
    static int _nextSlot;
};
Image *Image::_prototype[];
int Image::_nextSlot;
Image *Image::findAndClone(imageType type)
{
    for(int i = 0; i < nextSlot; i++){
        if(_prototypes[i]->returnType() == type){
            return _prototype[i]->clone();
        }
    }
}

//..................................................................
//派生类
class LandSatImage: public Image
{
public:
    //返回类名
	imageType returnType(){
        return LSAT;
    }    
    //推断是第几个副本
    void draw(){
        cout << "LandSatImage::draw" << _id << endl;
    }
    //重写clone
    Image *clone(){
        return new LandSatImage(1);
    }
protected:
    //用参数dummy做了区分
    //加过一次prototype就不可以加了
    LandSatImage(int dummy){
        _id = _count++;		//推断自己的id
    }
private:
    //静态的自己会调用构造函数，于是就被加入到prototype
    static LandSatImage _landSatImage;
    //构造函数，只使用一次
    LandSatImage(){
        addPrototype(this);
    }
    int _id;
    static int _count
};
LandSatImage LandSatImage::_landSatImage;
//所有该类共享计数
int LandSatImage::_count = 1;
//....................................................................

//同上
class SpotImage:public Image
~~~



### 兼谈对象模型

“勿在浮沙筑高台”

#### 转换函数conversion function

  ~~~c++
  class Fraction
  {
  public:
      //1
      //分母默认为3，将别的类型转换为这种类型
  	explict Fraction(int num, int den = 1)
      	:m_numerator(num), m_denominator(den){}
      
      Fraction operator+(const Fraction &f){
          return Fraction(...);
      }
      //2
      //转换函数无返回类型，将这种的类型转换为别的类型
      operator double() const {
          return (double)m_numerator / m_denominator;
      }
  private:
      int m_numerator;	//分子
      int m_denominator;	//分母
  }
  ~~~

当编译器对Fraction的对象进行操作的时候，编译器首先会找类里有没有转换函数，如果转换函数符合规则，则编译通过。

**non-explicit-one-argument ctor:**只要一个实参的构造函数

例：Fraction d2 = f + 4;

这里会调用 non-explicit ctor将4转换成Fraction(4, 1)，然后再调用operator+

**1、2并存会导致error: ambiguous**，但是如果加了**explict**，编译器就不会自动去调用，就可以避免二义，explict一般用在构造函数前面。

#### pointer-like classes

都是对操作符做一些重载

##### 智能指针

 ~~~c++
 template<class T>
 class shared_ptr
 {
 public:
     //必写
     T& operator*() const {return *px;}
     T& operator->() const {return px;}
     
     shared_ptr(T *p) : px(p) { }
     
 private:
     T* px;
     long* pn;
     ...
 };
 ~~~

~~~c++
struct Foo
{
	...
	void method(void){...}
};

shared_ptr<Foo> sp(new Foo);

Foo f(*sp);

sp->method(); //等价px->method，只是->符号不会被消耗掉
~~~

##### 迭代器

~~~c++
reference operator*() const {return (*node).data;}

pointer operator->() const {return &(operator*())}
//*it与it->method()
~~~

#### function-like classes，仿函数

也是对操作符()做一些重载，使它看起来像函数

#### namespace

在不同的namespace里写同名变量和函数以避免冲突

~~~c++
namespace jj01
...

namespace jj02
...
~~~

#### class template

泛型编程

~~~c++
template<typename T>
class complex
{
public:
	complex(T r = 0, T i = 0)
    	: re(r), im(i)
    {}
    complex& operator += (const complex&);
    T real() const {return re;}
    T imag() const {return im;}
private:
	T re, im;
	
	friend complex& __doapl(complex*, const complex&);
};

{
	complex<double> c1(2.5, 1.5);
	complex<int> c2(2, 6);
	...
}
~~~

#### function template

~~~c++
template <class T>
inline
const T& min(const T& a, const T& b){
	return b < a ? b : a;
}
//stone 已经操作符重载，使用函数不必在乎类型(相同)
stone r1(2,3), r2(3,3), r3;
r3 = min(r1, r2);
~~~

#### member template

可以将派生类当作基类的模板，但是不能反过来

~~~c++
class Base1{};
class Derived1:public Base1{};

class Base2{};
class Derived2:public Base2{};

pair<Derived1, Derived2> p;
pair<Base1, Base2> p2(p);
//即
pair<Base1, Base2> p2(pair<Derived1, Derived2>());
~~~

~~~c++
 template <class T1, class T2>
 struct pair{
 ...
 	T1 first;
 	T2 second;
 	pair()
 		:first(T1()), second(T2())
 	{}
 	pair(const T1& a, const T2 &b)
 		:first(a), second(b)
 	{}
 	
 	template <class U1, class U2>	//U1, U2分别为T1,T2的派生类
 	pair(const pair<U1, U2> &p)
 		:first(p.first), second(p.second)
 	{}
 };
~~~

这种用法在标准库里大量存在

~~~c++
template<typenmae _Tp>
class shared_ptr:public __shared_ptr<_Tp>
{
...
    //模拟up-cast
	template<typename _Tp1>
	explicit shared_ptr(_Tp1* __p)
		:__shared_ptr<_Tp>(__p)
	{}
...
}
~~~

#### specialization 模板特化

特殊类型特殊设计，所以要用到特别化某种类型

~~~c++
//泛化
template <class Key>
struct hash{};
~~~

~~~c++
//特化
template<>
struct hash<char>{
	size_t operator() (char x) const {return x;}
};

template<>
struct hash<int>{
	size_t operator() (int x) const {return x;}
};

template<>
struct hash<long>{
	size_t operator() (int x) const {return x;}
};

//编译器会先去找有没有特化
//这里先用hash<long> ()去创建一个临时对象，然后用(1000)去赋值
cout << hash<long>() (1000);
~~~

#### partial specialization 模板偏特化

##### 个数上偏

~~~c++
template <typename T, typename Alloc = ...>
class vector
{
...
};

template <typename Alloc = ...>
class vector<bool, Alloc>		//绑定为bool
{
...
}
~~~

##### 范围上偏

~~~c++
template <typename T>
class C
{
	...
};
C<string> obj1;

//指定是指针就要用这个特化
template <typename T>
class C<T*>
{
	...
};

C<string*> obj2;
~~~

#### template template parameter 模板模板参数

可以让模板的使用更有弹性（模板里面的委托的容器用装模板）

 ~~~c++
 template<typename T, template <typename T> class Container>
 class XCLs
 {
 private:
     Container<T> c;	//底层容器
 public:
     ...
 };
 
 template<typename T>
 using Lst = list< T, allocator<T>>;
 
 XCLs<string, list> mylst1; //错误用法
 XCLs<string, Lst> mylst2;
 ~~~

这里的容器的类型已经固定死了，所以不能算是模板模板参数

~~~c++
//不是模板模板参数
template <class T, class Sequence = deque<T>>
class stack{
    friend bool operator== <> (const stack&, const stack&);
    friend bool operator< <> (const stack&, const stack&);

protected:
    Sequence c;	//底层容器
    ...
};

stack<int, list<int>> s2;
~~~

#### 关于c++标准库

多用...

 #### variadic templates 数量不定的模板参数(重要)

注意...的位置，这也是语法的一部分

...就是一个所谓的包（pack）

* 用于template parameters，就是template parameters pack（模板参数包）
* 用于function parameter types，就是function parameter types pack（函数参数类型包）
* 用于function parameters，就是function parameters pack（函数参数包）

可以用sizeof...(args)表示参数包里有几个参数

~~~c++
void print()
{
    
}

template <typename T, typename... Types>	//模板参数包
void print(const T& firstArg, const Types&... args)	//函数参数类型包
{
    cout << firstArg << endl;
    //迭代运行，到没有参数的时候会调用上面的print
    print(args...);		//函数参数包
}
~~~

实际运行：

![image-20221121221452576](/C:/Users/Xiao Linfeng/AppData/Roaming/Typora/typora-user-images/image-20221121221452576.png)

#### auto

~~~c++
auto ite = func(typename args);	//自动取返回值的类型
~~~

#### range for

~~~c++
//注意&的使用
for(decl : coll){statement}		//pass by value这里生成了一个副本缓存

for(decl : coll){statement}		//pass by reference传引用
~~~

#### reference

int* 表示的就是地址的一种形式，表示这里存储了一个地址，具体地址多少位取决于机器，比如int* p，表示的就是p是一个变量，是一个pointer to interger型的变量。

int& 表示的就是变量的一种代表（必须用变量初始化，而且绑定之后就不能解绑了），在逻辑上要看成是int型的变量，int& r = x表示的就是r是x的代表，其内部也是用指针去实现的。

注意：

1. sizeof(r) == sizeof(x);
2. &x == &r;

**object和绑定到其上的reference的大小相同，地址也相同（都是假象）**

~~~c++
void func1(Cls* pobj){pobj->xxx();}	//写法不同
void func2(Cls* obj){obj.xxx();}	//慢
void func3(Cls* obj){obj.xxx();}	//被调用端 写法相同，快
...
Cls obj;
func1(&obj);	//接口不同，困扰
func2(obj);		
func3(obj);		//调用端接口相同，很好
~~~

~~~c++
//以下在调用的时候无法区分，被视为"same signature",
//而中间的const也是函数签名的一部分
double imag(const double& im) const{...}
double imag(const double  im) const{...}
~~~

#### Object Model

##### 关于构造和析构

继承：构造base->构造自己；

​			析构自己->析构base;

复合：构造component->构造自己;

​			析构自己->析构component;

继承+复合：	构造(base、component)->构造自己

​						  析构自己->析构->(component、base)

##### 关于vptr和vtbl

 vptr:虚函数指针

vtbl:虚函数继承表

vptr和vtbl的作用就是使c++的虚函数继承变成一个动态的过程，而不是静态的去访问函数的地址



