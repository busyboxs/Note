# C++ 学习笔记（类）

## 类和对象

### 类中如何定义编译时常量表达式

```cpp
class X {
    constexpr int c1 = 42; // preferred
    static const int c2 = 7;
    enum { c3 = 19 };

    array<char,c1> v1;
    array<char,c2> v2;
    array<char,c3> v3;

    // ...
};
```

### 空类的大小不为 0

为什么空类的大小不能为 0 呢？为了保证两个类对象拥有不同的地址。

```cpp
class Empty { };

void f()
{
    Empty a, b;
    if (&a == &b) cout << "impossible: report error to compiler supplier";

    Empty* p1 = new Empty;
    Empty* p2 = new Empty;
    if (p1 == p2) cout << "impossible: report error to compiler supplier";
} 
```

当空类作为基类时，派生类定义对象时基类的大小被认为是 0 [Empty base optimization](https://en.cppreference.com/w/cpp/language/ebo)

例如下面的例子中 `Base` 的大小为 `1`，`Derived1` 的大小为 `sizeof(int)`

```cpp
#include <cassert>
 
struct Base {}; // empty class
 
struct Derived1 : Base {
    int i;
};
 
int main()
{
    // the size of any object of empty class type is at least 1
    assert(sizeof(Base) >= 1);
 
    // empty base optimization applies
    assert(sizeof(Derived1) == sizeof(int));
}
```

## 构造函数 (ctor)

### 初始化

在构造函数中对成员变量进行初始化时最好是使用 `initialization lists` 而不是 `assignment`。因为前者不会产生临时对象，而后者会长生临时对象。

非静态 const 成员和非静态引用数据成员只能够通过初始化列表进行初始化，而不能通过赋值初始化。因为赋值初始化实际的操作是先初始化，然后再重新赋值。而 const 成员和引用一旦初始化之后便不能在进行修改，所以只能通过初始化列表进行初始化。

### 使用初始化列表初始化的时候，初始化的顺序

先是基类，从左到右，然后是成员变量，从上往下。

### Named Constructor Idiom

`Named Constructor Idiom` 可以用来解决一个类的构造函数具有相同的类型，但是想要表达不同的意义。例如一个点的类

```cpp
class Point
{
public:
  Point(float x, float y);     // Rectangular coordinates
  Point(float r, float a);     // Polar coordinates (radius and angle)
  // ERROR: Overload is Ambiguous: Point::Point(float,float)
};

int main()
{
  Point p = Point(5.7, 1.2);   // Ambiguous: Which coordinate system?
  // ...
}
```

`Named Constructor Idiom` 的构造函数定义为 `private` 或者 `protected`，当这个类是基类的时候需要指定为 `protected`，然后提供 `public static` 的方法返回对象。这些静态的方法就被称作 `Named Constructors`。

```cpp
#include <cmath>               // To get std::sin() and std::cos()

class Point
{
public:
  static Point rectangular(float x, float y);      // Rectangular coord's
  static Point polar(float radius, float angle);   // Polar coordinates
  // These static methods are the so-called "named constructors"
  // ...
private:
  Point(float x, float y);     // Rectangular coordinates
  float x_, y_;
};

inline Point::Point(float x, float y)
  : x_(x), y_(y) 
{
}

inline Point Point::rectangular(float x, float y)
{ 
    return Point(x, y); 
}

inline Point Point::polar(float radius, float angle)
{ 
    return Point(radius*std::cos(angle), radius*std::sin(angle));
}
```

### Retur-by-value

[Does return-by-value mean extra copies and extra overhead?](https://isocpp.org/wiki/faq/ctors#return-by-value-optimization)

由于编译器的优化，通过值返回不会造成额外的拷贝。

```cpp
class Foo { /*...*/ };

Foo rbv();

void caller()
{
  Foo x = rbv();  // The return-value of rbv() goes into x
  // ...
}
```

以上的代码被编译器处理后可能的伪代码为

```cpp
// Pseudo-code
void rbv(void* put_result_here)  // Original C++ code: Foo rbv()
{
  // ...code that initializes (not assigns to) the variable pointed to by put_result_here
}

// Pseudo-code
void caller()
{
  // Original C++ code: Foo x = rbv()
  struct Foo x;  // Note: x does not get initialized prior to calling rbv()
  rbv(&x);       // Note: rbv() initializes a local variable defined in caller()
  // ...
}
```

当调用以下构造函数时

```cpp
Foo rbv()
{
  // ...
  return Foo(42, 73);  // Suppose Foo has a ctor Foo::Foo(int a, int b)
}
```

编译器可能的优化为：

```cpp
// Pseudo-code
void Foo_ctor(Foo* this, int a, int b)  // Original C++ code: Foo::Foo(int a, int b)
{
  // ...
}

// Pseudo-code
void rbv(void* put_result_here)  // Original C++ code: Foo rbv()
{
  // ...
  Foo_ctor((Foo*)put_result_here, 42, 73);  // Original C++ code: return Foo(42,73);
  return;
}
```

## 析构函数（dtor）

析构函数不会被显示调用，其不能有参数。

### 析构函数执行顺序

与构造函数相反，先构造的后析构。

## 继承

### 接口

类没有数据成员并且所有函数成员都是纯虚函数被称作接口(Interface)

### 虚函数

原理：带有虚函数类对象会为每个对象存储一个虚指针，每个类都会有一个虚表用于存储该类的虚函数地址，在对象调用函数时则是通过虚指针获取到对应虚表中的函数。

例如对于含有 5 个虚函数的类：

```cpp
// Your original C++ source code

class Base {
public:
  virtual arbitrary_return_type virt0( /*...arbitrary params...*/ );
  virtual arbitrary_return_type virt1( /*...arbitrary params...*/ );
  virtual arbitrary_return_type virt2( /*...arbitrary params...*/ );
  virtual arbitrary_return_type virt3( /*...arbitrary params...*/ );
  virtual arbitrary_return_type virt4( /*...arbitrary params...*/ );
  // ...
};
```

编译器在处理时会生成一个虚表：

```cpp
// Pseudo-code (not C++, not C) for a static table defined within file Base.cpp

// Pretend FunctionPtr is a generic pointer to a generic member function
// (Remember: this is pseudo-code, not C++ code)
FunctionPtr Base::__vtable[5] = {
  &Base::virt0, &Base::virt1, &Base::virt2, &Base::virt3, &Base::virt4
};
```

同时会在类中创建一个虚指针

```cpp
// Your original C++ source code

class Base {
public:
  // ...
  FunctionPtr* __vptr;  // Supplied by the compiler, hidden from the programmer
  // ...
};
```

并且会在构造函数中初始化虚指针

```cpp
Base::Base( /*...arbitrary params...*/ )
  : __vptr(&Base::__vtable[0])  // Supplied by the compiler, hidden from the programmer
  // ...
{
  // ...
}
```

对于继承类，则会替换虚表中的继承函数为自己的。例如一个继承了 `Base` 的 `virt0`, `virt1` 和 `virt2` 的继承类的虚表为

```cpp
// Pseudo-code (not C++, not C) for a static table defined within file Der.cpp

// Pretend FunctionPtr is a generic pointer to a generic member function
// (Remember: this is pseudo-code, not C++ code)
FunctionPtr Der::__vtable[5] = {
  &Der::virt0, &Der::virt1, &Der::virt2, &Base::virt3, &Base::virt4
                                          ↑↑↑↑          ↑↑↑↑ // Inherited as-is
};
```

虚函数与非虚函数比较：

* 虚函数需要额外的空间消耗用于存储虚指针和虚表；
* 调用虚函数很快，和调用非虚函数一样快；

### 什么时候需要使用虚析构函数

当想要通过基类指针删除继承类对象时。通常情况下，类不会被当做基类，所以默认情况下析构函数不是虚的。

### 不能讲继承类数组赋值给基类数组作为参数

当基类和继承类的大小不同时，将继承类数组作为实参传给基类数组形参，会出现问题。因为使用基类数组作为参数的函数在调用数组中某一个值时会涉及到指针的偏移，而这个偏移的大小和具体的类型大小有关，如果是基类就是 `n * sizeof(Base)`，所以当传入继承类数组作为参数时，可能获取不到正确的指针。

```cpp
class Base
{
public:
  virtual void f();             // 1
};

class Derived : public Base
{
public:
  // ...
private:
  int i_;                       // 2
};

void userCode(Base* arrayOfBase)
{
  arrayOfBase[1].f();           // 3
}

int main()
{
  Derived arrayOfDerived[10];   // 4
  userCode(arrayOfDerived);     // 5
  // ...
}
```

