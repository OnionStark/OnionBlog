---
title: 并行与分布式计算课程笔记
date: 2020-08-31 13:44:18
categories: CS基础
---

# 引言

## 并行的维度

* 位级别：32位ALU
* 指令级别：流水线、超标量、VLIW
* 数据级别：SIMD
* 线程级别：芯片多处理器
* 作业级别：云计算

## 不同计算模式的区别

* 集中计算：所有计算机资源集中在一个物理系统中
* 并行计算：所有处理器要么与集中共享内存紧密耦合，要么与分布式内存松散耦合
* 分布式计算：由多台自主计算机组成
* 云计算：虚拟资源

## 阿姆达尔定律

**加速比=没有改进前的算法耗时/改进后的算法耗时**

公式：

![image-20200826170340392](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082617-03-40-08c39f4be05ffaf227dcab5a99c3ecbd-image-20200826170340392-80be9f.png)

Fraction表示可以并行的比例，Speedup表示这部分并行的代码能够加速多少倍

例子：

![image-20200826170357245](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082617-03-57-dba2009971638dcf71ea6b2a524c7778-image-20200826170357245-6ad454.png)

串行代码比例决定了最大加速比

## 摩尔定律

自从集成电路发明以来，每平方英寸集成电路上的晶体管数量每年都翻一番，英特尔联合创始人戈登·摩尔（Gordon Moore）1965年提出的这一观察结果。

# 指令级别的并行（ILP）

## CPU性能公式

![image-20200826171831217](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082617-18-31-f4a92ae3137d3ff44976634d1eabd948-image-20200826171831217-295650.png)

## 如何减少CPU计算时间

* 提高CPU频率

  流水线

* 增加IPC（每个时钟周期的指令数）

  * 超标量


  * VLIW（极长指令字）

* 减少程序的指令数

  复杂指令集与简单指令集

## CPU数据格式

• Bit (b)
• Byte (8 bits, B)
• Halfword (16 bits, H)
• Word (32 bits, W)
• Doubleword (64 bits, D)

## MIPS指令集

### MIPS指令编码结构

MIPS指令格式(32位)：MIPS共32条指令，有三种类型格式：R型指令、I型指令、J型指令

![img](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082617-49-58-0604544ee174249f6d7db007a10b70ca-FFK-Q3LC8UP5Z2PU58F}-MP-b9cf4a.png)

**R格式指令(算数类、逻辑类、位移类、跳转类指令)：纯寄存器指令，所有的操作数（除移位外）均保存在寄存器中。**Op字段均为0，使用funct字段区分指令

R型指令组成：
OP：指令的基本操作---操作码
Rs：第一个源操作数寄存器
Rt：第二个源操作寄存器
Rd：存放结果的目的操作寄存器
Shamt：偏移量，用于移位指令
Funct：函数，对操作码进行补充
**I格式指令（算数类、逻辑类指令）：带立即数的指令，最多使用两个寄存器，同时包括了load/store指令。**使用Op字段区分指令
I型指令组成：
OP：指令的基本操作---操作码
Rs：第一个源操作数寄存器
Rt：第二个源操作寄存器
立即数：参与运算的数据，16位
**J格式指令（跳转指令，跳转并链接指令，自陷指令，异常返回指令）：**
长跳转指令，仅有一个立即数操作数。使用Op字段区分指令
J型指令组成：
OP：指令的基本操作---操作码
立即数：参与跳转地址运算，26位

### Load/Store指令 - I Format

![image-20200826175712180](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082617-57-12-a49c4a84ffaf3c3711b89254a473da1d-image-20200826175712180-304cd9.png)

### Add指令  - R Format和I Format

![image-20200826195434434](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082619-54-34-b6774974658cb28ec98274b83da228cc-image-20200826195434434-70b048.png)

### Branch指令 - I Format

![image-20200826195602621](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082619-56-02-69743ae7b532baf4ba02f25c077611c2-image-20200826195602621-06573f.png)

### Jump指令 - J Format

![image-20200826195829630](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082619-58-29-405bd6198bf309ac9ddd6739fa54e18a-image-20200826195829630-49cb71.png)

## 5-Stage  MIPS 流水线

计算机体系结构教材中被提及最多的经典MIPS五级流水线如图所示。在此流水线中一条指令的生命周期分为：

![image-20200826201724488](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082620-17-24-85c12fb6cc5b895baa05d0b9e83e77a1-image-20200826201724488-5dd710.png)

**取指：**

指令取指（Instruction Fetch）是指将指令从存储器中读取出来的过程。

**译码：**

指令译码（Instruction Decode）是指将存储器中取出的指令进行翻译的过程。经过译码之后得到指令需要的操作数寄存器索引，可以使用此索引从通用寄存器组（Register File，Regfile）中将操作数读出。

**执行：**

指令译码之后所需要进行的计算类型都已得知，并且已经从通用寄存器组中读取出了所需的操作数，那么接下来便进行指令执行（Instruction Execute）。指令执行是指对指令进行真正运算的过程。譬如，如果指令是一条加法运算指令，则对操作数进行加法操作；如果是减法运算指令，则进行减法操作。

在“执行”阶段的最常见部件为算术逻辑部件运算器（Arithmetic Logical Unit，ALU），作为实施具体运算的硬件功能单元。

**访存：**

存储器访问指令往往是指令集中最重要的指令类型之一，访存（Memory Access）是指存储器访问指令将数据从存储器中读出，或者写入存储器的过程。

**写回：**

写回（Write-Back）是指将指令执行的结果写回通用寄存器组的过程。如果是普通运算指令，该结果值来自于“执行”阶段计算的结果；如果是存储器读指令，该结果来自于“访存”阶段从存储器中读取出来的数据。

在工业制造中采用流水线可以提高单位时间的生产量，同样在处理器中采用流水线设计也有助于提高处理器的性能。以上述的五级流水线为例，由于前一条指令在完成了“取指”进入“译码”阶段后，下一条指令马上就可以进入“取指”阶段，依次类推，如图所示，如果流水线没有停顿，理论上可以取得每个时钟周期都完成一条指令的性能。

![image-20200826201813809](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082620-18-13-fe88c8c31e05493d70c97f6ddf826c3a-image-20200826201813809-dedf0a.png)

### 流水线冲突

实际流水线中会出现三种相关也就是使流水线很难充分实现的三个冲突：结构相关、数据相关、控制相关。

#### **结构相关**

