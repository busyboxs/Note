# 自定义数据类型使用 unordered_map

`unordered_map` 的模板声明如下:

```cpp
template < 
    class Key, 
    class Val, 
    class Hash = std::hash<key>, 
    class KeyEqual = std::equal_to<Key>,
    class Allocator = std::allocator<std::pair<const Key, T>>>
class unordered_map;
```

从声明中可以看到 `unordered_map` 有一个 `Hash` 和 `KeyEqual` 模板参数，所以要想使用自定义的数据类型作为 `unordered_map` 的 key，需要自定义 hash 函数和键相等的函数。

下面是一个简单实现的使用自定义数据类型作为 `unordered_map` 的键。

```cpp
#include <unordered_map>
#include <iostream>

struct MyInt
{
    MyInt(int v) : val(v) {}

    bool operator== (const MyInt& other) const {
        return val == other.val;
    }

    int val;
};

struct MyHash {
    std::size_t operator() (MyInt m) const {
        std::hash<int> hashVal;
        return hashVal(m.val);
    }
};

std::ostream& operator<< (std::ostream& st, const MyInt& myIn) {
    st << myIn.val;
    return st;
}

int main()
{
    typedef std::unordered_map<MyInt, int, MyHash> MyIntMap;

    MyIntMap myMap{ {MyInt(-2), 2}, {MyInt(-1), -1}, {MyInt(0), 0}, {MyInt(1), 1} };

    for (auto m : myMap) {
        std::cout << "{" << m.first << ", " << m.second << "}\n";
    }
}
```