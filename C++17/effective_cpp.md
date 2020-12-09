## More Effective C++

### item 1: 区别指针和应用

* 不存在空引用，应用必须指向一个对象；
* 引用必须进行初始化；
* 使用引用比使用指针更有效，引用不需要验证有效性，指针需要验证是否为空；
* 指针可以重新指向其他对象，引用只能指向初始化对象；
* 对于运算符得实现，通常返回引用；

### item 2: Prefer C++-style casts

* static_cast
* const_cast : 去 const 转换
* dynamic_cast : 基类的指针或者引用转换为继承类或者兄弟类的指针或者引用
* reinterpret_cast : 很少用，通常用于函数指针的转换

### Item 3: Never treat arrays polymorphically

* 对于多态类时，使用数组（array）的操作容易出错。比如在基类中定义了一个参数为数组的打印函数，然后继承类调用这个函数，因为数组被看作时指针，则数组的下一个元素是当前的指针加上对象的字节数，但是基类和继承类的字节数通常是不同的，所以继承类调用基类的带数组参数的函数时会出现问题。

### Item 4: Avoid gratuitous default constructors 

* 对于没有不带参数的默认构造函数的类，在创建数组时会比较麻烦，通常需要借助 operator new [] 和 operator delete [] 来实现

### Item 5: Be wary of user-defined conversion functions

* 对于只有一个参数的构造函数，最好是加上 `explicit` 避免隐式类型转换

### Item 6: Distinguish between prefix and postfix forms of increment and decrement operators 

* prefix : `T& T::operator++()`，前缀++不带参数，返回为引用类型
* postfix : `T T::operator++(int)`，后缀++带一个参数，返回为值类型（非引用）
* overload of prefix and postfix ++
    ```cpp
    class Number {
    public:
    Number& operator++ ();    // prefix ++
    Number  operator++ (int); // postfix ++
    };

    Number& Number::operator++ ()
    {
    *this += 1;
    return *this;
    }

    const Number Number::operator++ (int)
    {
    const Number oldValue = *this;
    ++(*this);  // or just call operator++()
    return oldValue;
    }
    ```

### Item 7: Never overload &&, ||, or , 

* `&&` 和 `||` 运算符具有短路原理，即当某些条件不满足时，其他后面的条件将不再执行
* `,` 运算符从左往右计算，最后的返回值时最右边的部分的结果
* 不可 overload 的运算符 `.`, `.*`, `::`, `?:`, `new`, `delete`, `sizeof`, `typeid`, `static_cast`, `dynamic_cast`, `const_cast`, `reinterpret_cast`
* 可以 overload 的运算符 
  * `operator new`, `operator delete`, `operator new[]`, `operator delete[]`
  * `+`, `-`, `*`, `/`, `%`, 
  * `^`, `&`, `|`, `~`, `!`, 
  * `=`, `<`, `>`, `+=`, `-=`, `*=`, `/=`, `%=`, `^=`, `&=`, `|=`
  * `<<`, `>>`, `>>=`, `<<=`, `==`, `!=`, `<=`, `>=`, 
  * `&&`, `||`, `++`, `--`, `,`, `->*`, `->`, `()`, `[]`

### Item 8: Understand the different meanings of new and delete 

* `new` 操作符做了两件事：一是申请足够的内存，而是调用构造函数初始化对象；
* 申请内存的操作符为 `operator new`，函数为 `void * operator new(size_t size);`
* `delete` 操作符： 先调用对象的析构函数，然后调用 `operator delete()` 释放内存；

### Item 9: Use destructors to prevent resource leaks 

* 当遇到异常时，有些资源可能不能正确释放，可以使用 `RAII`，智能指针防止内存泄漏

### Item 10: Prevent resource leaks in constructors

* 在构造函数中，也可能会有异常抛出，导致指针未能正确释放资源，因此也需要使用智能指针来避免。

### Item 11: Prevent exceptions from leaving destructors

### Item 12: Understand how throwing an exception differs from passing a parameter or calling a virtual function 

* throw 异常时会传递参数，通常是通过拷贝（copy）进行传递的

### Item 13: Catch exceptions by reference 

### Item 14: Use exception specifications judiciously 

### Item 15: Understand the costs of exception handling 

* 抛出和处理异常会有一定的消耗

### Item 16: Remember the 80-20 rule 

### Item 17: Consider using lazy evaluation

* 对于不必要的拷贝时可以使用懒加载（比如字符串读的时候共享，写的时候才进行拷贝）
* 对于不必要的数据库读时可以使用懒加载
* 对于不必要的数字运算可以使用懒加载，比如只需要矩阵的某一部分时不用计算整个矩阵

### Item 18: Amortize the cost of expected computations

* over-eager evaluation 通过缓存实现空间换时间，对于一些经常需要计算的值可以用缓存存储起来

### Item 19: Understand the origin of temporary objects

* 减少临时对象的出现，因为临时对象会调用构造函数和析构函数会有一定的消耗。临时对象的出现通常有两种情况，一种是对于一些参数需要进行隐式转换时，会创建一个临时对象；另一种情况是函数返回对象时会创建一个无名的淋湿对象。

### Item 20: Facilitate the return value optimization

### Item 21: Overload to avoid implicit type conversions 

### Item 22: Consider using op= instead of stand-alone op 

* 对于运算符，使用带有赋值操作的运算会比直接运算更优，例如 `+=` 比 `+` 更优，因为 `+` 运算会在计算过程中创建临时对象

### Item 23: Consider alternative libraries

### Item 24: Understand the costs of virtual functions, multiple inheritance, virtual base classes, and RTTI 

### Item 25: Virtualizing constructors and non-member functions 

### Item 26: Limiting the number of objects of a class 

### Item 27: Requiring or prohibiting heap-based objects 

### Item 28: Smart pointers

### Item 29: Reference counting 

### Item 31: Making functions virtual with respect to more than one object 

