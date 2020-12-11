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