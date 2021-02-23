# xC++基础

[TOC]



#### C到C++

C++继承了C的特性，在C的基础上提供了更多的语法和特性

#### 布尔类型

bool类型可取的值只有ture（非0）和false（0），只占用一个字节

#### 引用

引用可以看作是一个已定义的变量的别名，引用的语法如下

```C++
Type& name = var;
```

引用在定义时必须使用同类型的变量进行初始化

```C++
int a = 1;
int&b = a;  // b是a的别名
b = 5;      // 操作b就是操作a
```

引用作为变量名而存在，因此在一些场合可以代替指针，引用相对于指针来说具有更好的可读性和实用性

##### const引用

const引用会使变量拥有只读属性，

```C++
int a = 1；
const int& b = a;
int *p = (int *)&b;
b = 2;  // 报错，b只读
*p = 2; // 可以，会修改a的值
```

使用常量对const引用进行初始化时，C++编译器会为常量值分配空间，并将引用名作为这段空间的别名

```C++
const int& a = 1;
int *p = (int *)&a;
a = 2;  // 错，a是只读的 
*p = 3; // 对，修改了a的值
```

引用在C++内部是一个指针常量，C++编译器在编译过程中用指针常量作为引用的内部实现，因为引用所占用的空间大小与指针相同，引用只是一个别名，C++为了实用性隐藏了引用的存储空间的细节

小结：

- const引用可以使变量具有只读属性
- 引用作为变量名旨在代替指针，最终本质为指针

#### 内联函数

C++使用内联函数代替宏代码片段，内联函数关键字inline

```C++
inline int f(int a, int b)
{
    return (a + b);
}
```

inline关键字必须和函数定义结合在一起，否则编译器会忽略内联请求，C++代码在编译时，直接将内联函数体插入在函数调用的地方，所以内联函数没有普通函数的额外开销（保存上下文，入栈，出栈等等）

函数的内联请求可能被编译器拒绝，还有一些其他限制：

- 不能存在任何形式的循环语句
- 不能存在过多的条件判断语句
- 函数体不能过大

#### 函数的参数

##### 默认参数

C++可以在函数声明时候为参数提供一个默认值，当函数调用时，没有提供参数的值，则使用默认值

```C++
int f(int a = 0)
{
    return (a*a);
}

int main()
{
    cout<<f(3)<<endl; // 9
    cout<<f()<<endl;  // 0

    return 0;
}
```

##### 占位参数

只有参数的类型声明，没有参数名的声明，在函数体内部无法使用占位参数

```C++
int f(int a, int)
{
    return (a*a);
}

int main()
{
    cout<<f(2,5)<<endl;  // 不报错

    return 0;
}
```

#### 重载overload

同一个标识符在不同的上下文中有不同的意义，函数重载是同名的函数有不同的功能：

- 参数个数不同
- 参数类型不同
- 参数顺序不同

函数在调用时，编译器会准确的找到对应的函数，注意函数的返回值不能作为函数重载的依据，只能由函数名和参数列表决定

重载遇上函数指针

将重载函数名赋值给函数指针时，根据冲在规则挑选与函数指针曹拿书列表一致的候选者，严格匹配候选者的函数类型与函数指针的函数类型。不能直接通过函数名得到重载函数的的入口地址

##### 普通函数承载



##### 类中函数重载

类中的函数重载、普通成员函数重载、静态成员函数重载

函数名字相同，参数不一样，调用函数时，系统会根据参数的类型，调用相应的函数：

```c++
#include <iostream>

#include "animals.h"

using namespace std;

int add(int a, int b, int c)
{
    cout <<"int add(int a, int b, int c)"<<endl;
    return 0;
}
float add(float a, float b, float c, float d)
{
    cout <<"float add(float a, float b, float c, float d)"<<endl;
    return 0;
}

void add(int a, float b)
{
    cout <<"void add(int a, float b)"<<endl;
}

int main()
{
    add(12,3,9);
    add(2,6.3);
    add(1.1, 2.1, 3.3,3.3);

}
```