当指令在重叠执行的过程中，硬件资源满足不了指令重叠执行的要求，发生资源冲突时将产生结构相关。解决方法：添加硬件资源，例如解决访存冲突就使用指令cache和数据cache分开的哈弗结构。也可以采用一条空指令隔开来解决，如下图：

![image-20200826202418903](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082620-24-18-083236be45680cc088cd104f5c1e867e-image-20200826202418903-63b352.png)

访存阶段和取址阶段都需要使用存储器，可以用一个空指令隔开：

![image-20200826202540677](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082620-25-40-4e201529456fd970a5d29fafe07ba007-image-20200826202540677-a39d36.png)

流水线加速比计算公式：

CPI（Clock cycle Per Instruction）表示每条计算机指令执行所需的时钟周期

![image-20200826203403525](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082620-34-03-4de03ffedff811ae7754d90e0d698db4-image-20200826203403525-c1e34f.png)



如果采用双端口内存与上述方法的加速比比较：

假设机器A：双端口内存（哈弗结构）

假设机器B：单端口内存，流水线实现中，时钟速率快1.05倍

双方的IdealCPI = 1，假设20%的指令是load指令

![image-20200826204052705](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082620-40-52-dc6ea30eaa069fd662e2bb7c44eb2359-image-20200826204052705-240fe7.png)

因此，机器A比机器B快1.14倍

#### **数据相关**

当一条指令需要用到前面指令的执行结果，而这些指令均在流水线中重叠执行时，就可能引起数据相关。解决方法：数据重定向技术，或者称为旁路技术（forwarding）。

![image-20200826204142728](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082620-41-42-5ee6a40266151248db6460c13d6a7e3e-image-20200826204142728-4f3d17.png)

如图，r1的数据还没写回，而后面的第2和第3条指令已经用到了r1的数据。

三种常见的数据冲突如下：

1. Read After Write (RAW)：InstrJ尝试在InstrI写入操作数之前读取

   ![image-20200826204536530](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082620-45-36-04fdcf3d4d15f64c07254ed9130ce041-image-20200826204536530-22f9f6.png)

2. Write After Read (WAR)：InstrJ在InstrI读取操作数之前写入操作数，这种情况在MIPS 5阶段流水线中不会发生

   ![image-20200826204805738](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082620-48-05-3b6b857ba4acc127771430d125b0b3bf-image-20200826204805738-0a0745.png)

3. Write After Write (WAW)：InstrJ在InstrI写入操作数之前写入操作数，这种情况在MIPS 5阶段流水线中不会发生

   ![image-20200826205048039](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082620-50-48-aa04b262b259daaffff3bec4c9bac25c-image-20200826205048039-0d69a0.png)

重定向技术：

   ![image-20200826205613590](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082620-56-13-8a8a9e556e934e63d804cd2dcb6ea6ac-image-20200826205613590-054366.png)

重定向技术的实现：

![image-20200826205707084](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082620-57-07-98ab38f6c652a92df9dd6601555bc38b-image-20200826205707084-b6641f.png)

#### **控制相关**

当流水线遇到分支指令和其他会改变PC值的指令时，会发生控制相关冲突。解决方法：分支预测技术，投机执行，延迟分支。

如下图，第四阶段才能决定跳转到哪个分支：

![image-20200826210141640](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082621-01-41-1205dfaf34f44db03d92a927dae10300-image-20200826210141640-3b1917.png)

流水线中，就会有3个空指令，极大影响了CPI。

两种解决办法：

1. 尽快决定是否发生分支跳转
2. 尽早计算跳转分支地址

早期的MIPS解决办法：

* 在第2阶段进行寄存器的zero测试，同时计算出跳转后的指令地址

![image-20200826211215477](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082621-12-15-95b6c3029735389e4fdae6ebd620db35-image-20200826211215477-48c196.png)

比原始的延迟3个空指令降低到延迟1个空指令。

其他问题：

异常：比如除以0或未定义的操作码

中断：例如硬件发送信号切换到一个新的指令流

## 超标量（Super Scalar）

目标：使CPI<1，发出多个指令（Ops）/周期

* Superscalar：由编译器或硬件（Tomasulo）调度的不同数量的指令/周期（1到8）
* (Very) Long Instruction Words (V)LIW：由编译器调度的固定数量的指令（4-16），多个指令合并为一个超长指令

### 发出多条指令/周期

**超标量MIPS**：2条指令，1个FP（浮点运算）&1个anything（非FP算术运算）

![image-20200828103608939](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082810-36-15-bfd2cf2ddc54fda84a70daf1c982321d-image-20200828103608939-636881.png)

设计复杂性：

* 如果指令由于执行中的早期指令或发出数据包中的较早指令而导致结构危险或数据危险，则不应发出指令

* 在1个周期内执行问题检查可能会增加时钟周期时间：需要O（N²）数量级的比较

* 更高的分支惩罚->提高预测精度很重要

面临的挑战：

* 整数/浮点指令的拆分对于硬件来说很简单，但CPI为0.5时只适用于具有以下特点的程序：
  –50%的指令为FP操作，且没有冲突

* 解码和指令发布难度更大：

  -检查2个操作码，6个寄存器说明符，决定是否可以发出1条或2条指令

  -寄存器文件：每个时钟周期需要2次读取和1次写入（1≤x≤N）

  -总线：需要多条结果总线

超标量动态调度的简单方法：

* Load/Store 指令可能会导致Integer和浮点数运算之间的依赖性问题

  -将Load指令保存到Load队列；操作数必须按获取Load指令的顺序读取

  -Load队列检查Store队列中的地址，以避免RAW冲突

  -Store队列检查Load和Store队列中的地址，以避免WAR&WAW

### 寄存器重命名，虚拟寄存器与重排序缓冲区

* 虚拟寄存器集和寄存器重命名可以替换重排序缓冲区
* 虚拟寄存器包含架构上可见的寄存器+临时值
* 重命名过程将寄存器的名称映射到虚拟寄存器集中的寄存器

##  ILP（指令级别的并行）的局限

* 应用程序基准测试（矢量化Fortran FP与整数C程序）
* 硬件复杂度
* 编译器复杂度

## VLIW（超长指令字）

将多个操作打包到一条指令中

缺点：

* 静态查找并行性
* 代码规模
* 没有冲突检测的硬件
* 二进制代码兼容性

# 线程级别并行（TLP）

性能超越单线程ILP：

