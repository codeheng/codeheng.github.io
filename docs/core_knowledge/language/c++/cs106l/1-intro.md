---
comments: true
---

## 1.介绍

> [CS106L](https://web.stanford.edu/class/cs106l/)——Standard C++ Programming
>
> - Focus is on code: what makes it good, what powerful and elegant code looks like
> - The real deal: No Stanford libraries, only STL
> - Understand **how and why** C++ was made

C++历史（C++的演变）：

![](./assets/1_c++history.jpg)

- 1983年C++被[Bjarne Stroustrup](https://www.stroustrup.com/)设计

**设计哲学:**

- 仅添加能解决实际问题的功能
- 程序员应该可以自由选择自己的风格
- 如果程序员想要的话，允许他们完全控制
- Compartmentalization（划分） is key
- 除非万不得已，否则不要牺牲性能
- 在编译时尽可能加强安全性

## 2. 类型和结构体

!!! Note 
    STL(Standard Template Library): 包含大量强大和维护良好的通用功能
    
    - e.g: maps, sets, vectors...
    - 使用`std::`进行访问，即STL的命名空间

### Type

!!! Note "基础类型"

    ```c++
    int val = 5; // 32bits (usually)
    char ch = 'f' // 8bits
    float decimalVal1 = 5.0; // 32bits
    double decimalval2 = 5.0; // 64bits
    bool bVal = true; // 1bit
    std::string str = "Haven";
    ```

    **c++是一种静态类型（statical type）语言** --> 编译执行（compiled）

    - 动态类型语言: 如Python --> 解释执行 (interpreted)

> 静态类型: 所有有名字的东西(如变量，函数)在运行前被给定一个类型

### struct

!!! Tip "auto关键字"

    在声明变量时用来代替某类型的关键字，告诉编译器来推断该类型

    ```c++
    auto a = 3; //int
    auto b = 4.3; // double
    auto c = 'x'; // char
    auto d = "hello"; // char* (a C string)
    ```

struct是一组命名的变量，每个变量都有自己的类型，允许程序员将不同的类型捆绑在一起!

!!! example "例子"

    ```c++ 
    struct Student {
        string name; // these are called fields（字段）
        string state;
    };
    Student s;  
    s.name = "yh"; // use . to access fields
    ```

STL有自己的struct --> `std::pair` (具有任意类型的两个字段的STL内置结构体)

- `std::pair`是一个template，通过`<>`指定内部字段的类型
- 在`std::pair`中的两个字段被命名为first和second

```c++
std::pair<int, string> p = {1, "st"};
std::cout << p.first << p.second; //打印： 1st
```

## 3. 初始化和引用