忽略警告

##### 前置后后置操作符的重载（++，--）

重载前置++不需要额外的参数

重载后置++需要一个int类型的占位符



##### 运算符重载

通过operator关键字可以定义特殊的函数，本质是通过运算符重载

下面这个示例实现了+运算符的重载，可以做两个二维坐标点的相加

```C++
class Point
{
private:
     int x;
     int y;

public:
    Point(){}
    Point(int x, int y)
    {
        this->x = x;
        this->y = y;
    }
    void print(){
        cout<<"("<<x<<","<<y<<")"<<endl;
    }
    friend Point operator+ (const Point& p1, const Point& p2);

};

Point operator+ (const Point& p1, const Point& p2)
{
    Point ret;

    ret.x = p1.x + p2.x;
    ret.y = p1.y + p2.y;

    return ret;
}

int main()
{
    Point p1(1,2);
    Point p2(3,4);
    Point p3(3,7);
    p3 = p1 + p2;

    p3.print();

    return 0;C
}
```

这个例子是使用了这个类的友元，不依赖友元的方式：将操作符重载函数定义为类的成员函数。

```C++
class Point
{
private:
     int x;
     int y;
public:
    Point(){}
    Point(int x, int y)
    {
        this->x = x;
        this->y = y;
    }
    void print(){
        cout<<"("<<x<<","<<y<<")"<<endl;
    }
    Point operator+ (const Point& p2);
};

Point Point::operator+(const Point& p2)
{
    Point ret;
    ret.x = this->x + p2.x;
    ret.y = this->y + p2.y;
    return ret;
}

int main()
{
    Point p1(1,2);
    Point p2(3,4);
    Point p3;
    p3 = p1 + p2;

    p3.print();

    return 0;
}
```



#### 强制类型转换

四种类型转换：static_cast、const_cast、dynamic_cast、reinterpret_cast

static_cast

- 用于基本类型转换，不能用于基本类型指针间的转换
- 用于有继承关系类对象之间的转换和类指针之间的转换

const_cast

- 用于去除变量的只读属性
- 强制转换的目标类型必须是指针或者引用

dynamic_cast

- 用于指针类型间的强制转换
- 用于整数和指针类型之间的强制转换

reinterpret_cast

- 用于有继承关系的类指针间的转换
- 用于有交叉关系的类指针间的转换
- 具有类型检查的功能，需要虚函数的支持

```C++
#include <stdio.h>

typedef void(PF)(int);

struct Point
{
    int x;
    int y;
};

int main()
{
    int v = 0x12345;
    PF* pf = (PF*)v;
    char c = char(v);
    Point* p = (Point*)v;
    
    pf(5);
    
    printf("p->x = %d\n", p->x);
    printf("p->y = %d\n", p->y);

    return 0;
}

```

2

```C++
#include <stdio.h>

void static_cast_demo()
{
    int i = 0x12345;
    char c = 'c';
    int* pi = &i;
    char* pc = &c;
    
    c = static_cast<char>(i);
    pc = static_cast<char*>(pi);
}

void const_cast_demo()
{
    const int& j = 1;
    int& k = const_cast<int&>(j);
    
    const int x = 2;
    int& y = const_cast<int&>(x);
    
    int z = const_cast<int>(x);
    
    k = 5;
    
    printf("k = %d\n", k);
    printf("j = %d\n", j);
    
    y = 8;
    
    printf("x = %d\n", x);
    printf("y = %d\n", y);
    printf("&x = %p\n", &x);
    printf("&y = %p\n", &y);
}

void reinterpret_cast_demo()
{
    int i = 0;
    char c = 'c';
    int* pi = &i;
    char* pc = &c;
    
    pc = reinterpret_cast<char*>(pi);
    pi = reinterpret_cast<int*>(pc);
    pi = reinterpret_cast<int*>(i);
    c = reinterpret_cast<char>(i); 
}

void dynamic_cast_demo()
{
    int i = 0;
    int* pi = &i;
    char* pc = dynamic_cast<char*>(pi);
}

int main()
{
    static_cast_demo();
    const_cast_demo();
    reinterpret_cast_demo();
    dynamic_cast_demo();
    
    return 0;
}
```





