# C++ Templates The Complete Guide

## Chapter 1 Function Templates

### 模板定义 

```cpp
template <typename T>
...
```

example

```cpp
template <typename T>
T max (T a, T b)
{
    return b < a ? a : b;
}
```

### 模板参数推导

在进行模板参数推导时，`const` 和 `volatile` 会被忽略掉，引用会被推导为被引用的类型，原数组或者函数会被推导为关联的指针类型。

```cpp
template <typename T>
T max (T a, T b);
...
int const c = 42;
max(i, c);    // T is deduced as int
max(c, c);    // T is deduced as int

int& ir = i;
max(i, ir);   // T is deduced as int

int arr[4];
foo(&i, arr); // T is deduced as int*
```

对于有默认参数的，需要指定默认的参数类型

```cpp
template <typename T = std::string>
void f(T = "");
...
```

### 多模板参数

需要使用不同或者指定的返回参数类型时，可以通过模板参数传入，由于返回参数类型不能进行类型推导，所以通常需要显式指定，较好的方法是把返回参数类型放在第一个，其他可以被推导的函数参数放在后面，这样就可以只显式指定返回参数。

```cpp
template <typename RT, typename T1, typenamr T2>
RT max (T1 a, T2 b);
...
::max<double>(4, 7.2);
```

也可以让编译器进行返回类型推导（C++ 14），将返回类型指定为 `auto`。

```cpp
template <typename T1, typename T2>
auto max (T1 a, T2 b)
{
    return b < a ? a : b;
}
```

在 C++ 14 之前，有一种推导方式叫做 `trailing return type` 可以指定函数返回的类型。

```cpp
template <typename T1, typename T2>
auto max (T1 a, T2 b) -> decltype(b<a?a:b>)
{
    return b < a ? a : b;
}
```

还可以使函数返回多个中更加通用的参数类型

```cpp
#include <type_traits>

template <typename T1, typename T2>
// typename std::common_type<T1,T2>::type max (T1 a, T2 b)  // for c++ 11
std::common_type_t<T1, T2> max (T1 a, T2 b)   // for c++ 14
{
    return b < a ? a : b;
}
```

还可以指定默认的函数返回类型

```cpp
#include <type_traits>

template <typename T1, typename T2,>
          typename RT = std::decay_t<decltype(true ? T1() : T2())>>
// std::decay_t 用于指定不能返回引用类型
RT max (T1 a, T2 b)
{
    return b < a ? a : b;
}
```

```cpp
#include <type_traits>

template <typename T1, typename T2, 
          typename RT = std::common_type_t<T1, T2>>
RT max (T1 a, T2 b)
{
    return b < a ? a : b;
}
```

## Chapter 2 Class Template

```cpp
#include <vector>
#include <cassert>

template <typename T>
class Stack 
{
private:
    std::vector<T> elems;

public:
    void push(T const& elem);
    void pop();
    T const& top() const;
    bool empty() const { return elems.empty(); }
}

template <typename T>
void Stack<T>::push (T const& elem)
{
    elems.push_back(elem);
}

template <typename T>
void Stack<T>::pop()
{
    assert(!elems.empty());
    elems.pop_back();
}

template <typename T>
T const& Stack<T>::top() const
{
    assert(!elems.empty());
    return elems.back();
}
```

### 类模板定义

类模板定义

```cpp
template<typename T>
class Stack
{
    ...
};
```

当需要定义拷贝构造函数和赋值运算符时，可以指定类型 `Stack<T>` 也可以不指定

```cpp
template <typename T>
class Stack
{
    ...
    Stack (Stack const&);
    Stack& operator= (Stack const&);
    ...
}
```

```cpp
template <typename T>
class Stack
{
    ...
    Stack (Stack<T> const&);
    Stack<T>& operator= (Stack<T> const&);
    ...
};
```

但是对于类外的结构，则需要指定类型

```cpp
template <typename T>
bool operator== (Stack<T> const& lhs, Stack<T> const& rhs);
```

类模型在实例化时，用到某个函数才会去实例化对应的类成员函数模板，没有使用的则不会被实例化。

### 类模板 friend 定义

