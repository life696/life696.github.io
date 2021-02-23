## Java基础

[TOC]

参考资料：《Java开发实战经典》李兴华、网文

### Java基础知识

java源文件后缀名字是 `.java`，编译后生成可执行文件`.class`。java中的所有程序都是在JVM（Java Virtual Machine）上运行的，与操作系统和平台无关。



#### Java程序分类

Java程序分为Application程序和Applet程序，其中有main方法的都是Application程序。本篇都是Application程序



#### 第一个java程序分析

源文件`Hello.java`，代码如下：

```java
/*     第一个java程序
 *
 */
public class Hello{    // 声明一个类
    public static void main(String args[]){
        //输出 Hello Java！
        System.out.println("Hello Java！");
    }
}
```

执行结果如下：

> Hello Java！

程序分析：

- 在所有的java Application程序zhong ，所有的的程序都是从`public static void main(String args[])`函数开始执行的
- `public class`是java中的关键字，表示定义的是一个类，java中的所有操作都是由类构成的
- `Hello`是程序中类的名称，因为主方法main，写在此类中，所以该类可以成为主类

##### public class和calss声明的类的区别

public class和calss都可以声明类，区别如下：

- public class：声明一个类时，类名必须与文件名保持一直，否则无法编译。只能有一个
- class：声明一个类时，类名可以与文件名不一致，但是在执行时一定要执行生成后的`.class`文件。可以有多个

##### Java中的标识符

包、类、方法、参数和变量的名字可以使用字母、数字、下划线、$符号组成，不能以数字开头

##### Java中的关键字

关键字也叫保留字，有特殊的功能，如下表

![](java_img/key_words.png)

一些说名：

- goto、const在Java中没有定义，但也是关键字，不能用来做自定义的标识符
- true、false、null不是关键字，但是却作为一个单独标识符类型，不能直接使用
- assert、enum是java中新增的内容，

##### 转义字符

![](java_img/exchange_char.png)





#### 数据类型

boolean布尔类型，只有真true和假false两种

其余和C++的变量基本相似。

数据溢出：int等数据类型是由范围的，使用时不能超出范围。

java中小数默认是double类型的

##### 数据默认值

声明时，如果没有赋值，则会赋一个默认值

![](java_img/default_value.png)

##### 类型装换

类型转换时，必须要满足一下两点，才能成功：

1. 转换前的数据和转换后的数据类型兼容
2. 转化后的数据类型的表示范围比转换钱的大（小转大）

注意：任何数据类型碰到String类型数据都向String类型转换

转换时有两种方式：

- 自动转换：不同类型的数据赋值，运算时发生

- 强制转换：

  ```java
  double d = 2.2;
  int int= (int)d；  // 强制类型转换
  ```

##### 运算符

和C++基本相似



#### 程序控制语句

##### if...else、switch、while、do...while、for、break、continue

和C++基本相同，switch还支持枚举类型

##### 

### 数组

#### 声明一维数组与分配内存

声明一维数组并且分配内存空间：

- 先声明，再分配
- 声明时直接分配

如下代码：

```java

public class Hello{
    public static void main(String args[]){
        System.out.println("Hello Java！");

        // 声明一维数组，没有区别。
        // 这个变量中没有内容，编译器会在
        int a1[] = null;
        int [] a2 = null;
        // 分配内存
        a1 = new int[3];
        a2 = new int[3];

		// 声明时，直接分配
        int a3[] = new int[3];
    }
}

```

数据属于引用型数据。java的数组和C一样从0开始编号

引用型数据：引用型数据默认值是null，表示暂时还没有任何指向的内存空间，变量名保存的不是数据实体，而是数据在堆内存的参考地址。

声明数组变量时，变量中没有内容，编译器会在栈内存中分配一块内存给他，他来保存指向数组实体的地址的名称。

为数组分配内存时，是在堆上分配的，栈上的变量名指向堆中给的数组实体，如下图

![](java_img/array_stack_heap.png)

数组操作中，在栈中保存的永远都是数组的名称，知识开辟了栈内存空间的数组是永远无法使用的，必须有指向的堆内存才可以使用，开辟堆内存必须使用new关键字，然后只是将这个堆内存的使用权交给了对应的栈内存空间，而堆内存空间可以同时被多个栈内存空间指向。

