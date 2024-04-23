# OOP及其它C++的特性


## 1. ***何为object？***
---- *在程序设计语言中,object就是variable*
###  *Objects = Attributes + Services*
`Data`: the properties or status<br> 
`Operations`: the functions<br>
OOP的*最基本*的原则: 不能直接操作`attributes`, 而是只能使用`services`.
### *Mapping*
oop和pop的差别在于mapping策略不同,前者涉及对象属性及各属性的不同状态变化,后者涉及逻辑或时间上的先后<br>
举例: 在教室里上课<br>
过程导向: 学生走进教室->上课铃响->老师走进教室->老师打开课件->...<br>
对象导向: 对象为教室;教室中有学生,老师,投影仪等;学生,老师等有自身属性和相互关系...<br>

## 2. ***OOP的基本原理***

### *Objects process messages*
***Messages are***<br>
composed by sender<br>
interpreted by reciever<br>
implemented by methods<br><br>
***Messages***<br>
may cause reciever to change status<br>
may return a val.<br><br>
编译和实现都是在对象内部完成的.理想的对象是不需要程序员去触碰对象内部的.<br>
*(理想的对象使用者也不应该去触碰对象内部,但这是诱惑性的.就好像现代医学会尝试去通过直接电击神经来控制肌肉----但合格oop程序员应该做的是给予参试者指令,让他运动肌肉.)*
### *Classes*
***class,被认为来自Aristotle.oop中的calss定义了某些对象的共同属性,从而得到这些对象的集合.***<br><br>
***oop characteristics***<br>
- Everything is an object.
- A program is a bunch of objects telling each oter what todo by sending messages.
- Eahc object has its own memory made up of other objects.
- Each object has a type.
- All objects of a particular type can receive the same messages.
<br><br>

第一原则是废话;<br>
第二原则说的不是源代码,而是描述程序的运行.重点:oop注重`what`,而不是`how`.(还是那个例子:oop程序员会告诉ta的同事窗户没关,而不是操纵的同事的肌肉.好的对象就是好的'同事':同事自己会关窗,不需要oop程序员教ta走路或者抬手.);<br>
第三原则,对象里面是对象.使用对象,了解到每一个比特(This is what we call `abstract`.);<br>
第四原则,在程序中先要申明class,才能讨论specified object.<br>
第五原则应从正反两个方面理解:所有同class的object,都有相同的status及status和messages的方式;反过来,能进行完全相同操作的object可视为一个class.<br><br>
***An object has `interfaces`***
在class中定义了objects接收messages的方式,称为`interfaces`.
(比如不同的灯泡可能有不同的光强,色温等,但灯泡的螺口是有相同标准的.)<br><br>
***Functions of interface***
- Communication
- Protection<br><br>

***Once again, the inner implementation(and many other things) is hidden.***<br>

## 3. ***The header files***
### ***头文件的作用***
***任何使用类的文件都应该包含声明了该类的头文件***
- 头文件是 `interface`.
- `#include`在编译预处理中被处理.
- ~~~cpp
  #include "xx.h"
  #include <xx.h>
  #include <xx>
  ~~~
  三种都是可以的.

***头文件只包含声明,而不能拥有定义.***<br>
防止在不同的`.cpp`及`.h`中发生重名.保证成员在类对象被构造的时候才被定义.
### ***头文件的标准声明***
`a.h`
~~~cpp
#ifndef _A_H_ //防止头文件在被多次 #include 时被多次声明.
#define _A_H_

class A {
public:
    A();
};

#endif
~~~
`a.cpp`
~~~cpp
#include "a.h"
A::A() {

};
~~~
要求:<br>
- 一个类用一个`.h`文件声明,用同名`.cpp`文件定义.
- `.h`要使用上述标准声明.

## 4. ***成员变量和成员函数***

- 成员变量的生存周期由对象实例被构造到对象实例被析构.<br>
(也就是说成员变量只有在对象实例被构造后才真正存在,在类声明中,成员在类中只是被声明,在实例中才真正存在;即"如果有对象被构造,那么这个对象将有一个成员被定义".)<br>
***只有实例才拥有成员变量,类是虚的.***
- 成员变量能被所有成员函数使用.
- 成员函数是属于类的,不是属于某个对象的.<br>
***成员函数是接口:对于所有对象来说是一样的.***<br>
成员函数被对象a调用时,使用的是对象a的内存.<br>

