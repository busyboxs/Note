# Windbg 使用方法

## host & target

* `host` system: The debuger runs on the host system.
* `target` system : the code that you want to debug runs on the target system.

## kernel-mode & user-mode

* `kernel-mode` : 拥有更高的权限，
* `user-mode` : 访问某些硬件和内存受限

## 指令

* `.sympath srv*` : The symbol search path tells WinDbg where to look for symbol (PDB) files. The debugger needs symbol files to obtain information about code modules (function names, variable names, and the like).

* `.reload` : tells WinDbg to do its initial finding and loading of symbol files

* `bu` : put a breakpoint

* `bl` : verify that your breakpoint was set.

* `g` : start running until it come to breakpoint.

* `lm` : see a list of code modules that are loaded in the process.

* `k` : see a stack trace.

* `~` : see a list of all threads in the process.

* `~0s` + `k` : look at the stack trace for thread 0.

* `qd` : quit debugging and detach from the process.

* `.symfix` + `.sympath+ C:\MyApp\Debug` : add sym path

## breakpoint

* `bl(Breakpoint List)` : list existing breakpoints and their current status
* `bpcmds(Display Breakpoint Commands)` : list all breakpoints along with the commands that were used to create them
* `bp(Set Breakpoint)` : set a new breakpoint
* `bu(Set Unresolved Breakpoint)` : set a new breakpoint
* `bm(Set Symbol Breakpoint)` : set new breakpoints on symbols that match a specified pattern
* `ba(Break on Access)` : set a processor breakpoint, also known as a data breakpoint
* `bc(Breakpoint Clear)` : permanently remove one or more breakpoints
* `bd(Breakpoint Disable)` : temporarily disable one or more breakpoints
* `be(Breakpoint Enable)` : re-enable one or more disabled breakpoints
* `br(Breakpoint Renumber)` : change the ID of an existing breakpoint
* `bs(Update Breakpoint Command)` : change the command associated with an existing breakpoint
* `bsc(Update Conditional Breakpoint)` : change the condition under which an existing conditional breakpoint occurs

Breakpoints support many kinds of address syntax, including virtual addresses, function offsets, and source line numbers.

```cmd
0:000> bp 0040108c
0:000> bp main+5c
0:000> bp `source.c:31`
```

## memory

* `d*(Display Memory)` : displays the contents of a specified memory address or range.
* `e*(Enter Values)` : writes a value to the specified memory address.
* `dt(Display Type)` : finds a variety of data types and displays data structures that have been created by the application that is being debugged. This command is highly versatile and has many variations and options.
* `ds, dS (Display String)` : displays a STRING, ANSI_STRING, or UNICODE_STRING data structure.
* `dl(Display Linked List)` : traces and displays a linked list.
* `d*s(Display Words and Symbols)` : finds double-words or quad-words that might contain symbol information and then displays the data and the symbol information.
* `!address` extension : displays information about the properties of the memory that is located at a specific address.
* `m(Move Memory)` : moves the contents of one memory range to another.
* `f(Fill Memory)` : writes a pattern to a memory range, repeating it until the range is full.
* `c(Compare Memory)` : compares the contents of two memory ranges.
* `s(Search Memory)` : searches for a specified pattern within a memory range or searches for any ASCII or Unicode characters that exist in a memory range.
* `.holdmem(Hold and Compare Memory)` : compares one memory range to another.

## sympath

* `.sympath srv*localcache*symbolstore`
  * `.sympath srv*c:\MyServerSymbols*https://msdl.microsoft.com/download/symbols`

* `cache*;` 
  * `.sympath cache*;srv*https://msdl.microsoft.com/download/symbols` ： tells the debugger to use a symbol server to get symbols from the store at https://msdl.microsoft.com/download/symbols and cache them in the default symbol cache directory

* `cache*localsymbolcache;`
  * `.sympath cache*c:\MySymbols;srv*https://msdl.microsoft.com/download/symbols` : tells the debugger to use a symbol server to get symbols from the store at https://msdl.microsoft.com/download/symbols and cache the symbols in the c:\MySymbols directory

## Debugger Commands Syntex

* 不区分大小写
* 可以用空格或者逗号分隔多个命令行
* 可以忽略第一个参数和命令行之间的空格
* 