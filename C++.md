### 定义常量

在C++中，有两种简单的定义常量的方式：

- 使用`#define` 预处理器。
- 使用`const`关键字。

#### define预处理器

`#define identifier value `

```c++
#include <iostream>
using namespace std;

#define LENGTH 10   
#define WIDTH  5
#define NEWLINE '\n'

int main()
{

   int area;  
   
   area = LENGTH * WIDTH;
   cout << area;
   cout << NEWLINE;
   return 0;
}
```

#### const 关键字

`const type variable = value; `

**const**类型的对象在程序执行期间不能被修改改变。

```C++
#include <iostream>
using namespace std;
int main()
{
    const int length = 10;
    const int width = 5;
    const char newline = '\n';
    int area;
    
    area = length * width;
    cout<<area;
    cout<<newline;
    
    return 0;
}
```

### C++ 修饰符类型

可以在数据类型之前写修饰符，用来改变基本类型的含义

下面列出了数据类型修饰符：

- signed
- unsigned
- long
- short

修饰符 **signed、unsigned、long 和 short** 可应用于整型，**signed** 和 **unsigned** 可应用于字符型，**long** 可应用于双精度型。

修饰符 **signed** 和 **unsigned** 也可以作为 **long** 或 **short** 修饰符的前缀。例如：**unsigned long int**。

C++ 允许使用速记符号来声明**无符号短整数**或**无符号长整数**。您可以不写 int，只写单词 **unsigned short** 或 **unsigned long**，int 是隐含的。例如，下面的两个语句都声明了无符号整型变量。

```C++
unsigned x;
unsigned int y;
```

为了理解 C++ 解释有符号整数和无符号整数修饰符之间的差别，我们来运行一下下面这个短程序：

```c++
#include <iostream>
using namespace std;
 
/* 
 * 这个程序演示了有符号整数和无符号整数之间的差别
*/
int main()
{
   short int i;           // 有符号短整数
   short unsigned int j;  // 无符号短整数

   j = 50000;

   i = j;
   cout << i << " " << j;

   return 0;
} 
```

当上面的程序运行时，会输出下列结果：

```
-15536 50000
```

上述结果中，无符号短整数 50,000 的位模式被解释为有符号短整数 -15,536。

### C++ 中的类型限定符

类型限定符提供了变量的额外信息。

|    限定符    | 含义                                                         |
| :----------: | :----------------------------------------------------------- |
|    const     | **const** 类型的对象在程序执行期间不能被修改改变。           |
| ==volatile== | 修饰符 **volatile** 告诉编译器，变量的值可能以程序未明确指定的方式被改变。 |
|   restrict   | 由 **restrict** 修饰的指针是唯一一种访问它所指向的对象的方式。只有 C99 增加了新的类型限定符 restrict。 |

### C++ 存储类

存储类定义C++程序中变量/函数的范围（可见性）和生命周期，说明符放在所修饰的变量的类型之前

- auto
- register
- static
- extern
- mutable

#### auto 存储类

> 在C++11 中， auto 关键字不再是C++存储类说明符。从C++11开始，**==auto 关键字声明一个变量，该变量的类型是从其声明中的初始化表达式推导出来的。==**

**auto** 存储类是所有局部变量默认的存储类。

#### register 存储类

**register** 存储类用于定义存储在寄存器中而不是**RAM**中的局部变量。这意味着变量的最大尺寸等于寄存器的大小（通常是一个词），且不能对他应用一元的'&'运算符（因为它没有内存位置（地址））。

```
{
   register int  miles;
}
```

寄存器只用于需要快速访问的变量，比如计数器。还应注意的是，定义 'register' 并不意味着变量将被存储在寄存器中，它意味着变量可能存储在寄存器中，这取决于硬件和实现的限制。

#### static 存储类