- `this`指针. 所有的成员函数默认包含的一个本地变量.
~~~C++ 
#include <iostream>
using namespace std;
class A {
public:
    int i;
    void f();
};
void A::f() {
    this->i = 20;
    cout << this->i << endl;
}

int main() {
    A a;
    a.f();//a.f()中 i 的地址就是 a.i 的地址.
}
~~~


## 5. ***构造和析构***

cpp 不会自动清理内存.对象生存周期结束后不会自动清理.<br>
### ***Constructor(构造器/构造函数)***
对象会被自动初始化.如果没有主动定义构造函数, 会默认一个空的构造函数.<br>
调用构造函数的方法如下.一个类可以有多个构造函数,调用时会发生重载.
~~~ cpp
#include <iostream>
#include <stdio.h>
using namespace std;
class A {
public:
    int i;
    A(int k);
    void f();
};

void A::f() {
    cout << this << ' ' << this->i << endl;
}

A::A(int k) {
    this->i = k;
    cout << "constructor is called" << endl;
}

int main() {
    A a(97);
    A b(98);
    A c[2] = {99, 100};
    a.f();
    b.f();
    c[0].f();
    c[1].f();
}
~~~
### ***Destructor(析构器/析构函数)***.
对象生存周期结束之前,析构函数会被调用.<br>
~~~cpp
#include <iostream>
#include <stdio.h>
using namespace std;
class A {
public:
    int i;
    A(int k);
    ~A();
    void f();
};

void A::f() {
    cout << this << ' ' << this->i << endl;
}

A::~A() {
    cout << "destructor!"<< endl;
}

A::A(int k) {
    this->i = k;
    cout << "constructor is called" << endl;
}

int main() {
    {
        A a(97);
        a.f();
    }
    cout << "a's life ended!"<< endl;
}

~~~
析构函数不接收参数(所以不能重载),无返回值.<br>
不主动声明析构函数,会默认一个空的析构函数.<br><br>
### ***空间分配***
从进入环境的时候就发生了,但是要运行到构造函数被调用的那行才构造对象.<br>
~~~cpp
#include <iostream>
#include <stdio.h>
using namespace std;
class A {
public:
    int i;
    A(int k);
    void f();
};

void A::f() {
    cout << this << ' ' << this->i << endl;
}

A::A(int k) {
    this->i = k;
    cout << "constructor is called" << endl;
}

int main() {
//a, b的空间分配
    A a(97);
    a.f();
    A b(98);
//b 被构造   
}
~~~
### ***缺省构造函数*** 
指所有没有参数的构造函数.这有一点迷惑性, 因为英文是"Default Constructor": <br>
默认的构造函数是缺省的, 但是只有你写了一个有参数的constructor, 那么默认的构造函数就是被屏蔽. 除非你写一个缺省构造函数, 不然无法缺省构造.
~~~cpp
class Y{
    int i;
    Y(int a);//没有缺省构造函数
}
int main() {
    Y y();//Error
}
~~~
### ***动态内存分配*** 
涉及`new`和`delete`, 分配动态内存给指针以及清理空间.<br>
`new`分配内存,会构造对象,并在另一块内存中记录下对象的地址和空间.<br>
`delete`会析构对象,清理内存.<br>
`int *p = new int [10]`指分配 10 个 int 的空间(运行十次`int`类的构造函数).这时要使用`delete []p`清理从`*p`开始的数组的空间(首先找到 创建`*p`时`new`的内存记录,然后根据内存记录决定运行`int`类的析构函数的次数).<br>
~~~cpp
#include <iostream>
#include <stdio.h>
using namespace std;
class Y{
public:
    int i;
    ~Y() {
        cout << i << " deconstruct" << endl;
    };
    void set(int k);
};
void Y::set(int k) {
    i = k;
}
int main() {
    Y *y = new Y[10];
    for (int i = 0; i < 10; i++) {
        y[i].set(i);
    }
    delete []y;
}
~~~

> 运行析构是从最后一个对象开始的.

在析构时`delete`指针是一个好习惯.目的是防止内存泄露.

