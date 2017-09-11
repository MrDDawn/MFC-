全局变量和全局函数最好集中封装，不要在文档、视图等类内部定义，这样用起来才有全局的感觉。

例：

1、添加一个没有基类的新类，设类名起为CPublic，姑且称之为公用类

单击“Insert”菜单下的“New Class”命令，选择“Class type”为“Generic Class”，在“Name”栏中填入类名“CPublic”，单击“OK”，则新类建立完毕。

2、包含公用类的头文件，使各个类都能访问它

CPublic的头文件应包含在应用程序类的头文件中，这样在其它类中引用CPublic类时就不需要再包含了。

Test.h：（应用程序类头文件） (如我创建了 AOI的工程则放到 AOI.h)

#include "Public.h" //包含公用类头文件

class CTestApp : public CWinApp
{
…………
};

3、在公用类中定义全局变量和全局函数，均使用static修饰，静态变量还必须在类外定义和初始化

Public.h：（公用类头文件）

class CPublic
{
public:
CPublic();
virtual ~CPublic();

public:
static int x; //全局变量
static int time; //全局变量
static int f(int y); //全局函数
…………
}

在公用类中对静态变量进行初始化和定义函数体：

Public.cpp：（公用类程序文件）

int CPublic::x = 0; //初始化全局变量
int CPublic::time; //定义全局变量

CPublic::CPublic()
{

}

CPublic::~CPublic()
{

}

int CPublic::f(int y) //全局函数，这里不要再加static
{
y++;
return y;
}

4、全局量的使用

使用变量：CPublic::变量名

使用函数：CPublic::函数()

如在视图的某函数中访问变量x和函数f()：

void CTestView::xyz()
{
CPublic::x = 0; //访问变量x
CPublic::time = CPublic::f(1); //访问函数f()
…………
}

在其它类中访问x、time和f()的方法与此相同。

5、几点注意：

① 由于静态量可独立于类存在，不需要生成CPublic类的实例。

② 静态数据成员的定义和初始化必须在类外进行，如例中x的初始化；变量time虽然没有初始化，但也必须在类外进行定义。由于没有生成CPublic类的实例，所以它的构造函数和析构函数都不会被执行，在里面做什么工作都没有什么意义。

③ 如果静态函数需要访问CPublic类内的变量，这些变量也必须为静态的。因为非静态量在不生成实例时都不会存在。
