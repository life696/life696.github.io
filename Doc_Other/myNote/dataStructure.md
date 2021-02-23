## 数据结构

[TOC]

#### 参考书：

- 《数据结构(C语言版)_严蔚敏》

[单项链表综合测试](code\单项链表综合测试.c)

#### 说明

代码语言是C语言

#### 基本概念

##### 数据

描述客观事物的符号，所有能输入到计算机中并被程序处理的符号总和。图像、声音等都算是数据

- 能被输入到计算机
- 能被程序处理

##### 数据元素

数据的基本单位，通常作为整体处理

##### 数据对象

性质相同的数据元素的集合，是数据的子集

##### 数据结构

是相互之间存在一种或多种特定关系的数据元素的集合，数据元素不是孤立存在的，他们之间存在着某种关系，这种关系称为结构。

###### 逻辑结构

逻辑结构分为：集合结构、线性结构、树形结构、图状/网状结构

- 集合结构：数据元素之间同属于一个集合
- 线性结构：数据元素存在一个对应一个的关系
- 树形结构：数据元素之间的关系是一对多
- 图状/网状结构：数据元素之间的关系是多对多

![](img/data_4data.png)

###### 物理结构

物理结构又叫存储结构。是数据在内存中存储的方式：顺序结构、链式结构

- 顺序结构：数据在一块内存空间中是连续存储的（挨着的）
- 链接结构：数据之间的不是连续的，位置是任意的，而是通过地址找到下一个数据元素，比较灵活

##### 数据类型

数据类型：是指一组性质相同的值的总称。C中的数据类型有：

- 原子类型：不可再分，像整形、字符型
- 结构类型：由多个类型构成，可以再分

抽象数据类（Abstract Data Type，ADT）：一个数学模型及定义在该模型上的一组操作，仅是逻辑关系，不是存储关系

##### 算法

算法是解决问题的办法，最基础的原则是能够解决问题，进而优化到快速解决、少占用资源解决，5个重要特性：

- 有穷性：必须在一定时间内完成
- 确定性：相同的输入能有相同的输出
- 可行性：
- 输入：
- 输出：

算法设计的要求：正确性，可读性、健壮性、时间效率高，占用内存低

##### 算法的复杂度

空间复杂度和时间复杂度，可以粗略地估计算法的执行效率

###### 时间复杂度

代码执行时间随数据规模增长的变化趋势，时间复杂度的分析只关注循环执行次数最多的一段代码即可

###### 空间复杂度

算法所需要的存储空间

#### 线性表

##### 线性表

线性表是n个数据的有序排列的数据结构，有个缺点：

- 删除或者插入新的数据时，需要移动其他数据，时间复杂度太大

##### 链表

链表也是一种数据结构，数据在内存中的位置是随意的，为了表示数据前后关系，只用指针指向后一个数据或者前一个数据，或者两个指针分别指向后一个和前一个。每一个数据和指向下一个数据的指针组成要给存储映像，称为结点，其中分为数据域和指针域

结点中只包含要给指针域的，叫做线性链表或单链表，链表的好处是删除或者插入数据，直接修改前后数据的指针域，不用动其他数据

##### 头指针

整个链表中的存取是从头指针开始的，头指针指向链表中的第一个结点，最后一个结点的之指针为空（NULL）

##### 头结点

单链表中第一个结点前增设一个结点，称为头结点。

- 头结点的数据域：可以存储结点个数，或者其他有用的数据
- 头结点的指针域：指向数据的第一个结点

![](img/data_headList.png)

##### 代码实现单链表

###### 链表初始化（带头结点）

带头结点的链表初始化使用的是二级指针，困扰我半天，先看要给架构体指针的例子：

```C
#include <stdio.h>

typedef struct A{
	
	int data;
	int *p;
	
}A, *pA;


int main()
{
	A a;
	pA pa;
	
	a.data = 2;
	pa = &a;
	
	printf("%d\n", pa->data); // 2 是a.data 
	printf("-----------\n");

	printf("&pa = %d\n", &pa ); // &pa = 2293304
	printf("pa = %d\n", pa );   // pa = 2293312
	printf("&(pa->data) = %d\n",&(pa->data)); // &(pa->data) = 2293312
	printf("pa->data = %d\n",pa->data);	      // pa->data = 2
	
	printf("-----------\n");
	
	printf("&a = %d\n", &a); // &a = 2293312
	printf("&(a.data) = %d\n", &(a.data)); // &(a.data) = 2293312
	printf("a.data = %d\n", a.data); // a.data = 2
	
	return 0;	
}
```

