# C++ 学习笔记

----

## Const 说明

### const 作为参数

const 用于保证 const 指定的对象不能被改变。

```cpp
void f1(const std::string& s);     // Pass by reference-to-const
void f2(const std::string* sptr);  // Pass by pointer-to-const
void f3(std::string s);            // Pass by value

void g1(std::string& s);           // Pass by reference-to-non-const
void g2(std::string* sptr);        // Pass by pointer-to-non-const
```

`f1`, `f2` 函数中，传入的参数不能被改变，如果函数中有对参数做修改，则会在编译的时候出错；`f3` 函数传值，会创建临时变量，对实参不会有影响；当需要对传入参数做改变时，可以使用 `g1` 和 `g2` 的参数传入方式。

对于 const 的错误检查是在编译时进行的，因此 const 不会使你的程序运行速度减慢，也不需要额外编写测试用例在运行时进行检测。

### const 变量的解读方法

从右往左读

* `const X* p` : `p` points to `X` that is `const`, the `X` object can't be changed via `p`
* `X* const p` : `p` is a `const` pointer to an `X` that is `non-const`, you can't change the pointer p itself, but you can change the `X` object via `p`
* `const X* const p` : `p` is a `const` pointer to an `X` that is const, you can't change the pointer `p` itself, nor can you change the `X` object via `p`

## Reference 引用

C++ 中引入引用的主要原因是为了支持运算符的重载。例如

```cpp
void f1(const complex* x, const complex* y) // without references
{
    complex z = *x+*y;  // ugly
    // ...
}
void f2(const complex& x, const complex& y) // with references
{
    complex z = x+y;    // better
    // ...
} 
```

### 什么是引用

引用是一个对象的别名，引用是对象的机器地址，当程序员看到 `i++` 是，编译器生成的代码是增加 `x`。

```cpp
void swap(int& i, int& j)
{
  int tmp = i;
  i = j;
  j = tmp;
}
int main()
{
  int x, y;
  // ...
  swap(x,y);
  // ...
}
```

## 返回参数为引用

在某些情况下，函数的返回参数可以是引用，因此出现 `f() = 8;` 也是正常的现象，但是可能看起来有点奇怪。很多运算符的实现就是返回的引用，例如 `operator[]` 和 `operator<<`。

对于返回引用对象的函数可以使用链式方法（method chaining），所以可以看到 `obj.method1().method2()` 这种类型的用法。例如 `cout` 的 `<<` 就属于这种用法。

## 内存管理

### Named Constructor Idiom

With the Named Constructor Idiom, you declare all the class’s constructors in the `private` or `protected` sections, and you provide `public static` methods that return an object. These `static` methods are the so-called “Named Constructors.” In general there is one such static method for each different way to construct an object.

```cpp
#include <cmath>               // To get std::sin() and std::cos()
class Point {
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
  : x_(x), y_(y) { }
inline Point Point::rectangular(float x, float y)
{ return Point(x, y); }
inline Point Point::polar(float radius, float angle)
{ return Point(radius*std::cos(angle), radius*std::sin(angle)); }
```

如果想要 `Point` 类有派生类，则需要将 `Point` 的默认构造函数设置为 `protected`。

### delete pointer

对于同一个指针执行两次 `delete` 是不安全的

```cpp
class Foo { /*...*/ };
void yourCode()
{
  Foo* p = new Foo();
  delete p;
  delete p;  // DISASTER!
  // ...
}
```

第二次 `delete p` 可能会产生一些不可预测的问题，例如：破坏堆数据、使程序崩溃、使堆栈中的对象数据改变等。

删除指针时不需要判断指针为空，当指针为空时，`delete p` 不会做任何事。

```cpp
// wrong
if (p != nullptr)   // or just "if (p)"
  delete p;

// right
delete p; 
```

### new 和 delete 操作

`new` 和 `delete` 实际上都是执行的两步操作：

* `new`: 分配内存 + 执行对象的构造函数
* `delete`: 执行对象的析构函数 + 释放内存

在使用 `new` 创建对象时，如果内存不足，会抛出异常。抛出异常时会进行分配内存的释放，所以不会出现内存泄漏的问题。

```cpp
// Original code: Fred* p = new Fred();
Fred* p;
void* tmp = operator new(sizeof(Fred));
try {
  new(tmp) Fred();  // Placement new
  p = (Fred*)tmp;   // The pointer is assigned only if the ctor succeeds
}
catch (...) {
  operator delete(tmp);  // Deallocate the memory
  throw;                 // Re-throw the exception
}
```