#### 数组的相关函数

- 数组名.length”：数组的长度
- 

#### 数组的静态初始化

静态初始化是指在数组声明时，就指定其内容，使用大括号完成，关键代码如下：

```java
int a4[] = {2,3,4};

for ( int i = 0; i < a4.length; i++ )
{
	System.out.println("a4[" + i + "] : " + a4[i]);
}
```



#### 二维数组

java的二维数组和C++的二维数组类似

```java
int aa1[][];         // 声明
aa1 = new int[3][3]; // 分配堆内存

int aa2[][] = new int[3][3]; // 声明时，分配内存
```

#### 多维数组

类似C++



### 方法

#### 方法的定义

java中的方法就是C中的函数，定一个方法，功能是计算两个int数据的和：

```java
public class Hello{

	// 定义一个方法
    public static int add(int a, int b){
        int ret = 0;
        ret = a + b;
        return ret;
    }


    public static void main(String args[]){
    
        int a = 0;
        a = add(2,5);
        System.out.println(a);
    }
}
```

执行结果如下：

> 7

程序分析：这个方法是属于这个类的，public类型，main方法可以直接调用

#### 方法命名

在定义类时，要求所有单词首字母大写（GoodGoodStudy）。定义方法时，第一个单词的首字母小写，其余单词首字母大写（goodGoodStudy）

#### 方法重载

通过传入的参数个数和类型不同完成不同功能函数的功能，只有返回值不同不算重载

#### 数组的引用传递

数组作为方法的参数，在方法中修改了数组，那么将保留修改的内容，示例如下：

```java

public class Hello {

    // 定义一个方法
    public static void add(int a[]) {
        for (int i = 0; i < a.length; i++) {
            a[i] += 1;
        }
    }
    
    public static void main(String args[]) {
        int a[] = {1, 2, 3};
        add(a);

        for (int i = 0; i < a.length; i++) {
            System.out.println(a[i]);
        }
    }
}
```

执行结果：

> 2
>
> 3
>
> 4

可见，原数组中的内容被修改了

#### 方法的可变参数

向方法中传递可变参数后，其中的参数以数组的形式保存下来。示例如下：

```java
public class Hello {

    // 可变参数，可以接收任意多个可变参数
    public static void fun(int... arg) {
        for (int i = 0; i < arg.length; i++) {
            System.out.print(arg[i] + ",");
        }

    }

    public static void main(String args[]) {
        fun();  // 无参数
        System.out.print("\n");
        fun(1);  // 一个参数
        System.out.print("\n");
        fun(1,2,3); // 三个参数
        System.out.print("\n");
    }
}
```

执行结果：

> 1,
> 1,2,3,

利用可变参数，写一个返回任意个数据的平均值，如下：

```java
public class Hello {

    // 可变参数，可以接收任意多个可变参数
    public static double fun(double... arg) {
        float sum = 0;
        int len = arg.length;
        for (int i = 0; i < len; i++) {
           sum += arg[i];
        }

        return (sum/len);
    }

    public static void main(String args[]) {

        System.out.println(fun(1, 2));
        System.out.println(fun(1.5,2.5));
    }
}
```

java中小数默认是double类型的，如果形参是float类型的话，程序报错



#### foreach输出

foreach语法格式如下：

```java
for ( 数据类型 变量名 : 数组名称 )
```

```java
int a[] = {1,2,3,4,5,6};
for ( int x : a )    // x遍历了数组a的元素
    System.out.print(x + ",");
```

执行结果：

> 1,2,3,4,5,6,



### Java面向对象（入门）

#### 面向对象的基本概念

面向对象的特性：封装性、继承性、多态性：

- 封装性：把对象的属性和行为看成一个整体，隐藏一些属性不让外界访问
- 继承性：一个类可以继承其他类的功能，在其它类的基础上，在开发自己特有的功能
- 多态性：多态允许程序出现重名。Java中含有方法重载与对象多态两种形式的多态
  - 方法重载：重载
  - 对象多态：子类对象可以和父类对象进行相互转换，而且根据其使用的子类不同，完成的功能也不同

类和数组一样都是引用类型，再栈空间中保存一个引用，他指向在堆空间中对象的实体

#### 类和对象

##### 创建对象

