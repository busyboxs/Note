# windbg command

|command|description|usage|
|:------|:----------|:----|
|`ba`|break on access|<ul><li>user-mode : `[~Thread] ba[ID] Access Size [Options] [Address [Passes]] ["CommandString"]`</li><li>`ba[ID] Access Size [Options] [Address [Passes]] ["CommandString"]`</li></ul>|
|`bc`|breakpoint clear|`bc Breakpoints`|
|`bd`|breakpoint disable|`bd Breakpoints`|
|`be`|breakpoint enable|`be Breakpoints`|
|`bl`|breakpoint list|`bl [/L] [Breakpoints]`|
|`bp`|set breakpoint|<ul><li>user-mode : `[~Thread] bp[ID] [Options] [Address [Passes]] ["CommandString"]`</li><li>kernel-mode : `bp[ID] [Options] [Address [Passes]] ["CommandString"]`</li></ul>|
|`bu`|Set Unresolved Breakpoint|<ul><li>user-mode : `[~Thread] bu[ID] [Options] [Address [Passes]] ["CommandString"]`</li><li>kernel-mode : `bu[ID] [Options] [Address [Passes]] ["CommandString"]`</li></ul>|
|`bm`|Set Symbol Breakpoint|<ul><li>user-mode : `[~Thread] bm [Options] SymbolPattern [Passes] ["CommandString"]`</li><li>kernel-mode : `bm [Options] SymbolPattern [Passes] ["CommandString"]`</li></ul>|
|`br`|breakpoint renumber|`br OldID NewID [OldID2 NewID2 ...] `|
|`bs`|update breakpoint command|`bs ID ["CommandString"]`|
|`bsc`|Update Conditional Breakpoint|`bsc ID Condition ["CommandString"] `|
|`c`|Compare Memory|`c Range Address `|
|<ul><li>`d`</li><li>`da`</li><li>`db`</li><li>`dc`</li><li>`dd`</li><li>`dD`</li><li>`df`</li><li>`dp`</li><li>`dq`</li><li>`du`</li><li>`dw`</li></ul>|Display Memory|<ul><li>`d{a\|b\|c\|d\|D\|f\|p\|q\|u\|w\|W} [Options] [Range]`</li><li>`dy{b\|d} [Options] [Range]`</li><li>`d [Options] [Range]`</li></ul>|
|<ul><li>`dda`</li><li>`ddp`</li><li>`ddu`</li><li>`dpa`</li><li>`dpp`</li><li>`dpu`</li><li>`dqa`</li><li>`dqp`</li><li>`dqu`</li></ul>|Display Referenced Memory|<ul><li>`ddp [Options] [Range]`</li><li>`dqp [Options] [Range]`</li><li>`dpp [Options] [Range]`</li><li>`dda [Options] [Range]`</li><li>`dqa [Options] [Range]`</li><li>`dpa [Options] [Range]`</li><li>`ddu [Options] [Range]`</li><li>`dqu [Options] [Range]`</li><li>`dpu [Options] [Range]`</li></ul>|
|<ul><li>`dds`</li><li>`dps`</li><li>`dqs`</li></ul>|Display Words and Symbols|<ul><li>`dds [Options] [Range]`</li><li>`dqs [Options] [Range]`</li><li>`dps [Options] [Range]`</li></ul>|
|`dg`|Display Selector|`dg FirstSelector [LastSelector]`|
|`dl`|Display Linked List|`dl[b] Address MaxCount Size`|
|<ul><li>`ds`</li><li>`dS`</li></ul>|Display String|`d{s\|S} [/c Width] [Address]`|
|`dt`|Display Type|<ul><li>user-mode</li><ul><li>`dt [-DisplayOpts] [-SearchOpts] [module!]Name [[-SearchOpts] Field] [Address] [-l List]`</li><li>`dt [-DisplayOpts] Address [-l List]`</li><li>`dt -h`</li></ul><li>kernel-mode</li><ul><li>`[Processor] dt [-DisplayOpts] [-SearchOpts] [module!]Name [[-SearchOpts] Field] [Address] [-l List]`</li><li>`dt [-DisplayOpts] Address [-l List]`</li><li>`dt -h`</li></ul></ul>|
|`dtx`|Display Type - Extended Debugger Object Model Information|`dtx -DisplayOpts [Module!]Name Address`|
|`dv`|Display Local Variables|`dv [Flags] [Pattern]`|
|`dx`|Display Debugger Object Model Expression|<ul><li>`dx [-g\|-gc #][-c #][-n\|-v]-r[#] Expression[,<FormatSpecifier> ]`</li><li>`dx [{-?}\|{-h}]`</li></ul>|
|<ul><li>`e`</li><li>`ea`</li><li>`eb`</li><li>`ed`</li><li>`eD`</li><li>`ef`</li><li>`ep`</li><li>`eq`</li><li>`eu`</li><li>`ew`</li><li>`eza`</li></ul>|Enter Values|<ul><li>`e{b\|d\|D\|f\|p\|q\|w} Address [Values]`</li><li>`e{a\|u\|za\|zu} Address "String"`</li><li>`e Address [Values]`</li></ul>|
|<ul><li>`f`</li><li>`fp`</li></ul>|Fill Memory|<ul><li>`f Range Pattern`</li><li>`fp [MemoryType] PhysicalRange Pattern`</li></ul>|
|`g`|Go|<ul><li>user-mode : `[~Thread] g[a] [= StartAddress] [BreakAddress ... [; BreakCommands]]`</li><li>kernel-mode : `g[a] [= StartAddress] [BreakAddress ... [; BreakCommands]]`</li></ul>|
|`gc`|Go from Conditional Breakpoint|`gc`|
|`gh`|Go with Exception Handled|<ul><li>user-mode : `[~Thread] gh[a] [= StartAddress] [BreakAddress ... [; BreakCommands]] `</li><li>kernel-mode : `gh[a] [= StartAddress] [BreakAddress ... [; BreakCommands]] `</li></ul>|
|<ul><li>`gn`</li><li>`gN`</li><ul>|Go with Exception Not Handled|<ul><li>user-mode</li><ul><li>`[~Thread] gn[a] [= StartAddress] [BreakAddress ... [; BreakCommands]]`</li><li>`[~Thread] gN[a] [= StartAddress] [BreakAddress ... [; BreakCommands]]`</li></ul><li>kerbel-mode</li><ul><li>`gn[a] [= StartAddress] [BreakAddress ... [; BreakCommands]]`</li><li>`gN[a] [= StartAddress] [BreakAddress ... [; BreakCommands]]`</li></ul></ul>|
|`gu`|Go Up|<ul><li>user-mode : `[~Thread] gu` </li><li>kernel-mode : `gu`</li></ul>|
|<ul><li>`ib`</li><li>`iw`</li><li>`id`</li></ul>|Input from Port|<ul><li>`ib Address`</li><li>`iw Address`</li><li>`id Address`</li></ul>|
|`j`|Execute If - Else|<ul><li>`j Expression Command1 ; Command2`</li><li>`j Expression 'Command1' ; 'Command2'`</li></ul>|
|<ul><li>`k`</li><li>`kb`</li><li>`kc`</li><li>`kd`</li><li>`kp`</li><li>`kP`</li><li>`kv`|Display Stack Backtrace|<ul><li>User-Mode, x86 Processor</li><ul><li>`[~Thread] k[b\|p\|P\|v] [c] [n] [f] [L] [M] [FrameCount]`</li><li>`[~Thread] k[b\|p\|P\|v] [c] [n] [f] [L] [M] = BasePtr [FrameCount]`</li><li>`[~Thread] k[b\|p\|P\|v] [c] [n] [f] [L] [M] = BasePtr StackPtr InstructionPtr`</li><li>`[~Thread] kd [WordCount]`</li></ul><li>Kernel-Mode, x86 Processor</li><ul><li>`[Processor] k[b\|p\|P\|v] [c] [n] [f] [L] [M] [FrameCount]`</li><li>`[Processor] k[b\|p\|P\|v] [c] [n] [f] [L] [M] = StackPtr FrameCount`</li><li>`[Processor] k[b\|p\|P\|v] [c] [n] [f] [L] [M] = BasePtr StackPtr InstructionPtr`</li><li>`[Processor] kd [WordCount]`</li></ul><li>User-Mode, x64 Processor</li><ul><li>`[~Thread] k[b\|p\|P\|v] [c] [n] [f] [L] [M] [FrameCount]`</li><li>`[~Thread] k[b\|p\|P\|v] [c] [n] [f] [L] [M] = StackPtr FrameCount`</li><li>`[~Thread] k[b\|p\|P\|v] [c] [n] [f] [L] [M] = StackPtr InstructionPtr FrameCount`</li><li>`[~Thread] kd [WordCount]`</li></ul><li>Kernel-Mode, x64 Processor</li><ul><li>`[Processor] k[b\|p\|P\|v] [c] [n] [f] [L] [M] [FrameCount]`</li><li>`[Processor] k[b\|p\|P\|v] [c] [n] [f] [L] [M] = StackPtr FrameCount`</li><li>`[Processor] k[b\|p\|P\|v] [c] [n] [f] [L] [M] = StackPtr InstructionPtr FrameCount`</li><li>`[Processor] kd [WordCount]`</li></ul><li>User-Mode, ARM Processor</li><ul><li>`[~Thread] k[b\|p\|P\|v] [c] [n] [f] [L] [M] [FrameCount]`</li><li>`[~Thread] k[b\|p\|P\|v] [c] [n] [f] [L] [M] = StackPtr FrameCount`</li><li>`[~Thread] k[b\|p\|P\|v] [c] [n] [f] [L] [M] = StackPtr InstructionPtr FrameCount`</li><li>`[~Thread] kd [WordCount]`</li></ul><li>Kernel-Mode, ARM Processor</li><ul><li>`[Processor] k[b\|p\|P\|v] [c] [n] [f] [L] [M] [FrameCount]`</li><li>`[Processor] k[b\|p\|P\|v] [c] [n] [f] [L] [M] = StackPtr FrameCount`</li><li>`[Processor] k[b\|p\|P\|v] [c] [n] [f] [L] [M] = StackPtr InstructionPtr FrameCount`</li><li>`[Processor] kd [WordCount]`</li></ul></ul>|
|<ul><li>`l+`</li><li>`l-`</li></ul>|Set Source Options|<ul><li>`l+Option`</li><li>`l-Option`</li><li>`l{+|-}`</li></ul>|
|`ld`|Load Symbols|`ld ModuleName [/f FileName]`|
|`lm`|List Loaded Modules|`lm Options [a Address] [m Pattern \| M Pattern]`|
|`ln`|List Nearest Symbols|<ul><li>`ln Address`</li><li>`ln /D Address`</li></ul>|
|<ul><li>`ls`</li><li>`lsa`</li></ul>|List Source Lines|<ul><li>`ls [.] [first] [, count]`</li><li>`lsa [.] address [, first [, count]]`</li></ul>|
|`lsc`|List Current Source|`lsc`|
|`lse`|Launch Source Editor|`lse`|
|<ul><li>`lsf`</li><li>`lsf-`</li></ul>|Load or Unload Source File|<ul><li>`lsf Filename`</li><li>`lsf- Filename`</li></ul>|
|`lsp`|Set Number of Source Lines|<ul><li>`lsp [-a] LeadingLines TrailingLines`</li><li>`lsp [-a] TotalLines`</li><li>`lsp [-a] `</li></ul>|
|`m`|Move Memory|`m Range Address `|
|`n`|Set Number Base|`n [Radix]`|
|<ul><li>`ob`</li><li>`ow`</li><li>`od`</li></ul>|Output to Port|<ul><li>`ob Address Value`</li><li>`ow Address Value`</li><li>`od Address Value`</li></ul>|
|`p`|Step|<ul><li>user-mode : `[~Thread] p[r] [= StartAddress] [Count] ["Command"]`</li><li>kernel-mode : `p[r] [= StartAddress] [Count] ["Command"]`</li></ul>|
|`pa`|Step to Address|<ul><li>user-mode : `[~Thread] pa [r] [= StartAddress] StopAddress ["Command"]`</li><li>kernel-mode : `pa [r] [= StartAddress] StopAddress ["Command"]`</li></ul>|
|`pc`|Step to Next Call|<ul><li>user-mode : `[~Thread] pc [r] [= StartAddress] [Count]`</li><li>kernel-mode : `pc [r] [= StartAddress] [Count]`</li></ul>|
|`pct`|Step to Next Call or Return|<ul><li>user-mode : `[~Thread] pct [r] [= StartAddress] [Count]`</li><li>kernel-mode : `pct [r] [= StartAddress] [Count]`</li></ul>|
|`ph`|Step to Next Branching Instruction|<ul><li>user-mode : `[~Thread] ph [r] [= StartAddress] [Count]`</li><li>kernel-mode : `ph [r] [= StartAddress] [Count]`</li></ul>|
|`pt`|Step to Next Return|<ul><li>user-mode : `[~Thread] pt [r] [= StartAddress] [Count] ["Command"]`</li><li>kernel-mode : `pt [r] [= StartAddress] [Count] ["Command"]`</li></ul>|
|<ul><li>`q`</li><li>`qq`</li></ul>|Quit|<ul><li>`q`</li><li>`qq`</li></ul>|
|`qd`|Quit and Detach|`qd`|
|`r`|Registers|<ul><li>user-mode</li><ul><li>`[~Thread] r[M Mask\|F\|X\|?] [ Register[:[Num]Type] [= [Value]] ]`</li><li>`r.`</li></ul><li>kernel-mode</li><ul><li>`[Processor] r[M Mask\|F\|X\|Y\|YI\|?] [ Register[:[Num]Type] [= [Value]] ]`</li><li>`r.`</li></ul></ul>|
|`rdmsr`|Read MSR|`rdmsr Address`|
|`rm`|Register Mask|<ul><li>`rm`</li><li>`rm ?`</li><li>`rm Mask`</li></ul>|
|`s`|Search Memory|<ul><li>`s [-[[Flags]Type]] Range Pattern`</li><li>`s -[[Flags]]v Range Object`</li><li>`s -[[Flags]]sa Range`</li><li>`s -[[Flags]]su Range`</li></ul>|
|`so`|Set Kernel Debugging Options|`so [Options]`|
|`sq`|Set Quiet Mode|<ul><li>`sq`</li><li>`sq{e|d}`</li></ul>|
|`ss`|Set Symbol Suffix|`ss [a|w|n]`|
|<ul><li>`sx`</li><li>`sxd`</li><li>`sxe`</li><li>`sxi`</li><li>`sxn`</li><li>`sxr`</li><li>`sx-`</li></ul>|Set Exceptions|<ul><li>`sx`</li><li>`sx{e|d|i|n} [-c "Cmd1"] [-c2 "Cmd2"] [-h] {Exception|Event|*}`</li><li>`sx- [-c "Cmd1"] [-c2 "Cmd2"] {Exception|Event|*}`</li><li>`sxr`</li></ul>|
|`t`|Trace|<ul><li>user-mode : `[~Thread] t [r] [= StartAddress] [Count] ["Command"]`</li><li>kernel-mode : `t [r] [= StartAddress] [Count] ["Command"]`</li></ul>|
|`ta`|Trace to Address|<ul><li>user-mode : `[~Thread] ta [r] [= StartAddress] StopAddress`</li><li>kernel-mode : `ta [r] [= StartAddress] StopAddress`</li></ul>|
|`tb`|Trace to Next Branch|`tb [r] [= StartAddress] [Count]`|
|`tc`|Trace to Next Call|<ul><li>user-mode : `[~Thread] tc [r] [= StartAddress] [Count]`</li><li>kernel-mode : `tc [r] [= StartAddress] [Count]`</li></ul>|
|`tct`|Trace to Next Call or Return|<ul><li>user-mode : `[~Thread] tct [r] [= StartAddress] [Count]`</li><li>kernel-mode : `tct [r] [= StartAddress] [Count]`</li></ul>|
|`th`|Trace to Next Branching Instruction|<ul><li>user-mode : `[~Thread] th [r] [= StartAddress] [Count]`</li><li>kernel-mode : `th [r] [= StartAddress] [Count]`</li></ul>|
|`tt`|Trace to Next Return|<ul><li>user-mode : `[~Thread] tt [r] [= StartAddress] [Count]`</li><li>kernel-mode : `tt [r] [= StartAddress] [Count]`</li></ul>|
|<ul><li>`u`</li><li>`ub`</li><li>`uu`</li></ul>|Unassemble|<ul><li>`u[u|b] Range`</li><li>`u[u|b] Address`</li><li>`u[u|b]`</li></ul>|
|`uf`|Unassemble Function|`uf [Options] Address`|
|`up`|Unassemble from Physical Memory|<ul><li>`up Range`</li><li>`up Address`</li><li>`up `</li></ul>|
|`ur`|Unassemble Real Mode BIOS|<ul><li>`ur Range`</li><li>`ur Address`</li><li>`ur`</li></ul>|
|`ux`|Unassemble x86 BIOS|`ux [Address]`|
|`vercommand`|Show Debugger Command Line|`vercommand`|
|`version`|Show Debugger Version|`version`|
|`vertarget`|Show Target Computer Version|`vertarget`|
|`wrmsr`|Write MSR|`wrmsr Address Value`|
|`wt`|Trace and Watch Data|`wt [WatchOptions] [= StartAddress] [EndAddress]`|
|`x`|Examine Symbols|<ul><li>`x [Options] Module!Symbol`</li><li>`x [Options] *`</li></ul>|
|`z`|Execute While|<ul><li>user-mode : `Command ; z( Expression )`</li><li>kernel-mode : `Command ; [Processor] z( Expression )`</li></ul>|