在内存中的存储结构如下：

![](img/data_head_link.png)

结构体指针只占用四个字节（所有指针都是4个，与机器有关），其中保存的是该类型结构体的地址，并不保存有结构体成员（空间也不够），所以上面例子中**pa->data**是pa指向的结构体中的data数据

以前一直以为是pa中的data，所以导致在链表的初始化过程中，以为pa->pnext指针是指向下一个结构体的。下面是链表初始化的函数：

```C
// 定义一个结点数据结构
typedef struct StudentNode{
    int id;
    char age;
    struct StudentNode *pnext;
}StudentNode, *pStudentNode;

// 创建空链表，必须用二级指针，涉及到头指针的创建
// 头指针的指针，才能修改头指针指向的内存空间地址
// 一级指针只能修改头指针指向的内存的中的数据
int iniList(pStudentNode *List){
    *List = (StudentNode *)malloc(sizeof(StudentNode )); // 申请头结点空间
    if (NULL == *List){
        return 0;
    }

    (*List)->pnext = NULL; // 头结点指针域为空
    (*List)->id = 0;      // 链表头结点id数据，用来当作数据量

    return 1;
}

int main()
{
    StudentNode *pt = NULL;
    int ret = iniList(&pt); // 得到一个空链表，链表头指向了头结点，头结点的指针域为空，即没有有效数据结点

    if ( NULL == pt || 0 == ret )  // 其实只用一个就行了，
    {
        printf("iniList is failed!\n");
    }
    else{
        printf("iniList is ok!\n");
    }

    return 0;
}
```

这里的得到pt是指向头结点的，pt->pnext就是头结点的指针域，他是空的。

###### 判断链表示否为空

```C 
// 判断链表是不是空的，是空的返回1，非空0,复合逻辑
int isListEmpty(StudentNode *L)
{
    if ( NULL == L->pnext )
    {
        return 1;
    }
    else{
        return 0;
    }
}
```

###### 计算有效数据节点的个数

```C
// 计算列表的有效节点数
int getListLen(StudentNode *L)
{
    int len = 0;
    pStudentNode pL;
    // 头结点指针域为空，则是空链表
    if ( isListEmpty(L) )
    {
        len = 0;
        return len;
    }
    pL = L->pnext; // p1
    len++;
    while(!isListEmpty(pL))
    {
        len++;
        pL = pL->pnext;
    }

    return len;
}
```

###### 手动添加结点

```C
int main()
{
    StudentNode *pt = NULL;
    iniList(&pt);
    if ( isListEmpty(pt) )
    {
        printf("pt list is empty\n");   // 执行这句是空的
    }
    else{

        printf("pt list is not empty\n");
    }

    // 手动创建4个结点
    StudentNode *p1 = (StudentNode *)malloc(sizeof(StudentNode )); // 申请头结点空间
    p1->pnext = NULL;
    pt->pnext = p1;

    StudentNode *p2 = (StudentNode *)malloc(sizeof(StudentNode )); // 申请头结点空间
    p2->pnext = NULL;
    p1->pnext = p2;

    StudentNode *p3 = (StudentNode *)malloc(sizeof(StudentNode )); // 申请头结点空间
    p3->pnext = NULL;
    p2->pnext = p3;
    StudentNode *p4 = (StudentNode *)malloc(sizeof(StudentNode )); // 申请头结点空间
    p4->pnext = NULL;
    p4->id = 999;      // 设置最后一个结点的数据域是999，便于表示
    p3->pnext = p4;

    int len = 99;  // 初始化一个值，方便调试
    len = getListLen(pt);
    printf("main len = %d\n", len); // 打印数据结点

     if ( isListEmpty(pt) )
    {
        printf("pt list is empty\n");
    }
    else{

        printf("pt list is not empty\n"); // 执行这句不是空的
    }
｝
```

