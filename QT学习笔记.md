### day 1

#### 第一个程序

pro文件用于打开项目（project）

main.cpp

~~~c++
#include "Widget.h"
#include <QApplication>	//应用程序头文件

int main(int argc, char *argv[])
{
    QApplication a(argc, argv);	//定义应用程序对象
    Widget w;	//定义空窗口对象
    w.show();	//调用空窗口子函数，显示窗口
    
    return a.exec();	//调用应用程序对象，消息循环函数
}
/*
QT编辑器快捷键
注释	ctrl+/
运行	ctrl+r
构建	ctrl+b
转定义	F2
*/
~~~

widget.h

~~~cpp
#ifdef WIDGET_H
#define WIDGET_H

#include <QWidget>	//包含Qt空窗口头文件

class Widget : public QWidget
{
    Q_OBJECT	//支持信号和槽
        
public:
    Widget(QWidget *parent = 0);	//构造函数
    ~Widget();	//析构函数
};

#endif
~~~

widget.cpp

~~~cpp
#include "Widget"

Widget::Widget(QWidget *parent)
    :QWidget(parent)
{
	//调接口环节     
	this->setWindowTitle("Hello qt"); //设置窗口标题
    this->resize(122,234); //设置窗口大小
    this->setFixedSize(229,222);//设置窗口固定大小，无法改变
}

Widget::~Widget()
{
    
}
~~~

#### 按钮

~~~c++
...
#include <QPushButton>

Widget::Widget(QWidget *parent)
	:QWidget(parent)
{
	...
	//创建按钮
	QPushButton *btn = new QPushButton;
	btn->resize(50, 100);	//大小
	btn->setParent(this);	//设置父窗口
    btn->setText("按钮");		//内容
    QFont font("华文行楷", 20, 10, 1);	//创建字体对象
    btn->setFont(font);	//设置字体
	btn.show();				//展示
    btn->move(100, 100);	//位置
    //风格，前端写，可以在另一个文件
    btn->setStyleSheet("QPushButton{background: red;}\
   	QPushButton:hover{background: green}\
   	QPushButton:pressed{background: rgba{255,255,255,1}}");
}
~~~

控制台输出内容

~~~c++
#include <QDebug>
...
qDebug() << "内容";
...
~~~

#### 对象树概念

1. QObject是以对象树的形式组织起来的，当创建qt对象的时候，需为其设置一个父对象，那么这个对象就会自动添加到其父对象的children()列表。当父列表析构时，这个列表的所有对象也都会被析构。
2. QWidget是能够在屏幕上显示的窗口父类。QWidget继承自QObject，因此也继承了对象树关系，添加QWidget为父亲的组件会自动添加到列表中自动析构。
3. 析构由内而外，先析构孩子（孩子没有孩子）再析构父亲；构造由外而内，先构造父亲再构造孩子。

### day 2

#### 信号与槽

观察者模式

使用connect函数可以为信号以及槽建立好链接，当信号广播触发时，槽做出相应的处理，信号(signal)来自于被观察者，槽(slot)来自于观察者

信号：函数声明，无需实现，系统大多数类都内置了信号，开发者也可以自定义信号

槽：函数，一般是类的成员函数，有声明且有实现，系统大多数类都内置了槽，开发者也可以自定义槽

例：最大化按钮被点击时，窗口最大化

* 观察者：窗口
* 被观察者：最大化按钮
* 信号：点击
* 槽：变最大化
* connect连接信号与槽->  connect(被观察者，信号，观察者，槽)；