类创建（实例化）对象，可以先声明再分配空间，也可以声明时直接分配空间

```java
class Student{
    String name;
    int age;

    public void fun(){
        System.out.println("this is a Student Object!");
    }
}


public class Hello {

    public static void main(String args[]) {

        Student stu1 = null; // 声明一个该类型对象
        stu1 = new Student(); //分配空间

        Student stu2 = new Student(); // 声明时，直接分配空间
        stu2.age = 20;
        
    }
}
```

所有对象的名称都在栈中保存，而对象的具体内容都是在堆空间中保存，必须使用new关键字才能开辟堆空间，所以对象刚被示例化完，对象中的属性内容都是默认值，字符串默认是null，整数默认值是0

访问成员：对象名.成员，和C++ 差不多



Java本身提供垃圾收集机制（Garbage Collection，GC），会不定期的释放不用的空间，只要对象不使用了，就会等待GC

#### 封装性

封装就是把相关属性和行为放到一起，使有一些成员不能被外部访问。java的类中的成员和C++类似，也有private和public

- private的成员，只能被类内的成员访问，不能被对象直接访问
- public的成员，可以被对象直接访问
- default：默认

关于private的补充：

- 类中的属性必须要封装，然后对象通过类中的相关public函数，来说设置或者访问private成员
- 类中的函数方法相互调用，直接调用即可。如果非要强调是本类中的方法时，也可以使用`this.方法名()`，来调用

#### 构造方法

在面向对象程序中，构造方法的主要作用是为类中的属性初始化，Java类中的构造方法和C++类中的构造函数类似。

- 构造方法的名字必须和类名一直
- 构造方法的声明处不能有任何返回值类型的的声明
- 不能在构造方法中使用return

只要是类就必须有一个构造方法，但是在java中如果一个类没有明确的声明的一个构造方法，在编译时会直接生成一个无参数的、生么都不做的构造方法，自动加入如下：

```java
class Student{

    public  Student(){}
}
```

如果类中实现了一个构造方法，那么就会使用这个构造方法，不会使用默认的。与C++相同，java类中的构造方法也是可以重载的

#### 匿名对象

匿名对象就是没有明确的给出名字的对象，一般匿名对象只使用一次，而匿名对象旨在堆中开辟空间，不在栈中保存引用

```java
public class Hello {

    public static void main(String args[]) {
        new Student().fun();  // 这是一个匿名对象

    }
}
```

匿名对象和普通对象不同，没有任何栈内容引用他，所以此对象使用一次之后就等待垃圾收集机制回收

#### String

可以使用String，但他也是一个类，实例化一个String对象有两种方法，需要知道的是，一个字符串就是一个String类的匿名对象。使用new就是真正的对象，在栈用需要保存引用

```java
String name = "zhang"; // 实例化 String类的对象

String name1 = new String("lisi");

System.out.println(name + " and " + name1);
```

执行结果：

> zhang and lisi

String的内容比较，可以使用==，相同返回true，不用返回false

字符串的内容一旦声明就不能修改。如果在程序中修改了，输出时，看到确实是修改了，但是实际上在内存中没有修改，只不过是引用指向了堆的其他地址

![](java_img/String.png)



##### String中的方法

###### 字符串与字符数组的互转

字符串使用toCharArray()方法变成一个字符数组，也可以使用String类的构造方法把一个字符数组变为一个字符串

```java
String str1 = "zhang";

char c[] = str1.toCharArray();

System.out.println(str1); // zhang

for ( int i = 0; i < c.length; i++ )
    System.out.print(c[i] + "," );// z,h,a,n,g,

String str2 = new String(c); // 可以通过参数指定字符数组中那些字符转为字符串
System.out.println(str2); // zhang

```

###### 从字符串中取出指定位置的字符

可以直接使用String类中的charAt()方法取出字符串指定位置的字符

```java
String str1 = "zhang";

System.out.println(str1.charAt(0)); // a
System.out.println(str1.charAt(1)); // h
```



###### 字符串和byte数组之间转换

###### 字符串的长度

length()是一个方法，数组的长度是length，不是方法

```java
String str1 = "zhang";

System.out.println(str1.length()); // a
```

###### 查找一个指定的字符串是否存在

###### 去掉左右空格

###### 字符串截取

###### 按照指定的字符串拆分字符串