###### 查找尾结点指针（指向的应该是NULL）

```c
// 获取链表的尾指针
pStudentNode getLinkTailPointer(StudentNode *L)
{
    pStudentNode ptemp;
    ptemp = L;
    if ( isListEmpty(ptemp) )
    {
        return ptemp;
    }

    // 非空就继续往下找
    while ( !isListEmpty(ptemp->pnext ))
    {
        ptemp = ptemp->pnext;
    }
    ptemp = ptemp->pnext;

    return ptemp;
}
int main()
{
    StudentNode *pt = NULL;
    iniList(&pt);

    // 手动创建4个结点
    StudentNode *p1 = (StudentNode *)malloc(sizeof(StudentNode )); // 申请头结点空间
    p1->pnext = NULL;
    pt->pnext = p1;
    pt->id++;

    StudentNode *p2 = (StudentNode *)malloc(sizeof(StudentNode )); // 申请头结点空间
    p2->pnext = NULL;
    p1->pnext = p2;
    pt->id++;

    StudentNode *p3 = (StudentNode *)malloc(sizeof(StudentNode )); // 申请头结点空间
    p3->pnext = NULL;
    p2->pnext = p3;
    pt->id++;

    StudentNode *p4 = (StudentNode *)malloc(sizeof(StudentNode )); // 申请头结点空间
    p4->pnext = NULL;
    p4->id = 999;      // 设置最后一个结点的数据域是999，便于表示
    p3->pnext = p4;
    pt->id++;

    pStudentNode pTail= getLinkTailPointer(pt);
    if ( isListEmpty(pTail) )
    {
        printf("num = %d\n", pt->id); // 头结点中的计数器
        printf("find tail...............\n");
        printf("pTail->id = %d\n", pTail->id);
    }
    else{
        printf("not find tail...............\n");
    }
    
    return 0;
}
```

###### 尾部插入/追加一个数据

```C
// 列表末尾追加数据
int listAppend(StudentNode *L)
{
    pStudentNode ptemp;
    ptemp = getLinkTailPointer(L);

    pStudentNode new_data = (StudentNode *)malloc(sizeof(StudentNode )); // 申请结点空间
    if (NULL == new_data){
        printf("申请空间失败，追加数据失败！\n");
        return 0;
    }
    new_data->pnext = NULL;
    new_data->id = 666;  // 测试用
    ptemp->pnext = new_data;

    return 0;
}
int main()
{
    StudentNode *pt = NULL;
    iniList(&pt);

    // 手动创建4个结点
    StudentNode *p1 = (StudentNode *)malloc(sizeof(StudentNode )); // 申请头结点空间
    p1->pnext = NULL;
    pt->pnext = p1;
    pt->id++;

    StudentNode *p2 = (StudentNode *)malloc(sizeof(StudentNode )); // 申请头结点空间
    p2->pnext = NULL;
    p1->pnext = p2;
    pt->id++;

    StudentNode *p3 = (StudentNode *)malloc(sizeof(StudentNode )); // 申请头结点空间
    p3->pnext = NULL;
    p2->pnext = p3;
    pt->id++;

    StudentNode *p4 = (StudentNode *)malloc(sizeof(StudentNode )); // 申请头结点空间
    p4->pnext = NULL;
    p4->id = 999;      // 设置最后一个结点的数据域是999，便于表示
    p3->pnext = p4;
    pt->id++;

    listAppend(pt); // 追加一个数据
    pStudentNode pTail= getLinkTailPointer(pt);
    if ( isListEmpty(pTail) )
    {
        printf("num = %d\n", pt->id); // 应该是666
        printf("find tail...............\n");
        printf("pTail->id = %d\n", pTail->id);
    }
    else{
        printf("not find tail...............\n");
    }

    return 0;
}
```

###### 获取链表第n的结点的指针