#### 面向对象（类class）



##### 继承

继承中，子类是一种特殊的父类，子类拥有父类所有属性和行为，子类可以添加父类没有的方法和属性

继承权限：

| 基类权限  | public继承 | private继承 | protected继承 |
| :-------: | :--------: | :---------: | :-----------: |
|  public   |   public   |   private   |   protected   |
|  private  |  private   |   private   |    private    |
| protected | protected  |   private   |   protected   |

关于protected：

- 修饰的成员不能被外界访问
- 使得子类能够访问父类的成员
- 关键字protected专门为了继承而设计

父子之间的冲突：

- 子类可以定义父类中的同名成员
- 子类中的成员将隐藏父类中的同名成员
- 父类中的同名成员依然存在于子类中
- 通过作用域分辨符（::）访问父类中的同名成员

```
Child c;
c.mi = 100;
c.Parent::mi = 200；
```

继承示例：

```C++
class Father{
private:
    int age;   // 私有成员，只供类内成员使用，
public:
    int a;
    void setAge(int age);
    int getAge(){ return age; }
    void printInfo(void);
};
void Father::setAge(int age) // 但是可以通过public函数访问private成员
{
    this->age = age;       // 后面的age是参数，前者是成员
}
void Father::printInfo()
{
    cout <<"a = "<<a<<endl;
}

class Son : public Father{  //　继承时，指明继承权限

};

int main()
{
    Son son;
    son.a = 20;
    son.setAge(50);
    cout<<son.getAge()<<endl; 
    son.age = 20;   // 报错，对象不可访问私有成员
    son.printInfo();
    return 0;
}
```

```C++
class Parent 
{
public:
    Parent()
    {
        cout << "Parent()" << endl;
    }
    Parent(string s)
    {
        cout << "Parent(string s) : " << s << endl;
    }
};

class Child : public Parent
{
public:
    Child()
    {
        cout << "Child()" << endl;
    }
    Child(string s) : Parent(s)
    {
        cout << "Child(string s) : " << s << endl;
    }
};

int main()
{       
    Child c; 
    Child cc("cc");
    
    return 0;
}
```

###### 多继承

父类有多个时，称为多继承，多继承数据冗余。使用虚继承可以解决数据冗余的问题，





##### 多态

子类可以重写父类的函数，通过作用域分辨符（::）可以访问到父类的中的函数

```C++
Child c;
Parent* p;
c.Parent::print(); // 父类中的
c.print()；   // 在子类中重写
p->print（）;   // 父类中定义
```

根据实际的对象类型判断如何调用重写函数，父类指针（引用）指向：

- 父类对象则调用父类中的定义函数
- 子类对象则调用子类中定义的重写函数

多态的概念：根据实际的对象类型决定函数调用的具体目标，同样的调用语句在实际运行时候有多种不同的表现形态。

- 通过关键字virtual对多态进行支持
- 被virtual声明的函数被重写后具有多态特性
- 被virtual声明的函数叫做虚函数

注意：函数重写只发生在子类和父类之间，重写不是重载



##### 对象的初始化（构造函数）

类创建一个对象，可以叫做实例化。对象中的成员变量的初始值：

- 在栈上创建的对象，成员变量初始值随机值
- 在堆上创建的对象，成员变量初始值随机值
- 在静态存储区创建的对象，成员变量初始值为0





##### 转换构造数

参数满足下列条件时称为转换构造函数

- 有且仅有一个参数
- 参数是基本类型
- 参数是其他类类型

转换构造函数被explicit修饰时只能进行显示转换

```C++
static_cast<ClassName>(value);
ClassName(value);
```





##### 拷贝构造函数



需要使用深拷贝的情况：

- 对象中有成员只带了系统中的资源：成员指向了动态内存空间、成员打开了外存中的文件等等

