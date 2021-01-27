# C++ 如何在一行中刷新输出

在写程序时，有时想要在 console 上一行输出信息，例如输出进度条，进度条是随着时间不断增加的，所以会慢慢变长。

要实现在 console 里的一行持续刷新输出需要使用 `std::flush` 手动刷新 cout 的缓存，同时使用 `\r` 退格字符来删除原来的缓存。

```cpp
#include <iostream>
#include <chrono>
#include <thread>

using namespace std::chrono_literals;

int main()
{
    for (auto i = 0; i < 10000; ++i)
    {

        std::cout << "----->   " << i << "\r" << std::flush;
        std::this_thread::sleep_for(1s);
    }
    return 0;
}
```

以上代码会在 console 上一秒输出一个递增的数字

```cpp
#include <iostream>
#include <chrono>
#include <thread>
#include <sstream>
#include <iomanip>  // std::put_time

using namespace std::chrono_literals;

int main()
{
    for (auto i = 0; i < 10000; ++i)
    {
        auto now = std::chrono::system_clock::now();
        auto in_time_t = std::chrono::system_clock::to_time_t(now);

        std::stringstream ss;
        ss << std::put_time(std::localtime(&in_time_t), "%Y-%m-%d %X");
        std::cout << "----->   " << ss.str() << "   <-----\r" << std::flush;
        std::this_thread::sleep_for(1s);
    }
    return 0;
}
```

以上代码会在 console 上每个一秒刷新一下当前时间。