## 6. ***访问控制***
访问属性
- `public`
- `private`
- `protective`
### ***Public 和 Private***
- `public`: 任何`#include`了类文件的文件可访问.
- `private`: 只有类对象能访问.<br>
`private`是对类而言,而不是对某个对象<br>
在下面代码中,`y[0]`可以访问`b`的`private`成员.
~~~cpp
#include <iostream>
#include <stdio.h>
using namespace std;
class Y{
private:
    int i;
public:
    ~Y() {
        cout << i << " deconstruct" << endl;
    };
    void set(int k);
    void g(Y *q) {
        cout << q->i <<endl;
    }
};
void Y::set(int k) {
    i = k;
}
int main() {
    Y *y = new Y[10];
    for (int i = 0; i < 10; i++) {
        y[i].set(i);
    }
    Y *b = new Y;
    b->set(1);
    y[0].g(b);
    delete []y;
}
~~~
### ***Friends***
允许别的函数,类等访问自己的`priva`成员.
friends是不能被传递的.
~~~cpp
struct X;

struct Y {
    void f(X*);
};

struct X {
private:
    int i;
public:
    void initialize();
    friend void g(X*, int);//Global friend.
    friend void Y::f(X*);//Class member friend.
    friend class Z;//Entire class friend.
};
~~~
>注:`struct`中默认`public`,`class`中默认`private`.
### ***初始化列表***
可进行列表初始化方式.
~~~cpp
#include <iostream>
using namespace std;
 class A {
    int x, y;
public:
    A(int i, int j):x(i), y(j) {
        x = j;
        y = i;
        cout << i <<' '<< j << endl;
    }
};

int main() {
    A a{1, 2};
}
~~~
初始化发生在运行构造函数中的语句.<br>
建议:用初始化列表来初始化类.不用构造函数中的赋值.<br>

## 7. ***Inheritance(继承)***
继承是 oop 完成软件重用的方式.
### ***另一种软件重用 ----Composition(组合)***
将别的对象当作某个对象的成员.
- Fully: 别的对象直接是对象的一部分.被组合的对象仍然保有对象的独立性.
- By reference: 只是能通过引用或者指针调用另一个对象.
~~~cpp
class Person{};
class Currency{};
class Saving_Account {
public:
    Saving_Account();
    ~Saving_Account();
private:
    Person saver;//Fully Composed.
    Currency balance;//Fully Composed.
    Saving_Account *friend;//By reference. 
};
~~~

>对象指针的类型是`pointer`而非对象本身. 

- fully composed 对象应在初始化列表中初始化.否则将会被视为缺损构造,而被组合的对象没有缺损构造函数的话就会报错.
~~~cpp
class A {
private:
    int i;
public:
    A(int i);
}
class B {
public: 
    A a;
    B(int i);
};
B::B(int i) {
    a.i = i;
}
//error, class A 没有缺损构造函数, B 中的 a 无法初始化.
B::B(int i): a(i) {
    cout << a.i;
}
// error,类型 B 下无法访问 a (类型 A 对象)的 private 成员 a.i .
~~~
### ***何为Interface?***
类中所有 public 的部分,可以接入类的部分(包括成员对象和成员函数).
### ***何为继承?***
一种将一个类的行为和实现作为另一个类的*超集*的能力
### ***类的关系***
- eg. Student{Person}
- Student 具有 Person 的所有属性和功能,并且拥有更多的属性和功能.<br>
所以说 Student `继承`了 Person 的所有内容.<br>
Person 是 Based class, Super class, Parent class;<br>
Student 是 Derived class, Sub class, Child class.
- 通过public方式继承的子类可以访问父类的`protected`成员. 但不能访问`private`成员
- 子类的构造函数会默认调用父类的构造函数. 所以必须小心父类的构造函数需要的参数, 如果要向父类构造函数传参, 必须写在子类构造函数的初始化列表中.
- 先构造父类再构造子类, 先析构子类, 再析构父类. 
- 初始化列表的顺序是先初始化声明早的, 再初始化声明玩的. 所以父类会被先初始化.
### ***重载和默认参数***
- 默认参数是编译过程中被绑定的, 所以默认参数必须在原型中声明.
- 如果父类和子类中存在参数列表相同的同名成员函数时, 父类中的成员函数会被覆盖. 也就是说在子类中可以修改父类的成员函数.
- 也就是说缺省访问成员优先访问子类成员. 访问父类的成员要使用`"父类"::`.
### ***如何使用?***
~~~cpp
#include <iostream>
using namespace std;
class A {
    int i;
protected:
    int j;
public:
    A(int jj):i(0), j(jj) {cout << "A::A()"<< endl;}
    ~A() {cout << "A::~A()"<<endl;}
    void print() {cout << this->i << endl;}
    void set(int ii) {this->i = ii;}
};