* 任意代码的ILP现在被限制为3到6条指令/时钟周期，在某些应用程序（例如，数据库或科学代码）中可以有更高的自然并行性
* 显式（由编译器指定）线程级并行或数据级并行
* 数据级别并行：对数据执行相同的（lock-step）操作，并且拥有大量数据
* ILP在循环或直线代码段中隐式地使用并行操作
* TLP通过使用多个固有并行的执行线程显式表示
* 在许多应用中，TLP比ILP更具成本效益。

多线程：多线程通过重叠执行来共享一个处理器的功能单元

​	-处理器必须复制每个线程的独立状态，例如，单独的寄存器文件副本，单独的PC，如果作为独立程序运行，则单独的页表

​	-通过虚拟内存机制共享的内存，已经支持多个进程

​	-线程切换（0.1到10个时钟）比完全复制状态（状态=寄存器和内存）的进程切换（100到1000秒的时钟）更快速

什么时候在线程之间切换时？
	–来自新线程的指令（细粒度）

​	-当一个线程暂停时，可能是由于缓存未命中，可以执行另一个线程（粗粒度）

​	-在无cache的多处理器中，在每次内存访问开始时

## 多线程处理器

* 超标量执行中的单线程：依赖关系导致大多数空指令

* 思路：当一个线程停止运行时，另一个线程开始运行

* 多线程的不同粒度

  -粗MT：每隔几个时钟周期更换线程

  -精细MT：可以每周期更换一次线程

  -Simultaneous多线程（SMT）

  ​	即使在同一个周期内，也可以从不同的线程插入指令

  ​	又名超线程

![image-20200828115721295](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082811-57-21-dd48cd79acbfdfd5d2ffeed349e6e771-image-20200828115721295-2f0a8e.png)

### 细粒度多线程

* 在每个指令周期的线程之间切换，导致多个线程的执行交错进行
* 通常以循环方式完成，跳过任何暂停的线程
* CPU必须能够每时钟切换一次线程
* 优点是它可以隐藏短时间和长时间的暂停，因为在一个线程暂停时执行其他线程的指令
* 缺点是它减慢了单个线程的执行速度，因为一个准备好执行而没有暂停的线程将被来自其他线程的指令延迟
* 用于Sun的Niagara芯片（8核，将在后面看到）

### 粗粒度多线程

* 为代价高昂的暂停操作切换线程，例如二级缓存未命中（如果没有缓存，则在任何数据内存引用上切换）

* 优点

  –减轻了对快速线程切换的需求（如果使用缓存）。
  –不会减慢任何线程的速度，因为只有在活动线程遇到代价高昂的暂停时才会发出来自其他线程的指令

* 缺点是，由于流水线启动成本的原因，很难克服较短暂停造成的吞吐量损失

  -因为CPU通常只从一个线程发出指令，当发生暂停时，必须清空或冻结流水线

  –新线程必须在指令完成之前填充流水线

* 由于这种流水线启动开销，粗粒度多线程处理对于减少高成本暂停的惩罚是有效的当暂停时间>>流水线重新填充时间

* 用于IBM AS/400（1988，用于中小型企业）

### 同步多线程

SMT：想法是使用单个大型单处理器作为多处理器

当它工作时，它用其他线程的工作填充空闲的“指令发布槽”，提高吞吐量

#  数据级别并行

* SIMD体系结构可以利用显著的数据级并行性来实现：
  ◼ 面向矩阵的科学计算

  ◼ 面向媒体的图像和声音处理器

* SIMD比MIMD更节能

  ◼ 每个数据操作只需要获取一条指令

  ◼ SIMD对个人移动设备具有吸引力

* SIMD允许程序员继续按顺序思考

## SIMD并行

* 矢量体系结构

* SIMD扩展

* 图形处理器单元（GPU）

* 对于x86处理器：
  ◼ 预计每个芯片每年会增加两个核心

  ◼ SIMD宽度每四年翻一番

  ◼ SIMD的潜在加速比是MIMD的两倍！

NVIDIA GPU内存架构：

*  每个SIMD通道都有片外DRAM的专用部分

​	◼ “私人内存”

​	◼ 包含堆栈帧、溢出寄存器和私有变量

* 每个多线程SIMD处理器也有本地内存

  ◼ 由块内的SIMD通道/线程共享

* SIMD处理器共享的内存是GPU内存

  ◼ 主机可以读写GPU内存

Fermi架构的创新：

* 每个SIMD处理器都有

  ◼ 两个SIMD线程调度器，两个指令调度单元

  ◼ 16个SIMD通道（SIMD宽度=32，蜂鸣音=2个周期），16个负载存储单元，4个特殊功能单元

  ◼ 因此，每两个时钟周期调度两个SIMD指令线程

* 快速双精度

* GPU内存缓存

* 64位寻址和统一地址空间

* 纠错码

* 更快的上下文切换

* 更快的原子指令

## 并行设计引论

### 任务图

* 将整个计算任务分解成若干小的任务，在将这些任务分配到不同处理器上，以获取并行性。
* 这些小任务之间可以完全独立运行，但是也可能存在相互依赖关系，构成任务依赖图(Task-dependency graph)

![image-20200828125721134](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082812-57-21-5d8ea8239842c05a5ac121c22860f8b1-image-20200828125721134-90c48a.png)

相关概念：

* 并发度，任意时刻可以并行执行的最大任务数

  * 最大并发度

  * 平均并发度

* 关键路径
  * 从起始节点到结束节点之间的最长路径

* 粒度(granularilty)，任务的大小

  * 细粒度(fine-grained)，大量的小任务；

  * 粗粒度(coarse-grained), 少量的大任务

![image-20200828125845615](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082812-58-45-285ff4495142362005dd93af808b9722-image-20200828125845615-e097a2.png)

### 并行化程序设计模型

发现并行性、算法结构设计、选择合适的并行模型、系统实现等四个步骤

![image-20200828130302097](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082813-03-02-bce07755251ac87966ad2d38148d03cb-image-20200828130302097-4d5067.png)