- **static** 存储类告诉编译器在程序的生命周期内保持局部变量的存在，这样它所修饰的局部变量在函数执行结束后也不会被销毁，在函数下次被调用时数据还存在；
- 修饰的全局变量时，会使变量的作用域限制在声明它的文件内，文件内的所有函数都可以访问它；
- 用在类数据成员变量时，则所有成员共享这个变量的一份副本。

```c++
#include <iostream>
 
// 函数声明 
void func(void);
 
static int count = 10; /* 全局变量 */
 
int main()
{
    while(count--)
    {
       func();
    }
    return 0;
}
// 函数定义
void func( void )
{
    static int i = 5; // 局部静态变量
    i++;
    std::cout << "变量 i 为 " << i ;
    std::cout << " , 变量 count 为 " << count << std::endl;
}
```

#### extern 存储类

**extern** 存储类用于提供一个全局变量的引用，全局变量对所有的程序文件都是可见的，当你使用`extern`时，是用来在另一个文件中声明一个全局变量或函数。

第一个文件：main.cpp

```
#include <iostream>
 
int count ;
extern void write_extern();
 
int main()
{
   count = 5;
   write_extern();
}
```

第二个文件：support.cpp

```c++
#include <iostream>
 
extern int count;
 
void write_extern(void)
{
   std::cout << "Count is " << count << std::endl; 
} 
```

在这里，第二个文件中的 *extern* 关键字用于声明已经在第一个文件 main.cpp 中定义的 count。现在 ，编译这两个文件，如下所示：

```
$g++ main.cpp support.cpp -o write
```

这会产生 **write** 可执行程序，尝试执行 **write**，它会产生下列结果：

```c++
$ ./write
Count is 5
```

### C++ 运算符

#### 位运算符

位运算符作用于位，并逐位执行操作。真值表如下所示：

|  p   |  q   | p & q | p \| q | p ^ q |
| :--: | :--: | :---: | :----: | :---: |
|  0   |  0   |   0   |   0    |   0   |
|  0   |  1   |   0   |   1    |   1   |
|  1   |  1   |   1   |   1    |   0   |
|  1   |  0   |   0   |   1    |   1   |

假设A = 60，B = 13，现在以二进制格式表示，它们如下所示：

> ​       A = 0011 1100
>
> ​       B = 0000 1101
>
> ---------------------

> A & B = 0000 1100
>
>  A | B = 0011 1101
>
>  A ^ B = 0011 0001
>
> ​      ~A = 1100 0011

下表显示了C++支持的位运算符。假设A = 60，B = 13，则：

| 运算符 | 描述                                                         | 实例                                                         |
| :----- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| &      | 如果同时存在于两个操作数中，二进制 AND 运算符复制一位到结果中。 | (A & B) 将得到 12，即为 0000 1100                            |
| \|     | 如果存在于任一操作数中，二进制 OR 运算符复制一位到结果中。   | (A \| B) 将得到 61，即为 0011 1101                           |
| ^      | 如果存在于其中一个操作数中但不同时存在于两个操作数中，二进制异或运算符复制一位到结果中。 | (A ^ B) 将得到 49，即为 0011 0001                            |
| ~      | 二进制补码运算符是一元运算符，具有"翻转"位效果。             | (~A ) 将得到 -61，即为 1100 0011，2 的补码形式，带符号的二进制数。 |
| <<     | 二进制左移运算符。左操作数的值向左移动右操作数指定的位数。   | A << 2 将得到 240，即为 1111 0000                            |
| >>     | 二进制右移运算符。左操作数的值向右移动右操作数指定的位数。   | A >> 2 将得到 15，即为 0000 1111                             |

A = 0011 1100 ----> A << 2（将A的二进制数向左移动两位，多出来的用’0‘补足）---> A = 1111 0000

#### 赋值运算符

下表列出了 C++ 支持的赋值运算符：