class B : public A {
public:
    B(int jj): A(jj) {
        cout << "B::B()" << endl;
    };
    void f(int ii) {
        this->set(ii);
        this->print();
    }
    void print_j() {
        cout << this->j;
    }//子类可以访问父类的protected成员.
    void print_i() {
        cout << this->i;
    }//子类无法访问父类private成员.
    ~B() {
        cout << "B::~B()" << endl;
    }
};

int main( )
{
    B b(19);
    b.set(10);//A类的功能.
    b.print();//A类的功能.
    b.f(20);//B类比A类多的功能.
    cout << b.j; //用户不能访问protected成员.
}
~~~
*result*: 
~~~
A::A()
B::B()
10
20
B::~B()
A::~A()
~~~


## 8. ***Inline Functions , Const和 References***
### 内联函数
- 用于节省空间.
- 在声明和定义时必须重复'inline'关键字. 
- 也就是说, 内联函数的body必须也放在`.h`文件中. 也就是原型声明和定义要包括在一个`.h`文件中.
- 从汇编上看, 内联函数不会被定义为一个独立的函数. 内联函数只有声明.
- 内联函数是用空间换时间.
- 与宏相比的优势是, 内联函数会做类型检查.
- 如果内联函数过于巨大, 比如使用了多重循环或者递归, 编译器会拒绝. 有时候内联函数比较小, 会默认为内联函数.
- 类的成员函数的body如果直接写在声明之后(在类声明的body内), 则默认为内联函数. 或者在`.h`中写在类的body外, 此时需要关键字`inline`
### 常量
- `const`是变量不是真正的常数, 被存放在内存中.
- `const`变量必须被初始化. 除非使用`extern`声明.
- `extern const int bufsize`意味着一个全局变量`bufsize`被当作常量引入, 在这个域内无法改变`bufsize`, 但不影响其他部分.
- `char * const q`, q is const.q指的地址不能改变, 但能改变地址上的东西.<br>
`const char* q`或`char const* q`, (编译器认为)q指的地址上放了一个const(也就是不一定真的指向const), 不能通过q来改变地址上存的东西.
- const的地址不能被赋给非常量指针, 但是能将非常量的地址赋给常量指针.
- 类似`"hello world"`的`string`默认是常量, 赋给`char *s1`, `s1`默认是常量指针. <br>
但是`char s2[] = "hello world"`实际上是copy字符串用于初始化`s2`, 而不是直接将地址赋给`s2`.
~~~cpp
    char *s1 = "hello world";
    char s2[] = "hello world";
    cout << s1 << endl;
    cout << s2 << endl;//s1和s2地址不同
    return 0;
~~~
- 以const返回, 做右值则只能赋给常量. 按const指针/引用传入, 可以在减少copy用时的同时防止函数修改变量.
### 常量对象
- 对象实例是const, 则只有const函数能被调用. const函数不能进行任何成员变量. const在原型和定义都需要加const关键字.<br>
- const函数意味着`this`是常量指针, 也是指针常量.
- const可以引发重载. 因为成员函数默认额外含有参数`this`, 而const函数默认参数`const ClassName* this`.
~~~cpp
    class A {
        const int i;
    public: 
            A(int k): i(k){};//有成员变量是const, 只能通过初始化列表初始化.
            void f() {
                //默认参数A* this
                cout << "f()" << endl;
            };
            void f() const {
                //默认参数const A* this
                // i++; 不能修改成员
                cout << "const f()" << endl;};
    }
    int main() {
        const A a(1);
        a.f();//重载, 调用f() const
        return 0;
    }
~~~
### References 
- 一般引用需要显式初始化, 且要用左值初始化. 只有参数列表中才不需要初始化(显然是在传参的时候初始化了).
- 引用就是别名, 使用引用就是使用被引用的对象. 从字面上来说, 被引用的对象捆绑在引用上, 且不能解绑(所以没有引用常量, 只有常量引用).
- `const void& x = y`表明不能通过x改变y, 但是改变y则x也随着y改变.
~~~cpp
    int* f(int* x) {
        (*x)++
        return x;
    }
    int& g(int& x) {
        x++;
        return x;
    }

    int x;
    int *y = &x;

    int& h() {
        int q;
        // return q; Error! 类似不能返回本地变量地址, 一样不能返回本地变量的引用.
        return x;//直接返回对于x的引用
    }
    int* l() {
        return y;//直接返回对于指向x的指针
    };

    int main() {
        int a = 0;
        // f(&a) ugly
        g(a);// pretty
        h() = 16;
        cout << "x=" << x << endl;
        *(l()) = 17;
        cout << "x=" << x << endl;
    }
