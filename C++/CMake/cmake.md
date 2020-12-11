# CMake note

CMake 的文件为 `CMakeLists.txt`，其中的关键字支持大写、小写或者大小写的混合。

## 一个简单的 `CMakeLists.txt` 

```cmake
cmake_minimum_required(VERSION 3.10)

# set the project name
project(Tutorial)

# add the executable
add_executable(Tutorial tutorial.cxx)
```

## 设置项目版本

```cmake
cmake_minimum_required(VERSION 3.10)

# set the project name and version
project(Tutorial VERSION 1.0)

configure_file(TutorialConfig.h.in TutorialConfig.h)

target_include_directories(Tutorial PUBLIC "${PROJRCT_BINARY_DIR}")
```

创建 `TutorialConfig.h.in`

```cpp
// the configured options and setttings for Tutorial
#define Tutorial_VERISON_MAJOR @Tutorial_VERSION_MAJOR@
#define Tutorial_VERSION_MINOR @Tutorial_VERSION_MAJOR@
```

## 添加包含目录

```cmake
target_include_directories(Tutorial PUBLIC "${PROJRCT_BINARY_DIR")
```

## 设置 C++ 标注库版本

必须在 `add_executable` 前设置

```cmake
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)
```

## 添加可选择的库和头文件目录

```cmake
option(USE_MYMATH "Use tutorial provided math implement" ON)
```

```cmake
if(USE_MYMATH)
    add_subdirectory(MathFunctions)
    list(APPEND EXTRA_LIBS MathFunctions)
    list(APPEND EXTRA_INCLUDES "${PROJECT_SOURCE_DIR}/MathFunctons")
endif()

# add the executable
add_executable(Turorial tutorial.cxx)

target_link_libraries(Tutorial PUBLIC ${EXTRA_LIBS})
target_include_diredtories(Tutorial PUBLIC 
                            "${PROJECT_BINARY_DIR}" 
                            ${EXTRA_INCLUDES})
```

## INTERFACE

对于库使用了 INTERFACE 之后，在主目录中可以不用再添加 include 目录

`INTERFACE` means things that consumers require but the producer doesn't

```cmake
target_include_directories(MathFunctions
    INTERFACE S{CMAKE_CURRENT_SOURCE_DIR}
    )
```

## 安装

```cmake
install(TARGETS MathFunctions DESTINATION lib)
install(FILES MathFuncitons DESTINATION include)
```

```cmake
install(TARGETS Tutorial DESTINATION bin)
install(FILES "${PROJECT_BINARY_DIR}/TutorialConfig.h" DESTINATION include)
```

## 设置安装目录

`CMAKE_INSTALL_PREFIX`

```bash
cmake --install . --prefix "/home/myuser/installdir"
```