```C
////得到第n的结点的指针
pStudentNode getLinkNDataPointer(pStudentNode L, int n)
{
    pStudentNode ptemp;
    ptemp = L;

    int len = getListLen(ptemp);
    if ( len < n )
    {
        printf("Error:list len < n,so return the last data\n ");
        ptemp = getLinkTailPointer(ptemp);
        return ptemp;
    }

    for ( int i = 0; i < n; i++ )
    {
        ptemp = ptemp->pnext;
    }

    return ptemp;
}
int main()
{
    StudentNode *pt = NULL;
    iniList(&pt);

    // 在空链表的基础追加结点
    listAppend(pt);
    listAppend(pt);
    listAppend(pt);
    listAppend(pt);
    listAppend(pt);
    listAppend(pt);
    listAppend(pt);
    listAppend(pt);
    listAppend(pt);

    pStudentNode pn = getLinkNDataPointer(pt, 7);
    printf("pn->age = %d\n", pn->age);

    return 0;
}
```

###### 链表中间插入数据

```C
// 插入一个结点
// pos:0在末尾插入
// pos：1，在第一个结点之前插入，其他类似
int ListInsert(StudentNode *L, int position)
{
    int len;
    len = getListLen(L);

    // 如果目标位置大于链表长度，则插入失败，也可以选择在尾部追加
    if ( len < position )
    {
        printf("Error:list len < position, so insert failed!,but append a data\n");
        listAppend(L);  // 可以选择在尾部追加
        return 0 ;
    }

    pStudentNode ptemp = L;
    pStudentNode ppre;
    pStudentNode pnext;

    pStudentNode pnew;
    pnew = (StudentNode *)malloc(sizeof(StudentNode )); // 申请结点空间
    if (NULL == pnew){
        return 0;
    }

    pnew->age = 123456; // 测试用

    ppre = getLinkNDataPointer(ptemp, position - 1);
    pnext = getLinkNDataPointer(ptemp, position + 1);

    ppre->pnext = pnew;
    pnew->pnext = pnext;
    L->id++;

    return 0;
}


int main()
{
    StudentNode *pt = NULL;
    iniList(&pt);

    // 在空链表的基础追加
    listAppend(pt);
    listAppend(pt);
    listAppend(pt);
    listAppend(pt);
    listAppend(pt);
    listAppend(pt);
    listAppend(pt);
    listAppend(pt);
    listAppend(pt);

    pStudentNode pTail= getLinkTailPointer(pt);
    if ( isListEmpty(pTail) )
    {
        printf("find tail, num = %d, Tail->age = %d\n", pt->id,pTail->age );
    }

    ListInsert(pt, 44);

    pTail= getLinkTailPointer(pt);
    if ( isListEmpty(pTail) )
    {
        printf("find tail, num = %d, Tail->age = %d\n", pt->id,pTail->age );
    }

    return 0;
}
```

###### 删除一个数据

```c
// delete a data
int ListDelete(StudentNode *L, int position)
{
    int len;
    len = getListLen(L);

    // 如果目标位置大于链表长度，则插入失败，也可以选择在尾部追加
    if ( len < position )
    {
        printf("Error:list len < position, so delete failed!\n");
        //listAppend(L);  // 可以选择在尾部追加
        return 0 ;
    }
    pStudentNode ppre;
    pStudentNode pnext;
    pStudentNode ptemp = L;

    ppre = getLinkNDataPointer(ptemp, position - 1);
    pnext = getLinkNDataPointer(ptemp, position + 1);

    ppre->pnext = pnext;

    return 0;
}
int main()
{
    StudentNode *pt = NULL;
    iniList(&pt);

    // 在空链表的基础追加
    listAppend(pt);
    listAppend(pt);
    listAppend(pt);
    listAppend(pt);
    listAppend(pt);
    listAppend(pt);
    listAppend(pt);
    listAppend(pt);
    listAppend(pt);

    int de = 7;

    pStudentNode pTail= getLinkNDataPointer(pt, de);
    printf("Tail->age = %d\n",pTail->age );

    ListDelete(pt, de);


    pTail= getLinkNDataPointer(pt, de);
    printf("Tail->age = %d\n",pTail->age );

    return 0;
}
```

###### 打印链表数据

