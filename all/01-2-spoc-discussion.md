# 操作系统概述思考题

## 个人思考题

---

分析你所认识的操作系统（Windows、Linux、FreeBSD、Android、iOS）所具有的独特和共性的功能？
- [x]  

>    我个人没有使用过FreeBSD，下面就其他四个操作系统做分析。
     Windows与Linux、Android与iOS乍一看就是两组系统，分别作用于PC端和移动端。首先讨论一下Linux与Windows，我个人觉得就用户角度而言，Windows系统本身的存在感很低。使用这个系统时人们感觉他更多的是一个运行应用的平台，而非一个调用行为的管理员。这样的设计可能更加符合微软普及Windows的设计初衷吧，从完善的图形界面，到完善的接口与兼容，作为一款商业软件，它很好地体现了操作简便与封装完善的特点。相比下Linux的用户相对专业许多，通过Linux能发挥出更多操作系统具体控制各个任务执行的功能。Linux创造之初就秉承着高效灵活的设计需求，并落实的非常好。无论是采用带链接的树形目录结构文件管理系统，分时管理方法实现所有的任务共同分享系统资源，还是多道和分时提高处理机使用率，都是高效与灵活的具体实现。当然，现在无论是Linux与Windows在功能上都已经越来越来完善，就实现而言，并不存在必须采用哪种的说法。
     关于Android与iOS，很多人坚持认为Android完全没有竞争的资格。虽说iOS的确在很多地方的优化非常细致用心，但Android也大有自己的优势与生存知道。iOS的优势很多，非常显著的就是更加完善的安全机制。懂得Recovery模式的人一下子就可以直接闯入系统内部为所欲为，而苹果的产品每爆出安全漏洞往往能被当作新闻看待。此外，在节电续航iOS也有自己的优化，采用独立唤醒技术，以及为处理器量身定制的芯片，在待机时更省电，使用时的耗电详情呈“线性”趋势。相比某些Android设备，待机耗电可以节省70%。当然了，Android也有大量的设计亮点，最显然的一点就是容量问题。Android设备连接电脑后可读写手机内建存储和外置SD卡的内容。机身容量的不足能轻易得通过外接储存卡解决。而iOS除了读取相片外只能通过iTunes传输其他内容，虽然后者的安全性要比前者高，但很多用户经常对额外rom必须付相当高昂价格不满。另外，在针对多任务支持上，Android很早就实打实得完成了这个工作。而iOS在这点上向来“真假难辨”、“众说纷纭”。据说到iOS8方完全支持。不过还是这句话，无论是哪个系统在功能的支持上都基本没有什么盲区了，都能胜任移动设备面临的问题。
   

请总结你认为操作系统应该具有的特征有什么？并对其特征进行简要阐述。
- [x]  

>   操作系统设计得初衷是为了达到方便用户和提高利用率的目的。它是控制和管理计算机硬件和软件资源，合理的组织计算机工作流程的程序的集合。具有并发。共享，虚拟，异步性四个基本特征。其中最基本的特征为并发性。

请给出你觉得的更准确的操作系统的定义？
- [x]  

>   操作系统是管理和控制计算机硬件与软件资源的计算机程序，是直接运行在硬件上的最基本的系统软件，任何其他软件都必须在操作系统的支持下才能运行。它是用户和计算机的接口，同时也是计算机硬件和其他软件的接口。

你希望从操作系统课学到什么知识？
- [x]  

>   学习操作系统基本工作原理啊，在程序控制、人机交互、进程管理、内存管理、虚拟内存、用户接口、用户界面方面的具体表现以及功能实现。观察学习一款具体的操作系统，尝试掌握写一部分代码。

---

## 小组讨论题

---

目前的台式PC机标准配置和价格？
- [x]  

> 主流台式机主流指标标准配置如下：
推荐用途	家用电脑
平台	Intel平台
操作系统	Windows 7 家庭普通版
机箱类型	大机箱
主板
显卡类型	独立显卡
声卡	集成声卡
网卡	集成10/100/1000以太网卡+Dell 无线-N 1705 2.4GHz + 蓝牙 4.0
CPU
类型	Intel
速度	3.2 GHz，最大 Turbo 频率 3.4 GHz
CPU型号	i5-4460
核心数	四核
三级缓存	6MB
显卡
显示芯片	NVIDIA GeForce 705
显存容量	独立1GB
显存规格	DDR3
内存
容量	8GB 双通道 DDR3 1600MHz (4GBx2)
速度	DDR3 1600Mhz
插槽数量	2个
最大支持容量	8GB
硬盘
容量	1TB 7200 rpm SATA 6Gb/s
类型	SATA 串行
转速	7200转/分钟
价格一般在5000左右

