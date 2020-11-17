# 配置 VSCode 创建执行 CMake 项目

## 配置

添加 `.vscode/launch.json`, `.vscode/settings.json`, `.vscode/tasks.json`

```json
// launch.json
{
    "version" :  "0.2.0" ,
     "configurations" : [
        {
             "name" :  "g++ - 生成和调试活动文件" ,
             "type" :  "cppdbg" ,
             "request" :  "launch" ,
             //目标执行文件，根据具体情况修改
             "program" :  "${workspaceFolder}/build/boostLog" ,   
             "args" : [],
             "stopAtEntry" :  false ,
             "cwd" :  "${workspaceFolder}" ,
             "environment" : [],
             "externalConsole" :  false ,
             "MIMode" :  "gdb" ,
             "setupCommands" : [
                {
                     "description" :  "为 gdb 启用整齐打印" ,
                     "text" :  "-enable-pretty-printing" ,
                     "ignoreFailures" :  true
                }
            ],
             "preLaunchTask" :  "make" ,
             "miDebuggerPath" :  "/usr/bin/gdb"
        }
    ]
}
```

```json
// settings.json
{
    "files.defaultLanguage": "c", // ctrl+N新建文件后默认的语言
    "editor.formatOnType": true, // （对于C/C++）输入分号后自动格式化当前这一行的代码
    "editor.suggest.snippetsPreventQuickSuggestions": false, // clangd的snippets有很多的跳转点，不用这个就必须手动触发Intellisense了
    "editor.acceptSuggestionOnEnter": "off", // 我个人的习惯，按回车时一定是真正的换行，只有tab才会接受Intellisense
    "C_Cpp.clang_format_sortIncludes": true,// 格式化时调整include的顺序（按字母排序）
    "C_Cpp.clang_format_fallbackStyle": "{ BasedOnStyle: Google, IndentWidth: 4, ColumnLimit: 0}",
    "files.associations": {
        "array": "cpp",
        "atomic": "cpp",
        "bit": "cpp",
        "*.tcc": "cpp",
        "cctype": "cpp",
        "chrono": "cpp",
        "clocale": "cpp",
        "cmath": "cpp",
        "codecvt": "cpp",
        "cstdarg": "cpp",
        "cstddef": "cpp",
        "cstdint": "cpp",
        "cstdio": "cpp",
        "cstdlib": "cpp",
        "ctime": "cpp",
        "cwchar": "cpp",
        "cwctype": "cpp",
        "deque": "cpp",
        "unordered_map": "cpp",
        "vector": "cpp",
        "exception": "cpp",
        "algorithm": "cpp",
        "functional": "cpp",
        "iterator": "cpp",
        "memory": "cpp",
        "memory_resource": "cpp",
        "numeric": "cpp",
        "optional": "cpp",
        "random": "cpp",
        "ratio": "cpp",
        "string": "cpp",
        "string_view": "cpp",
        "system_error": "cpp",
        "tuple": "cpp",
        "type_traits": "cpp",
        "utility": "cpp",
        "fstream": "cpp",
        "initializer_list": "cpp",
        "iomanip": "cpp",
        "iosfwd": "cpp",
        "iostream": "cpp",
        "istream": "cpp",
        "limits": "cpp",
        "new": "cpp",
        "ostream": "cpp",
        "sstream": "cpp",
        "stdexcept": "cpp",
        "streambuf": "cpp",
        "typeinfo": "cpp"
    }
    //强烈推荐，vscode自带的C/C++格式化太恶心了
}
```

```json
// tasks.json
{
  "tasks" : [
    {
             "label" :  "mkdir" ,
             "type" :  "shell" ,
             "command" : "mkdir" ,
             "args" : [ "-p" , "build" ],
             "options" : {
                 "cwd" :  "${workspaceFolder}"
            }
        },
        {
             "label" :  "cmake" ,
             "type" :  "shell" ,
             "command" : "cmake" ,
             "args" : [ ".." ],
             "options" : {
                 "cwd" :  "${workspaceFolder}/build"
            },
             "dependsOn" :[ "mkdir" ]
        },
        {   
             "label" :  "make" ,  //需要和 lauch.json 中的  "preLaunchTask" 相同
             "type" :  "shell" ,
             "command" :  "make" ,
             "args" : [],
             "options" : {
                 "cwd" :  "${workspaceFolder}/build"
            },
             "dependsOn" :[ "cmake" ] //执行当前命令前先执行 label 为 cmake 的命令
        }
    ],
     "version" :  "2.0.0"
}
```