* 发现并行性阶段

  * 分解，又可以分为任务分解和数据分解两方面。任务分解是将整个工作分解为相互独立的、可以并行执行的任务，数据分解是将整个工作所需要的数据分解为独立的块，每个独立的数据块和特定的任务相关联。

  * 依赖性分析，包括任务聚合、任务排序和数据共享等三个方面，其中任务聚合是指如何将多个相似的任务映射到线程等相关结构上；任务排序是分清任务之间的相互依赖关系，并确定任务之间的执行次序；数据共享是考虑任务之间数据共享和交换。

  * 设计评估阶段主要考虑任务之间的并发性、数据和任务之间的局部性、不同任务之间数据交换和同步的开销等方面因素，从而决定当前的并行化方法是否合理，需要进一步优化，还是可以进入下一个设计阶段。

* 算法结构设计

  ![image-20200828130510881](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082813-05-10-2e2aa22f715232566b4d95e85f404bd4-image-20200828130510881-29c6f9.png)

* 任务并行化

  在任务并行化模式中，系统的任务被分解为很多独立的可以并行计算的任务。对于规则化的数据而言，这往往也和几何数据划分相关联。

  * 例如，求两个向量之和：*C*=*A*+*B*，Ci=Ai+Bi。两个向量中每个元素的求和都是相互独立和可以并行计算的，可以将单个元素的求和理解为一个任务，与之相对应的数据分解方法就是每个任务对应向量的一个元素。
  * 更复杂的问题中，每个任务之间的数据划分可能存在着重叠，需要在计算过程中，交换两个任务之间的数据。例如热传导问题。这时需要更为精细地考虑，数据交换和计算之间的时间比例，以及通过计算掩盖数据交换的延迟。
  * 如果每个任务执行的计算是相同的，可以使用静态的任务调度策略。但是如果每个任务所执行的计算是不同的，就需要仔细考虑每个任务所需要的时间，必要时将采用动态任务调度策略。

* 分而治之

  分而治之的关键是将原始任务分解为若干小的独立任务，并行计算这些独立的任务，然后再将这些任务的结果合并在一起。

  * FFT计算就是一种典型的分而治之方法。
  * 分而治之方法将任务不断细化直至到最小的门限，或者可以并行的任务数量达到了系统能支持的可以并行量（例如多核处理器的核数）。
  * 在分而治之的任务划分中，还需要考虑在两个任务结果归并过程中的数据共享方法。

* 递归

  对于链表、树、图等数据结构，可以考虑递归数据的并行方法。这个方法和分而治之方法有些类似的地方。但是由于这些数据结构本身具有相似性（递归性），可以使用指针跳跃的技巧。

* 流水线与协同模式

  流水线模式：任务被分解成多个前后依赖的子任务。在执行多个任务时，每个任务位于流水线中的某一段上，从而使得多个任务可以并行执行。

  * 流水线模型可以有效提升系统的吞吐率，但是会增加单个任务的延迟。
  * 其关键在于保持每个流水线段上的任务执行时间比较接近，否则执行时间最长的段将成为整个系统的瓶颈。

  更为复杂的数据流可能存在着分支、反馈等结构，这使得各个任务之间的交互关系也更为复杂，就需要使用事件协同模式。在事件协同模式中，每个任务都在等待事件。在相应的事件发生后，将处理该事件，并将结果作为新的事件发送到其他任务。

  * 事件协同模式需要预防系统死锁，还要考虑高效的事件传递机制。

# Cache

* 内存层次结构

![image-20200828131324422](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082813-13-24-5fa14f6b57b94814fe4be8d93704e72d-image-20200828131324422-0a27ba.png)

* 聚合峰值带宽随核数增长：
  -英特尔酷睿i7每时钟每核可生成两个引用

  -四核3.2GHz时钟

  ​	-256亿 64位数据引用/秒+

  ​	-128亿 128位指令引用

  ​	=409.6 GB/s！

* DRAM带宽仅为该带宽的6%（25GB/s）

* 要求：
  多端口，流水线缓存

  每个核心有两个级别的缓存

  共享三级片上缓存

## 内存类型

* 静态RAM（SRAM）
  * 每比特6 0r 8个晶体管
  * 两个逆变器（4个晶体管）+读/写晶体管
  * 针对速度（第一个）和密度（第二个）
  * 快速（小SRAM的亚纳秒延迟）
  * 速度大致与其面积成比例
  * 与标准处理器逻辑混合良好

* 动态RAM（DRAM）
  * 1个晶体管+1个电容器每位
  * 针对密度混合良好（按每位成本计算）
  * 慢（>40ns内部访问，~100ns针对针）
  * 不同制造步骤（与逻辑不太好）

* 非易失性存储：磁盘、闪存

成本：SRAM>DRAM>Flash>disk

延迟：

• SRAM: <1 to 2ns (on chip)

•DRAM: ~50ns – 100x or more slower than SRAM

•Flash: 75,000ns (75 microseconds) – 1500x vs. DRAM

•Disk: 10,000,000ns (10ms) – 133x vs Flash (200,000x vs DRAM)

带宽：

• SRAM: 300GB/sec (e.g., 12-port 8-byte register file @ 3Ghz)

• DRAM: ~25GB/s

• Flash: 0.25GB/s (250MB/s), 100x less than DRAM

• Disk: 0.1 GB/s (100MB/s), 250x vs DRAM, **sequential access only**

## 主存的背景

* 主存性能

  -延迟：缓存未命中时的惩罚

  ​	-访问时间：请求和字到达之间的时间

  ​	-周期时间：请求之间的最小时间带宽：I/O+块未命中的惩罚时间（L2）

* 主存为DRAM：动态随机存取存储器

  -动态是因为需要定期刷新（每8毫秒，1%的时间）

  -地址分为两部分（内存为2D矩阵）：
  	-RAS（行访问存取）

  ​	-CAS（列访问存取）

* 缓存使用SRAM：静态随机存取存储器

  -无刷新（但需要6个晶体管/位，而DRAM需要1个晶体管/位）
  大小：SRAM/DRAM = ­4-8 
  成本/周期时间：SRAM/DRAM = ­8-16

## DRAM的性能

1. 快速页面模式

   -添加允许重复访问行缓冲而无需访问另一行的定时信号

   -这样的缓冲区很自然，因为每个阵列缓冲区1024到2048位都用于每个访问

2. 同步DRAM（SDRAM）

   -向DRAM接口添加一个时钟信号，这样重复传输就不会有额外的同步开销

3. 双数据速率DRAM（DDR SDRAM）

   -在DRAM时钟信号的上升沿和下降沿传输数据，将峰值数据速率提高一倍

   -DDR2通过将电压从2.5伏降低到1.8伏来降低功耗+提供更高的时钟速率：高达400兆赫

   -DDR3降到1.5伏+更高的时钟速率：高达800兆赫

