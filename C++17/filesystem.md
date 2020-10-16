# C++ 17 filesystem

filesystem 提供对路径、常规文件和目录的操作的库。有了该库，对于文件和目录的操作变得更加容易。

##  库相关的几个定义

* `file` : 存放数据的文件系统对象，可读可写。拥有文件名字和属性。
  * `directory` : 文件目录
  * `hard link` : 硬链接
  * `symbolic link` : [符号链接](https://zh.wikipedia.org/wiki/%E7%AC%A6%E5%8F%B7%E9%93%BE%E6%8E%A5)
  * `regular file` : 常规文件

* `file name` : 文件的名字，通常为一个字符串

* `path`
  * `absolute path` : 绝对路径
  * `canonical path` : 规范路径（不包含符号链接，`.` 和 `..` 的路径）
  * `relative path` : 相对路径（一个路径相对于另一个路径的路径，字符串中可能包含 `.` 和 `..`）

## Path 类及其相关内容

### 路径的定义

文件系统中对于路径名的定义包括以下几个部分

* `path name syntax`
  * `root-name` : 根的名字，例如 `C:` 或 `//myserver`
  * `root-directory` : 根目录，将该路径标记为绝对路径的目录分隔符。
  * `file-name` : 文件名
  * `directory-separator` : 目录分隔符

path 类有一个常成员变量 `constexpr value_type preferred_separator`。在 windows 系统中为 `\`，在 POSIX 中为 `/`。

### 路径规范化算法

path 可以通过以下算法进行规范化：

1. 如果路径为空，则停止；
2. 替换连续的多个目录分隔符为单独的一个 `preferred_separator`，eg: `/usr///////lib` -> `/usr/lib`
3. 替换 `root-name` 中的每个斜杠字符为 `prefered_separator`
4. 移除紧跟斜杠字符后的 `.` 符号
5. 移除紧跟斜杠字符后的 `..` 符号，并将 `..` 前的非 `..` 文件名去掉，eg: `/usr/local/include/../opencv2` -> `/usr/local/include/opencv2`
6. 如果存在根目录，删除所有 `..` 和紧随其后的所有目录分隔符
7. 如果最后一个文件名是 `..`，删除所有结尾的目录分隔符
8. 如果路径为空，则添加 `.`

### 路径的格式

路径有两种格式，`native_format` 和 `generate_format`。对于 POSIX-like 的文件系统，这两种格式是相同的。对于 Windows 文件系统，则是不同的。

* generic format 路径格式：`/home/mypc/myfile.txt`
* native format 路径格式（windows）：`F:\\MyDirectory\\myFile.txt` 或 `F:\MyDirectory\myFile.txt`

### 路径的操作

* `(constructor)` & `(destructor)` & `operator=` & `assign`
  * 路径有构造函数和析构函数，可以通过 `operator=` 进行赋值，也可以使用 `assign` 进行赋值。

* `append` & `operator/=` & `concat` & `operator+=`
  * 路径有两个连接操作，`operator/=` 和 `append` 可以将两个路径进行连接，同时会添加一个分隔符。`operator+=` 和 `concat` 也可以将两个路径进行连接，但是不会添加分隔符。

  ```cpp
  #include <iostream>
  #include <filesystem>

  namespace fs = std::filesystem;

  int main()
  {
      fs::path p1 = "/a/b/c/d";
      fs::path p2 = "/e/f/g/h";

      p1 /= "m";
      std::cout << p1.generic_string() << std::endl;  // /a/b/c/d/m
      p1.append("n");
      std::cout << p1.generic_string() << std::endl;  // /a/b/c/d/m/n

      p2 += "x";
      std::cout << p2.generic_string() << std::endl;  // /e/f/g/hx
      p2.concat("y");
      std::cout << p2.generic_string() << std::endl;  // /e/f/g/hxy
      
      return 0;
  }
  ```

* `clear` : 清除路径中的内容
* `swap` : 交换两个路径中的内容
* `make_preferred` : 将路径中的分隔符替换为 `preferred_separator`
* `remove_filename` : 将路径中的文件名去掉，具体是指删除最后一个文件分隔符之后的内容

  ```cpp
  #include <iostream>
  #include <filesystem>
  namespace fs = std::filesystem;

  int main()
  {
      fs::path p1 = "/a/b/c/d";
      std::cout << p1.remove_filename() << '\n';  // /a/b/c
      return 0;
  }
  ```

* `replace_filename` : 替换路径中的最后一部分为其他路径
* `replace_extension` : 替换文件后缀
* `c_str` & `native` & `operator string_type`
  * 将路径转换为字符串
* `string` & `wstring` & `u8string` & `u16string` & `u32string`
  * 根据具体的编码将路径转换为具体的 native 字符串格式

* `generic_string` & `generic_wstring` & `generic_u8string` & `generic_u16string` & `generic_u32string`
  * 根据具体的编码将路径转换为具体的 generic 字符串格式

* `compare` : 比较两个路径
* `lexically_normal` : 转换路径为 normal 格式（不包含 `.` 和 `..`）
* `lexically_relative` : 转换路径为 relative 格式 
* `lexically_proximate` : 转换路径为 proximate 格式 
* `empty` : 检测路径是否为空
* `is_absolute` : 检查路径是否为绝对路径
* `is_relative` : 检查路径是否为相对路径
* `begin` : begin 迭代器
* `end` : end 迭代器

获取以及检查路径的几个部分

|获取函数|检查函数|描述|
|:---|:---|:----|
|`root_name` | `has_root_name` | 获取（检查）路径根的名字|
|`root_directory` | `has_root_directory` | 获取（检查）路径根目录（绝对路径的目录分隔符）|
|`root_path` | `has_rootpath` | 获取（检查）根路径，`root_name` + `root_directory`|
|`relative_path` | `has_relative_path` | 获取（检查）相对路径，除去 `root_path` 之后剩余的部分|
|`parent_path` | `has_parent_path` | 获取（检查）除去文件名外的路径部分|
|`filename` | `has_filename` | 获取（检查）文件名|
|`stem` | `has_stem` | 获取（检查）文件名称|
|`extension` | `has_extension` | 获取（检查）文件扩展|

```cpp
#include <iostream>
#include <filesystem>

namespace fs = std::filesystem;

int main()
{
    fs::path p = R"(F:\CppProgram\filesystem\main.cpp)";
    if (p.has_root_name())
        std::cout << p.root_name() << "\n";        // "F:"
    if (p.has_root_directory())
        std::cout << p.root_directory() << "\n";   // "\\"
    if (p.has_root_path())
        std::cout << p.root_path() << "\n";        // "F:\\"
    if (p.has_relative_path())
        std::cout << p.relative_path() << "\n";    // "CppProgram\\filesystem\\main.cpp"
    if (p.has_root_path())
        std::cout << p.parent_path() << "\n";      // "F:\\CppProgram\\filesystem"
    if (p.has_filename())
        std::cout << p.filename() << "\n";         // "main.cpp"
    if (p.has_stem())
        std::cout << p.stem() << "\n";             // "main"
    if (p.has_extension())
        std::cout << p.extension() << "\n";        // ".cpp"

    return 0;
}
```




## file status

* `type` : `file_type`
  * `none`
  * `not_found`
  * `regular`
  * `directory`
  * `symlink`
  * `block`
  * `character`
  * `fifo`
  * `socket`

* `permissions` : `perms`
  * `none`
  * `owner_read`
  * `owner_write`
  * `owner_exec`
  * `owner_all`
  * `group_read`
  * `group_write`
  * `group_exec`
  * `group_all`
  * `others_read`
  * `others_write`
  * `others_exec`
  * `others_all`
  * `all`
  * `set_uid`
  * `set_gid`
  * `sticky_bit`
  * `mask`

* `perm_options`
  * `replace`
  * `add`
  * `remove`
  * `nofollow`

## copy_options

* `none`
* `skip_existing` : Keep the existing file, without reporting an error.
* `overwrite_existing` : Replace the existing file
* `update_existing` : Replace the existing file only if it is older than the file being copied
* `recursive` : Recursively copy subdirectories and their content
* `copy_symlinks` : Copy symlinks as symlinks, not as the files they point to
* `skip_symlinks` : Ignore symlinks
* `directories_only` : Copy the directory structure, but do not copy any non-directory files
* `create_symlinks` : Instead of creating copies of files, create symlinks pointing to the originals
* `create_hard_links` : Instead of creating copies of files, create hardlinks that resolve to the same files as the originals

## 函数

* `absolute` : 获取绝对路径
* `canonical` : 获取典型路径格式，会去掉 `.` 和 `..`
* `relative` : 获取相对路径，默认为相对于当前路径的路径
* `copy` : 拷贝文件或者目录
* `copy_file` : 拷贝文件内容
* `copy_symlinks` : 拷贝符号链接
* `create_directory` : 创建目录
* `create_hard_link` : 创建硬链接
* `create_symlink` & `create_directory_symlink` : 创建符号链接
* `current_path` : 当前目录
* `exists` : 检查路径是否存在
* `equivalent` : 检查两个路径是否相等
* `file_size` : 获取文件的大小
* `hard_link_count` : 获取硬链接的数量
* `last_write_time` : 获取或者设置数据修改的最后时间
* `permissions` : 修改文件权限
* `read_symlink` : 获取符号链接的目标
* `remove` : 删除一个文件或者空目录 
* `remove_all` : 删除一个文件或者递归删除一个文件目录
* `rename` : 移动或者重命名文件或者目录
* `resize_file` : 通过截断或者补零的方式改变常规文件的大小
* `space` : 确定文件系统上的可用空间
* `status` : 获取文件属性
* `symlink_status` : 获取文件属性，检查符号链接目标
* `temp_directory_path` : 获取临时目录路径