| 运算符 | 描述                                                         | 实例                            |
| :----- | :----------------------------------------------------------- | :------------------------------ |
| =      | 简单的赋值运算符，把右边操作数的值赋给左边操作数             | C = A + B 将把 A + B 的值赋给 C |
| +=     | 加且赋值运算符，把右边操作数加上左边操作数的结果赋值给左边操作数 | C += A 相当于 C = C + A         |
| -=     | 减且赋值运算符，把左边操作数减去右边操作数的结果赋值给左边操作数 | C -= A 相当于 C = C - A         |
| *=     | 乘且赋值运算符，把右边操作数乘以左边操作数的结果赋值给左边操作数 | C *= A 相当于 C = C * A         |
| /=     | 除且赋值运算符，把左边操作数除以右边操作数的结果赋值给左边操作数 | C /= A 相当于 C = C / A         |
| %=     | 求模且赋值运算符，求两个操作数的模赋值给左边操作数           | C %= A 相当于 C = C % A         |
| <<=    | 左移且赋值运算符                                             | C <<= 2 等同于 C = C << 2       |
| >>=    | 右移且赋值运算符                                             | C >>= 2 等同于 C = C >> 2       |
| &=     | 按位与且赋值运算符                                           | C &= 2 等同于 C = C & 2         |
| ^=     | 按位异或且赋值运算符                                         | C ^= 2 等同于 C = C ^ 2         |
| \|=    | 按位或且赋值运算符                                           | C \|= 2 等同于 C = C \| 2       |

#### 杂项运算符

下表列出了 C++ 支持的其他一些重要的运算符。

