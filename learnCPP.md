# C++ 笔记

## 构造函数调用

在定义类的构造函数重载时，可以在一个构造函数中调用另一个构造函数：也可以直接继承构造函数

```cpp
#include <iostream> 

class Base 
{ 
public: 
    int value1; 
    int value2; 
    
    Base() { value1 = 1; } 
    
    Base(int value) : Base() 
    { // delegate Base() constructor 
        value2 = value; 
    } 
}; 

class Subclass : public Base 
{ 
public: 
    using Base::Base; // inheritance constructor 
}; 

int main() 
{ 
    Base b(2); 
    std::cout << b.value1 << std::endl; 
    std::cout << b.value2 << std::endl; 
} 
```

## 面向对象

* 显式虚函数重写可以在函数后面添加 `override` 关键字。如果继承是最后一个，后面不会再存在进一步的继承时，可以使用 `final` 关键字。

* 如果用户没有定义构造函数，则编译器会自动生成一个默认构造函数。当希望某个类不能被拷贝时，应该将其拷贝构造函数和赋值操作符重载设置为 `private`

```cpp
class Magic 
{ 
public: 
    Magic() = default; // explicit let compiler use default constructor 
    Magic& operator=(const Magic&) = delete; // explicit declare refuse constructor 
    Magic(int magic_number); 
} 
```

## 强类型枚举

以前的枚举类型都是整型的，因此可以在不同的枚举类型之间进行比较，这是类型不安全的。c++ 11 中引入了 enum class，使得类型安全。

```cpp
enum class new_enum : unsigned int 
{ 
    value1, 
    value2, 
    value3 = 100, 
    value4 = 100 
}; 
```