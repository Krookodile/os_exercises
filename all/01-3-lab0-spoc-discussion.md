# lab0 SPOC思考题

## 个人思考题

---

能否读懂ucore中的AT&T格式的X86-32汇编语言？请列出你不理解的汇编语言。
- [x]  

>  http://www.imada.sdu.dk/Courses/DM18/Litteratur/IntelnATT.htm
   基本能看懂，但是在Memoery Operand部分还是有一些不熟悉。 
   instr   %segreg:disp(base,index,scale),foo
   上面这条语句就不太理解。

虽然学过计算机原理和x86汇编（根据THU-CS的课程设置），但对ucore中涉及的哪些硬件设计或功能细节不够了解？
- [x]  

>  ucore中设计的知识包括文件系统、虚实地址转换、互斥锁、中断、分时、内存管理等。我觉得文件管理系统与内存管理部分我不是    很熟悉，需要花时间好好学习。


哪些困难（请分优先级）会阻碍你自主完成lab实验？
- [x]  

>  由于代码阅读要求量非常大，所以不能理解代码可能会极大程度的影响对lab实验的完成。
   此外，考虑到大多数内核对c用法要求与之前遇到有所不同，所以花在学习相应用法上的时间也会造成一些影响。

如何把一个在gdb中或执行过程中出现的物理/线性地址与你写的代码源码位置对应起来？
- [x]  

>   利用GDB工具的自带功能就可以实现物理地址与实际源代码位置之间的翻译工作。
    GDB提供了两种能力：显示源代码位置与指令地址之间的映射；显示指定位置的汇编代码。
    info line linespec：显示源代码linespec处对应的汇编地址范围。
    info line *addr：显示地址addr处对应的源代码位置。

了解函数调用栈对lab实验有何帮助？
- [x]  

>   

你希望从lab中学到什么知识？
- [x]  

>   

---

## 小组讨论题

---

搭建好实验环境，请描述碰到的困难和解决的过程。
- [x]  

> 

熟悉基本的git命令行操作命令，从github上的[ucore git repo](http://www.github.com/chyyuu/ucore_lab)下载ucore lab实验
- [x]  

> 

尝试用qemu+gdb（or ECLIPSE-CDT）调试lab1
- [x]  

> 

对于如下的代码段，请说明”：“后面的数字是什么含义
```
/* Gate descriptors for interrupts and traps */
struct gatedesc {
    unsigned gd_off_15_0 : 16;        // low 16 bits of offset in segment
    unsigned gd_ss : 16;            // segment selector
    unsigned gd_args : 5;            // # args, 0 for interrupt/trap gates
    unsigned gd_rsv1 : 3;            // reserved(should be zero I guess)
    unsigned gd_type : 4;            // type(STS_{TG,IG32,TG32})
    unsigned gd_s : 1;                // must be 0 (system)
    unsigned gd_dpl : 2;            // descriptor(meaning new) privilege level
    unsigned gd_p : 1;                // Present
    unsigned gd_off_31_16 : 16;        // high bits of offset in segment
};
```
- [x]  

> 表示这些变量在segment中所占据的位数

对于如下的代码段，
```
#define SETGATE(gate, istrap, sel, off, dpl) {            \
    (gate).gd_off_15_0 = (uint32_t)(off) & 0xffff;        \
    (gate).gd_ss = (sel);                                \
    (gate).gd_args = 0;                                    \
    (gate).gd_rsv1 = 0;                                    \
    (gate).gd_type = (istrap) ? STS_TG32 : STS_IG32;    \
    (gate).gd_s = 0;                                    \
    (gate).gd_dpl = (dpl);                                \
    (gate).gd_p = 1;                                    \
    (gate).gd_off_31_16 = (uint32_t)(off) >> 16;        \
}
```
如果在其他代码段中有如下语句，
```
unsigned intr;
intr=8;
SETGATE(intr, 0,1,2,3);
```
请问执行上述指令后， intr的值是多少？
- [x]  

> 65538

请分析 [list.h](https://github.com/chyyuu/ucore_lab/blob/master/labcodes/lab2/libs/list.h)内容中大致的含义，并能include这个文件，利用其结构和功能编写一个数据结构链表操作的小C程序
- [x]  

> [list.h]中实现了一个简单的双端链表结构，支持简单的初始化，删除，插入（作为前后元素），清空以及返回某个元素的前后继的   基本功能。下面是用这个头文件实现的一个简单的数据结构。
  利用该头文件 简单写了一个小程序：
  ```
  #include<iostream>
  #include<stdlib>
  #include "list.h"
  using namespace std;
  int main()
  {
     //构造节点 
     list_entry_t* node0=new list_entry_t;
     list_entry_t* node1=new list_entry_t;
     list_entry_t* node2=new list_entry_t;
     //构造链表 分别用了前插后插两种
     list_init(node1);
     list_add(node1, node2);
     list_add_before(node1, node0);
     //检验结果
     cout<<node0<<endl;
     list_entry_t* te=node0;
     for（int i=1; i<3; ++i）
     {
        cout<<list_next(te)<<' '<<elm3;
        te=list_next(te);
      }
     return 0;   
  }    
  ```

---

## 开放思考题

---

是否愿意挑战大实验（大实验内容来源于你的想法或老师列好的题目，需要与老师协商确定，需完成基本lab，但可不参加闭卷考试），如果有，可直接给老师email或课后面谈。
- [x]  

>  

---