自定义拷贝构造函数，必然要实现深拷贝

```C++
class Father{

public:
    int age;
    int *pi;

    Father(){cout<<"Father()"<<endl;}   // 无参构造函数

    Father(const Father& f)  // 拷贝构造函数
    {
        age = f.age;

        pi = new int;
        *pi = *f.pi;
    }

    void printInfo(void){ cout<<"age = "<<age<<endl; }
};

int main()
{
    Father f1;
    f1.age = 5;

    Father f2(f1); // 调用拷贝构造函数

    cout<<"f1 address = "<<&f1<<endl;
    cout<<"f2 address = "<<&f2<<endl;

    cout<<"f1.age = "<<&f1.age<<endl;
    cout<<"f2.age = "<<&f2.age<<endl;

    cout<<"&f1.pi = "<<&f1.pi<<endl;
    cout<<"&f2.pi = "<<&f2.pi<<endl;

    cout<<"f1.pi = "<<f1.pi<<endl;
    cout<<"f2.pi = "<<f2.pi<<endl;

    return 0;
}
```

为了验证f1h和f2的关系，做了很多打印

>  Father()
> f1 address = 0x28ff08
> f2 address = 0x28ff00
> f1.age = 0x28ff08
> f2.age = 0x28ff00
> &f1.pi = 0x28ff0c
> &f2.pi = 0x28ff04
> f1.pi = 0x42723e
> f2.pi = 0x37ba60

可以看到这完全是两个类，类的地址，类中的变量的地址，类中指针指向的空间都不懂



##### 初始化列表（Qt有这种）

当类A中有其他类B的对象时，A的构造函数可以同时调用B的构造函数

```C++
class Father{

public:
    int age;

    Father(){cout<<"Father()"<<endl;}
    Father(int age){
        this->age = age;
        cout<<"Father(int age)"<<endl;}

    void printInfo(void){ cout<<"age = "<<age<<endl; }
};
class Son{

public:
    Father f1;
    Father f2;
    Father f3;

    Son(): f1(1),f2(2),f3(3)
    {
        cout<<"Son(): f1(1),f2(2),f3(3)"<<endl;
    }
    void prin()
    {
        cout<<f1.age<<f2.age<<f3.age<<endl;
    }
};

int main()
{
    Son s1;
    s1.prin();
    return 0;
}

```

> Father(int age)
> Father(int age)
> Father(int age)
> Son(): f1(1),f2(2),f3(3)
> 123

可以看出是先执行成员对象的构造函数，再执行自己的构造函数，自己的构造函数可以是初始化成员变量的成员变量



##### 对象构造的顺序

- 局部对象：程序运行到局部对象位置时，进行构造
- 堆对象：程序执行到new语句时，创建对象，使用new创建的对象自动触发构造函数的调用
- 全局对象：对象的构造顺序是不确定的，和编译器有关



单个对象创建时，构造函数的调用顺序：

1. 用父类的构造过程
2. 调用成员变量的构造过程
3. 调用类自己的构造函数

即，保证这个类使用到的资料先构造（析构函数的调用顺序和构造函数相反）



##### 析构函数（销毁对象）

需要销毁的对象都应该做清理

- 为每个类提供一个public的free函数
- 对象不在进行需要时立即调用free函数清理
- 可以成为虚函数
- 不可能发生多态行为

但是free函数是普通函数，必须显示调用，对象销毁之前没有清理的话，可能会内存泄漏

析构函数：与构造函数功能相反，清理相关资源，没有参数和返回值，在对象销毁之前，自动调用

```c++
class Father{

public:
    int age;

    Father(){cout<<"Father()"<<endl;}
    Father(int age){
        this->age = age;
        cout<<"Father(int age)"<<endl;}
    ~Father()
    {
        cout<<"~Father()";
    }
};

int main()
{
    Father s1;
    return 0;
}
```

这个例子知识简单的示意，析构函数的调用顺序和构造函数相反：先自购自己，再析构成员变量，最后析构父类