```cpp
// Original code: delete p;
if (p) {    // or "if (p != nullptr)"
  p->~Fred();
  operator delete(p);
}
```

### 数组指针删除

在删除数组指针时，必须使用 `delete[] p`。

那么对于数组，编译器是如何知道删除数组指针时应该删除 `n` 个呢？

运行时系统将对象的数量 `n` 存储在只知道指针 `p` 也能被检索出的地方。有两种技术来实现。

* [Over-allocate the array and put n just to the left of the first Fred object](https://isocpp.org/wiki/faq/compiler-dependencies#num-elems-in-new-array-overalloc).
* [Use an associative array with p as the key and n as the value](https://isocpp.org/wiki/faq/compiler-dependencies#num-elems-in-new-array-assocarray).

第一种是在创建对象申请内存的阶段会多申请一个 `WORDSIZE` 的内存，然后这个内存中存的值就是 `n`。

```cpp
// Original code: Fred* p = new Fred[n];
char* tmp = (char*) operator new[] (WORDSIZE + n * sizeof(Fred));
Fred* p = (Fred*) (tmp + WORDSIZE);
*(size_t*)tmp = n;
size_t i;
try {
  for (i = 0; i < n; ++i)
    new(p + i) Fred();           // Placement new
}
catch (...) {
  while (i-- != 0)
    (p + i)->~Fred();            // Explicit call to the destructor
  operator delete[] ((char*)p - WORDSIZE);
  throw;
}
```

删除指针数组的时候也会对这部分内存做处理。

```cpp
// Original code: delete[] p;
size_t n = * (size_t*) ((char*)p - WORDSIZE);
while (n-- != 0)
  (p + n)->~Fred();
operator delete[] ((char*)p - WORDSIZE);
```

第二种是使用一个全局变量来保存对象的数量 `n`

```cpp
// Original code: Fred* p = new Fred[n];
Fred* p = (Fred*) operator new[] (n * sizeof(Fred));
size_t i;
try {
  for (i = 0; i < n; ++i)
    new(p + i) Fred();           // Placement new
}
catch (...) {
  while (i-- != 0)
    (p + i)->~Fred();            // Explicit call to the destructor
  operator delete[] (p);
  throw;
}
arrayLengthAssociation.insert(p, n);
```

删除数组指针时也同会对这部分做处理

```cpp
// Original code: delete[] p;
size_t n = arrayLengthAssociation.lookup(p);
while (n-- != 0)
  (p + n)->~Fred();
operator delete[] (p);
```

### 如何指定对象只能通过 new 来进行创建

使用 [Named Constructor Idiom](https://isocpp.org/wiki/faq/ctors#named-ctor-idiom)

将构造函数指定为 `private` 或者 `protected`（当需要派生类时，需要指定为 `protected`），同时指定 `public` 的 `static` 函数用于创建对象。

```cpp
class Fred {
public:
  // The create() methods are the "named constructors":
  static Fred* create()                 { return new Fred();     }
  static Fred* create(int i)            { return new Fred(i);    }
  static Fred* create(const Fred& fred) { return new Fred(fred); }
  // ...
private:
  // The constructors themselves are private or protected:
  Fred();
  Fred(int i);
  Fred(const Fred& fred);
  // ...
};
```

### 如何实现简单的引用计数

```cpp
// Fred.h
class FredPtr;
class Fred {
public:
  Fred() : count_(0) /*...*/ { }  // All ctors set count_ to 0 !
  // ...
private:
  friend class FredPtr;     // A friend class
  unsigned count_;
  // count_ must be initialized to 0 by all constructors
  // count_ is the number of FredPtr objects that point at this
};
class FredPtr {
public:
  Fred* operator-> () { return p_; }
  Fred& operator* ()  { return *p_; }
  FredPtr(Fred* p)    : p_(p) { ++p_->count_; }  // p must not be null
 ~FredPtr()           { if (--p_->count_ == 0) delete p_; }
  FredPtr(const FredPtr& p) : p_(p.p_) { ++p_->count_; }
  FredPtr& operator= (const FredPtr& p)
        { // DO NOT CHANGE THE ORDER OF THESE STATEMENTS!
          // (This order properly handles self-assignment)
          // (This order also properly handles recursion, e.g., if a Fred contains FredPtrs)
          Fred* const old = p_;
          p_ = p.p_;
          ++p_->count_;
          if (--old->count_ == 0) delete old;
          return *this;
        }
private:
  Fred* p_;    // p_ is never NULL
};
```