```
// print list data
void printList(StudentNode *L)
{
    pStudentNode ptemp;
    ptemp = L;
    int i = 1;
    while( !(NULL == ptemp->pnext) )
    {
        printf("%d data->age = %d\n", i, ptemp->id);
        i++;
        ptemp = ptemp->pnext;
    }
}
```

###### 单项综合测试

```C
#include <stdio.h>
#include <stdlib.h>

typedef struct StudentNode{
    int id;
    char age;
    struct StudentNode *pnext;
}StudentNode, *pStudentNode;

int iniList(pStudentNode *List){
    *List = (StudentNode *)malloc(sizeof(StudentNode )); // 申请头结点空间
    if (NULL == *List){
        return 0;
    }

    (*List)->pnext = NULL; // 头结点指针域为空
    (*List)->id = 0;      // 链表头结点id数据，用来当作数据量
    (*List)->age = 0;
    return 1;
}

// 获取链表的尾指针
pStudentNode getLinkTailPointer(StudentNode *L)
{
    pStudentNode ptemp;
    ptemp = L;
    if ( isListEmpty(ptemp) )
    {
        return ptemp;
    }


    // 非空就继续往下找
    while ( !isListEmpty(ptemp->pnext ))
    {
        ptemp = ptemp->pnext;
    }
    ptemp = ptemp->pnext;

    return ptemp;
}

// 判断链表是不是空的，是空的返回1
int isListEmpty(StudentNode *L)
{
    if ( NULL == L->pnext )
    {
        return 1;
    }

    else{
        return 0;
    }
}

// 计算列表的有效节点数
int getListLen(StudentNode *L)
{
    int len = 0;
    pStudentNode pL;
    // 头结点指针域为空，则是空链表
    if ( isListEmpty(L) )
    {
        len = 0;
        return len;
    }

    // 非空
    pL = L->pnext; // p1
    len++;
    while(!isListEmpty(pL))
    {
        len++;
        pL = pL->pnext;
    }

    return len;
}

// 列表末尾追加数据
int listAppend(StudentNode *L)
{
    pStudentNode ptemp;
    ptemp = getLinkTailPointer(L);

    pStudentNode new_data = (StudentNode *)malloc(sizeof(StudentNode )); // 申请结点空间
    if (NULL == new_data){
        printf("申请空间失败，追加数据失败！\n");
        return 0;
    }
    new_data->pnext = NULL;
    new_data->id = 666;  // 测试用
    new_data->age = ptemp->age + 11;  // 测试用
    ptemp->pnext = new_data;

    L->id++;

    return 0;
}

////得到第n的结点的指针
pStudentNode getLinkNDataPointer(pStudentNode L, int n)
{
    pStudentNode ptemp;
    ptemp = L;

    int len = getListLen(ptemp);
    if ( len < n )
    {
        printf("Error:list len < n,so return the last data\n ");
        ptemp = getLinkTailPointer(ptemp);
        return ptemp;
    }

    for ( int i = 0; i < n; i++ )
    {
        ptemp = ptemp->pnext;
    }

    return ptemp;
}
// 插入一个结点
// pos:0在末尾插入
// pos：1，在第一个结点之前插入，其他类似
int ListInsert(StudentNode *L, int position)
{
    int len;
    len = getListLen(L);

    // 如果目标位置大于链表长度，则插入失败，也可以选择在尾部追加
    if ( len < position )
    {
        printf("Error:list len < position, so insert failed!,but append a data\n");
        listAppend(L);  // 可以选择在尾部追加
        return 0 ;
    }

    pStudentNode ptemp = L;
    pStudentNode ppre;
    pStudentNode pnext;

    pStudentNode pnew;
    pnew = (StudentNode *)malloc(sizeof(StudentNode )); // 申请结点空间
    if (NULL == pnew){
        return 0;
    }

    pnew->age = 123456; // 测试用

    ppre = getLinkNDataPointer(ptemp, position - 1);
    pnext = getLinkNDataPointer(ptemp, position + 1);

    ppre->pnext = pnew;
    pnew->pnext = pnext;
    L->id++;

    return 0;
}

// delete a data
int ListDelete(StudentNode *L, int position)
{
    int len;
    len = getListLen(L);

    // 如果目标位置大于链表长度，则插入失败，也可以选择在尾部追加
    if ( len < position )
    {
        printf("Error:list len < position, so delete failed!\n");
        //listAppend(L);  // 可以选择在尾部追加
        return 0 ;
    }
    pStudentNode ppre;
    pStudentNode pnext;
    pStudentNode ptemp = L;

    ppre = getLinkNDataPointer(ptemp, position - 1);
    pnext = getLinkNDataPointer(ptemp, position + 1);

    ppre->pnext = pnext;

    return 0;
}

// print list data
void printList(StudentNode *L)
{
    pStudentNode ptemp;
    ptemp = L;
    int i = 1;
    while( !(NULL == ptemp->pnext) )
    {
        printf("%d data->age = %d\n", i, ptemp->id);
        i++;
        ptemp = ptemp->pnext;
    }
}

int main()
{
    StudentNode *pt = NULL;  // 创建链表头
    iniList(&pt);            // 初始化链表头，有头结点

    // list is empty?
    if ( isListEmpty(pt) )
    {
        printf("pt is empty!\n");
    }

    // 在空链表的基础追加9个结点，
    listAppend(pt);
    listAppend(pt);
    listAppend(pt);
    listAppend(pt);
    listAppend(pt);
    listAppend(pt);
    listAppend(pt);
    listAppend(pt);
    listAppend(pt);

    // print list data
    printList(pt);

    // list is empty?
    if ( !isListEmpty(pt) )
    {
        printf("pt is not empty!\n");
    }

    // get n data pointer
    int pos = 3;
    pStudentNode p = getLinkNDataPointer(pt, pos);
    printf("NO.%d data->age = %d\n",pos , p->age );

    // det list data number
    int len = getListLen(pt);
    printf("pt len = %d\n", len);

    // get list tail data pointer
    p = getLinkTailPointer(pt);
    if ( NULL == p->pnext )
    {
        printf("it is tail data!\n");
    }

    return 0;
}
```