##### 静态成员

静态成员变量属于整个类所有，不依赖与任何对象，可以通过类名、对象名直接方问共有静态成员变量

静态成员的特性：

- 使用static关键字定义
- 需要在类外单独分配空间
- 位于全局数据区
- 生命周期为程序运行周期

静态成员函数：属于整个类所有，通过类名，对象名调用共有静态函数。类名使用双冒号访问，对象使用.访问

![](img/class_static_fun.png)



```C++
class Father{

public:
    static int cnt;
    static void staticFun();
    Father()
    {
       cnt++;
    }
    ~Father()
    {
       cnt--;
    }

};
int Father::cnt = 0;
void Father::staticFun()
{
    cout<<"static void staticFun()"<<endl;
}
int main()
{
    cout<<Father::cnt<<endl;
    Father::staticFun();
    Father s1;
    s1.staticFun();
    cout<<s1.cnt<<endl;

    return 0;
}
```

##### C++类的二阶构造

学习Qt时，看到一段不太理解的代码，类写的很复杂，问了别人才知道这个是C++类的二阶构造，我不知道为什么把类写的那么复杂，但是既然这么写肯定是有好处，于是就百度找资料学习了一下。

###### 以前写的类

以前的写过的类代码如下：

```c++
#include <iostream>

using namespace std;

class MyClass{

private:
    int age;   // private

public:
    MyClass();   // 无参构造函数
    MyClass(int age);// 有参构造函数
    ~MyClass();    // 析构函数
    void setAge(int age);    // 设置私有变量函数
    int getAge(void);   // 获取私有变量函数
    void printInfo(void); // 打印对象信息函数
};
void MyClass::setAge(int age)
{
    this->age = age;    // 后边的age是实参age，C++就近原则
}

int MyClass::getAge(void)
{
    return age;
}

void MyClass::printInfo(void)
{
    cout <<"age = "<<age<<endl;
}
MyClass::MyClass(int age)
{
    this->age = age;
}
MyClass::MyClass()
{
}
MyClass::~MyClass()
{
}

int main()
{
    MyClass c1;
    MyClass c2(40);
    c1.setAge(30);

    c1.printInfo();
    c2.printInfo();

    return 0;
}
```

这是之前写过得函数，使用起来没有问题，对象c1和c2是在栈上创建的。但是要明构造函数的功能：

- 构造函数用于对象的初始化 

- 构造函数与类同名并且没有返回值
- 构造函数在对象定义时自动被调用
- **只提供自动初始化成员变量的机会**
- **不能保证初始化逻辑一定成功**

所以可以知道：构造函数能决定的只是对象的初始状态，不能决定是对象的诞生。如果在构造函数中产生了问题，会导致以后不能正常使用这个对象，这样的对象角**半成品对象**。半成品对象：

- 初始化操作不能按照预期完成而得到的对象
- 半成品对象是合法的C++对象，但是他也是Bug的重要来源



###### 二阶构造

工程开发中的构造过程可以分为**资源无关**的初始化过程和**需要使用系统资源**的过程。与资源无关就不会出现异常，与资源有关就可能出现异常（例如内存申请，文件访问）。

**二阶构造**的流程：

1. 第一阶段：资源无关的初始化操作
2. 第二阶段：系统资源申请操作

示例代码

```c++
#include <iostream>

using namespace std;

class MyClass{
private:
    int age;   // private

    MyClass(){}       // 第一阶段构造
    bool construct(); // 第二阶段构造

public:
    int getAge(void){return age;}
    static MyClass* NewInstance(); // 对象创建函数
};
MyClass* MyClass::NewInstance()
{
    MyClass* ret = new MyClass(); //示例化一个对象，调用无参构造函数（第9行），实现第一阶段的构造

    // 若第二阶段构造失败，返回 NULL
    if( !(ret && ret->construct()) ) // 第二阶段的构造
    {
        delete ret;
        ret = NULL;
    }

    return ret; // 返回这个对象的指针（地址）
}
bool MyClass::construct()
{
    age = 10;
    return true;  // 假设申请资源没有问题
}
int main()
{
    MyClass* c1 = MyClass::NewInstance();
    cout<<"sge = "<<c1->getAge()<<endl;
    return 0;
}
```