```cpp
template <typename T>
class Stack
{
    ...
    template <typename U>
    friend std::ostream& operator<< (std::ostream&, Stack<U> const&);
};
```

```cpp
template <typename T>
class Stack
template <typename T>
std::ostream& operator<< (std::ostream&, Stack<T> const&);

// 然后在类中进行声明
template <typename T>
class Stack
{
    ...
    friend std::ostream& operator<< <T> (std::ostream&, Stack<T> const&);
};
```

### 类模板特例化

在特殊化某个特定类型的类时，需要指定为 `template<>`

```cpp
template<>
class Stack<std::string>
{
    ...
};
```

### 类模板默认参数

<details>
<summary>默认参数的容器</summary>

```cpp
#include <vector>
#include <cassert>

tempalte <typename T, typename Cont = std::vector<T>>
class Stack
{
private:
    Cont elems;

public:
    void push(T const& elem);
    void pop();
    T const& top() const;
    bool empty() const { return elems.empty(); }
};

template <typename T, typename Cont>
void Stack<T, Cont>::push (T const& elem)
{
    elems.push_back(elem);
}

template <typename T, typename Cont>
void Stack<T, Cont>::pop()
{
    assert(!elems.empty());
    elems.pop_back();
}

template <typename T, typename Cont>
T const& Stack<T, Cont>::top () const
{
    assert(!elems.empty());
    return elems.back();
}

```
</details>

### 类型别名

使用 `typedef`

```cpp
typedef Stack<int> IntStack;
void foo(IntStack const& s);
IntStack istack[10];
```

使用 `using`

```cpp
using IntStack = Stack<int>
void foo(IntStack const& s);
IntStack istack[10];
```

模板别名

```cpp
template <typename T>
using DequeStack = Stack<T, std::deque<T>>;
```

### 类模板类型推导

C++ 17 后可以进行类模板类型的自动推导

----

## Chapter 3 Nontype Template Parameters

对于模板，可以指定特定的类型的参数给类中使用。也就是在指定模板时，可以指定参数。

<details>
<summary>用户指定大小的栈模板</summary>

```cpp
#include <array>
#include <cassert>

template <typename T, std::size_t Maxsize>
class Stack
{
private:
    std::array<T, Maxsize> elems;
    std::size_t numElems;

public:
    Stack();
    void push(T const& elem);
    void pop();
    T const& top() const;
    bool empty() const { return numElems == 0; }
    std::size_t size() const { return numElems; }
};

template <typename T, std::size_t Maxsize>
Stack<T, Maxsize>::Stack() : numElems(0)
{
}

template <typename T, std::size_t Maxsize>
void Stack<T, Maxsize>::push(T const& elem)
{
    assert(numElems < Maxsize);
    elems[numElems] = elem;
    ++numElems;
}

template <typename T, std::size_t Maxsize>
void Stack<T, Maxsize>::pop()
{
    assert(!elems.empty());
    --numElems;
}

template <typename T, std::size_t Maxsize>
T const& Stack<T, Maxsize>::top()
{
    assert(!elems.empty());
    return elems[numElems - 1];
}
```
</details>

----

## Chapter 4 Variadic Templates

```cpp
#include <iostream>

void print()
{
}

template <typename T, typename... Types>
void print(T firstArg, Types... args)
{
    std::cout << firstArg << '\n';
    print(args...);
}
```

```cpp
#include <iostream>

template <typename T>
void print (T arg)
{
    std::cout << arg << '\n';
}

template <typename T, typename... Types>
void print(T firstArg, Types... args)
{
    print(firstArg);
    print(args...);
}
```

可以通过 `sizeof...` 运算符获取可变类型或者可变参数的 `size`.

### 折叠表达式（fold expressions）

C++ 17 支持 fold expressions

|Fold Expression| Evaluation|
|:---:|:---|
|`(... op pack)`|`(((pack1 op pack2) op pack3)... op packN)`|
|`(pack op ...)`|`(pack1 op (...(packN-1 op packN)))`|
|`(init op ... op pack)`|`(((init op pack1) op pack2)... op packN)`|
|`(pack op ... op init)`|`(pack1 op(...(packN op init)))`|

```cpp
template <typename... Types>
void print(Types const&... args)
{
    (std::cout << ... << args) << '\n';
}
```