##### 循环链表

循环链表是在单链表基础上，最后一个结点的指针域指向了头结点



##### 双向链表

双向链表在单向链表基础上，每个结点又加了要给指针域，指向前一个结点的数据域，这样的话就可以往回查找了







#### 栈和队列

##### 栈

栈是只能在表尾插入或者删除的线性表。表尾称为栈顶，表头称为栈底。类似于带底的桶，只能在最上面添加和删除。

##### 队列

队列是先进先出的结构（FIFO），没有底的水桶。允许插入的一端叫做队尾，允许删除的一端叫做队头

#### 串



#### 数组和广义表

##### 广义表

广义表是线性表的推广，广泛用于人工智能等领域的表语言LISP语言，把广义表作为基本的数据结构，就连程序也表示为一系列的广义表。

广义表中的数据元素可以有不同的结构，所以很难用顺序存储结构表示，通常采用链式存储结构



#### 树和二叉树



##### 树

树是n结点的有限集，树是以分支关系定义的层次结构。在任意一棵非空树中：

- 有且仅有一个特定的叫做根的节点
- n > 1时，其余结点可以分为m个互不相交的有限级，没一个集又是一棵树，称为根的子树

树的结点包含一个数据元素及若干指向其子树的分支，结点拥有的子树数称为**节点的度**，度为0的结点称为**叶子**或终端结点。即度是一个结点再分的数目，不能再分就是叶子了。结点的子树的根称为该节点的孩子，该结点称为孩子的双亲。树中结点的最大层次称为树的**深度**，从根结点开始数，根是第一层

有序树：树中结点的各个子树看成从左到右是有次序的（不能互换的）

森林：是m棵互不相交的树的集合

##### 二叉树

二叉树的特点是每个结点之多只有两颗子树（即不存在度大于2的结点），并且二叉树的子树有左右之分，次序不能任意颠倒

###### 二叉树的性质1

在二叉树的第i层上，最多有2^(i-1)个结点，第一层有一个，1,24,8,16,32等比数列。

###### 二叉树的性质2

深度为k的二叉树最大的结点数为(2^k)-1，

###### 二叉树的性质3

对任何一颗二叉树T，如果其终端节点数为n0，度为2的结点数为n2，n0=n2+1，即叶子的个数=有两个叉的节点数+1



