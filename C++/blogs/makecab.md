# 使用 makecab 命令压缩多个文件到 .cab 文件中

## makecab 语法

```bash
makecab [/v[n]] [/d var=<value> ...] [/l <dir>] <source> [<destination>]
makecab [/v[<n>]] [/d var=<value> ...] /f <directives_file> [...]
```

参数说明：

|参数|描述|
|:---|:---|
|`<source>`|需要压缩的文件|
|`<destination>`|压缩后生成的文件名。如果未指定，则将最后一个字符替换为 `_` 作为输出文件名|
|`/f <directives_file>`|	A file with makecab directives (may be repeated).|
|`/d var=<value>`|定义指定值得变量|
|`/l <dir>`|Location to place destination (default is current directory).|
|`/v[<n>]`|Set debugging verbosity level (0=none,...,3=full).|
|`/?`|Displays help at the command prompt.|

## 例子

### 将单个文件压缩为 .cab 文件

```bash
> makecab a.txt a.cab
```

### 将多个文件压缩为 .cab 文件

首先需要创建一个 ddf 文件，我这里创建了一个 `makecab.ddf` 文件，文件内容如下：

```
.Set CabinetNameTemplate=0.cab
.Set Cabinet=on
.Set Compress=on
.Set CompressionMemory=21
.Set CompressionType=LZX
.Set MaxDiskSize=0
.Set DiskDirectory1=G:\unPack
"G:\unPack\patchlib0\0.txt" /inf=no
"G:\unPack\patchlib0\1.txt" /inf=no
"G:\unPack\patchlib0\2.txt" /inf=no
```

然后执行以下命令行：

```bash
> makecab /F makecab.ddf
```

执行命令行之后会生成一个 `0.cab` 文件，里面包含 `0.txt`，`1.txt` 和 `2.txt`。

## ddf 文件中各参数说明

----

|Syntax|Description|
|:-----|:----------|
|`;`|Comment (anywhere on a DDF line) <br>The ; character is also a standard batch file delimiter so this makes it possible to create a self-contained batch file/directive script, lines prefixed with ; will be commented out only for makecab.|
|`src [dest] [/inf=yes|no] [/unique=yes|no] [/x=y ...]`|File Copy command|
|`dest [/x=y ...]`|File Reference command|
|`.Define variable=[value]`|Define variable to be equal to value (see .Option Explicit)|
|`.Delete variable`|Delete a variable definition|
|`.Dump`|Display all variable definitions|
|`.InfBegin Disk | Cabinet | Folder`|Copy lines to specified INF file section|
|`.InfEnd`|End an `.InfBegin` section|
|`.InfWrite string`|Write "string" to file section of INF file|
|`.InfWriteCabinet string`|Write "string" to cabinet section of INF file|
|`.InfWriteDisk string`|Write "string" to disk section of INF file|
|`.New Disk | Cabinet | Folder`|Start a new Disk, Cabinet, or Folder|
|`.Option Explicit`|Require `.Define` first time for user-defined variables|
|`.Set variable=[value]`|Set variable to be equal to value|
|`%variable%`|Substitute value of variable|
|`<blank line>`|Blank lines are ignored|

----

|Standard Variables|Description|
|:-----------------|:----------|
|`Cabinet=ON | OFF`|Turns Cabinet Mode on or off|
|`CabinetFileCountThreshold=count`|Threshold count of files per Cabinet|
|`CabinetNamen=filename`|Cabinet file name for cabinet number n|
|`CabinetNameTemplate=template`|Cabinet file name template; * is replaced by Cabinet number|
|`ChecksumWidth=1 | 2 | ... | 8`|Max low-order hex digits displayed by INF csum parameter|
|`ClusterSize=bytesPerCluster`|Cluster size on diskette (default is 512 bytes)|
|`Compress=ON | OFF`|Turns compression on or off|
|`CompressedFileExtensionChar=char`|Last character of the file extension for compressed files|
|`CompressionMemory=15 | 16 | ... | 21`|The window size for LZX compression|
|`CompressionType=MSZIP | LZX`|Compression engine|
|`DestinationDir=path`|Default path for destination files (stored in cabinet file)|
|`DiskDirectoryn=directory`|Output directory name for disk n|
|`DiskDirectoryTemplate=template`|Output directory name template; * is replaced by disk number|
|`DiskLabeln=label`|Printed disk label name for disk n|
|`DiskLabelTemplate=template`|Printed disk label name template; * is replaced by disk number|
|`DoNotCopyFiles= ON | OFF`|Controls whether files are actually copied (ACME ADMIN.INF)|
|`FolderFileCountThreshold=count`|Threshold count of files per Folder|
|`FolderSizeThreshold=size`|Threshold folder size for current folder|
|`GenerateInf=ON | OFF`|Control Unified vs. Relation INF generation mode. A File Copy command is distinguished from a File Reference command by the setting of the GenerateInf variable.|
|`InfXxx=string`|Set default value for INF Parameter Xxx|
|`InfCabinetHeader[n]=string`|INF cabinet section header text|
|`InfCabinetLineFormat[n]=format string`|INF cabinet section detail line format|
|`InfCommentString=string`|INF comment string|
|`InfDateFormat=yyyy-mm-dd | mm/dd/yy`|INF date format|
|`InfDiskHeader[n]=string`|INF disk section header text|
|`InfDiskLineFormat[n]=format string`|INF disk section detail line format|
|`InfFileHeader[n]=string`|INF file section header text|
|`InfFileLineFormat[n]=format string`|INF file section detail line format|
|`InfFileName=filename`|Name of INF file|
|`InfFooter[n]=string`|INF footer text|
|`InfHeader[n]=string`|INF header text|
|`InfSectionOrder=[D | C | F]*`|INF section order (disk, cabinet, file)|
|`MaxCabinetSize=size`|Maximum cabinet file size for current cabinet|
|`MaxDiskFileCount=count`|Maximum count of files per Disk|
|`MaxDiskSize[n]=size`|Maximum disk size|
|`MaxErrors=count`|Maximum errors allowed before pass 1 terminates|
|`ReservePerCabinetSize=size`|Base amount of space to reserve for FCRESERVE data|
|`ReservePerDataBlockSize=size`|Amount of space to reserve in each data block|
|`ReservePerFolderSize=size`|Amount of additional space in FCRESERVE for each folder|
|`RptFileName=filename`|Name of RPT file|
|`SourceDir=path`|Default path for source files|
|`UniqueFiles=ON | OFF`|Control whether duplicate destination file names are allowed|

