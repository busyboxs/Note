# VSCode 添加 VS2019 开发工具 terminal

在 setting.json 文件中添加一下内容：

```json
"terminal.integrated.profiles.windows": {
    "vs2019": {
        "path": "${env:windir}\\System32\\cmd.exe",
        "args": [
            "/k",
            "C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\Community\\Common7\\Tools\\VsDevCmd.bat"
        ],
        "icon": "terminal-cmd"
    }
}
```