一颗深度为k且有(2^k)-1个结点的二叉树称为**满二叉树**，可以对满二叉树编号，从根结点开始，自上而下，从左到右。深度为k，有n个结点的二叉树，当且仅当每一个结点都与深度k的满二叉树中标号从1到n的节点一一对应时，才叫完全二叉树。

完全二叉树的性质：

- 具有n个结点的完全二叉树的深度为|log2n|+1（以2为底n的对数取绝对值在加1）
- 如果一颗有n个结点的完全二叉树（深度为|log2n|+1）的节点按照层序标号（从第2层到第|log2n|+1层，每层从左到右）则对任一结点i（1<=i<=n）有：
  - 如果i=1，则节点i是二叉树的根，无双亲；如果i>1，则其双亲是结点i/2
  - 如果2i>n，则结点i无左孩子（结点i为叶子结点）；否则左孩子结点为2i
  - 如果2i+1>n，则结点i无右孩子，否则右孩子结点是2i+1

##### 二叉树的存储结构

顺序结构只适用于完全二叉树，其他的用链式结构

##### 遍历二叉树

##### 线索二叉树

##### 树和森林

###### 树的存储结构

双亲表示法，孩子表示法，孩子兄弟表示法

###### 二叉树的转换

给定一棵树没可以找到唯一的一颗二叉树与之对应，从物理结构看，他们的二叉链表是相同的，知识解释不同

###### 树和森林的遍历



##### 树和等价问题



##### 赫夫曼树

又称最优树，是带权路径长度最短的树。先看最优二叉树

###### 最优二叉树

路径长度：可以理解为结点之间的连接线数目

树的路径长度：从树根结点到每一个叶子的路径长度之和。

结点可以带有权值，长度乘权值之和为树的路径长度，最小的二叉树乘坐最优二叉树或赫夫曼树

![](img/data_tree.png)



##### 赫夫曼编码

报文

##### 回溯法与树的遍历

八皇后问题

回溯法是一种设计递归过程的方法，求解过程实质上是一个先序遍历一颗状态树的过程知识这棵树不是遍历前预先建立的，而是隐含在遍历过程中，

#### 图

图数据结构比线性表和树更复杂

- 线性表：数据元素之间仅有线性关系，每个数据只有一个直接前驱和一个直接后驱
- 树形结构：数据元素之间有明显的层次关系，每一层上的数据可能和下一层中多个数据元素（子结点）相关，但只能和
- 图形结构：结点之间的关系可以是任意的，图中的任意两个数据元素之间都可能相关

##### 有向图和无向图

图中的数据元素叫做**顶点**（Vertex），V是顶点的集合，VR是两个顶点之间关系的集合，若<u,w>属于VR，则<u,w>表示从u到w的一条弧(Arc)，且称u为弧尾或起始点，w称为弧头或终点，此时的图称为**有向图**（Digraph）。

若<u,w>属于VR，必有若<w,u>属于VR，即VR是对称的，则以无序对<u,w>代替这两个有序对，表示u和w之间的一条边（Edge），此时称为**无向图**

![](img/data_graph.png)

##### 其他概念

完全图、有向完全图、稀疏图、稠密图

顶点数目n，边或弧的数目e，不考虑顶点到自身的弧或边

- 对于无向图：e的取值范围是0到1/2(n-1)n，有1/2(n-1)n条边的无向图称为完全图。
- 对于有向图：e的取值范围是0到(n-1)n，有(n-1)n条边弧的有向图成为有向完全图
- 稠密图：
- 稠密图：
- 权：边长或弧相关的数，权可以表示从一个顶点到另一个顶点的距离或耗费
- 网：带权的图成为网
- 邻接点
- 出度
- 连通图
- 

##### 有向图生成森林



##### 图的存储结构

图没有顺序映像的存储结构，可以借助数组的数据类型表示元素之间的关系。多重链表可以表示图

###### 数组表示法



###### 邻接表

###### 十字链表

###### 邻接多重表

##### 图的遍历

##### 图的连通性

##### 有向无环图

##### 最短路径



#### 动态存储管理