~~~
- 引用的实现方式是常量指针, 但使用方式更类似宏(除了宏是直接inline这一点之外), 理解时直接用被引用对象替换引用就行.
- 不允许有指向引用的指针(`int&* p` is illegal), 但可以有对指针的引用(`int*& p`). 
- 不存在引用数组.


## 9. ***Casting and polymorphism***
### ***造型***
- 允许用派生类指针指向基类对象, 向下造型; 允许用基类指针指向派生类对象, 向上造型.(引用同理)
- 类型转换会丢失数据, 比如将`double`转换为`int`会丢失32位数据. 构型不影响地址上存储的内容, 只是影响指针.
### ***多态***
- `virtual`关键字将成员函数声明为虚函数, 并将所有子类的同名函数声明为虚函数.
- 发生向上构型时, 调用虚函数不由指针类型决定, 而是由指针指向的对象类型决定.(引用同理)
- 补充: 指针同时有动态绑定和静态绑定两种绑定方式. 静态绑定就是指针的类型, 在编译时就被确定. 动态绑定是指指针对象的类型, 在运行时才确定. 虚函数的调用是依靠动态绑定实现的.
- 有虚函数的类对象的内存会比一般的对象大一点, 因为这个对象包括了一个指针`vtable`, 这个指针指向一个记录了所有虚函数的表. <br>这个指针是属于对象实例的, 但这个表是属于类的.(符合"成员属于对象, 函数属于类"这一原理)
- 如果用派生类对象直接赋值给基类对象, 没有发生造型, 故不会发生多态.
- 很抽象的一点: 可以改变对象的`vtable`指向的部分.(邪恶的代码)
~~~cpp
#include <iostream>
using namespace std;
struct A{
    int i;
    A():i(10){};
    virtual void f() {
        cout << "A::" << i << endl;
    }
};
struct B: public A {
    int j;
    B():j(20){};
    virtual void f(){
        cout << "B::" << j << endl;
    }
};

int main() {
    A a;
    B b;
    a.f();
    b.f();
    int * a_ = (int*) (&a);//得到a的vtable
    int * b_ = (int*) (&b);//得到b的
    cout << "A::a vtable "<< *a_ << endl;
    cout << "B::b vtable "<< *b_ << endl;
    A* ptr = &b;
    ptr->f();
    *a_ = *b_;//将b的vtable赋给a的vtable
    ptr = &a;
    ptr->f();//虚函数是b的; j成员寻址将会是错误的(这符合a_是属于a的指针, a_之后的空间不包括j)
    cout << *a_ << " " << *(++a_) << " " << *(++a_) << " " << *(++a_) << " " << a_ << endl;
    cout << *b_ << " " << *(++b_) << " " << *(++b_) << " " << *(++b_)<< " " << b_ << endl;
}
~~~
- 析构函数一般要声明为虚函数, 否则在析构时很可能只是调用了基类的析构.
- 构造函数不能是虚函数.
- 虚函数的返回值必须相同. 但是返回不同的指针和引用是可行的, 因为允许造型.
~~~cpp
struct A{
    virtual A* f();
    virtual int g();
    virtual A h();
};
struct B: public A {

    virtual B* f();//向下构型
    virtual int g();
    virtual B h();//wrong!
};
~~~
- 重载的虚函数必须在派生类中被分别定义. 不能只重写一个而不重写另一个.
~~~cpp
struct A{
    virtual A* f();
    virtual A* f(int);
};
struct B: public A {
    virtual A* f();
    virtual A* f(int);//必须两个都重写!
};
~~~

## 10. ***Static***
- `Cpp`语言中`static`有三重可能的含义: 持久储存的变量; 静态对象(共享对象); 静态成员(类共享的静态成员变量, 只能调用静态成员的静态成员函数). 
- 静态成员变量在类的所有变量中保持一致. 在编译时静态成员变量只是被声明, 要在运行才被定义. 只有在定义时被初始化. (类似于C语言中的`extern`)
- 在初始化列表中不能初始化静态成员. 因为静态成员只能在定义时被唯一一次初始化.
- `this`指针可以访问所有成员变量, 包括静态和非静态.
- 所有成员函数是属于类的. 但是静态成员函数的特殊性在于其没有隐藏的参数`this`指针. 所以静态成员函数能在没有对象时被调用, 在内部只能调用静态成员.