这是要给二阶的构造的例子，第一阶段和第二阶段的构造函数是private，外部无法调用（实例化的对象不能调用，但是类内的函数可以），但是定义了一个public的NewInstance函数

```c++
static MyClass* NewInstance(); // 对象创建函数
```

NewInstance的返回值是一个这个类类型的指针，并且是static的，类没有实例化处对象之前使用类名就可以调用（参见类的静态成员相关知识），于是就有了：

```c++
MyClass* c1 = MyClass::NewInstance();
```

注意这里就不能像上面那样实例化对象了：

```c++
MyClass c1;  // 是错的
```



这里要注意的是：使用了这种二阶构造模式后，对象只能在堆空间上构造，而不能在栈空间上构造。

对象只能在堆空间上进行构造而不能在栈空间上构造，肯定是好的，因为工程上的对象往往是巨大的，一般都会放到堆空间上进行构造。

总结：

- **构造函数只能决定对象的初始化状态**
- **构造函数中初始化操作的失败不影响对象的诞生**
- **初始化不完全的半成品对象是Bug的主要来源**
- **二阶构造人为的将初始化过程分成两部分**
- **二阶构造能够确保创建的对象都是完整初始化的**



##### 友元

友元关系，类外函数可以使用类内函数的的私有变量，使用friend声明

先看一个例子：

```C++
#include <iostream>

#include "animals.h"
#include <string.h>
using namespace std;

class Point{

private:
    int x;
    int y;

public:
    Point() {}   //  如果不加则报错
    Point(int x, int y)
    {
        this->x = x;
        this->y = y;
    }
    void printInfo();
    friend Point add(Point &p1, Point &p2);

};
Point add(Point &p1, Point &p2)
{
    Point n;
    n.x = p1.x + p2.x;
    n.y = p1.y + p2.y;
    return n;

}

void Point::printInfo()
{
    cout<<"x = "<<x<<", y = "<<y<<endl;
}
int main()
{
    Point p1(1,2);
    Point p2(5,6);

    p1.printInfo();
    p2.printInfo();

    Point c;
    c = add(p1, p2);
    c.printInfo();

    return 0;
}
```

本例中，add函数Point类型的n，直接访问私有变量x、y，这应该是不可以的
私有变量实不能直接访问的，按照以前的内容，应该是调用一个公有函数
setX和setY来给Point的私有变量赋值。

但是在友元中，friend Point add(Point &p1, Point &p2);
这个函数是Point类的友元，他可以直接访问私有变量，不用通过公有函数了，
程序中有一句： Point() {}   //  如果不加则报错

在类实例化对象时，如果没有参数则会调用这个无参数的构造函数，
不写的话，就找不到了。那么之前也没写为什么没错误呢，因为还有一个：

Point(int x, int y)函数，定义了自己的构造函数了，就不会默认使用c++
的构造函数了，然后Point c;就会调用类中自己实现的无参构造函数，
但是又没有，所以报错。当然，这也有办法解决，比如所有的Ponit实例化时，
都有参数，那么就不会调用没有的那个无参构造函数了

总结：

1. 写类时，如果自己写了一个有参数的构造函数，，那么
   一定要再写一个无参的构造函数。
2. 类的友元可以直接访问类的私有变量

友元类：