## 脚本实现多文件夹压缩为 .cab 文件

保存下面的代码到 cabdir.bat 文件中，然后执行 `call cabdir.bat /h` 能够输出使用说明

```bash
;@echo off

;;;;; rem start of the batch part  ;;;;;
;
;for %%a in (/h /help -h -help) do ( 
;   if /I "%~1" equ "%%~a" if "%~2" equ "" (
;       echo compressing directory to cab file  
;       echo Usage:
;       echo(
;       echo %~nx0 "directory" "cabfile"
;       echo(
;       echo to uncompress use:
;       echo EXPAND cabfile -F:* .
;       echo(
;       echo Example:
;       echo(
;       echo %~nx0 "c:\directory\logs" "logs"
;       exit /b 0
;   )
; )
;
; if "%~2" EQU "" (
;   echo invalid arguments.For help use:
;   echo %~nx0 /h
;   exit /b 1
;)
;
; set "dir_to_cab=%~f1"
;
; set "path_to_dir=%~pn1"
; set "dir_name=%~n1" 
; set "drive_of_dir=%~d1"
; set "cab_file=%~2"
;
; if not exist %dir_to_cab%\ (
;   echo no valid directory passed
;   exit /b 1
;)

;
;break>"%tmp%\makecab.dir.ddf"
;
;setlocal enableDelayedExpansion
;for /d /r "%dir_to_cab%" %%a in (*) do (
;   
;   set "_dir=%%~pna"
;   set "destdir=%dir_name%!_dir:%path_to_dir%=!"
;   (echo(.Set DestinationDir=!destdir!>>"%tmp%\makecab.dir.ddf")
;   for %%# in ("%%a\*") do (
;       (echo("%%~f#"  /inf=no>>"%tmp%\makecab.dir.ddf")
;   )
;)
;(echo(.Set DestinationDir=!dir_name!>>"%tmp%\makecab.dir.ddf")
;   for %%# in ("%~f1\*") do (
;       
;       (echo("%%~f#"  /inf=no>>"%tmp%\makecab.dir.ddf")
;   )

;rem makecab /F "%~f0" /f "%tmp%\makecab.dir.ddf" /d DiskDirectory1=%cd% /d CabinetNameTemplate=%cab_file%.cab
;makecab /f "%tmp%\makecab.dir.ddf" /d DiskDirectory1=%cd% /d CabinetNameTemplate=%cab_file%.cab
;rem del /q /f "%tmp%\makecab.dir.ddf"
;exit /b %errorlevel%

;;
;;;; rem end of the batch part ;;;;;

;;;; directives part ;;;;;
;;
.New Cabinet
.set GenerateInf=OFF
.Set Cabinet=ON
.Set Compress=ON
.Set UniqueFiles=ON
.Set MaxDiskSize=1215751680;

.set RptFileName=nul
.set InfFileName=nul

.set MaxErrors=1
;;
```

## 参考链接

* [makecab](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/makecab)
* [MAKECAB.exe](https://ss64.com/nt/makecab.html)
* [MAKECAB Directive File syntax](https://ss64.com/nt/makecab-directives.html)
* [Makecab.exe](https://realmpkdotnet.wordpress.com/2014/11/23/makecab-exe/)
* [makecab compression issue](https://msfn.org/board/topic/25374-makecab-compression-issue/)
* [What's maxsize of Cab file generated by makecab?](https://social.msdn.microsoft.com/Forums/windowsdesktop/en-US/37819447-f7ac-4a5d-92a4-05c101043f50/whats-maxsize-of-cab-file-generated-by-makecab?forum=windowssdk#:~:text=The%20CAB%20file%20format%20has,CAB%20(compressed)%3A%200x7FFFFFFF%20bytes.)