## 11. ***Overload***
- 不能重载的运算符: `.`, `.*`, `::`, `?:`(三目运算符), 三种`cast`(强制类型转换). 其他都行, 包括`delete`, `new`, `->`这些.
- 重载的限制: 只能重载给定的一些运算符, `Cpp`中不能自创运算符; 只能在自定义的类或者全局中重载运算符; 不能改变操作数和运算符优先级.

## 12. ***String***
- 无法用字符或者整形初始化`string`对象; 但是能用字符给`string`对象赋值.
- `length()` 或者 `size()`成员函数可以返回长度.
- 支持流读取运算符, 支持`getline()`.
- `s.assign(s_, 1, 3)`将`s_`下标`1`开始的三个元素复制给`s`.
- `+` `+=`可以连接两个字符串; `s.append(s_, 3, 4)`将s_下标3开始的4个字符连接到之后.
- `==` `>` `<` `!=` `<=` `>=`可以使用.
- `s1.compare(1,2,s2,0,3)`, 比较是s1从下标开始两个字符和s2下标0开始3个字符. 若相等返回0, 若s1大返回1, 若s2大返回2. 
- `s.substr(1, 3)`返回下标1开始3个字符或者到末尾.**常用**
- `s.find("lo")`从前向后查找"lo"第一次出现的位置并返回, 否则返回常量`string::npos`; `rfind()`反向查找; `find(1, "lo")`从下标1开始查找.
- `s.find_first_of("abcd")` 查找"abcd"中任何一个字符出现的位置; 同理有`find_no_first_of`, 第一个不是给定字母中的字母出现的位置; 有`last`的版本.
- `s.erase(1)`删除下标1及其之后的字符;  `s.replace(2, 3, "haha", 1, 2)`下标为2的三个字符串替换为"haha"中下标1开始的两个字符; `s.insert(2, s_, 3, 5)`s_下标为3开始的5个字符插入到s的下标2的位置. 
- `s.c_str()`返回传统的以`\0`结束的`const char*`字符串; `s.data()`返回可变得`char*`.

## 13. ***STL***
### ***概述***
- 顺序容器包括vector, deque, list
    - `vector`动态数组, 元素内存连续存放, 在尾部增删元素性能较佳. 往往会分配比元素更多的空间, 往往不需要重新分配空间. 
    - `deque`在两端增删元素都有较佳性能. 有队头和队尾两个指针, 所以是双端队列. 如果`tail`超出尾部, 会跳到头部, 使用头部的内存.
    - `list`双向链表. 不支持随机访问, 只能从头部开始一个一个往后走.
- 关联容器: 插入元素会发生排序, 关联容器的查找有较佳性能.
- 容器适配器: 包括stack(lifo), queue(fifo), priority_queue(优先级最高的先出列).
- 共享的成员函数: `begin` `end` `rbegin` `rend` `erase` `clear`;<br>
顺序容器: `front` `back`(都是返回引用而不是迭代器) `push_back` `pop_back` `erase`(返回被删除元素后面那个元素的迭代器).
### ***迭代器***
- 用于指向顺序容器和关联容器的元素.
- `类名::iterator 变量名`或者`类名::const_iterator 变量名`; `*`用来获得迭代器指向的元素. (支持反向迭代器`类名::reverse_iterator`)
- 双向迭代器支持`++` `--` `=` `==` `!=`; 随机访问迭代器支持初上述外`+=` `-=` `[]` `+` `-` `<`(等比较运算符).
- vector和deque是随机访问的, 其他的是双向的. 适配器不支持迭代.
- `find(InIt first, InIt last, aim)`在[first, last)中查找, 返回迭代器; 如果没有找到, 返回last迭代器.
### ***大小和排序***
- pair:
~~~ cpp
template<class _T1, class _T2>
struct pair {
    typedef _T1 first_type;
    typedef _T2 secont_type;
    _T1 first;
    _T2 second;
    pair():first(), second() {}
    pair(const _T1& __a, const _T2& __b): first(__a), second(__b) {}
    template<class _U1, class _U2>
    pair(const pair<_U1, _U2>& __p): first(__p.first), second(__p.second){}
};
~~~
- 在缺省情况下, 以下说法等价:
    - `x < y`表达式为真
    - x小于y
    - y大于x
