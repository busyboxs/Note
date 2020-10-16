# C++ Core Guidelines

## P: Philosophy

### P.1: Express ideas directly in code

* 始终使用 `const`，检查成员函数是否修改成员变量
* 使用定义的数据类型，让代码更容易理解（eg. `Speed` <- `double`）
* 检测代码是否模仿了标准库中的函数，尽量使用标准库中的算法

### P.2: Write in ISO Standard C++
### P.3: Express 
### P.4: Ideally, a program should be statically type safe
### P.5: Prefer compile-time checking to run-time checking
### P.6: What cannot be checked at compile time should be checkable at run time
### P.7: Catch run-time errors early
### P.8: Don’t leak any resources

* 当申请了资源（内存）时，需要注意在中途条件退出时，需要释放资源，推荐使用 RAII

### P.9: Don’t waste time or space

* 在写代码的时候，尽量少使用一些隐含浪费时间的函数或者操作，例如 `strlen()` 会遍历字符串，因此可以先获取长度，再进行其他的一些循环操作。例如可以使用前缀加加和减减。

### P.10: Prefer immutable data to mutable data

* 对于常量的推理比对于变量的推理更容易。不变性可以实现更好的优化。

### P.11: Encapsulate messy constructs, rather than spreading through the code
### P.12: Use supporting tools as appropriate
### P.13: Use support libraries as appropriate

## I: Interfaces

### I.1: Make interfaces explicit

### I.2: Avoid non-const global variables

* 非静态全局变量可能会在某个函数中受到不可预测的更改
* 全局对象的初始化不是完全有序的，如果使用全局对象，请使用常量对其进行初始化。注意，即使对于const对象，也有可能获得未定义的初始化顺序。
* 全局对象通常比单例更好

### I.3: Avoid singletons

### I.4: Make interfaces precisely and strongly typed

* 在定义借口时，应该使得它的参数类型具有更强的语义，例如在画 rectangle 时，可以指定其参数为 point top-left 与 point buttom-right，或者为 point top-left 和 size weight-height。

### I.5: State preconditions (if any)
### I.6: Prefer `Expects()` for expressing preconditions

* 对于前置条件可以使用 GSL 中的 `Expects` 或者 C++ 20 中的 `[[excepts]]`

### I.7: State postconditions
### I.8: Prefer `Ensures()` for expressing postconditions

* 对于后置条件可以使用 GSL 中的 `Ensures` 或者 C++ 20 中的 `[[ensures]]`

### I.9: If an interface is a template, document its parameters using concepts

### I.10: Use exceptions to signal a failure to perform a required task

### I.11: Never transfer ownership by a raw pointer (T*) or reference (T&)

### I.12: Declare a pointer that must not be null as `not_null` (GSL)
### I.13: Do not pass an array as a single pointer

### I.22: Avoid complex initialization of global objects
### I.23: Keep the number of function arguments low
### I.24: Avoid adjacent parameters of the same type when changing the argument order would change meaning

### I.25: Prefer abstract classes as interfaces to class hierarchies

### I.26: If you want a cross-compiler ABI, use a C-style subset

### I.27: For stable library ABI, consider the Pimpl idiom
### I.30: Encapsulate rule violations

## F: Functions

### F.def: Function definitions

#### F.1: “Package” meaningful operations as carefully named functions

* 将有意义的操作封装成一个函数，方便重复使用

#### F.2: A function should perform a single logical operation

* 一个函数应该只执行一个操作

#### F.3: Keep functions short and simple
#### F.4: If a function may have to be evaluated at compile time, declare it `constexpr`
#### F.5: If a function is very small and time-critical, declare it `inline`

* 默认情况下，在类中定义的成员函数是内联的。
* 函数模板通常在头文件中定义，因此是内联的。

#### F.6: If your function may not throw, declare it `noexcept`
#### F.7: For general use, take T* or T& arguments rather than smart pointers
#### F.8: Prefer pure functions
#### F.9: Unused parameters should be unnamed

### F.call: Parameter passing

#### F.15: Prefer simple and conventional ways of passing information
#### F.16: For “in” parameters, pass cheaply-copied types by value and others by reference to `const`
#### F.17: For “in-out” parameters, pass by reference to non-const
#### F.18: For “will-move-from” parameters, pass by X&& and std::move the parameter
#### F.19: For “forward” parameters, pass by TP&& and only `std::forward` the parameter
#### F.20: For “out” output values, prefer return values to output parameters
#### F.21: To return multiple “out” values, prefer returning a struct or tuple

#### F.22: Use T* or owner<T*> to designate a single object
#### F.23: Use a not_null<T> to indicate that “null” is not a valid value
#### F.24: Use a span<T> or a span_p<T> to designate a half-open sequence
#### F.25: Use a zstring or a not_null<zstring> to designate a C-style string
#### F.26: Use a unique_ptr<T> to transfer ownership where a pointer is needed
#### F.27: Use a shared_ptr<T> to share ownership
#### F.60: Prefer T* over T& when “no argument” is a valid option
#### F.42: Return a T* to indicate a position (only)
#### F.43: Never (directly or indirectly) return a pointer or a reference to a local object
#### F.44: Return a T& when copy is undesirable and “returning no object” isn’t needed
#### F.45: Don’t return a T&&
#### F.46: `int` is the return type for `main()`
#### F.47: Return T& from assignment operators
#### F.48: Don’t return `std::move(local)`
#### F.50: Use a lambda when a function won’t do (to capture local variables, or to write a local function)
#### F.51: Where there is a choice, prefer default arguments over overloading
#### F.52: Prefer capturing by reference in lambdas that will be used locally, including passed to algorithms
#### F.53: Avoid capturing by reference in lambdas that will be used non-locally, including returned, stored on the heap, or passed to another thread
#### F.54: If you capture this, capture all variables explicitly (no default capture)
#### F.55: Don’t use va_arg arguments
