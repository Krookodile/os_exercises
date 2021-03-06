#lec 3 SPOC Discussion

## 第三讲 启动、中断、异常和系统调用-思考题

## 3.1 BIOS
 1. 比较UEFI和BIOS的区别。
 >  UEFI是在BIOS之后发展起来的一种加载接口，全称“统一的可扩展固件接口”是一种详细描述类型接口的标准。这种接口用于操作系统自动从预启动的操作环境，加载到一种操作系统上。与BIOS显著不同的是，UEFI是用模块化、C语言风格的参数堆栈传递方式、动态链接的形式构建系统，它比BIOS更易于实现，容错和纠错特性也更强，从而缩短了系统研发的时间。此外，BIOS对于不同系统的启动流程标准不能统一支持，而是用UEFI接口能在所有平台上提供一致的操作系统启动服务。

 2. 描述PXE的大致启动流程。
 >  PXE是一种网络启动服务，不同于一般的BIOS，PXE启动是远程启动，是通过本地网卡通迅直接启动远程服务器上的镜像系统,         而不依赖于本地硬盘。

## 3.2 系统启动流程
 1. 了解NTLDR的启动流程。

>  NTLDR是NT内核操作系统的启动器，主引导记录在找到NTLDR之后初始化并启动NTLDR，并把系统控制权转移给启动器。之后就开始由    NTLDR组织引导过程。初始引导加载器阶段中，NTLDR将NT系统从计算机的微处理器从实模式转换为32位平面内存模式，再执行适当    的小型文件系统驱动程序识别每一个用NTFS或FAT格式的文件系统分区，在活动分区根目录寻找并加载Boot.ini文件。通过Boot.ini    文件的配置，NTLDR展开之后的启动流程。在处理完boot.ini文件之后，ntldr会启动ntdetect.com程序。在基于X86的系统中，ntde    tect.com会通过调用系统固件程序收集安装的硬件信息，然后由ntdetect.com将收集的计算机硬件信息列表并将列表返回到ntldr。    Ntldr获取从ntdetect.com发来的信息后，将这些信息组织成为内部结构形式，然后由ntldr启动ntoskrnl.exe程序,并将这些信息和    boot.ini文件中的信息，以及注册表中的硬件和软件信息传递给ntoskrnl.exe 程序，即下放给控制权传递给Windows XP内核。
   至此，Ntldr启动任务完成。

 2. 了解GRUB的启动流程。

>  Grub的实质是一个mini os，它拥有shell，支持script，支持特定文件系统。grub由stage1，stage1-5，stage2以及/boot/grub目    录下的诸多文件（包括Grub的配置文件与相关文件系统定义文件等）组成，其核心是stage2，主要功能在于完成操作系统的引导工    作。计算机启动时，BIOS加载硬盘主引导扇区总共512字节的二进制代码，执行IPL，IPL就是存储于446个字节的MBR中，是GRUB的第    一个部分（stage1）。stage1的工作是加载stage1_5（ext3文件系统就是e2fs_stage1_5），它位于0面0道第2扇区开始的十几个扇    区内。stage1_5运行后，就可以识别/boot所在分区的文件系统了。当加载并运行了stage2后grub，会根据menulist或用户输入加载    kernel，然后控制权就转到Linux了。
 
 3. 比较NTLDR和GRUB的功能有差异

>   从功能上说，Grub是一个更加强大的引导管理器。ntldr只是为windows NT系统专门配置的引导文件，而Grub是多系统引导管理器，许多多系统以及linux环境都在使用Grub作为引导。

 4. 了解u-boot的功能。

>   根据一些资料，u-boot的功能主要包括：
    系统引导支持NFS挂载、RAMDISK(压缩或非压缩)形式的根文件系统；
    支持NFS挂载、从FLASH中引导压缩或非压缩系统内核；
    基本辅助功能强大的操作系统接口功能；
    可灵活设置、传递多个关键参数给操作系统，适合系统在不同开发阶段的调试要求与产品发布，尤以Linux支持最为强劲；
    支持目标板环境参数多种存储方式，如FLASH、NVRAM、EEPROM；
    CRC32校验可校验FLASH中内核、RAMDISK镜像文件是否完好；
    设备驱动串口、SDRAM、FLASH、以太网、LCD、NVRAM、EEPROM、键盘、USB、PCMCIA、PCI、RTC等驱动支持；
    上电自检功能SDRAM、FLASH大小自动检测；SDRAM故障检测；CPU型号；
    特殊功能XIP内核引导；
 