```c++
#include <stdio.h>

class ClassC
{
    const char* n;
public:
    ClassC(const char* n)
    {
        this->n = n;
    }
    
    friend class ClassB;
};

class ClassB
{
    const char* n;
public:
    ClassB(const char* n)
    {
        this->n = n;
    }
    
    void getClassCName(ClassC& c)
    {
        printf("c.n = %s\n", c.n);
    }
    
    friend class ClassA;
};

class ClassA
{
    const char* n;
public:
    ClassA(const char* n)
    {
        this->n = n;
    }
    
    void getClassBName(ClassB& b)
    {
        printf("b.n = %s\n", b.n);
    }
    /*
    void getClassCName(ClassC& c)
    {
        printf("c.n = %s\n", c.n);
    }
    */
};

int main()
{
    ClassA A("A");
    ClassB B("B");
    ClassC C("C");
    
    A.getClassBName(B);
    B.getClassCName(C);
    
    return 0;
}
```



#### C++标准库

关于C++标准库：

- 标准库不是C++语言的一部分
- 标准库是由类库和函数库组成的集合
- 标准库中定义的类和对象位于std命名空间
- 标准库的头文件都不带.h后缀
- 标准库涵盖了C库的功能



#### C++中的字符串

C语言是不支持字符串的，，而是使用字符数组和一组函数实现字符串操作。C++中也没有用原生的字符串类型。但C++标准库提供了string类型：

- string直接支持字符串大小比较
- string直接支持字符串的链接
- string直接支持子串查找和提取
- string直接支持字符串的插入和替换

```C++
int main()
{
    string str = "qwer";
    cout <<"str len = "<<str.length()<<endl; // 4

    for ( int i = 0; i < str.length(); i++ )
    {
        cout <<str[i]<<endl;
    }
    return 0;
}
```

##### 字符串和数字的转换

字符串流类sstream用于string的转换...



#### 函数对象

函数对象：

- 使用具体的类对象取代函数
- 该类的对象具备函数调用的行为
- 构造函数指定具体数列项的起始位置

```C++
#include <iostream>
#include <string>

using namespace std;

class Fib
{
    int a0;
    int a1;
public:
    Fib()
    {
        a0 = 0;
        a1 = 1;
    }
    
    Fib(int n)
    {
        a0 = 0;
        a1 = 1;
        
        for(int i=2; i<=n; i++)
        {
            int t = a1;
            
            a1 = a0 + a1;
            a0 = t;
        }
    }
    
    int operator () ()
    {
        int ret = a1;
    
        a1 = a0 + a1;
        a0 = ret;
        
        return ret;
    }
};

int main()
{
    Fib fib;
    
    for(int i=0; i<10; i++)
    {
        cout << fib() << endl;
    }
    
    cout << endl;
    
    for(int i=0; i<5; i++)
    {
        cout << fib() << endl;
    }
    
    cout << endl;
    
    Fib fib2(10);
    
    for(int i=0; i<5; i++)
    {
        cout << fib2() << endl;
    }
    
    return 0;
}
```



#### 智能指针

智能指针的使用规定：只能用来指向堆空间中的对象或者变量

```C++
class Test
{
private:
    int i;
public:
    Test(int i)
    {
        cout << "Test(int i)" << endl;
        this->i = i;
    }
    int value(){ return i; }
    ~Test()
    { cout << "~Test()" << endl; }
};

class Pointer
{
private:
    Test* mp;
public:
    Pointer(Test* p = NULL)
    {
        mp = p;
    }
    Pointer(const Pointer& obj)
    {
        mp = obj.mp;
        const_cast<Pointer&>(obj).mp = NULL;
    }
    Pointer& operator = (const Pointer& obj)
    {
        if( this != &obj )
        {
            delete mp;
            mp = obj.mp;
            const_cast<Pointer&>(obj).mp = NULL;
        }
        return *this;
    }
    Test* operator -> ()
    {
        return mp;
    }
    Test& operator * ()
    {
        return *mp;
    }
    bool isNull()
    {
        return (mp == NULL);
    }
    ~Pointer()
    {
        delete mp;
    }
};

int main()
{
    Pointer p1 = new Test(0);
    cout << p1->value() << endl;
    Pointer p2 = p1;
    cout << p1.isNull() << endl;
    cout << p2->value() << endl;
    return 0;
}
```



#### 抽象类和接口

C++中没有抽象类的概念，通过纯虚函数实现抽象类