###### 字符串大小写转换

###### 判断是否以指定的字符串开头或结尾

###### 不区分大小写进行字符串比较

###### 将一个指定的字符串替换成其他的字符串

#### 引用传递及基本应用

应用传递是指将对内存空间的使用权交给多个栈内存空间，类也是引用类型数据，作为方法的参数时也是传递的引用，在方法内修改了类的成员变量，

##### 对象引用传递

对象引用传递，参数是类的对象

```java
class Demo{
    int temp;
}

public class Hello {

    public static void main(String args[]) {

        Demo d1 = new Demo();
        d1.temp = 50;
        System.out.println(d1.temp); // 调用方法之前，50
        fun(d1);
        System.out.println(d1.temp); // 调用方法之后，100

    }

    public  static void fun(Demo d2){

        d2.temp = 100;

    }
}
```

在方法中形参修改了实参的成员变量，整个过程如下图：

![](java_img/method_param_class.png)

从这个图中可以看出，在方法内部操作类对象，只不过是声明了形参，形参在栈中，指向了在堆中的（类对象）实参实体。操作时，操作的是形参，但是最终也会修改实参在堆中的内容。

##### 引用传递

引用传递，传递的引用类型的数据，像数组、字符串

```java
public class Hello {

    public static void main(String args[]) {

        String str1 = "zhang";

        System.out.println(str1); // 调用方法之前，zhang
        fun(str1);
        System.out.println(str1); // 调用方法之后，zhang

    }

    public  static void fun(String d2){

        d2 = "lisi";
    }
}
```

参数是一个String类的对象，函数没有修改实参的内容，因为字符串的内容一旦声明就不可以改变，改变的只是内存地址的指向，如下图：

![](java_img/method_param_class2.png)

##### 引用传递的另一个例子

```java
class Demo{
    String temp = "hello";
}

public class Hello {

    public static void main(String args[]) {

        Demo d1 = new Demo();
        d1.temp = "world";

        System.out.println(d1.temp); // 调用方法之前，world
        fun(d1);
        System.out.println(d1.temp); // 调用方法之后，MLDN

    }

    public  static void fun(Demo d2){

        d2.temp = "MLDN";

    }
}
```

这个例子中，String只是Demo类的一个属性，操作时，改变的是类Demo中的属性

![](java_img/method_param_class3.png)



#### this关键字

this关键字的作用：

- 强调本类中的方法
- 表示类中的属性
- 可以使用this调用本类的构造方法
- this表示当前对象

##### this调用本类中的属性

和C++差不多

```java
class Demo{
    private int age;
    
    public Demo(int age){
        this.age = age;    // 后面的age是形参age
    }
}
```

##### this调用构造方法

一个类中有多个构造方法，可以使用this相互调用。假如需要这样的功能：不管类中有多少个构造方法，只要对象被实例化出来，就必须打印一行“构造出了一个对象”信息。方案以：在类中所有构造函数中都使用`System.out.println`打印出来，但是如果要打印的内容改变了，就要修改所有的构造方法。

使用this调用本类中的构造方法，方案二如下：

```java
class Demo{
    private int age;
    public Demo(){
        System.out.println("构造出了一个对象");
    }
    public Demo(int age){
        this();   // 调用本类中的无参构造方法
        // ...
    }
    public Demo(int age,int name){
        this();
        // ...
    }
}

public class Hello {

    public static void main(String args[]) {

        Demo d1 = new Demo();

        Demo d2 = new Demo(4);
    }
    
}
```

但是要注意使用this调用本类中的构造方法的为止不是随意的，必须在构造方法的第一行，下面是错误的

```java
public Demo(int age){
    this.age = age;
    this();    // 错误的
    // ...
}
```

另外，要注意一定要留一个构造方法不使用this，作为出口，否则会产生循环调用

##### this表示当前对象

```java
class Demo{
    private int age;
    public void test(){
        System.out.println(this); // this表示调用test方法的对象
        
    }     
}
```



#### static关键字

static声明的成员（变量、方法）可以被类调用，static属性是类的所有对象共享的，都可以使用的

##### java中的内存区域

主要有4块内存空间

- 栈内存：保存所有的对象名称，即引用的堆内存的地址
- 堆内存：保存了每个对象的具体属性内存（成员）
- 全局数据区：保存static类型的属性
- 全局代码去：保存所有的方法定义



