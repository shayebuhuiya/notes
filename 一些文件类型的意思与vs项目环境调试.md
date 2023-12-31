### 文件路径

* Bin文件夹下存放的是dll文件;
* Lib文件夹下存放对应dll文件的动态lib文件（依赖dll文件）；
* staticlib文件夹下存放静态lib文件（与dll文件独立）；
* include文件夹下存放头文件

### 静态lib文件

静态lib文件是多个obj文件的集合（obj文件就是cpp文件编译之后产生的一种文件，一个cpp文件编译之后只会产生一个obj文件，而工程中多个cpp文件编译之后obj文件就可以链接生成lib库文件）

使用静态lib文件的缺点：静态lib文件实际上是包含了所有的声明和实现。如果将lib文件链接到工程后，该lib文件中的所有代码都会嵌入进来（代码冗余），所以需要用dll，需要时才将所需代码嵌入。

#### 调用静态lib文件

1. 添加工程的头文件目录：项目--属性--配置属性--C/C++---附加包含目录，此处加上头文件的存放路径（include文件夹）；
2. 添加工程的静态库文件目录：项目--属性--配置属性--链接器--常规--附加库目录，此处加上静态lib文件的存放路径（staticlib文件夹）；
3. 添加工程引用的lib文件名：项目--属性--配置属性--链接器--输入--附加依赖项，把用到的lib的名字都输入到这里

### 静态lib文件与dll文件

​	生成一个dll文件的时候，总伴随着生成一个动态lib文件，其大小小于静态lib，因为lib文件只包含函数索引信息，记录dll中函数的入口和位置，dll中存放具体的函数实现。dll其实就是exe文件，但由于没有main函数，故不能单独执行。有些应用程序被分割成一些单独的相对独立的动态链接库，只有在执行应用程序的时候，用到的dll才会被调用。故经常会出现无法加载“xxx.dll”

#### 调用动态lib文件与dll文件：

1. 添加工程的静态库文件目录：项目--属性--配置属性--链接器--常规--附加库目录，此处加上动态lib文件的存放路径（lib文件夹），告诉编译器动态lib库的位置；
2. 添加工程引用的lib文件名：项目--属性--配置属性--链接器--输入--附加依赖项，把用到的lib文件夹下所需要用到的xxx.lib的名字都输入到这里，告诉编译器所需要的动态lib库的名字；
3. 将所需的dll文件复制到exe文件所在文件夹；

### 项目属性页

#### VC++目录（全局项目的包含目录与库目录）

* 包含目录：寻找#include<xxx.h>中xxx.h的搜索目录
* 库目录：寻找.lib文件的搜索目录

#### C/C++（仅作用于当前项目）：

* 附加包含目录：寻找#include<xxx.h>中的xxx.h的搜索目录

 #### 链接器

* 附加库目录：寻找.lib文件的路径（文件夹）
* 附加依赖项：所需lib库的名字(xxx.lib)