C++类中存在纯虚函数就成为了抽象类，纯虚函数：只定义原型的成员函数

```C++
class A
{
public：
	virtual int f() = 0;  // 纯虚函数
}
```

virtual 函数名 = 0;   告诉编译器这个纯虚函数，不需要定义函数体

抽象类和纯虚函数：

- 抽象类只能用作父类被继承，抽象类不能实例化对象
- 子类必须实现纯虚函数的具体功能
- 纯虚函数被实现后成为虚函数
- 如果子类没有实现纯虚函数，则子类称为抽象类

接口，满足一下条件的类称为接口：

- 类中没有定义的任何成员变量
- 所有的成员函数都是共有的
- 所有的成员函数都是纯虚函数
- 接口是一种特殊的抽象类

```C++
class Shape
{
public:
    virtual double area() = 0;
};

class Rect : public Shape
{
    int ma;
    int mb;
public:
    Rect(int a, int b)
    {
        ma = a;
        mb = b;
    }
    double area()
    {
        return ma * mb;
    }
};

class Circle : public Shape
{
    int mr;
public:
    Circle(int r)
    {
        mr = r;
    }
    double area()
    {
        return 3.14 * mr * mr;
    }
};

void area(Shape* p)
{
    double r = p->area();
    
    cout << "r = " << r << endl;
}

int main()
{
    Rect rect(1, 2);
    Circle circle(10);
    
    area(&rect);
    area(&circle);
    
    return 0;
}
```



#### 函数模版

##### 泛型编程

不考虑具体数据类型的编码方式

函数模版是C++中的泛型，是一种特殊的函数可用不同类型调用，

```
template<typename T>
void swap(T& a, T& a)
{
    T t = a;
    a = b;
    b = t;
}
```

template关键字用于声明开始进行泛型编程，typename关键字用于泛指类型

```C++
#define SWAP(t, a, b)    \
do                       \
{                        \
    t c = a;             \
    a = b;               \
    b = c;               \
}while(0)


void Swap(int& a, int& b)
{
    int c = a;
    a = b;
    b = c;
}

void Swap(double& a, double& b)
{
    double c = a;
    a = b;
    b = c;
}

void Swap(string& a, string& b)
{
    string c = a;
    a = b;
    b = c;
}

int main()
{
    int a = 0;
    int b = 1;
    
    Swap(a, b);
    
    cout << "a = " << a << endl;
    cout << "b = " << b << endl;
    
    double m = 2;
    double n = 3;
    
    Swap(m, n);
    
    cout << "m = " << m << endl;
    cout << "n = " << n << endl;
    
    string d = "Delphi";
    string t = "Tang";
    
    Swap(d, t);
    
    cout << "d = " << d << endl;
    cout << "t = " << t << endl;
    
    return 0;
}
```

```C++
template < typename T >
void Swap(T& a, T& b)
{
    T c = a;
    a = b;
    b = c;
}

template < typename T >
void Sort(T a[], int len)
{
    for(int i=0; i<len; i++)
    {
        for(int j=i; j<len; j++)
        {
            if( a[i] > a[j] )
            {
                Swap(a[i], a[j]);
            }
        }
    }
}

template < typename T >
void Println(T a[], int len)
{
    for(int i=0; i<len; i++)
    {
        cout << a[i] << ", ";
    }
    
    cout << endl;
}

int main()
{
    int a[5] = {5, 3, 2, 4, 1};
    
    Println(a, 5);
    Sort(a, 5);
    Println(a, 5);
    
    string s[5] = {"Java", "C++", "Pascal", "Ruby", "Basic"};
    
    Println(s, 5);
    Sort(s, 5);
    Println(s, 5);
    
    return 0;
}
```



函数模版，编译器从函数模板通过具体类型产生不同的函数，编译器会对函数模板进行两次编译（代码本身和参数替换后的代码进行编译）



#### 类模板

#### 数组类模板

#### 智能指针类模板

#### 单例

#### 异常













#### 底部

<a href="#C到C++">到顶部</a>