以上改善了带宽，而不是延迟

* DDR:

  –DDR2

  ​	•Lower power (2.5 V -> 1.8 V)

  ​	•Higher clock rates (266 MHz, 333 MHz, 400 MHz)

  –DDR3

  ​	•1.5 V

  ​	•800 MHz

  –DDR4

  ​	•1-1.2 V

  ​	•1600 MHz

* GDDR5是图形存储器基于DDR3

* 显存：
  达到2-5倍带宽每DRAM相对于DDR3

  ​	-接口更宽（32对16位），

  ​	-更高的时钟速率

内存的延迟相对于CPU的主频来说是很长的，以下是粗略估计：

CPU：2GHz->0.5ns / cycle

内存：100ns -> 200 cycle memory latency!

解决方案：Caches

## 存储层次结构和局部性

![image-20200828144556808](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082814-45-56-e1cae14e4a67f736b8964de1d1a298fe-image-20200828144556808-c8e4cd.png)

### 数据局部性

* 时间局部性：如果数据项现在需要的，它很可能是在不久的将来再次需要
* 空间局部性：如果数据项现在需要的，很可能在不久的将来需要其附近的数据

利用局部性：缓存

将最近使用的数据保存在靠近处理器的快速内存中，同时也将附近的数据放到那里

## Cache基础知识

* 靠近处理器的快速（但很小）内存

* 引用数据时

  -如果在缓存中，则使用缓存而不是内存

  -如果不在缓存中，则将其放入缓存（实际上，也将整个数据块也带到缓存中）

  -可能需要踢出其他东西来完成！

* 重要决策

  -位置：块可以放在缓存中的哪个位置？

  -标识：如何在缓存中找到块？

  -替代品：为了在缓存里腾出空间，该踢出什么？

  -写策略：我们怎么处理存储？

* 缓存由块大小的行组成，行大小通常是两个字节的幂次方，大小通常为16到128个字节

* 放置策略

  -直接映射（块只能指向一行）

  -完全关联（块可以指向任何行）

  -设置关联（块可以转到N行之一）

## 缓存标识

* 当引用地址时，需要

  -找到它的数据是否在缓存中

  -如果它在缓存中，找到缓存中的哪个地方

  以上称为缓存查找

* 每个缓存行必须有

  - 一个有效位（如果行有数据，则为1；如果行为空，则为0）我们还称缓存行是有效的还是无效的

  - 一个标记，以标识行中的哪个块（如果行有效）

![image-20200828150758500](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082815-07-58-3ef9324d9bc190b4eb654a7d8257bf5d-image-20200828150758500-601d93.png)

## 缓存置换

需要一个空行来插入新的block，我们应该踢出哪个block？
几种策略

* 随机（随机选择的行）

* FIFO（缓存中最就久的行）
* LRU（最近最少使用的行）
* LRU近似
* NMRU 
* LFU

## 写策略

我们在写操作中分配缓存行吗？

* 写入分配

  写入未命中会将块带入缓存

* 没有写入分配

  写入未命中时缓存会像以前一样

我们是否会在写入时更新内存？

​	–Write-through

​		•每次写入时立即更新内存

​	–Write-back

​		•替换行时更新内存

### Write-Back缓存

* 每一行都需要一个脏位

  ​	-脏行包含比内存更新的数据

* cache行开始时为干净状态（不脏）

* 行在第一次写入时变脏

  ​	-内存尚未更新，缓存有脏行的唯一的最新数据副本

* 替换脏行

  ​	-必须将数据写回内存（写回）

## Cache性能

* Miss rate（缺失率）

  缓存中未命中的内存访问率

  命中率=1–未命中率

* Average memory access time平均内存访问时间

  AMAT = hit time + miss rate * miss penalty

* 内存暂停周期

  ![image-20200828153409031](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082815-34-09-9951d651657147700a00dfc20745a44a-image-20200828153409031-eb0c14.png)

### 增强Cache性能

* AMAT = hit time + miss rate * miss penalty

  -减少未命中惩罚

  -降低未命中率

  -减少命中时间

* CyclesMemoryStall = CacheMisses x (MissLatencyTotal – MissLatencyOverlapped)

  -增加重叠未命中延迟

#### 减少缓存未命中惩罚

* 多级缓存

  -非常快，小的1级（1级）缓存

  -快，不那么小的2级（L2）缓存

  -也可能有较慢的、大的L3级缓存等。

* 这有什么用？

  -一级缓存中的未命中可能在二级缓存中命中，等等。

![image-20200828153937420](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082815-39-37-db47bfe049003feeb3045b9e9e50d25e-image-20200828153937420-43c8f1.png)

* 提前重启&关键字优先

  -块传送需要时间（总线太窄）

  -在整个块到达之前加载数据

  * 提前重启

    -当需要的字到达时，让处理器使用它

    -然后继续块传输以填充缓存行

  * 关键字优先

    -首先传输加载的字，然后传输块的其余部分（使用换行来获得整个块）

    -与早期重新启动一起使用，以使处理器尽快运行

* 增加Load未命中优先级

  -Load可以有从属指令

  -如果一个Load未命中，而一个Store需要转到内存中，则先让Load未命中先运行

  -需要一个写缓冲区来记住Store

* 合并写入缓冲区

  -如果同一个块有多个写入未命中，将它们合并到写入缓冲区中

  -使用块写入，而不是许多小的写入

* Victim Caches

  -最近被踢出的块保存在小缓存中

  -如果我们错过了那些块，可以很快取得

  -为什么有效：冲突未命中

  N-way set 关联的缓存中存在未命中，但如果缓存是完全关联的，则不会发生

##### 缓存未命中的种类

“3个C”

* 强制性的（**Compulsory**）：一定要会发生

  -每个块的第一次访问一定会miss

* 容量：由于缓存容量有限，如果缓存大小是无限大，则不会发生

* 冲突：由于有限的关联性，如果缓存是完全关联的，则不会发生

#### 降低缓存未命中率

* 更大的块

  -如果有更多的空间局部性，会有帮助

* 更大的caches

  -容量miss更少，但命中延迟更长！

* 提高关联性

  -冲突miss更少，但命中延迟更长

* 路径预测

  -加速set-关联缓存

  -预测N种方式中哪一种可以将我们的数据作为直接映射缓存进行快速访问

  -如果预测失误，则作为set关联cache再次访问

