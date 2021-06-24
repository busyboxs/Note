# Go 语言学习笔记

## go 语言支持的类型

|Type|Description|
|:---|:----------|
|`int` |An integer. Holds whole numbers.|
|`float64`|A floating-point number. Holds numbers with a fractional part. (The 64 in the type name is because 64 bits of data are used to hold the number. This means that float64 values can be fairly, but not infinitely, precise before being rounded off.) |
|`bool`|A Boolean value. Can only be `true` or `false`. |
|`string`|A string. A series of data that usually represents text characters.|

## go 编译执行

```bash
go build hello.go # 生成 hello.exe 程序
```

|Command|Description|
|:------|:----------|
|`go build`|Compiles source code files into binary files. |
|`go run`|Compiles and runs a program, without saving an executable file.  |
|`go fmt`|Reformats source files using Go standard formatting. |
|`go version`|Displays the current Go version.|