## 3.3 中断、异常和系统调用比较
 1. 举例说明Linux中有哪些中断，哪些异常？
 
>   中断是指会改变处理器执行指令的顺序，通常与CPU芯片内部或外部硬件电路产生的电信号相对应一种事件。
    中断可以分为异步中断和同步中断。异步中断由硬件随机产生，在程序执行的任何时候可能出现。
    同步中断由在（特殊的或出错的）指令执行时由CPU控制单元产生的。
    异常一般也可以分为两种：处理器探测异常与编程异常
    处理器探测异常指由CPU执行指令时探测到一个反常条件时产生，如溢出、除0错等
    编程异常指由编程者发出的特定请求产生，通常由int类指令触发


 2. Linux的系统调用有哪些？大致的功能分类有哪些？  (w2l1)
 
 >  Linux下的系统调用种类繁多，在2.4.4版本中，狭义上的系统调用有244条之多。而广义上的以库函数形式实现的系统调用，可能     根本没有人统计过，至少也是数百条甚至上千条的。
    在常见的系统调用列表中，一般都会按进程控制、文件系统控制、系统控制、内存管理、网络管理、socket控制、用户管理、进程     间通信等方面来列举。这就很好地说明了Linux提供的系统调用功能也大多是按照这八个方面来展开的。系统调用由操作系统核心     提供，运行于核心态。应用程序利用系统调用访问自身无法直接获得的资源，运用这些系统命为上层应用程序提供快捷方便而且安     全可信的支持，并提高效率，优于许多c语言内库中提供的实现。

 ```
  + 采分点：说明了Linux的大致数量（上百个），说明了Linux系统调用的主要分类（文件操作，进程管理，内存管理等）
  - 答案没有涉及上述两个要点；（0分）
  - 答案对上述两个要点中的某一个要点进行了正确阐述（1分）
  - 答案对上述两个要点进行了正确阐述（2分）
  - 答案除了对上述两个要点都进行了正确阐述外，还进行了扩展和更丰富的说明（3分）
 ```
 
 3. 以ucore lab8的answer为例，uCore的系统调用有哪些？大致的功能分类有哪些？(w2l1)
 
>   在lab8_result/kern/syscall/syscall.c中，我们可以清楚地看到有22条系统调用。这22条命令主要涉及的功能有进程控制（如fo     rk、exit、exec、getpid）、进程间通信(kill)、系统控制（gettime）、文件系统控制（read、write、dup、getdirentry）、内     存管理（fstat、fsync）等方面。
    基本涵盖了操作系统所必须承载的接口调用，为上层应用调用外部设备资源提供了安全、可靠、高效的系统支持。
 
 
 ```
  + 采分点：说明了ucore的大致数量（二十几个），说明了ucore系统调用的主要分类（文件操作，进程管理，内存管理等）
  - 答案没有涉及上述两个要点；（0分）
  - 答案对上述两个要点中的某一个要点进行了正确阐述（1分）
  - 答案对上述两个要点进行了正确阐述（2分）
  - 答案除了对上述两个要点都进行了正确阐述外，还进行了扩展和更丰富的说明（3分）
 ```
 
## 3.4 linux系统调用分析
 1. 通过分析[lab1_ex0](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab1/lab1-ex0.md)了解Linux应用的系统调用编写和含义。(w2l1)

     objdump是用查看目标文件或者可执行的目标文件的构成的GCC工具，利用这个工具可以轻松的看到目标文件与源代码之间的丝丝      的关系。objdump功能强大，这里的分析分别用了-d （反汇编），-h（段概括）两个模式来观察。
     首先是反汇编模式：
     ```
     lab1-ex0.exe:     file format elf64-x86-64
     Disassembly of section .init:
     00000000004003a8 <_init>:
     4003a8:	48 83 ec 08          	sub    $0x8,%rsp
     4003ac:	48 8b 05 45 0c 20 00 	mov    0x200c45(%rip),%rax        # 600ff8 <_DYNAMIC+0x1d0>
     4003b3:	48 85 c0             	test   %rax,%rax
     4003b6:	74 05                	je     4003bd <_init+0x15>
     4003b8:	e8 33 00 00 00       	callq  4003f0 <__gmon_start__@plt>
     4003bd:	48 83 c4 08          	add    $0x8,%rsp
     4003c1:	c3                   	retq   
     ```
     这里就是.init部分的汇编代码。原文全部汇编代码很长，这里摘录一段，这里有一个调用库函数操作。
     
      nm命令用来列出目标文件的符号清单：（一部分结果）