* 伪关联缓存

  -类似于路径预测方法

  -从直接映射缓存开始

  -如果“主”项未命中，请尝试另一项

* 编译器优化

  -循环变换

  -Blocking

#### 减少命中时间

* 小而简单的缓存速度更快

![image-20200828163810866](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082816-38-10-31b0903441ae6b93520a3b09afb30e9c-image-20200828163810866-65ce87.png)

* 避免缓存命中时的地址转换

* 软件使用虚拟地址，使用物理地址访问内存

* 硬件必须将虚拟地址转换为物理地址

  -使用物理地址访问的缓存

  -在缓存查找之前等待转换

* 想法：索引缓存使用虚拟地址

* 流水线缓存

  -提高带宽

  -对于高频的L1缓存来说，即使是小缓存，在N Ghz下也有2-3个周期的延迟

  -也用于许多二级缓存

* 跟踪缓存

  -缓存指令

#### 隐藏未命中延迟

* 想法：用有用的工作（也称为“延迟隐藏”）来重叠未命中的延迟

* 非阻塞缓存

  -阻塞缓存一次为一个访问提供服务，服务miss时，其他访问被阻止（等待）

  -非阻塞缓存消除了这个限制，服务miss时，可以处理其他请求

* 预取

  -预测将需要什么并提前得到

##### 非阻塞缓存

* Hit Under Miss

  -一次未命中时允许缓存命中

  -但另一个miss必须等待

* Miss Under Miss, Hit Under Multiple Misses

  -当其他未命中正在进行时允许命中和未命中

  -内存系统必须允许多个挂起的请求

![image-20200828165532120](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082816-55-32-77a30c5a2b455630108f74f63523298a-image-20200828165532120-551508.png)

##### 预取

* 预测未来的未命中并将数据放入缓存

  -如果确实发生了访问，我们将立即命中（或者，如果数据正在传输中，则部分未命中）

  -如果没有访问，则造成缓存污染（用我们不需要的垃圾数据替换其他数据）

* 为了避免缓存污染，设置预取缓冲区

  -缓存污染对小型缓存来说是个大问题

  -为预取准备一个单独的小缓冲区，当我们访问它时，把数据放到缓存中，如果我们不访问它，缓存就不会被污染

简单顺序预取：

* 在缓存未命中时，获取两个顺序内存块

  -利用指令和数据中的空间局部性

  -利用高带宽进行顺序访问

  -英特尔称之为“相邻缓存线预取”或“空间预取”

* 扩展到获取N个顺序内存块

  -选取足够大的N来隐藏内存延迟

* 流预取是预取的连续版本

  -流缓冲区可以容纳N条缓存行

  -在未命中时，开始获取N个顺序缓存行

  -在流缓冲区命中时：将缓存行移到缓存，开始获取行（N+1）

快速预取：

* 思路：检测和预取跨步访问

  –for (i=0; i<N; i++) A[i*1024]++;

* 使用基于PC的表格检测步幅

  -对于每个PC，记住步幅

  -步幅检测

  ​	记住该PC的最后一个地址

  ​	与此PC使用的地址比较

  -使用两位饱和计数器跟踪置信度

  ​	步幅正确时增加，错误时减少

软件预取：

* 两种风格：寄存器预取和缓存预取

* 每种风格可以是错误的或非错误的，如果地址错误，它是否会创建异常？

* 错误的寄存器预取是绑定的，它正常加载，地址必须正常，使用寄存器

* 非故障缓存预取是非绑定的

  -如果地址不好，就会变成一个NOP

  -不影响寄存器状态

  -有更多的开销（加载仍然存在），
   ISA change（预取指令），
  使缓存复杂化（预取和加载不同）

#  缓存一致性

* 无缓存时，共享内存很轻松

  –P1 写, P2 可以读

  -只有一个数据副本存在（内存中）

* 缓存存储自己的数据副本

  -这些拷贝很容易变得不一致

  -经典例子：累加

P1加载allSum，添加其mySum，存储新的allSum

P1的缓存现在有脏数据，但内存未更新

P2从内存加载allSum，添加其mySum，存储 allSum

P2的缓存也有脏数据

最终P1和P2的缓存数据将进入内存而不管写回顺序如何，最终值是错误的

## 缓存一致性定义

存储系统是一致的，如果

1. 从处理器P1上的地址X读取R时，如果W和R之间没有其他处理器写入X，则返回P1上最近一次写入W到X的值。

2. 如果P1写入X，P2在足够的时间后读取X，并且其间没有其他写入X，P2的read返回P1写入的值。

3. 对同一位置的写入被序列化：对位置X的两次写入被所有处理器以相同的顺序看到。

* 属性1：保留程序顺序——也就是说，在没有共享的情况下，每个处理器的行为就像一个单处理器

* 属性2：表示对地址的任何写入操作最终都必须被所有处理器看到——如果P1写入X而P2继续读取X， P2最终必须看到新值。

* 属性3：保留因果关系

  -假设X从0开始。处理器P1增加X，处理器P2等待X为1，然后将其递增到2。处理器P3最终必须看到X变成2。

  -如果不同的处理器可以看到不同的顺序写入，P2可以看到P1的写入，并做了自己写的，而P3第一次看到由P2写，然后由P1写。现在我们有两个处理器，将永远不同意A的值。

## 维护Cache一致性

硬件方案：

* 共享缓存

  -平凡强制执行的一致性

  -没有可扩展性（L1高速缓存很快成为瓶颈）

* Snooping

  -需要一个广播网络（如总线）执行一致性

  -每个缓存跟踪自身的共享状态

* Directory

  -可以强制执行的一致性，即使一个点至点网络

  -块只有一个保持完全共享状态的位置

### Snooping

通常用于基于总线（SMP）的多处理器

* 在总线上序列化，用于维护一致性属性3

两种风格：

* Write-update (write broadcast)
  * 广播对共享数据的写入以更新所有副本
  * 所有后续读取都将返回新的写入值（属性2）
  * 所有人都能看到按广播顺序写的东西，一个总线==所有人看到的一个命令（属性3）

* Write-invalidate
  * 写入共享数据强制使所有其他缓存副本失效
  * 后续读取未命中并获取新值（属性2）
  * 按总线上的失效排序写入的顺序（属性3）

#### MSI协议