你理解的命令行接口和GUI接口具有哪些共性和不同的特征？
- [x]  

> 命令行与GUI本身从功能而言并无区别，都能够完成对操作系统地调用本完成相应的系统命令，这可以算作两者共性。
  而相比命令行，图形界面提供更好的操作体验，方便用户上手。但不方便批处理，仅适合个人常态低负荷使用。
  命令行虽然上手不易，但熟练掌握后能极大的提升执行效率，方便大规模高负荷调用执行。

为什么现在的操作系统基本上用C语言来实现？
- [x]  

>  从理论的角度说，c语言具有跨平台，编译型，方便嵌入，兼容性好，方便扩展，开放者等众多非常适合作为操作系统开发语言的特征。其次，由于最早的操作系统使用c语言开发，C语言开发积累了大量资料案例，对后来的开发项目提供了很好的参考与引导。

为什么没有人用python，java来实现操作系统？
- [x]  

>  pyhton是解释性的脚本语言，执行起来速度非常慢。而且Python优势在于强大的文字处理功能，用于设计操作系统不符合语言设计初衷。java其实是可以写操作系统的，Android本身就是用java写的。但是一般来说，我们认为java不方便融入硬件功能，不方便嵌入汇编中，不太适合操作系统。

请评价用C++来实现操作系统的利弊？
- [x]  

>  C++兼具了高效率与面向对象的特性，能在保证效率的情况下更加好的维护好的设计模式，保证体系结构更加分明。
但是C++引入的这些特性与接口更好的服务了上层调用，但作为一个内核级别的代码，追求精确地实现，精确控制代码体积。而且兼容性变差，与二进制代码之间对应关系减弱。

---

## 开放思考题

---

请评价微内核、单体内核、外核（exo-kernel）架构的操作系统的利弊？
- [x]  

>  单体内核：性能最快，效率最高，在一大块代码中实际包含了所有操作系统功能，并作为一个单一进程运行，具有唯一地址空间。但是内核层次可能很复杂很冗余；
   微内核：仅在内核里保留最核心的功能，如进程间通信和对硬件的支持，地址空间分配和基本的调度，灵活，可扩展，安全，但是性能大幅下降；
   外核：相比微内核进一步精简，仅保留资源的保护和隔离，管理交给用户态，这样可以链接到操作系统库，实现操作系统抽象，很像虚拟机，更灵活。

请评价用LISP,OCcaml, GO, D，RUST等实现操作系统的利弊？
- [x]  

>  

进程切换的可能实现思路？
- [x]  

>  可以采用分时与多和两种方案：
   分时即风格时间片，令每个进程占用一个时间片的cpu资源。。
   多核则是加强硬件支持，利用成百上千的核的超级计算机，或者是的GPU组成的新的集群。每个核负责一个进程的计算。虽然削弱了计算能力，但是可以并发处理的任务变多了。

计算机与终端间通过串口通信的可能实现思路？
- [x]  

>  用三根线结构：一根收，一根发，一根地线

为什么微软的Windows没有在手机终端领域取得领先地位？
- [x]  

>  这和微软公司的战略方针有关。其实在很早以前就有Windows Mobile，但是微软Windows  Mobile的战略思路是：使大量原有企业用户也能使用该手机平台，以提高企业用户员工的工作效率。但加拿大智能手机制造商RIM在其黑莓 (BlackBerry)智能手机中提供了性能更好的解决方案，从而导致Windows Mobile市场份额开始下滑。随后苹果iPhone横空出世，使处境本已艰难的Windows Mobile雪上加霜。之后iOS与Android两个更适合移动设备的操作系统不断完善，使得早先搭载Windows系统的上一代智能机完全溃败。后起步的windows phone无论在生态环境还是用户体验上一时半会也无法与苹果三星等抗衡，所以至今在手机端领域的位置依旧尴尬。

你认为未来（10年内）的操作系统应该具有什么样的特征和功能？
- [x]  

>  

---
