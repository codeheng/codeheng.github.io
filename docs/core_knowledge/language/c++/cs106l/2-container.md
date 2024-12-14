---
comments: true
---

![stl](./assets/stl.jpg)

All STL containers are **templates**.

## 有序容器

> 存储连续的元素 

### vector

`std::vector` (在`vector`头文件中)

```c++ linenums="1"
#include <iostream>
#include <vector>
using namespace std;
int main(int argc, char const *argv[])
{
    vector<int> vec { 1, 2, 3, 4 };
    vec.push_back(5);
    vec.push_back(6);
    vec[1] = 20;
    for(int i = 0; i < vec.size(); i ++) cout << vec[i] << " ";
    // 或采取范围进行遍历: for(auto a : vec) cout << a << " ";
    return 0;
}
```

并没办法从头插入元素(不支持`push_front`)

### deque

属于双端队列，在头文件`deque`中，有和`vector`相同的接口，但`deque`独有`push_front/pop_front`

## 关联式容器

> Associative containers organize elements by **unique keys**

### map

`std::map<K, V>` maps **key** to **values**.

```c++ linenums="1"
map<string, int> map {
    {"CS106L", 42},
    {"sean", 35},
};
int sean = map["sean"]; //35
```

map stores a collection of `std::pair<const K, V>`

!!! Note "遍历map"

    采取基于范围的遍历方式

    ```c++ linenums="1"
    std::map<string, int> map;
    for (auto kv : map) {
        // kv是std::pair<const std::string, int>
        string k = kv.first;
        int v = kv.second;
    }
    ```

    也可通过structured bindings **『C++17』**

    ```c++ linenums="1"
    std::map<string, int> map;
    for(const auto& [k, v] : map) {
        //k是 const std::string& 类型， v是const int&类型
    }
    ```

map如何实现的？ --> BST(红黑树)