大部分的多核处理器都是采用write invalidate的做法，具体的实现取决于不同的cache一致性协议，但其中最基础的是**MSI**，"M", "S", "I"这3个字母代表了一个cache line可能的三种状态，分别是**M**odified, **S**hared和**I**nvalid。

当一些CPU从内存读取了数据到自己的cache line，此时这些CPU中的这些cache line中的数据都是一样的，和内存对应位置的数据也是一样的，cache line都处于shared状态。

![image-20200829001009351](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082900-10-15-62b30f0d7d9050ced4460fddbf5c7672-image-20200829001009351-a35406.png)

接下来P2将自己cache line的数据更改为13，P2的这条cache line就变为modified状态（S-->M），其他CPU的cache line就变为invalid状态（S-->I）。

![image-20200829001051786](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082900-10-51-2f6c2304472d38018cd0cab5325a8caf-image-20200829001051786-eb97d1.png)

然后如果P1试图读取这条cache line中的数据，由于是invalid状态，于是将触发cache miss(细分的话叫read miss)，那么P2将会把自己cache line的数据写回(writeback)到内存，供P1从内存读取，之后P1和P2的cache line都将回到shared状态（I-->S, M-->S）。

![image-20200829001139017](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082900-11-39-c772ce9b6c9fbe1b8ad3400004f997ec-image-20200829001139017-6b9048.png)

这里包含着一个前提，就是某个CPU在自己的cache中，对内存某个位置对应的invalid cache line访问触发的cache miss，其他拥有这个内存位置对应的cache line的CPU都能识别，这个前提自然也属于MSI协议实现的一部分。

在上面图2的状态中，如果P1不是读取，而是写入这条cache line，那么也将触发cache miss(不过这次是write miss)，P1的cache line将变为modified状态（I-->M），而P2的cache line将变为invalid状态（M-->I）。

![image-20200829001203139](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082900-12-03-b4a4cde404e373f8cbab24eaee2713a3-image-20200829001203139-b6cadb.png)

无论什么时刻，在某个内存位置和它对应的所有cache line中，至多有一个CPU的cache line可以处于modified状态，代表着最新的数据。其他CPU中cache line中的数据过时没关系，把状态标记为失效就可以了。

至此，MSI协议中三种状态之间的转换就基本展现完了，这三种状态中的每两种，都可以在一定场景下相互转换(共6种)。此外，在cache line处于shared状态时，读这条cache line的操作不会改变原来的状态，而在cache line处于modified状态时，写这条cache line的操作也不会改变状态。整个状态转换关系如下图所示(共8种)，其中"remote"代表是其他CPU进行的cache line操作。

![image-20200829001251606](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082900-12-51-0afd9a1b6174a8bd8ce5e12820933c7f-image-20200829001251606-3a1e1b.png)

举例：

![image-20200829001615504](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082900-16-15-8e5df7fcefa8d0ac7b39d3881976a24a-image-20200829001615504-0df13b.png)

存在问题：

​	假如P1拥有B块处于M状态，P2想要读B，向总线发送RdReq。如果P1不处理，则内存会发送数据给P2，如果P1处理，那么它有两种解决方案：

1. 中止/重试：P1取消P2的请求，向总线发送写回请求，P2之后重新向总线发送RdReq，从内存中读取数据。这样太慢了（两次内存延迟）
2. 干涉：P1表示自己会提供数据（向总线发送干涉信号），内存收到信号，不提供数据，等到P1的数据。P1开始发送数据给总线，内存更新数据。P2监听到此次传输并取得数据块。

干涉方案在缓冲中有数据处于M时起作用（只有一个CPU有最新的数据）。但如果缓存需求的数据处于S状态呢？

* 有多个处理器拥有数据，哪个处理器需要提供数据？
  * 解决方案1：内存提供数据
  * 解决方案2：任一竞争获胜方提供数据
  * 解决方案3：类似于S的独立状态表示可能有其他人将块置于S状态，但是如果有人要求提供数据，我们应该提供它（MESI协议）

#### MESI协议

在MESI协议中，每个Cache line有4个状态

| 状态         | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| M(Modified)  | 这行数据有效，数据被修改了，和内存中的数据不一致，数据只存在于本Cache中。 |
| E(Exclusive) | 这行数据有效，数据和内存中的数据一致，数据只存在于本Cache中。 |
| S(Shared)    | 这行数据有效，数据和内存中的数据一致，数据存在于很多Cache中。 |
| I(Invalid)   | 这行数据无效。                                               |

 E状态示例如下：

![image-20200829002727834](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082900-27-27-2e8b0c08a855ea6aa63853908198d73a-image-20200829002727834-00217f.png)

只有Core 0访问变量x，它的Cache line状态为E(Exclusive)。

S状态示例如下：

![image-20200829002745741](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082900-27-45-eefde7cc67719baba19c1678c00adb19-image-20200829002745741-c31528.png)

3个Core都访问变量x，它们对应的Cache line为S(Shared)状态。

 M状态和I状态示例如下：

![image-20200829002822093](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082900-28-22-69031019e2c5d268bc783847d163fa37-image-20200829002822093-28a28d.png)

Core 0修改了x的值之后，这个Cache line变成了M(Modified)状态，其他Core对应的Cache line变成了I(Invalid)状态。

在MESI协议中，每个Cache的Cache控制器不仅知道自己的读写操作，而且也监听(snoop)其它Cache的读写操作。每个Cache line所处的状态根据本核和其它核的读写操作在4个状态间进行迁移。

​    MESI协议状态迁移图如下：

![image-20200829002928630](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082900-29-28-5f69291b2c1970d0191bfd3890cc0d7f-image-20200829002928630-8f9e08.png)

优点：减少写入只存在于一个缓存中的块所导致的流量。

怎么检测其他的共享者？假如P1需要读B块，发送RdReq请求给总线，收到数据，P1应该把B块的状态置为S还是E？

解决方案：Share状态总是置为low，当P2监听到P1的请求时，将Share置为high，P1把B块置为S如果Share为high，否则置为E

#### 基于目录的一致性协议

通常在分布式共享内存中，对于每个本地内存块，本地目录都有一个条目

目录项表示：

	- 谁缓存了块的副本
	- 它们的块位于什么状态

每个条目包括：

* 一个脏位（如果有脏缓存副本，则为1）
* 存在向量（每个节点1位）告诉哪些节点可能有缓存副本

所有的miss都发送到block所在的位置

目录执行所需要的一致性操作

最终，目录回复数据



Read Miss

