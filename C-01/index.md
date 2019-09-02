---
title: C++01
date: 2019-08-29 10:00:44
---
下一章：[传送门](https://blog.csdn.net/Ikaros_521/article/details/99602635)


思考题：C与C++的区别？
**一、C++介绍**
    本贾尼·斯特劳斯特卢普，与1979年4月份贝尔实验室的本贾尼博士在分析UNIX系统分布内核流量分析时，希望有一种有效的更加模块化的工具。
    1979年10月完成了预处理器Cpre，为C增加了类机制，也就是面向对象，1983年完成了C++的第一个版本，C with classes也就是C++。
    C++与C的不同点：
    1、C++完全兼容C的所有语法（内容）
    2、支持面向对象的编程思想
    3、支持运算符重载
    4、支持泛型编程、模板
    5、支持异常处理
    6、类型检查严格

<!--more-->

**二、第一个C++程序**
    1、文件扩展名
        .cpp .cc .C .cxx
    2、编译器
        g++ 大多数系统需要额外安装，Ubuntu系统下的安装命令：
            sudo apt-get update
            sudo apt-get install g++
        gcc也可以继续使用，但需要增加参数 -xC++ -lstdc++
    3、头文件
        #include <iostream>
        #include <stdio.h> 可以继续使用，但C++建议使用 #include <cstdio>
    4、输入/输出
        cin << 输入数据
        cout >> 输出数据
        cin/cout会自动识别类型
        scanf/printf可以继续使用
        注意：cout和cin是类对象，而scanf/printf是标准库函数。
    5、增加了名字空间
        std::cout
        using namespace std;
        

**三、名字空间**
    1、什么是名字空间
    在C++中经常使用多个独立开发的库来完成项目，由于库的作者或开发人员没见过面，因此命名冲突在所难免。
    2、为什么需要名字空间
    在项目中函数名、全局变量、结构、联合、枚举、类，非常有可能名字冲突，而名字空间就对这些命名进行逻辑空间划分（不是物理单元划分），
    为了解决命名冲突，C++之父为防止命名冲突给C++设计一个名字空间的机制。
    通过使用namespace XXX把库中的变量、函数、类型、结构等包含在名字空间中，形成自己的作用域，避免名字冲突。
    namespace xxx
    {
    }// 没有分号
    注意：名字空间也是一种标识符，在同一作用域下不能重名。
    3、同名的名字空间有自动合并（为了声明和定义可以分开写）
    同名的名字空间中如果有重名的依然会命名冲突
    4、名字空间的使用方法
    ::域限定符
    空间名::标识符 // 使用麻烦，但是非常安全
    using namespace 空间名; 把空间中定义的标识符导入到当前代码中
        不建议这样使用，相当于把垃圾分类后，又导入同一个垃圾车，依然会冲突

```
#include <iostream>

namespace test{
int cout = 100;
int cin = 99;
}

int main()
{
	std::cout<<test::cout<<' '<<test::cin<<std::endl;
}
```


   5、无名名字空间
    不属于任何名字空间中的标识符，隶属于无名名字空间。
    无名名字空间中的成员使用 ::标识符 进行访问。
    如何访问被屏蔽的全局变量。


   6、名字空间的嵌套
    名字空间内部可以再定义名字空间，这种名字空间嵌套
    内层的名字空间与外层的名字空间的成员，可以重名，内层会屏蔽外层的同名标识符。
    多层的名字空间在使用时逐层分解。

```
    n1::n2::num;
    namespace n1
    {
        int num = 1;
        namespace n2
        {   
            int num = 2;
            namespace n3
            {

            }
        }
    }


```

   7、可以给名字空间取别名
    由于名字空间可以嵌套，这样就会导致在使用内层成员时过于麻烦，可以给名字空间取别名来解决这类问题。
    namespace n123 = n1::n2::n3;


**四、C++的结构**


   1、不再需要 typedef ，在定义结构变量时，可以省略struct关键字
    2、成员可以是函数（成员函数），在成员函数中可以直接访问成员变量，不需要.或->，但是C的结构成员可以是函数指针。
    3、有一些隐藏的成员函数（构造、析构、拷贝构造、赋值构造）。
    4、可以继承，可以设置成员的访问权限（面向对象）。


```
#include <iostream>
#include <cstring>
using namespace std;

struct Man
{
	char id[18];
};

struct Student:public Man
{
	char name[20];
	char sex;
	short age;
	Student(void)
	{
		cout<< "我被调用了..." << endl;
	}
	void show(void)
	{
		cout << "我是秀" << name << " " << sex << " " << age << endl;
	}
};

int main()
{
	Student stu;
	strcpy(stu.name,"hehe");
	stu.sex = 'm';
	stu.age = 12;
	strcpy(stu.id,"1235214141231");

	cout << stu.name << " " << stu.sex << " " << stu.age << endl;
	cout<<stu.id<<endl;
	stu.show();
}
```



**五、C++的联合**


   1、不再需要 typedef ，在定义结构变量时，可以省略union关键字
    2、成员可以是函数（成员函数），在成员函数中可以直接访问成员变量，不需要.或->，但是C的结构成员可以是函数指针。
    3、有一些隐藏的成员函数（构造、析构、拷贝构造、赋值构造）。


```
#include <iostream>
using namespace std;

union Data
{
	char ch[5];
	int num;
	Data(void)
	{
		cout<<"我被调用了？"<<endl;
	}
	void show(void)
	{
		cout << ch <<" " << num << endl;
	}
	~Data(void)
	{
		cout<<"析构"<<endl;
	}
};

int main()
{
	Data d;
	d.show();
	cout << sizeof(d) << endl;

}
```
    

**六、C++的枚举**


   1、定义、使用方法与C语言基本一致。
    2、类型检查比C语言更严格


```
#include <iostream>

using namespace std;

enum Color
{
	RED,
	YELLOW,
	BLUE,
	WHITE,
	BLACK
};

int main()
{
	Color r;
	r = BLUE;
	//r = 0; error 类型检查更严格
	cout << r << endl;
}


```


**七、C++的布尔类型**


   1、C++具有真的布尔类型，bool是C++中的关键字，在C语言中使用布尔类型需要导入头文件stdbool.h（在C11中bool应该是数据类型了）。
    2、在C++中 true false 是关键字，而在C语言中不是。
    3、在C++中 true false 是1字节，而C语言中是4字节。




```
#include <iostream>
using namespace std;

int main()
{
	bool flag = 0;
	cout << flag << " " << sizeof(flag) << endl;
}
```


**八、C++的void***


   1、C语言中void* 可以与任意类型指针 自动转换。


```
#include <stdio.h>

int main()
{
	void* p = NULL;
	char* p1 = p;
	int* p2 = p;
	double* p3 = p;
	p = p1;
	p = p3;
}
```


   2、C++中void*不能给其他类型的指针直接赋值，必须强制类型转换，但其他类型的指针可以自动给void*赋值。
    3、C++为什么这样修改void*？
        为了更安全，所以C++类型检查更严格。
        C++可以自动识别类型，对万能指针的需求不再那么强烈。


**九、操作符别名**


   某些特殊语言的键没有~,&符合，所以C++标准委员会为了让C++更有竞争力，为符号定义了一些别名，让这些小语种也可以愉快编写C++代码
   
    and   	&&
    or      ||
    not     !
    {       <%    
    }       %>
    #       :%


```
%:include <iostream>
using namespace std;

int main()
<%
	cout << "hello" <<endl;
%>
```