| 运算符               | 描述                                                         |
| :------------------- | :----------------------------------------------------------- |
| sizeof               | [sizeof 运算符](https://www.w3cschool.cn/cpp/cpp-sizeof-operator.html)返回变量的大小。例如，sizeof(a) 将返回 4，其中 a 是整数。 |
| Condition ? X : Y    | [条件运算符](https://www.w3cschool.cn/cpp/cpp-conditional-operator.html)。如果 Condition 为真 ? 则值为 X : 否则值为 Y。 |
| ,                    | [逗号运算符](https://www.w3cschool.cn/cpp/cpp-comma-operator.html)会顺序执行一系列运算。整个逗号表达式的值是以逗号分隔的列表中的最后一个表达式的值。 |
| .（点）和 ->（箭头） | [成员运算符](https://www.w3cschool.cn/cpp/cpp-member-operators.html)用于引用类、结构和共用体的成员。 |
| Cast                 | [强制转换运算符](https://www.w3cschool.cn/cpp/cpp-casting-operators.html)把一种数据类型转换为另一种数据类型。例如，int(2.2000) 将返回 2。 |
| &                    | [指针运算符 &](https://www.w3cschool.cn/cpp/cpp-pointer-operators.html) 返回变量的地址。例如 &a; 将给出变量的实际地址。 |
| *                    | [指针运算符 *](https://www.w3cschool.cn/cpp/cpp-pointer-operators.html) 指向一个变量。例如，*var; 将指向变量 var。 |

### 函数

#### 指针调用

- 传值传进去的是实际参数的地址，所以在函数内修改形式参数，会影响实际参数的值，函数声明时要声明参数为指针类型。

```c++
// 函数定义
void swap(int *a, int *b) {
	int temp;
	temp = *a;	//保存a所指的地址上存储的变量的值
	*a = *b;
	*b = temp;
	
	return;
}
```

> `int *a = &x;`  **a**中存放的是整形x的地址，***a** 就可以将地址中的变量值取出来，所以`int temp = *a; `这样数据类型就是相同的

现在，让我们通过指针传值来调用函数 **swap()**：

```c++
#include <iostream>
using namespace std;

// 函数声明
void swap(int *x, int *y);

int main ()
{
   // 局部变量声明
   int a = 100;
   int b = 200;
 
   cout << "交换前，a 的值：" << a << endl;
   cout << "交换前，b 的值：" << b << endl;

   /* 调用函数来交换值
    * &a 表示指向 a 的指针，即变量 a 的地址 
    * &b 表示指向 b 的指针，即变量 b 的地址 
    */
   swap(&a, &b);

   cout << "交换后，a 的值：" << a << endl;
   cout << "交换后，b 的值：" << b << endl;
 
   return 0;
}
```

#### 引用调用

把参数的地址复制给形式参数

```c++
// 函数定义
void swap(int &x, int &y)
{
   int temp;
   temp = x; /* 保存地址 x 的值 */
   x = y;    /* 把 y 赋值给 x */
   y = temp; /* 把 x 赋值给 y  */
  
   return;
}
```

#### 形式参数的默认值

- 在定义函数时，可以将参数列表中的形参设定默认值，当调用函数时，如果传递进来的实际参数有留空，则就使用这个默认值。

  ```C++
  void func(int a, int b = 10) {}
  int main() {
      int a = 2, b = 3;
      func(a);	//形参b为空，所以使用默认值b = 10
  }
  ```

#### typedef 关键字

typedef 可以为创建的类型取一个“别名”。例如：

```C++
typedef sruct
{
    char title[50];
    char author[50];
    char subject[50];
    int book_id;
}Books;

Books book1, book2;		//直接用Books来定义变量，不需要struct关键字
```

### C++ 继承

继承允许我们依据一个类来定义另一个类，已有的类称为基类，新建的类称为派生类。

继承代表了**is a** 的关系。

==`class 派生类名 : 访问修饰符 基类名`== 

```c++
#include <iostream>
using namespace std;
//基类
class Shape
{
    public:
    	void setWidth(int w)
        {
            width w;
        }
    	void setHeight(int h)
        {
            height = h;
        }
    protected:
    	int width, height;
};
//派生类
class Traingle : public Shape
{
    public:
    	int getArea()
        {
            return width * height;
        }
};
int main()
{
    Traingle rect;
    rect.setWidth(10);
    rect.setHeight(20);
    cout << "rect`s area is :" << rect.getArea() << endl;
    
    return 0;
}
```

#### 多继承

多继承即一个子类可以有多个父类，它继承了多个父类的特性。

C++ 类可以从多个类继承成员，语法如下：

```
class <派生类名>:<继承方式1><基类名1>,<继承方式2><基类名2>,…
{
<派生类类体>
};
```

其中，访问修饰符继承方式是 **public、protected** 或 **private** 其中的一个，用来修饰每个基类，各个基类之间用逗号分隔。

#### 访问控制和继承

派生类可以访问基类中所有的非私有成员。因此基类成员如果不想被派生类的成员函数访问，则应在基类中声明为 private。

我们可以根据访问权限总结出不同的访问类型，如下所示：

| 访问     | public | protected | private |
| :------- | :----- | :-------- | :------ |
| 同一个类 | yes    | yes       | yes     |
| 派生类   | yes    | yes       | no      |
| 外部的类 | yes    | no        | no      |

一个派生类继承了所有的基类方法，但下列情况除外：

- ==基类的构造函数、析构函数和拷贝构造函数。==
- 基类的重载运算符。
- 基类的友元函数。

#### 继承类型

当一个类派生自基类，该基类可以被继承为 **public、protected** 或 **private** 几种类型。继承类型是通过上面讲解的访问修饰符 access-specifier 来指定的。

我们几乎不使用 **protected** 或 **private** 继承，通常使用 **public** 继承。当使用不同类型的继承时，遵循以下几个规则：

- **公有继承（public）：**当一个类派生自**公有**基类时，基类的**公有**成员也是派生类的**公有**成员，基类的**保护**成员也是派生类的**保护**成员，基类的**私有**成员不能直接被派生类访问，但是可以通过调用基类的**公有**和**保护**成员来访问。
- **保护继承（protected）：** 当一个类派生自**保护**基类时，基类的**公有**和**保护**成员将成为派生类的**保护**成员。
- **私有继承（private）：**当一个类派生自**私有**基类时，基类的**公有**和**保护**成员将成为派生类的**私有**成员。

## 多继承

### C++ 接口（抽象类）

接口描述了类的行为和功能，而不需要完成类的特定实现。

C++接口是使用**抽象类**来实现的，如果类中至少有一个函数被声明为纯虚函数，则这个类就是抽象类。纯虚函数是通过在生命中使用`= 0`来指定的，如下所示：

```C++
class Box 
{
public: 
    // 纯虚函数      
    virtual double getVolume() = 0;   
private:      
    double length;      // 长度      
    double breadth;     // 宽度      
    double height;      // 高度 
};
```

- 设计抽象类的目的，是为了给其他类提供一个可以继承的适当的基类。
- 抽象类不能被用于实例化对象，他只能作为**接口**使用。
- 如果一个 ABC 的子类需要被实例化，则必须实现每个虚函数

实例：

```C++
#include <iostream>
using namespace std;

//基类
class Shape
{
public:
    //提供接口框架的纯虚函数
    virtual int getarea() = 0;
    
    void setWidth(int w)
    {
        this->width = w;
    }
    void setHeight(int h)
    {
        this->height = h;
    }

protected:
    int width, height;
};

//派生类
class Rectangle : public Shape
{
public:
    int getarea()
    {
        return width * height;
    }
};

class Triangle : public Shape
{
public:
    int getarea()
    {
        return width * height / 2;
    }
};

int main()
{
    Rectangle rec;
    Triangle tri;

    rec.setHeight(7);
    rec.setWidth(5);
    cout << "rec的面积是：" << rec.getarea() << endl;

    tri.setHeight(10);
    tri.setWidth(2);
    cout << "tri的面积是：" << tri.getarea() << endl;

    return 0;
}
//rec的面积是：35
//tri的面积是：10
```

## ==C++ 模板==

模板是泛型编程的基础，泛型编程即一种独立于任何特定类型的方式编写代码。

模板是创建泛型类或者函数的蓝图或者公式。库容器，比如迭代器和算法，都是泛型编程。

每个容器都有一个单一的定义，比如**向量**，我们可以定义许多不同类型的向量，比如**vector<int>** 或者 **vector<string>**。

我们可以使用模板来定义函数和类。

==**使用模板的目的就是能够让程序员编写与类型无关的代码。**==比如编写了一个交换两个整型int 类型的swap函数，这个函数就只能实现int 型，对double，字符这些类型无法实现，要实现这些类型的交换就要重新编写另一个swap函数。使用模板的目的就是要让这程序的实现与类型无关，比如一个swap模板函数，即可以实现int 型，又可以实现double型的交换。模板可以应用于函数和类。

#### 函数模板

```C++
template <typename type> ret-type/*返回类型*/ func(parameter list/*参数列表*/) 
```

- teypename type 就是函数所使用的数据类型的占位符，**模板形参**
- 一但声明了模板函数就可以用模板函数的形参名声明类中的成员变量和成员函数，即可以在该函数中使用内置类型的地方都可以使用模板形参名。
- 当调用模板函数且传递实际参数的时候，模板形参就会自动变成实际参数的类型

```C++
#include <iostream>
#include <string>
using namespace std;

template <typename T>
inline T const& Max(T const& a, T const& b)
{
    return a < b ? b:a;
}
int main()
{
    int i = 39;
    int j = 41;
    cout << "max(i, j):" << Max(i, j) << endl;

    double f1 = 3.21;
    double f2 = 4.33;
    cout << "max(f1, f2):" << Max(f1, f2) << endl;

    string s1 = "hello";
    string s2 = "world";
    cout << "max(s1, s2):" << Max(s1, s2) << endl;

    return 0;
}
```

#### 调用规则

- 编译器优先考虑普通函数
- 可以通过空模板实参列表的语法限定编译器只能过模板匹配
- 函数模板可以像普通函数一样被重载

#### 模板的局限性

- 函数模板的具体实现会经过两次编译，第一次对模板进行编译，第二次对替换T类型后的代码进行二次编译。

- 模板不能解决所有的类型
- 如果出现了比如自定义数据类型这种无法自动转换的情况，就需要在使用函数模板时先具体化。

`template<> void func<自定义类型> (参数) {}`

#### 类模板

- 与函数模板相比，类模板可以有默认参数
- 类模板不能进行自动的类型推导，需要声明实际的数据类型
- 类模板中的成员函数，当类型不定为模板时，成员函数不会创建出来，只有对象实例化后，才会根据实际的类型来创建成员函数，检测类型是否匹配。

```C++
#include <iostream>
#include <string>
using namespace std;

template <class T>
class Person
{
public:
	Person();

	T multiply(T& a, T& b);
	T add(T& a, T& b);
};
//类内声明，类外实现
//实现的时候要定义声明符'T'
template <class T>
Person<T>::Person() {}

template <class T>
T Person<T>::multiply(T& a, T& b) {
	return a * b;
}

template <class T>
T Person<T>::add(T& a, T& b) {
	return a + b;
}

void test() {
	//具体化类对象，因为类模板不能自动转换
	Person<int> p;
	int a = 10;
	int b = 33;
	int res = p.add(a, b);
	int ret = p.multiply(a, b);

	cout << res << " " << ret << endl;
}

int main()
{
	test();

	return 0;
}
```

#### 类模板做函数的参数

声明函数时向参数列表传参，可以用类模板来做形参，形如`void func(int a, char b);`

- 三种方法：
  - 最详细，指定参数类型
  - 类构造的参数为模板
  - 类为模板

```C++
#include <iostream>
#include <string>
using namespace std;

//类模板
template <typename T1, class T2>
class Person
{
public:
    Person(T1 a, T2 b) {
        this->name = a;
        this->age = b;
    }

    T1 name;
    T2 age;
};

//1.指定类型
void doWork1(Person<string, int>& p) {
    cout << "姓名是：" << p.name << "年龄是：" << p.age << endl;
}

//2.参数模板化
template <typename T1, class T2>
void doWork2(Person<T1, T2>& p) {
    cout << "姓名是：" << p.name << "年龄是：" << p.age << endl;
}

//3.类模板具体化
template <class T>
void doWork3(T& p) {
    cout << "姓名是：" << p.name << "年龄是：" << p.age << endl;
}

void test() {
    Person<string, int> p1("tom", 15);
    doWork1(p1);
    Person<string, int> p2("jerry", 19);
    doWork2(p2);
    Person<string, int> p3("rock", 21);
    doWork3(p3);
}

int main()
{
    test();

    return 0;
}
```

- 查看类型名称：cout << typeid(T).name() << endl;

#### 类模板的继承

- 当一个类需要继承一个类模板的时候，要确定类模板的声明符'T'；因为子类的构造伴随着父类的构造，而模板类构造时有可能会初始化模板对象，这就需要对象的数据类型来分配内存，因此要确定`<T>`;
- 第二种方式不同写死父类的数据类型，只需要再写一个模板类，设定2个声明符`<T1，T2>`；

如下：

```C++
#include <iostream>
#include <string>
using namespace std;

template <class T> class Human {
public:
    Human();

    T n;
};

template <class T>
Human<T>::Human() {}

//第一种，写死父类的类型
class Man : public Human<int> {
public:
    Man();

    int m;
};
//第二种，创建模板类
template <class T1, class T2>
class Women : public Human<T2> {
public:
    Women() {}

    T1 sum;
};

int main()
{
    Women<int, double> girl;
    girl.n = 2.33;
    cout << girl.n <<endl;

    return 0;
}
```

#### 类模板的类内声明类外实现

```C++
template <class T>
T Person<T>::doWork(T a, T b) {}
```

https://zhuanlan.zhihu.com/p/359339802

#### 类模板的分文件编写问题及解决

- 当讲一个类模板的声明、实现、已经main()函数分开在不同的文件中实现时，编译器是按文件分块编译的，模板函数在具体化之前是不会实现的，只会在运行阶段才去创建，所以引用头文件是无法找到实现的，因此会报错
- 解决方案：模板用.hpp文件进行编写

## ==异常处理==

异常提供了一种转移程序控制权的方式，涉及到三个关键字：**try、catch、throw**。

- **throw:**当问题出现时，程序会抛出一个异常。这是通过使用**throw**关键字来完成的。
- **catch:**在想要处理问题的地方，通过异常处理程序捕获异常。

- **try: try** 块中的代码标识将被激活的特定异常。它后面通常跟着一个或多个 catch 块。

如果有一个块抛出一个异常，捕获异常的方法会使用 **try** 和 **catch** 关键字。try 块中放置可能抛出异常的代码，try 块中的代码被称为保护代码。使用 try/catch 语句的语法如下所示：

```c++
try
{
   // 保护代码
}catch( ExceptionName e1 )
{
   // catch 块
}catch( ExceptionName e2 )
{
   // catch 块
}catch( ExceptionName eN )
{
   // catch 块
}
```

如果 **try** 块在不同的情境下会抛出不同的异常，这个时候可以尝试罗列多个 **catch** 语句，用于捕获不同类型的异常。

### 抛出异常

您可以使用 **throw** 语句在代码块中的任何地方抛出异常。throw 语句的操作数可以是任意的表达式，表达式的结果的类型决定了抛出的异常的类型。

以下是尝试除以零时抛出异常的实例：

```c++
double division(int a, int b)
{
   if( b == 0 )
   {
      throw "Division by zero condition!";
   }
   return (a/b);
}
```

### 捕获异常

**catch** 块跟在 **try** 块后面，用于捕获异常。您可以指定想要捕捉的异常类型，这是由 catch 关键字后的括号内的异常声明决定的。

```c++
try
{
   // 保护代码
}catch( ExceptionName e )	//可以自定义异常类型
{
  // 处理 ExceptionName 异常的代码
}
```

上面的代码会捕获一个类型为 **ExceptionName** 的异常。如果您想让 catch 块能够处理 try 块抛出的任何类型的异常，则必须在异常声明的括号内使用省略号 ...，如下所示：

```c++
try
{
   // 保护代码
}catch(...)
{
  // 能处理任何异常的代码
}
```

下面是一个实例，抛出一个除以零的异常，并在 catch 块中捕获该异常。

```c++
//抛出一个除以零的异常，并在catch块中捕获该异常。
#include <iostream>
using namespace std;

/*
就是在定义一个有可能会出现异常的函数时，加上异常抛出语句throw
if()判断当出现这种情况时，抛出一个异常，异常的类型不定，可以是一句话，一个整型或是浮点型
之后在调用这个函数时，就要放进try{}语句中，称为保护代码，尝试执行它，后面接着catch(...)语句
有throw()就必须有catch()实现，否则会执行terminate()终止函数。catch()处理指定类型的异常，
通常是输出异常，使用 cerr << (...) << endl;
*/
double division(int a, int b)
{
    if (b == 0)
    {
        throw "除数是0";
    }
    return a / b;
}

int main()
{
    int x = 10;
    int y = 0;
    double z = 0;

    try {
        division(x, y);		//关键是里边的throw
    }catch (const char* msg) {
        cerr << msg << endl;
    }

    return 0;
}
```



由于我们抛出了一个类型为 **const char\*** 的异常，因此，当捕获该异常时，我们必须在 catch 块中使用 const char*。当上面的代码被编译和执行时，它会产生下列结果：

```c++
除数是0
```

- 有throw()就必须有catch()实现，否则会执行terminate()终止函数。