#### 代码块

上面的都是普通代码块

##### 静态代码块

谁用static声明的代码块。静态代码块优先于主方法执行，类中的静态代码块会优先于构造块执行，不管产生多少个对象，静态代码块只执行一次





#### 构造方法私有化

类的封装性不仅体现在对属性的封装，方法也可以封装，



#### 对象数组

很多个类的对象组成的数组。数组是引用型数据，要开辟空间，数组中每一个对象都是null值，则在使用时数组中的每一个对象必须分别进行实例化

```java
class Demo{
    private int age;
    public Demo(){
        System.out.println("构造出了一个对象");
    }

}

public class Hello {

    public static void main(String args[]) {

        Demo ds[] = new Demo[4]; // 数组，元素是Demo类的对象
                                 // 初始化之前，每一个元素都是默认值

        ds[0] = new Demo();  // 实例化第一个对象...
        ds[1] = new Demo();
        ds[2] = new Demo();
    }
}
```

#### 内部类

类内部可以定义另一个类，称为内部类

### Java面向对象（高级）

#### 继承

Java中只能多层继承（B继承A，C 再继承B），不能多重继承（C同时继承Ａ和B），子类不能访问父类的private成员。但是可以访问父类public成员，通过父类的public成员访问父类的private属性。相当于把父类的变量和方法都继承过来了，但是如果是private就不能访问，只能通过父类中的public方法访问

一个继承的例子如下：

```java
class A{    // 爷爷类     
    int a;
}
class B extends A{    // 父类
    private int b;

    public void setB(int b) {   // 访问本类中的private变量
        this.b = b;
    }
    public int getB(){
        return b;
    }
}
class C extends B{   // 本类
    int c;
}

public class Hello {

    public static void main(String args[]) {

        C c1 = new C();

        c1.a = 10; // 爷爷的变量
        c1.c = 20; // 自己的变量

        c1.setB(1);
        System.out.println(c1.getB());  // 1

    }

}
```

#### 子类实例化过程

子类对象在创建之前，先要调用父类的构造方法，再调用自己的构造方法。可以理解为子类需要父类中的材料，先把父类构造号，才能构造子类

#### 方法的重写

指的是，子类中定义和和父类中同名的方法，但是子类中被重写的方法不能有比父类方法中更加严格的访问权限

主要是有一个访问权限的问题

#### super关键字

super可以在子类中调用父类中的构造方法、普通方法和属性。和this一样，必须放在子类构造方法的首行

```java
class A{
    int a;

    public void print() {
        System.out.println("class A : public void print()");
    }
}
class B extends A {
    int b;
    public B(){
        super.print();  // 调用父类中的print方法
    }
}

public class Hello {

    public static void main(String args[]) {

        B b1 = new B();
    }
}
```



#### final关键字

final表示最终，也可以称为完结器，可以使用final声明子类、属性、方法：

- final声明类不能有子类
- final声明的方法不能被子类所覆写（重写）
- final声明的变量即称为常量，不可以修改

#### 抽象类

抽象类不能实例化对象，只能被继承产生子类，子类能实例化对象、一个子类只能继承一个抽象类

- 包含一个抽象方法的类属于抽象类
- 抽象类和抽象方法都要使用abstract关键字声明
- 抽象方法只需要声明，不需要实现
- 抽象类必须被子类继承，子类（非抽象类）必须重写抽象类中的全部抽象方法，如有存在没有重写的，那么这个子类依然属于抽象类，不能创建对象
- 抽象类不能使用final关键字声明
- 抽象方法不需要使用private修饰

```java
abstract class A{
    public abstract void fun1();
    public abstract void fun2();
}
class B extends A { // 报错：B不是抽象的, 并且未覆盖A中的抽象方法fun2()
    public void fun1(){
        System.out.println("class B extends A :　public fun1()");
    }
}
// 正确，C继承抽象类Ａ，但是没有重写所有的方法，那C也是抽象类
// C 需要使用abstract
abstract class C extends A {
    public void fun1(){
        System.out.println("class B extends A :　public fun1()");
    }
}
class D extends A {
    public void fun1(){
        System.out.println("class C extends A  :　public fun1()");
    }
    public void fun2(){
        System.out.println("class C extends A  :　public fun1()");
    }
}

public class Hello {

    public static void main(String args[]) {

        D d = new D();

        d.fun1();
        d.fun2();
    }
}
```