```
0000000000000002 a AF_INET
000000000060105c B __bss_start
000000000060105c b completed.6972
0000000000601028 D __data_start
0000000000601028 W data_start
0000000000400430 t deregister_tm_clones
00000000004004a0 t __do_global_dtors_aux
0000000000600e18 t __do_global_dtors_aux_fini_array_entry
0000000000601030 D __dso_handle
0000000000600e28 d _DYNAMIC
000000000060105c D _edata
0000000000601060 B _end
0000000000400564 T _fini
00000000004004c0 t frame_dummy
0000000000600e10 t __frame_dummy_init_array_entry
0000000000400670 r __FRAME_END__
0000000000601000 d _GLOBAL_OFFSET_TABLE_
                 w __gmon_start__
0000000000601038 d hello
00000000004003a8 T _init
0000000000600e18 t __init_array_end
0000000000600e10 t __init_array_start
0000000000400570 R _IO_stdin_used
0000000000000006 a IPPROTO_TCP
                 w _ITM_deregisterTMCloneTable
                 w _ITM_registerTMCloneTable
0000000000600e20 d __JCR_END__
……
```
 
 file命令用于确定文件类型：（一部分输出）
 ```
lab1-ex0.exe: ELF 64-bit LSB  executable, x86-64, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.24, BuildID[sha1]=337b4a665f50f5ae7931487124df3038c7318f17, not stripped
```
 系统调用是应用程序同系统之间的接口，由操作系统提供。
 操作系统的主要功能是为管理硬件资源和为应用程序开发人员提供良好的环境来使应用程序具有更好的兼容性，为了达到这个目的，内核提供一系列具备预定功能的多内核函数，通过一组称为系统调用（system call)的接口呈现给用户。 系统调用把应用程序的请求传给内核，调用相应的的内核函数完成所需的处理，将处理结果返回给应用程序。

 ```
  + 采分点：说明了objdump，nm，file的大致用途，说明了系统调用的具体含义
  - 答案没有涉及上述两个要点；（0分）
  - 答案对上述两个要点中的某一个要点进行了正确阐述（1分）
  - 答案对上述两个要点进行了正确阐述（2分）
  - 答案除了对上述两个要点都进行了正确阐述外，还进行了扩展和更丰富的说明（3分）
 
 ```
 
 1. 通过调试[lab1_ex1](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab1/lab1-ex1.md)了解Linux应用的系统调用执行过程。(w2l1)
 
>
     strace常用来跟踪进程执行时的系统调用和所接收的信号。它可以跟踪到一个进程产生的系统调用,包括参数，返回值，执行消耗的时间。（下面是一部分输出结果）
```
hello world
% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ----------------
 27.97    0.000087          29         3         3 access
 21.22    0.000066          33         2           open
 10.61    0.000033          33         1           brk
 10.29    0.000032          11         3           fstat
  9.97    0.000031           4         8           mmap
  6.11    0.000019           5         4           mprotect
  5.47    0.000017          17         1           execve
  3.54    0.000011          11         1           munmap
  2.89    0.000009           9         1           write
  0.96    0.000003           3         1           read
  0.64    0.000002           1         2           close
  0.32    0.000001           1         1           arch_prctl
------ ----------- ----------- --------- --------- ----------------
100.00    0.000311                    28         3 total
```
   /proc/interrupts是中断报告文件，more命令类似cat，这条命令可以观察其他的中断信息：（一部分输出）
