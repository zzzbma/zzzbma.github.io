# lambda 函数

## 什么是lambda函数
lambda表达式从 c++11 开始引进，是能够捕获作用域中变量名的匿名函数。
> “捕获作用域变量名”指的是在lambda定义时，有时会传入变量

## lambda的定义

```c++
[捕获列表](形参列表) mutable throw() -> return type {函数体}
```

### 捕获列表
捕获列表和形参列表都可以在函数体使用，捕获列表可以通过值或者引用传递

```cpp
int i = 100;
// 传递的是值
auto f = [i]() ->int {return i;};
// 更改没有用处
i = 0;
int j = f(); // j = 100
```

```cpp
int i = 100;
// 通过引用传递
auto f = [&i]() -> int {return i;};
i = 0;
int j = f(); // j = 0
```

> 1. [ ]：空捕获列表，lambda不能使用所在函数中的变量。
> 2. [=]：函数体内可以使用Lambda所在作用范围内所有可见的局部变量（包括Lambda所在类的this），并且是值传递方式（相当于编译器自动为我们按值传递了所有局部变量）
> 3. [&]：函数体内可以使用Lambda所在作用范围内所有可见的局部变量（包括Lambda所在类的this），并且是引用传递方式（相当于编译器自动为我们按引用传递了所有局部变量）
> 4. [this]：函数体内可以使用Lambda所在类中的成员变量。
> 5. [a]：将a按值进行传递。按值进行传递时，函数体内不能修改传递进来的a的拷贝，因为默认情况下函数是const的。要修改传递进来的a的拷贝，可以添加mutable修饰符。
> 6. [&a]：将a按引用进行传递。
> 7. [=，&a, &b]：除a和b按引用进行传递外，其他参数都按值进行传递。
> 8. [&, a, b]：除a和b按值进行传递外，其他参数都按引用进行传递。

### mutable 规范
通常，Lambda 的函数调用运算符是 `const-by-value`，但对 `mutable` 关键字的使用可将其取消。它不产生 `mutable` 数据成员。 利用 `mutable` 规范，Lambda 表达式的主体可以修改通过值捕获的变量。

### `constexpr` Lambda 表达式

如果对 `constexpr` 是什么不了解，可以点[这里](./2023-06-09-const%20and%20constexpr.md)。在常量表达式中允许初始化捕获或引入的每个数据成员时，可以将 Lambda 表达式声明为 `constexpr`（或在常量表达式中使用它）。

```cpp
int y = 32;
auto answer = [y]() constexpr
{
    int x = 10;
    return y + x;
};

constexpr int Increment(int n)
{
    return [n] { return n + 1; }();
}
```

> TODO: 
> 1. - [ ] 什么时候用lambda
> 2. - [ ] STL有哪些，如何传入函数类型的参数 