#### 接口

java中的接口是由全局常量和公共的抽象方法所组成的类，接口中不写public，也是public权限，接口中的方法都是public。

接口的概念中，已经明确的声明了接口是由全局常量和抽象方法组成的，所以接口中的函数可以省略public等

和抽象类一样接口若要使用也必须通过子类，子类通过implements关键字实现接口

一个子类可以实现两个接口，子类必须要覆写两个接口中的所有方法

一个子类可以同时实现接口和继承抽象类

```java
interface A{
    public void fun1();
}

interface B{
    public void fun2();
}
class D{
    public void fun3(){System.out.println("public void fun3()");}
}

// 类C继承类D，同时实现接口A和B
class C extends D implements A,B{
    public void fun1(){System.out.println("public void fun1()");}
    public void fun2(){System.out.println("public void fun2()");}
}

public class Hello {

    public static void main(String args[]) {

        C c1 = new C();
        c1.fun1();
        c1.fun2();
        c1.fun3();
        
    }
}
```

#### 对象的多态性

java中的面向对象有两种体现：

- 方法的重载和重写（覆写）
- 对象的多态性

对象的多态性主要分为一下两种类型：

- 向上转型：子类对象-->父类对象，程序自动完成
- 向下转型：父类对象-->子类对象，必须要明确的指明要转型的子类类型

##### 向上转型

```java
class A{
    public void fun1(){
        System.out.println("class A : public void fun1()");
    }
}

class B extends A{
    public void fun1(){
        System.out.println("class B extends A: public void fun1()");
    }
}


public class Hello {

    public static void main(String args[]) {

        B b1 = new B(); // 子类对象

        A a1 = b1; //  把子类对象赋值给父类对象
        
        a1.fun1(); // 调用父类对象的fun1方法，最终调用子类覆写的父类的方法
        
    }
}
```

此时虽然使用父类的方法调用fun1，但实际上调用的是被子类覆写的方法，即如果对象发生了向上转型（子类对象赋给父类），那么调用的方法一定是被子类覆写过的方法。但是这个父类对象不能调用子类自己的方法，需要向下转型

##### 向下转型

```java
class A{
    public void fun1(){
        System.out.println("class A : public void fun1()");
    }
    public void fun2(){
        this.fun1();
    }
}

class B extends A{
    public void fun1(){
        System.out.println("class B extends A: public void fun1()");
    }
    public void fun3(){
        System.out.println("class B extends A: public void fun3()");
    }
}


public class Hello {

    public static void main(String args[]) {

        A a = new B(); // 创建一个子类对象，给父类。向上转型，子类到父类

        B b = (B)a; // 把一个父类对象强制转为子类。向下转型

        b.fun1(); // class B extends A: public void fun1()
        b.fun2(); // class B extends A: public void fun1()
        b.fun3(); // class B extends A: public void fun3()
        
    }
}
```

如果要使用子类的对象，则一定指定只能用子类声明对象。在子类中调用父类的fun2，fun2要调用fun1，但此时的fun1已经被子类覆写了，所以调用的是子类覆写的fun1()

再进行对象向上转型之前，必须首先发生对象向上转型，否则将出现转换异常，

#### instanceof关键字

使用nstanceof关键字可以判断一个对象到底是哪一个类的实例，测试代码主要部分如下：

```java
A a = new A();

System.out.println((a instanceof A)); // true
System.out.println((a instanceof B)); // false
```

#### Objdet类

Java中所有的类都有一个父类Objdet，一个类只要没有明显地继承一个类，则肯定是Objdet类的子类，下面是两行的意义相同：

```java
class A extern Objdet{}
class A{}
```



#### 包装类

#### 匿名内部类



### 异常

236

### 包及访问控制权限

253

### 多线程

267



### 泛型

308

### 常用类库

337

### IO

408















### 其他知识

- java中严格区分大小写



#### String

使用”“，两个字符串之间可以使用+连接



#### 引用型数据

引用型数据默认值是null，表示暂时还没有任何指向的内存空间，变量名保存的不是数据实体，而是数据在堆内存的参考地址。如java中的数组