* 处理器Pk在块B上有一个读未命中时，向块所在的节点发送请求

* 目录控制器：找到B的条目，检查脏位。如果脏位=0，读取内存并发回数据，设置P[k]；如果脏位=1，向P位为1的处理器请求块，当块到达的时候，更新内存，清空脏位，发送块给Pk并设置P[k]

目录操作

* 网络控制器连接到每个总线，它是远程缓存和内存的一个代理
* 每个缓存仍有自己的一致性状态，目录只是防止广播和为每个地点的访问进行排序

举例：

![image-20200829005652108](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082900-56-52-3ac8ca347e40ae4bac338405ef76b0f8-image-20200829005652108-b82a6b.png)

# 分支预测

## 静态分支预测

最简单的静态分支预测方法就是任选一条分支。1）认为branch一定会token；2）认为branch一定不会token；这样平均命中率为50%。更精确的办法是根据原先运行的结果进行统计从而尝试预测分支是否会跳转。

任何一种分支预测策略的效果都取决于该策略本身的精确度和条件分支的频率。

## 动态分支预测



### 1位分支预测

无条件跳转指令必然会跳转,而条件跳转指令有时候跳转,有时候不跳转,一种简单的预测方式就是根据该指令上一次是否跳转来预测当前时刻是否跳转。如果该跳转指令上次发生跳转,就预测这一次也会跳转,如果上一次没有跳转,就预测这一次也不会跳转。这种预测方式称为:1位预测(1- bit prediction)

![image-20200829150818736](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082915-08-24-2dfd7be67c4ab1142c160ba62091902f-image-20200829150818736-c9b782.png)

### 2位分支预测

2位预测(2- bit predictor)。每个跳转指令的预测状态信息从1bit增加到2bit计数器,如果这个跳转执行了,就加1,加到3就不加了,如果这个跳转不执行,就减1,减到0就不减了,当计数器值为0和1时,就预测这个分支不执行,当计数器值为2和3时,就预测这个分支执行。2位的计数器比1位的计数器拥有更好的稳定性。

![image-20200829151150694](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082915-11-50-a5746e3582ea5939e121d4547ed8b6ba-image-20200829151150694-f5da5c.png)

举例：

![image-20200829152202531](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082915-22-02-551e47c1d94eb3c89afdf340f9f512be-image-20200829152202531-60ff47.png)

上面是1位分支预测，下面是2位分支预测

### 基于分支历史表的分支预测

![image-20200829155117692](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082915-51-17-30abc2285dfb81683e5ca3cec4c53092-image-20200829155117692-cd1935.png)

![image-20200829155132609](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082915-51-32-a10526d3c2b67961fa2c3e405f7232a1-image-20200829155132609-5408ee.png)

上图显示了不同历史长度的分支历史表。

分支历史又可以分为全局和局部的分支历史：

* 局部行为：根据分支A的历史输出预测分支A
* 全局行为：根据分支A、B...X、Y的所有历史输出来预测分支Z

为什么会存在全局的相关性？

![image-20200829155406772](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082915-54-06-48d697645db64d5a95c4b654c3bc2de7-image-20200829155406772-224eeb.png)

例如上图，分支A和分支B是两个互斥的条件。

偏相关：一个分支可以测试cond1，另一个分支可以测试cond1&&cond2（如果cond1为false，则第二个分支可以预测为false）

多重相关：一个分支测试cond1，第二个分支测试cond2，第三个分支测试cond1异或cond2（如果前两个分支已知，则始终可以预测）。



## 分支预测器的性能

没有哪个分支预测器是最好的，不同的分支情况表现出不同的行为。

想法：用一个分支预测器来预测哪个分支预测器的表现最好。

![image-20200829155827860](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082915-58-27-d098a6c09f0de470c8fffdce816d6cba-image-20200829155827860-d34ee0.png)

常见的组合有：

* 全局历史+局部历史

* 简单预测+全局历史
* 短期历史+长期历史

## 目标地址预测

Branch Target Buffer分支目标缓冲区

* IF阶段：每个时钟周期需要知道提取的指令的地址
* 每个时钟周期当提取到分支指令时，需要知道目标地址
* 一些分支指令的目标只有在EX阶段之后才能知道，很影响速度
* 即使最简单的计算分支目标也需要等到ID阶段才能知道（仍然有1个时钟周期的延迟）
* 因此需要有一个quick-and-dirty的预测器来预测分支指令的目标地址

BTB是指令地址的索引，我们甚至不知道它是一个分支。当地址与BTB中的条目匹配时，该地址被预测为一个分支。BTB的条目告诉我们它是否会发生，并且如果发生了，它会跳转到哪。BTB只存储指令地址，所以当我们从IF阶段提取一个指令时，我们可以预测下一条指令是提取哪条。

![image-20200829161339111](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082916-13-39-6fb7091785537e1533c137989c67f636-image-20200829161339111-11d74b.png)

需要非常高的指令带宽

## Return Address Stack(RAS)

函数返回十分频繁，地址很难计算，必须等到EX阶段完成才知道，并且使用BTB很难预测地址，因为函数可能从多个地方被调用。

但是返回地址很容易预测，它是上次调用指令之后的地址，我们还没有从中返回，因此可以使用返回地址栈。

# 多处理器

弗林的并行机制分类法：

* 有多少指令流
* 有多少数据流

## SISD: Single I Stream, Single D Stream

例子：一个单处理器

## SIMD: Single I, Multiple D Streams

每个处理器在自己的数据上工作，执行相同的指令

## MISD: Multiple I, Single D Stream

不常用

## MIMD: Multiple I, Multiple D Streams

每个处理器执行自己的指令并对自己的数据进行操作

这是典型的现成多处理器
分为：

![image-20200829163024601](https://gitee.com/wei_yang_song/image-resources/raw/master/img/2020082916-30-24-a3e9365cb3497505018d2f925bff03bd-image-20200829163024601-ad3efb.png)

### 集中存储机制

Also “Symmetric Multiprocessors” (SMP)

“Uniform Memory Access” (UMA) 统一的内存访问

存在问题：内存的竞争访问

### 分布式内存机制

两种类型：

* 分布式共享内存（DSM），也称为NUMA（非统一内存访问），所有处理器都可以寻址所有内存位置，不同内存位置的延迟可能不同（本地访问比远程访问快）
* 消息传递（Message-Passing），处理器只能直接寻址本地内存，要与其他处理器通信，必须显式地发送/接收消息
