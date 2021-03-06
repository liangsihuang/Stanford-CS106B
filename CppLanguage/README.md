# C++
* [C++程序设计](https://www.bilibili.com/video/av6892144?p=2)

## 函数指针 Function pointer
* 用起来和函数名一样，有什么意义？
* 事先不知道函数名，可以先定义一个函数指针占位
* 比如排序算法的比较函数

## 引用 Reference
* 引用以后，完全等价
* 必须初始化，且初始化后不能改变
* 有什么用？
    * 交换函数：C 必须用指针传递参数实现（ugly），C++ 可以用引用传递参数实现
    * 引用作为函数返回值，用处？以后再说
* 常引用：不能通过常引用去修改引用的变量
### 对比 python
* 有人说：python的参数传递都是引用传递！个人认为：这里的引用指的不是C++的引用
* 一种理解：python中函数的参数传递相当于C++的指针传递
    * 如果指针指向的对象可变（object, list ...），就地操作
    * 如果对象不可变（int...），创建一个新的，让指针指向新的

## Const 关键字
* 定义常量：多用 const，少用 define。因为 const 有类型，便于类型检查。
* 定义常量指针：不能通过常量指针修改其指向的内容，但可以修改常量指针的指向
    * 用处：函数传递参数定义为常量指针，可以避免修改指向地址的内容
* 定义常引用
* python 没有 const

## 动态内存分配 Dynamic 
* 有什么用？？？
    * 指针数组：每个数组元素的大小不同，用指针实现
* 返回类型是指针： T *
* new T : T 是一个类型，比如int
* new T[N]
* 用完必须删除
    * delete p
    * delete [] p : 否则释放不完全

## 内联函数 
* 函数名前面加 inline
* 为了减少调用函数的开销，C++ 效率狂魔
* 弊端：可能增加编译后的代码量，即可执行程序 exe 体积增大！

## 函数重载
* 名字相同，参数表不同
* C 不能用相同的名字，C++ 可以：编译器根据参数的类型和个数匹配
* 使函数名字简单好记
* 注意参数二义性会报错
* python 不需要函数重载
    * 参数的类型不同？python 不管类型
    * 参数的个数不同？用默认参数实现

## 函数的缺省参数
* 只能从最右边连续缺省
* 意义：提高程序可扩充性，少修改代码
* python 一样

## 类的定义
* 定义： 注意要有`分号；`
* `class 类名 {};`
### python
* `class 类型(object):`
* 注意要有`冒号：`
* 一定要填继承的父类，没有就是`object`

## 成员函数在类外定义
* 声明在头文件`class.h`，实现在`class.cpp`
    * 在类内只声明函数：`int 函数名()`
    * 在类外定义，函数名要改为`类名::函数名`
* python 不分开

## 类成员的可访问范围
* private : 缺省则默认 private
* public
* protected ：在继承中讲
### python
* 没有关键字，默认公有
* 私有成员，不论是变量还是方法，在名字前加两个下划线`__`

## 内联成员函数
* inline + 成员函数
* 整个函数体出现在类定义内部（熟悉的方式）

## 构造函数
* 不能有返回值，void也不行
* 构造函数不负责给类里面的成员变量分配空间（造房子），只是用来初始化（装修）
* 没有定义的话，编译器生成一个没有参数的构造函数，啥也不干。
* 生成对象自动调用，意义：初始化工作不怕忘
* 可以重载！
* 对象数组的初始化：`初始化列表`
    * `=`号不是赋值，而是初始化
    ```cpp
    class Test {
        public:
            Test(int n) {}
            Test(int n, int m) {}
            Test() {}
    };
    Test array[3] = {1, Test(1,2)};
    // 分别使用了三个构造函数
    Test * pArray[3] = {new Test(4), new Test(1,2)}; // 指针数组
    ```
### python
* 构造函数定义：`def __init__(self):`
    * 如果不定义，有默认，什么都不干
    * 相当于函数体为 `pass`

## 复制构造函数 copy constructor
* 编译器默认生成
* 只有一个参数：同类对象的引用
```cpp
class Complex {
    public:
        double real, imag;
        Complex() {};
        Complex(const Complex & c) {
            real = c.real;
            imag = c.imag;
        } 
};
Complex c1;
Complex c2(c1); // 等价形式 Complex c2 = c1;  
```
* 起作用的三种情况
    * 初始化 
        * `Complex c2(c1)`或者`Complex c2 = c1`，注意不是赋值语句
        * `c2 = c1`是赋值语句，不是初始化，不存在调用构造函数
    * 对象作为函数参数
    ```cpp
    void Func(A a1) {}
    int main() {
        A a2;
        Func(a2);
        return 0;
    }
    ```
    * 对象作为函数返回值
    ```cpp
    A Func() {
        A b();
        return b;
    }
    int main() {
        cout << Func().v <<end;
        return 0;
    }
    ```
* 自己定义，可以不做复制的工作
    * 可以使形参不等于实参！
    * 也可以使函数返回值不等于你想要的那个！
* 那为什么要自己写复制构造函数？？？
    * 下回分解：
    * 情况1：需要在所有构造函数里增加一个静态成员时

### 类型转换构造函数 type conversion
* 只有一个参数
* 不是复制构造函数
* 编译时自动调用的话，会建立一个临时变量
```cpp
class Complex {
    public:
        double real, imag;
        Complex(int i) { // this is 类型转换构造函数
            real = i;
            imag = 0;
        }
        Complex(double r, double i) {
            real = r; imag = i;
        }
};
int main() {
    Complex c1(7,8);
    Complex c2 = 12; // 人为调用：不会生成临时对象，= 是初始化，不是赋值
    c1 = 9; // 编译器自动调用：9被构造成一个临时的Complex对象，= 是赋值
}
```

## 析构函数 destructor
* ~ 类名()
* 没有参数没有返回值
* 没写，编译器会生成缺省的析构函数，但不会释放 new 生成的空间
* 作用域
    * 跃出作用于，会调用 destructor
### python
* 有垃圾回收机制，自动调用，而且一般不用定义，有默认
* 如果要定义：`def __del__(self):`
* 自己调用，`del object_name`

### 静态成员 static member
* 变量
    * 所有对象共享一个
    * sizeof 不会计算静态成员变量，因为不是放在对象内部
* 函数
    * 并不具体作用于某个对象
    * 类名::成员函数名
    * 对象名.成员函数名
    * 指针 -> 成员函数名
    * 引用.成员函数名
    * 第二三四虽然这样写，并不作用于某个对象
    * 成员函数换成成员变量也适用
    * 本质是全局变量，全局函数，为什么不直接写全局的呢？
        * 目的：将与某些类紧密相关的全局变量和函数写到类里面，看上去是一个整体，便于理解和维护！
        * 比如：矩形类，求举行总数和总面积，其他三角形类不能访问
* 静态成员变量要拿出来单独声明一下，可以不初始化
    * 否则编译能通过，链接不能通过？？
* 注意！在静态成员函数中，不能访问非静态的成员变量，也不能调用非静态成员函数！
    * 因为静态成员函数不是作用在对象上的！

### 成员对象 Member object 和 封闭类
* 包含成员对象的类：封闭类
* `初始化列表`，比赋值更好？？？
    * 在封闭类中使用比较好
    * 类名::构造函数(参数表): 成员变量1(参数表), 成员变量2(参数表), ...
* 封闭类特别注意：构造函数不定义，很大可能报错！
    * 因为默认构造函数不知道如何处理类成员变量

### Friend 友元
* 友元函数：用于别的类的函数 `访问` 该类的私有成员
* 友元类：不能传递，不能继承

## this 指针 this pointer
* C++ 程序的编译：可以完全理解为翻译成 C 程序， 再用 C 的编译器编译
* 作用：
    * 指向成员函数作用的对象
    * 静态函数中不能使用 this 指针，因为静态函数不作用于某个对象
* python 中的 `self`

## const object 
* 初始化后不再改变 ` const Sample o; `
* 常量成员函数 `void GetValue() const`
    * 不能修改其所作用的对象
    * 即不能修改成员变量的值（静态成员变量除外）
    * 也不能调用非常量成员函数（静态成员函数除外）
    * 一个有const ，一个没const，算重载？？？？ 非常量对象不能调用常量成员函数吗？
* 对象的常引用作为参数
    * 引用：效率高，不用复制
    * 保证函数里不会修改引用的对象

### 继承和派生 inherit and dirive
* 一回事
* 派生类拥有基类的全部成员函数和成员变量，不论是 private, protected, public
    * 在派生类的各个成员函数中，不能访问基类的 private 成员
* class 派生类名 : public 基类名 {...}
* 成员函数可以覆盖，参数表和返回类都相同，不是重载
    * 调用基类的同名成员函数，`基类名:: 同名函数名`
### 类与类之间的关系
* 继承关系
    * 派生类
* 复合关系
    * 像封闭类？但又不同。
    * 因为不能循环定义！A类有B，B类有A 是循环定义。
    * 怎么办？相互之间是指针！指针是知道的关系，不是拥有的关系。有自由！

### 基类和派生类有同名成员
* 同名成员变量
    * 不推荐
* 派生类的成员函数可以访问`当前对象`的基类的 `protected` 成员
### 派生类的构造函数
* 和封闭类的`初始化列表`一样，要用`构造函数表`，两者可混合使用
    * 因为基类的 `private` 不能访问


### public 继承的赋值兼容规则
* 派生类的对象可以赋值给基类对象
* 派生类对象可以初始化基类引用
* 派生类对象的地址可以赋值给基类指针

### 直接基类和间接基类
* 声明派生类，只需声明直接基类

### 虚函数和多态
* 函数声明前面加 `virtual` ，写函数体不用加
* 构造函数和静态成员函数不能是虚函数
* 什么是多态？有两种表现形式
    * 派生类的指针可以赋给基类指针
    * 通过基类指针调用基类和派生类中的同名`虚函数`时，
        * 若该指针指向一个基类对象，则调用基类的虚函数
        * 若该指针指向一个派生类对象，则调用派生类的虚函数
    * 派生类的对象可以赋给基类引用
    * 通过基类引用调用基类和派生类中的同名`虚函数`时，
        * 若该引用引用一个基类对象，则调用基类的虚函数
        * 若该引用引用一个派生类对象，则调用派生类的虚函数
* 多态的作用？ 可扩充性
* 用基类指针数组存放指向各种派生类对象的指针，然后遍历该数组，就能对各个派生类对象做各种操作
* 在非构造、非析构函数中，调用虚函数，是多态！
    * 派生类中和基类中虚函数同名同参数表的函数，不加 `virtual` 也自动成为虚函数。
* 在构造、析构函数中，调用虚函数，不能是多态！
    * 因为构造函数调用时，先调用基类的构造函数，此时派生类的变量还没有生成。所以，编译器设计的时候就不考虑多态。

* 纯虚函数：连函数体都没有的。声明时在后面加 `= 0`
    * 如果有函数体，但什么都不做？ `{}` 有区别吗？

### 多态的实现原理-动态联编
* 编译时会给有虚函数的对象增加一个4字节的指针，指向该类的`虚函数表`
* `虚函数表` 存放该类所有的虚函数地址
* 所以多态的关键：
    * 编译时不确定调用的是基类还是派生类的函数
* 所以多态的弊端：
    * 增加运行时的时间和空间开销
    * 时间：查找虚函数表
    * 空间：每一个对象多一个4字节

### 指向指针的指针
* `int * * `
* `某个数据类型 * *`
* `某个class * *`
* `void *` 强制转换为 `int * *`
```cpp
void * s1;
int * * p1;
p1 = (int * *) s1;
```
### 虚析构函数
* 类如果定义了虚函数，最好将析构函数也定义为虚函数
* 基类析构函数为虚函数，派生类的析构函数自动成为虚函数，不用加`virtual`
* 但是构造函数不能是虚函数

### 纯虚函数和虚构类
* 没有函数体的虚函数，后面加 `=0`
* 包含纯虚函数的类变为抽象类
    * 只能作为基类
    * 只能用来派生，没有对象
    * 可以定义抽象类的指针和引用，但是指向派生类的对象
* 有什么用？
    * 方便利用多态
* 抽象类注意：
    * 在成员函数内可以调用`纯虚函数`
    * 在构造函数/析构函数内不能

















