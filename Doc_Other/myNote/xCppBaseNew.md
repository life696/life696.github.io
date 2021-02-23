# xC++基础笔记

[TOC]

## C++的类

C++的类是对C中的结构体进行了功能扩展，功能更多、更强。

### 类和对象

类是抽象的，指一类事物。对象是实际存在的，具体的某个实体。可以使用一个类创建出多个对象，这些对象之间的独立存在的。

面向对象的三大特性：继承、封装、多态

- 继承：类Ａ可以继承类B，A叫做B的子类，B叫做A的父类/基类
- 封装：类中的某些属性不是对外公开的，private成员只能在类内使用，实例化出的对象不能访问，但是可以通过public函数访问private成员，如果不指定则默认是private
- 多态：

C++ 多态实现原理：

- 当类中声明虚函数时，编译器会在类中生成一个虚函数表
- 虚函数表是一个存储成员函数地址的数据结构
- 虚函数表是由编译器自动生成与维护的
- virtual成员函数会被编译器放入虚函数表中
- 存在虚函数时，每个对象都有一个指向虚函数表的指针



### 类的简单例子

一个类的简单例子如下：

```C++
#include<iostream>

using namespace std;

class Animal{
	
private:
	char *habbit;
		
public:
	char *name;
	int age;
	
	void printInfo()
	{
		cout<<this->age<<endl;
		cout<<this->name<<endl;
	}
};

int main()
{
	Animal Dog;
	
	Dog.age = 10;
	Dog.name = "dog";
	
	Dog.printInfo();
}
 
```

#### 类中的成员属性

类中的成员变量、成员函数有如下三种：p

- private：私有的，不能让对象直接访问，只能通过public的函数来访问，保护一些数据
- public：共有的，可以让对象直接访问
- protected：受保护的

类中的函数可以在类内实现，也可以在类外实现：

```C++
#include<iostream>

using namespace std;

class Animal{
	
private:
	int age;
		
public:

	// 在类内实现成员函数 
	void setAge(int age)
	{
		this->age = age;   // 这里的this是一个指针，这个实例化对象的指针
	}
	int getAge();	
};

// 在类外实现成员函数
int Animal::getAge()
{
	return this->age; 
}

int main()
{
	Animal Dog;
	
	//  age的属性是 private，不能由对象直接访问 
	//Dog.age = 10; 
	
	// 需要使用public的函数，来间接的访问private成员 
	Dog.setAge(4);
	int a = Dog.getAge();
	
	cout <<a<<endl;  // 打印 4
}
 
```

这样就保护了age变量，不能直接被实例化对象直接访问

### 构造函数

构造函数是类在创建一个对象时，最先调用的函数。完成某些初始化内容，函数名和名字相同，例子如下：

```C++
class Animal{

public:
	// 构造函数，可以重载，可以有参数 
	Animal(){ cout<<"Animal()"<<endl; }
	Animal(int a){ cout<<a<<endl; }
		
};

int main()
{
	Animal Dog;
	Animal Dog1(1);
}
```

构造函数特点：

- 没有返回值
- 可以有多个，即可以重载，调用时候根据参数选择对应的构造函数
- 不可为虚函数
- 不可能发生多态行为
- 调用无参构造函数时，直接类实例化对象，后面不能加（），否则和int fun();函数声明一样了。
- 构造函数的调用顺序：全局类，main函数中的类，然后遇到再调用

### 拷贝构造函数

一种特殊的构造函数，参数类型为const calss_name&的构造函数，拷贝构造函数默认的功能是：当一个实例对象当作另一个实例化对象参数时，前者的内容（值）会拷贝到后者中，这个“拷贝”，不是真正意义上的复制拷贝，而是让这两个对象中的相同变量，指向同一块内存。为了验证是否是同一块内存，打印这两个类的私有变量地址即可。当类中没有定义拷贝构造函数时，编译器默认提供一个拷贝构造函数，简单的进行成员变量的值复制。

深拷贝和浅拷贝（编译器提供的拷贝构造函数只进行浅拷贝）

- 浅拷贝：拷贝后对象的物理状态相同
- 深拷贝：拷贝后对象的逻辑状态相同

需要使用深拷贝的情况：对象中有成员只带了系统中的资源，成员指向了动态内存空间、成员打开了外存中的文件等等

自定义拷贝构造函数，必然要实现深拷贝

拷贝构造函数例子1

```C++
class Animal{

public:

	int age;
	int size;
};


int main()
{
	Animal Dog;
	
	Dog.age = 10;
	Dog.size = 1;
	
	// 类中没有实现拷贝构造函数，那么就会调用默认拷贝构造函数，只是进行简单的值复制
	Animal Cat(Dog); 
	
	cout<<&Cat.age<<endl;  //　0x22fe30 
	cout<<&Dog.age<<endl;  // 0x22fe40 

}
```

