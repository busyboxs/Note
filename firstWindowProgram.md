# 使用 VS2019 创建第一个 windows 程序

> 正在学习 windows 编程，记录一下笔记，方便以后查阅

## 创建 win32 桌面应用

首先打开 vs2019，选择 `创建新项目`

![](https://cdn.jsdelivr.net/gh/busyboxs/CDN@latest/blog/cpp/win32/first_windows_app_0001.png)

然后在模板中选择 `windows 桌面向导`，再点击`下一步`

![](https://cdn.jsdelivr.net/gh/busyboxs/CDN@latest/blog/cpp/win32/first_windows_app_0002.png)

然后在配置项目页中填写上项目的名称，配置好项目路径，点击`创建`

![](https://cdn.jsdelivr.net/gh/busyboxs/CDN@latest/blog/cpp/win32/first_windows_app_0003.png)

然后会弹出 windows 桌面配置页面，这里选择`空项目`，然后点击`确定`

![](https://cdn.jsdelivr.net/gh/busyboxs/CDN@latest/blog/cpp/win32/first_windows_app_0004.png)

之后进入项目后，右键项目名，选择 `添加` -> `新建项`

![](https://cdn.jsdelivr.net/gh/busyboxs/CDN@latest/blog/cpp/win32/first_windows_app_0005.png)

然后选择文件类型为 `c++文件` 填写上项目的名称，并点击`添加` 

![](https://cdn.jsdelivr.net/gh/busyboxs/CDN@latest/blog/cpp/win32/first_windows_app_0006.png)

之后拷贝[官方的教程代码](https://github.com/microsoft/Windows-classic-samples/blob/master/Samples/Win7Samples/begin/LearnWin32/HelloWorld/cpp/main.cpp)到创建的源文件中，执行程序，此时会出现一个错误

![](https://cdn.jsdelivr.net/gh/busyboxs/CDN@latest/blog/cpp/win32/first_windows_app_0007.png)

然后右键项目名，选择属性，修改链接器->系统->子系统为窗口

![](https://cdn.jsdelivr.net/gh/busyboxs/CDN@latest/blog/cpp/win32/first_windows_app_0008.png)

![](https://cdn.jsdelivr.net/gh/busyboxs/CDN@latest/blog/cpp/win32/first_windows_app_0009.png)

![](https://cdn.jsdelivr.net/gh/busyboxs/CDN@latest/blog/cpp/win32/first_windows_app_0010.png)

再次执行程序便可以展示一个窗口

![](https://cdn.jsdelivr.net/gh/busyboxs/CDN@latest/blog/cpp/win32/first_windows_app_0011.png)