```
            CPU0       CPU1       CPU2       CPU3       
  0:         17          0          0          0   IO-APIC-edge      timer
  1:        160       1961         91        122   IO-APIC-edge      i8042
  8:          0          1          0          0   IO-APIC-edge      rtc0
  9:        441        249         70        135   IO-APIC-fasteoi   acpi
 12:        896       9148        754        801   IO-APIC-edge      i8042
 16:       4563      50434        514       5416   IO-APIC-fasteoi   ehci_hcd:us
b1
 19:          0          0          0          0   IO-APIC-fasteoi   mmc0, jmb38
x_ms:slot0
 23:          8         66          1          1   IO-APIC-fasteoi   ehci_hcd:us
b2
 41:          0          0          0          0   PCI-MSI-edge      xhci_hcd
 42:       6689      29014       1761      10658   PCI-MSI-edge      ahci
 43:         13          0          0          0   PCI-MSI-edge      mei_me
 44:      65566        360         22         58   PCI-MSI-edge      iwlwifi
 45:          5         15          1          1   PCI-MSI-edge      nouveau
 46:       8250      76775       9019       9343   PCI-MSI-edge      i915
 47:         34        328          4          7   PCI-MSI-edge      snd_hda_int
el
 48:         52        189      73017         37   PCI-MSI-edge      eth0
NMI:          6          7          9          7   Non-maskable interrupts
LOC:      96435     105801      86782      75210   Local timer interrupts
SPU:          0          0          0          0   Spurious interrupts
PMI:          6          7          9          7   Performance monitoring interr
upts
```
 
   执行过程：（中断->系统调用号->系统调用表->函数库->应用程序）
   我们通过软件中断0x80，系统会跳转到一个预设的内核空间地址，它指向了系统调用处理程序，即在arch/i386/kernel/entry.S文件中使用汇编语言编写的system_call函数。
   软中断指令int 0x80执行时，系统调用号会被放进eax寄存器，同时，sys_call_table每一项占用4个字节。这样，system_call函数可以读取eax寄存器获得当前系统调用的系统调用号，将其乘以4天生偏移地址，然后以sys_call_table为基址，基址加上偏移地址所指向的内容即是应该执行的系统调用服务例程的地址。
 
 ```
  + 采分点：说明了strace的大致用途，说明了系统调用的具体执行过程（包括应用，CPU硬件，操作系统的执行过程）
  - 答案没有涉及上述两个要点；（0分）
  - 答案对上述两个要点中的某一个要点进行了正确阐述（1分）
  - 答案对上述两个要点进行了正确阐述（2分）
  - 答案除了对上述两个要点都进行了正确阐述外，还进行了扩展和更丰富的说明（3分）
 ```
 
## 3.5 ucore系统调用分析
 1. ucore的系统调用中参数传递代码分析。
 2. ucore的系统调用中返回结果的传递代码分析。
 3. 以ucore lab8的answer为例，分析ucore 应用的系统调用编写和含义。
 4. 以ucore lab8的answer为例，尝试修改并运行ucore OS kernel代码，使其具有类似Linux应用工具`strace`的功能，即能够显示出应用程序发出的系统调用，从而可以分析ucore应用的系统调用执行过程。
 
## 3.6 请分析函数调用和系统调用的区别
 1. 请从代码编写和执行过程来说明。

>  从代码编写上说，函数调用与系统调用使用的就是两套指令集。函数调用采用call、ret指令；系统调用使用int，iret。从功能实    现上说，常规函数调用没有堆栈切换，而系统调用需要到内核态执行，需要切换到内部的堆栈，防止被用户代码修改。系统调用更    加安全可靠，但是也会带来更大的开销。
 
 2. 说明`int`、`iret`、`call`和`ret`的指令准确功能

>  RET指令应安排在过程的出口即过程的最后一条指令处，它的功能是从堆栈顶部弹出由CALL指令压入的断点地址值，迫使CPU返回到    调用程序的断点去继续执行。
   IRET指令总是安排在中断服务程序的出口处，由它控制从堆栈中弹出程序断点送回CS和IP中，弹出标志寄存器内容送回F中，迫使CP    U返回到断点去继续执行后续程序。
   int指令其格式为：int n  n为中断类型码，它的功能是引发中断过程。CPU执行int n指令，相当于引发一个n号中断的中断过程。
   通过中断类型码n，可以在程序中使用int指令调用任何一个中断的中断处理程序。
   call指令就是一个压栈转移指令。当CPU 执行call指令时，先把当前的IP或CS和IP压入栈中，再转移到对应的位置开始执行指令。
 
 