在这个例子中，没有发现什么问题，生成了两个对象，并且这两个对象中的成员，地址也不同。但是当类中有动态成员、静态成员时，就不行了。



拷贝构造函数例子2

```C++
class Animal{

private: 
	static int count;  // 静态成员。用于计数 

public:

	int age;
	int size;
	
	Animal(){
		count++;
	} 
    Animal(const Animal& a)
    {
        age = a.age;
        size = a.size;
        count++;
    }
    static int getCount()
    {
        return count;
    }
};
int Animal::count = 0;

int main()
{
	Animal Dog;
	
	Animal Cat(Dog); 
	
	cout <<Animal::getCount()<<endl;	// 2
}
```

这个例子中，计数器为1，实际应该是2，所以有必要自己实现拷贝构造函数的



拷贝构造函数例子3

```c++
using namespace std;

class Animal{
public:
	int *p;
	int age;
	int size;
	
	Animal()
	{
		p = new int(4);
	}
	~Animal()
	{
		if (NULL != p)
			delete p; 
	}
};

int main()
{
	Animal Dog;
	Animal Cat(Dog); 
	
}
```

这个程序也会报错，程序流程为，先创建Dog，并且在堆中，分配了一块空间，给p。然后使用Dog创建Cat，并且也在堆中找到一块空间给Cat中的p，但是由于是使用的默认拷贝构造函数，属于浅拷贝，只是值的拷贝，所以这两个p指向的是同一个位置，在程序运行完时，Dog释放p没问题，Cat释放p，就有问题了，一块空间释放了两次。

拷贝构造函数例子4

```C++

class Animal{
public:
	int *p;
	int age;
	int size;
	
	Animal()
	{
		p = new int(4);
	}
	// 自定义拷贝构造函数 
	Animal(const Animal& a)
	{
		age = a.age;   // 可以简单值复制的，就值复制 
		size = a.size;
		
		// 不能简单值复制的就自己写
		// 自己申请一块空间 
		p = new int(4);
		
		// 把被复制的类中的堆中的数据，写入到刚申请好的空间中 
		*p = *(a.p); 
		
	}	
	
	~Animal()
	{
		if (NULL != p)
			delete p; 
	}
};

int main()
{
	Animal Dog;
	Animal Cat(Dog); 
	
}
```

这会就没有问题了

## 待整理

### C++的引用和C的指针

### C++的重载

### 命名空间

```C++
namespace Name
{
    namespace Internal
    {   
    }
}
```

C++的命名空间

- 不同命名空间中的标识符可以同名而不会发生冲突
- 命名空间可以嵌套

命名空间的使用

- 使用整个命名空间：using namespace name;
- 使用命名空间中的变量：using name::variable;
- 使用默认命名空间中的变量： ::variable;

类把变量和函数封装起来作为一个整体，命名空间又把类封起来做一个整体，如果文件中没有声明使用这个命名空间那么对类的使用，必须用  空间名字::类名如果使用using，那么就可以直接用这个类，或者这个命名空间中的函数.为了不引起冲突，最好是所有的命名空间名字、类名字、函数名都不要重复 

例如在demo.h中有如下代码：

```C++
namespace Name1
{
	class Dog{
		int a ;
	}; 
}

namespace Name2
{
	class Dog{
		int a ;
	}; 
}
```

有两个同名的类，但是她们属于不同命名空间，这样就不会有问题了，只要保证编译器不会产生奇异，不知道该选择那个dog即可。

但是如下还是存在问题：

```C++
using namespace Name1;

using namespace Name2;


int main()
{
	Dog dog;   // 变异不知道是那个Dog
	Dog dog1;
}
 
```



### 内联函数



### 动态内存分配

new关键字进行动态内存分配，C++中的动态内存申请是基于类型的，分配之后使用delete关键字释放。有一个new，必须有一个delete

变量申请：

```C++
Type* pointer = new Type;
delete point

int* a = new a;
delete a;
```

数组申请：

```C++
Type* pointer = new Type[N];
delete point

int* a = new a[10];
delete[] a;
```

new出来的数据是在堆上的

C的malloc和C++的new的区别

- new是C++的关键，malloc是C库提供的函数
- new以具体类型为单位进行内存分配，malloc以字节为单位进行分配
- new在申请单个类型变量时可进行初始化，malloc不具备内存初始化的特性

```C++
int* pi = new int(1);
float* pf = new float(3.5f);
char* pc = new char('a');
```



#### 









