- 在缺省情况下, 相等就是`x==y`表达式为真, 有的时候相等指'x小于y和y小于x同时为假'(比如binary_search中). 
### ***vector, deque***
- `s.insert(s.begin()+1, obj)`; `s.insert(s.begin()+1, s_.begin(), s_.begin()+2)`; `s.erase(s.begin()+1)`; `s.erase(s.begin+1, s.begin()+2)`.
- 可以实现多维数组. 举例: `vector< vector<int> > v(3)` v有三个元素, 每个元素都是`vector<int>`的数组; `deque< deque<int> > d`.
- deque的特殊在于有`push_front` `pop_front`函数.
### ***list***
- `push_front`, `pop_front`, `sort`(不支持stl的标准sort, 要用list自身的sort), `remove`, `unique`(删除和前一个元素相等的元素, 要做到清楚重复元素, 必须先sort), `merge`(会清空被合并的列表), `reverse`, `splice`(在指定位置插入另一链表中的一个或者多个元素, 并删除被插入的元素). 
- `splice`举例: `lst1.splice(p1, lst2, p2, p3)` p1是lst1的迭代器, p2和p3是lst2的迭代起, 将[p2, p3)中的内容插入到lst1的p1, 并从lst2中删除[p2, p3)的部分.
### ***函数对象***
- 一个类如果重载了`()`运算符, 它的对象实例就成了函数对象.
- Accumulate: `BinaryOperator`可以是一个函数, 也可以是一个函数指针, 也可以是一个函数成员.
~~~cpp
template <class InputIt，class T，class BinaryOperation>
T accumulate（InputIt first，InputIt last，T init，BinaryOperation op）
{
    for (; first！ = last; ++first) { 
        init = op（init， * first）; 
    }
    return init;    
}
~~~
- `greater`是函数对象类模板.
~~~cpp
template<class T>
struct greater: public binary_function<T, T, bool> {
    bool operator()(const T& x, const T& y) const {
        return x > y;
    }
};
~~~
list有两个sort成员函数:
~~~cpp
void sort();//按照'<'排序
template<class Compare>
void sort(Compare op);//op(x, y)返回真则为顺序(不改变顺序), 返回假为逆序(要改变顺序).
~~~
### ***set和multiset***
- multiset
~~~cpp
template<class Key, class Pred = less<K>, class A = allocator<Key> >
class multiset{...}
template less: public binary_function<T, T, bool> {
    bool operator()(const T& x, const T& y){return x < y;} const;
}
~~~
其中元素按照`Pred(x, y)`为真排序. 相等就是`Pred(x, y)` `Pred(y, x)`均为假.<br>
`iterator find(const T& val)`查找某个元素.<br>
`iterator insert(const T& val)`;`iterator insert(const T& first, const T& last)`.<br>
`int count(const T& val)`返回和val相等的元素个数.
`interator lower_bound(const T & val)`返回最大的迭代器It, 使[begin(), It)都比val小; 同理有`upper_bound`; `equal_range`返回一个`pair(iterator, interator)`的对象, first是lower_bound, last是upper_bound.<br>
- set很类似, 差别在于不能有重复元素. 重复即相等, 也就是`Pred(x, y)` `Pred(y, x)`都为假. `insert`会返回一个`pair<T, bool>`的对象, first是尝试插入的对象, second是表明是否成功的bool值. 
### map和multimap
- multimap
~~~cpp
template<class Key, class T, class Pred = less<Key>, class A = allocator<T> >
class multimap {
    typedef pair<const Key, T> value_type;
    ...
}
~~~
- map和mutimap类似, 只是Key不能重复. `insert`返回值和set类似.map可以用`[Key]`访问Key对应的值(和dictionary类似).
### 容器适配器
- 没有iterator, 只能用内置函数访问.
- stack: LIFO, 各种操作只能在栈顶进行. 可以用vector, deque, list来实现. 默认是deque.<br>
有`pop` `push` `top`(返回引用)三种基本操作.
- queue: FIFO, 默认是deque实现. 同样有`push`, `pop`, `top`, 特殊的是`back`.
- priority_queue: 有第三个类型参数比较器, 默认是`less<T>`. 保证优先级最高的元素(一个x, 满足所有的y, `Compare(x, y)`为假)总在队头.
