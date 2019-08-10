# 网易公开课——清华大学公开课：操作系统

------

## 5.1虚拟内存的起因

+ 覆盖技术
+ 交换技术
+ 虚存技术
  + 目标
  + 程序局部性原理
  + 基本概念
  + 基本特征
  + 虚拟页式内存管理

---

#### 程序规模的增长速度远远大于存储器容量的增长速度

#### 理想中的存储器：更大、更快、更便宜的非易失性存储器

#### 实际中的存储器：存储器的层次结构

---

有一个大内存的感觉

---

#### 在计算机系统中，尤其是在多道程序运行的环境下，可能会出现内存不够用的情况，怎么办？

+ 如果是程序太大，超过了内存的容量，可以采用**手动的覆盖overlay技术**，只把需要的指令和数据保存在内存当中
+ 如果是程序太多，超过了内存的容量，可以采用**自动的交换swapping技术**，把暂时不能执行的程序送到外存中（整个程序导入导出硬盘）
+ 如果想要在有限容量的内存中，以==更小的页粒度==为单位装入更多更大的程序，可以采用**自动的虚拟存储技术**（一部分程序导入导出硬盘）



---

## 5.2覆盖技术

上世纪八九十年代，代表为DOS操作系统

+ 目标
  + 是在较小的可用内存中运行较大的程序。常用于多道程序系统，与分区存储管理配合使用
+ 原理
  + 把程序按照其自身逻辑结构，划分为若干个功能上相对独立的程序模块
  + 那些不会同时执行的模块共享一块内存区域，按时间先后来运行
    + 必要部分（常用功能）的代码和数据常驻内存
    + 可选部分（不常用功能）在其他程序模块中实现，平时存放在外存中
      + 在需要用到时才装入内存
    + 不存在调用关系的模块不必同时装入到内存，从而可以相互覆盖，即这些模块共用一个分区

---

#### 缺点：

+ 由程序员来把一个大的程序划分为若干个小的功能模块，并确定各个模块之间的覆盖关系，费时费力，增加了编程的复杂性
+ 覆盖模块从外存装入内存，实际上是以时间延长来换取空间节省

---

## 5.3交换技术

UNIX提出的

+ 目标
  + 多道程序在内存中时，让正在运行的程序或需要运行的程序获得更多的内存资源
+ 方法
  + 可将暂时不能运行的程序送到外存，从而获得空闲内存空间
  + 操作系统把一个进程的整个地址空间的内容保存到外存中（换出swap out），而将外存中的某个进程的地址空间读入到内存中（换入swap in）。换入换出内容的大小为整个程序的地址空间

#### 导入导出开销比较大

---

#### 交换技术实现中的几个问题

+ 交换时机的确定：何时需要发生交换？只当内存空间不够或有不够的危险时换出
+ 交换区的大小：必须足够大以存放所有用户进程的所有内存映像的拷贝：必须能对这些内存映像进行直接存取
+ 程序换入时的重定位：换出后再换入的内存位置一定要在原来的位置上吗？最好采用动态地址映射的方法

#### 覆盖和交换的比较

+ 覆盖只能发生在那些相互之间没有调用关系的程序模块之间，因此程序员必须给出程序内的各个模块之间的逻辑覆盖结构
+ 交换技术是以在内存中的程序大小为单位来进行的，它不需要程序员给出各个模块之间的逻辑覆盖结构。换言之，交换发生在内存中程序与管理程序或操作系统之间，而覆盖则发生在运行程序的内部

---

## 5.4 虚存技术

+ 在内存不够用的情形下，可以采用覆盖技术和交换技术，但是
  + 覆盖技术：需要程序员自己把整个程序划分为若干个小的功能模块，并确定各个模块之间的覆盖关系，增加了程序员的负担；
  + 交换技术：以进程作为交换的单位，需要把进程的整个地址空间都换进换出，增加了处理器的开销。粒度太大
+ 虚拟内存管理技术
+ 目标：
  + 像覆盖技术那样，不是把程序的所有内容都放在内存中，因而能够运行比当前的空闲内存空间还要大的程序，但做得更好，由操作系统自动来完成，无须程序员的干涉
  + 像交换技术那样，能够实现进程在内存与外存之间的交换，因而获得更多的空闲内存空间。但做得更好，只对进程的部分内容在内存和外存之间进行交换

---

#### 对程序提出了要求——程序的局部性原理

+ 程序的局部性原理principle of locality：指程序在执行过程中的一个较短时期，所执行的==指令地址==和指令的==操作数地址==，分别局限于一定区域
  + 时间局部性：一条指令的一次执行和下次执行，一个数据的一次访问和下次访问都集中在一个较短时期内
  + 空间局部性：当前指令和邻近的几条指令，当前访问的数据和邻近的几个数据都集中在一个较小区域内
+ 程序的局部性原理表明，从理论上来说，虚拟存储技术是能够实现的，而且在实现了以后应该是能够取得一个满意的效果的

#### 一个例子：程序的编写方法对缺页率的影响

例子：页面大小为4K，分配给每个进程的物理页面数为1，在一个进程中，定义了如下的二维数组`int A[1024][1024]`，该数组按行存放在内存，每一行放在一个页面中

整型数字是4byte大小

+ 程序编写方法1：

  ```c
  for (j=0; j<1024; j++)
      for (i=0; i<1024; i++)
          A[i][j] = 0;
  //1024 x 1024次缺页中断
  ```

+ 程序编写方法2：

  ```c
  for (i=0; i<1024; i++)
      for (j=0; j<1024; j++)
          A[i][j] = 0;
  //1024次缺页中断
  ```

---

# ==局部性==

---

#### 虚存技术——基本概念

可以在页式或段式内存管理的基础上实现

+ 在装入程序时，不必将其全部装入到内存，而只需将当前需要执行的部分页面或段装入到内存，就可让程序开始执行；
+ 在程序执行过程中，如果需执行的指令或访问的数据尚未在内存（称为缺页或缺段），则由处理器通知操作系统将相应的页面或段调入到内存，然后继续执行程序；
+ 另一方面，操作系统将内存中暂时不使用的页面或段调出保存在外存上，从而腾出更多空闲空间存放将要装入的程序以及将要调入的页面或段

---

MMU + 局部性程序 + OS

内核不应该被换出，内核是常驻程序

---

#### 虚存技术——基本特征

+ 大的用户空间：通过把物理内存与外存相结合，提供给用户的虚拟内存空间通常大于实际的物理内存，即实现了这两者的分离。如32位的虚拟地址理论上可以访问4GB，而可能计算机上仅有256M的物理内存，但硬盘容量大于4GB
+ 部分交换：与交换技术相比较，虚拟存储的调入和调出是对部分虚拟地址空间进行的
+ 不连续性：物理内存分配的不连续，虚拟地址空间使用的不连续

---

#### 页式内存管理

页表page table：完成逻辑页logical pages到物理页帧physical frames的映射maps to

---

#### 虚存技术——虚拟页式内存管理

+ 大部分虚拟存储系统都采用虚拟页式存储管理技术，即在页式存储管理的基础上，增加==请求调页==和==页面置换==功能
+ 基本思路：
  + 当一个用户程序要调入内存运行时，不是将该程序的所有页面都装入内存，而只是装入部分的页面，就可启动程序运行
  + 在运行的过程中，如果发现要运行的程序或要访问数据不在内存，则向系统发出缺页中断请求，系统在处理这个中断时，将外存中相应的页面调入内存，使得该程序能够继续运行

---

#### 页表表项

+ 逻辑页号
+ ==驻留位==：表示该页是在内存还是在外村，如果该位等于1，表示该页位于内存当中，即该页表项是有效的，可以使用；如果该位等于0，表示该页当前还在外存当中，如果访问该页表项，将导致缺页中断
+ 保护位：表示允许对该页做何种类型的访问，如只读、可读写、可执行等
+ 修改位：表明此页在内存中是否被修改过。当系统回收该物理页面时，根据此位来决定是否把它的内容写回外存
+ 访问位：如果该页面被访问过（包括读操作或写操作），则设置此位，用于页面置换算法
+ 物理页帧号

---

#### 缺页中断处理过程

+ 如果在内存中有空闲的物理页面，则分配一物理页帧f，然后转第4步；否则转第2步
+ 采用某种页面置换算法，选择一个将被置换的物理页帧f，它所对应的逻辑页为q。如果该页在内存期间被修改过，则需把它写回外存
+ 对q所对应的页表项进行修改，把驻留位置为0
+ 有需要访问的页p装入到物理页面f当中
+ 修改p所对应的页表项的内容，把驻留位置为1，把物理页帧号置为f
+ 重新运行被中断的指令

---

#### 后备存储Backing Store

+ 在何处保存未被映射的页？
  + 能够简单地识别在二级存储器中的页
  + 交换空间（磁盘或者文件）：特殊格式，用于存储未被映射的页面
+ 概念：后备存储backing store
  + 一个虚拟地址空间的页面可以被映射到一个文件（在二级存储中）中的某个位置
  + 代码段：映射到可执行二进制文件
  + 动态加载的共享库程序段：映射到动态调用的库文件
  + 其他段：可能被映射到交换文件（swap file）没有上面三种文件直接映射的中间生成的数据

---

#### 虚拟内存性能

为了便于理解分页的开销，使用==有效存储器访问时间==effective memory access time(EAT)

EAT = 访存时间 x 页表命中几率 + page fault处理时间 x page fault几率

+ 例子
  + 访存时间：10ns
  + 硬盘访问时间：5ms
  + 参数p = page fault几率，缺页比率
  + 参数q = dirty page几率，写操作概率
  + EAT = 10(1-p) + 5,000,000p(1+q)

p足够小，就可以使EAT接近10ns

程序编写的局部性使得p足够小

---

## 6.1最优页面置换算法

+ 功能与目标
+ 实验设置与评价方法
+ 局部页面置换算法
  + 最优页面置换算法（OPT optional）
  + 先进先出算法（FIFO）
  + 最近最久未使用算法（LRU Least Recently Used）
  + 时钟页面置换算法（Clock）
  + 最不常用算法（LFU Least Frequently Used）
  + Belady现象
  + LRU，FIFO和Clock的比较
+ 全局页面置换算法
  + 局部页替换算法的问题
  + 工作集模型
  + 工作集页置换算法
  + 缺页率置换算法
  + 抖动问题

---

#### 功能与目标

+ 功能：当缺页中断发生，需要调入新的页面而内存已满时，选择内存当中哪个物理页面被置换
+ 目标：==尽可能地减少页面的换进换出次数==（即缺页中断的次数）。具体来说，把未来不再使用的或短期内极少使用的页面换出，通常只能在局部性原理指导下依据过去的统计数据来进行预测
+ ==页面锁定Frame locking==：用于描述必须常驻内存的操作系统的关键部分或时间关键time critical的应用程序。实现的方法是：在页表中添加锁定标志位lock bit

---

#### 设置一个模拟操作系统的环境

+ 记录一个进程对页访问的一个轨迹
  + 举例：（虚拟）地址跟踪（页号，位移）...
    + (3,0)，(1,9)，(4,1)，(2,1)，(5,3)，(2,0)，(1,9)，(2,4)，(3,1)，(4,8)
  + 生成页面轨迹
    + 3，1，4，2，5，2，1，2，3，4（替换如c，a，d，b，e，b，a，b，c，d）
+ 模拟一个页面置换的行为并且记录产生页缺失数的数量
  + 更少的缺失，更好的性能

---

#### 最优页面置换算法

+ 基本思路：当一个缺页中断发生时，对于保存在内存当中的每一个逻辑页面，计算在它下一次访问之间，还需要等待多长时间，从中选择等待时间最长的那个，作为被置换的页面
+ 这只是一种理想情况，在实际系统中是无法实现的，因为操作系统无从知道每一个页面要等待多长时间以后才会再次被访问
+ 可用作其他算法的性能评价的依据（在一个模拟器上运行某个程序，并记录每一次的页面访问情况，在第二遍运行时即可使用最优算法）

---

## 6.2先进先出算法

+ 先进先出算法First-In First-Out，FIFO
+ 基本思路：选择在==内存中驻留时间最长的页面==并淘汰之，具体来说，系统维护着一个链表，记录了所有位于内存当中的逻辑页面。从链表的排列顺序来看，链首页面的驻留时间最长，链尾页面的驻留时间最短，当发生一个缺页中断时，把链首页面淘汰出局，并把新的页面添加到链表的末尾
+ 性能较差，调出的页面有可能是经常要访问的页面，并且有==Belady现象==，FIFO算法很少单独使用

---

实现简单，缺页次数比较多

---

## 6.3最近最久未使用算法

+ 最近最久未使用算法：Least Recently Used，LRU
+ 基本思路：当一个缺页中断发生时，选择最久未使用的那个页面，并淘汰之
+ 它是对最优页面置换算法的一个==近似==，其依据是程序的局部性原理，即在最近一小段时间（最近几条指令）内，如果某些页面被频繁地访问，那么在将来的一小段时间内，它们还可能会再一次被频繁地访问。反过来说，如果在过去某些页面长时间未被访问，那么在将来它们还可能会长时间地得不到访问

---

局部性程序

LRU算法需要记录各个页面使用时间的先后顺序。

#### 开销比较大

两种可能的实现方法：

+ 系统维护一个页面链表，最近刚刚使用过的页面作为首结点，最久未使用的页面作为尾节点。每一次访问内存时，找到相应的页面，把它从链表中摘下来，再移动到链表之首，每次缺页中断发生时，淘汰链表末尾的页面
+ 设置一个活动页面栈，当访问某页时，将此页号压入栈顶，然后，考察栈内是否有与此页面相同的页号，若有则抽出，当需要淘汰一个页面时，总是选择栈底的页面，它就是最久未使用的

---

## 6.4时钟页面置换算法

+ Clock页面置换算法：LRU的近似，对FIFO的一种改进
+ 基本思路：
  + 需要用到页表项当中的访问位，当一个页面被装入内存时，把该位初始化为0，然后如果这个页面被访问（读/写），则把该位置为1
  + 把各个页面组织成环形链表（类似钟表面），把指针指向最老的页面（最先进来）
  + 当发生一个缺页中断时，考察指针所指向的最老页面，若它的访问位为0，立即淘汰，若访问位为1，则把该位置为0，然后指针往下移动一格，如此下去，直到找到被淘汰的页面，然后把指针移动到它的下一格

---

访问位置1是硬件操作，软件也可以操作，置1置0

当程序访问页表某一项时，被访问表项访问位会置1，由硬件完成

不精确的时间比较

操作系统会定期清0

---

+ 维持一个环形页面链表保存在内存中
  + 用一个时钟（或者使用/引用）位来标记一个页面是否经常被访问
  + 当一个页面被访问的时候，这个位被设置为1
+ 时钟头扫描页面寻址一个带有used bit - 0
  + 替换在一个周期内没有被引用过的页面

---

效果贴近LRU

---

## 6.5二次机会法

+ 这里有一个巨大的代价来替换“脏”页
+ 修改Clock算法，使它允许脏页总是在一次时钟头扫描中保留下来
  + 同时使用脏位和使用位来指导置换

#### Enhanced Clock algorithm

used_dirty：
0  0 → replace page
0  1 → 0  0
1  0 → 0  0
1  1 → 0  1

---

## 6.6最不常用算法

+ 最不常用算法，Least Frequently Used，LFU
+ 基本思路：当一个缺页中断发生时，选择访问次数最少的那个页面，并淘汰之
+ 实现方法：对每个页面设置一个访问计数器，每当一个页面被访问时，该页面的访问计数器加1，在发生缺页中断时，淘汰计数值最小的那个页面
+ LRU和LFU的区别：LRU考察的是==多久未访问==，时间越短越好，而LFU考察的是==访问的次数==，访问次数越多越好
  + 问题：一个页面在进程开始时使用的很多，但以后就不使用了。实现也费时费力
  + 解决方法：定期把次数寄存器右移一位

---

## 6.7Belady现象、LRU、FIFO、Clock的比较

+ ==Belady现象==：在采用FIFO算法时，有时会出现分配的物理页面数增加，缺页率反而提高的异常现象
+ ==Belady现象的原因==：FIFO算法的置换特征与进程访问内存的动态特征是矛盾的，与置换算法的目标是不一致的（即替换较少使用的页面），因此，被它置换出去的页面并不一定是进程不会访问的
+ LRU算法没有belady现象

---

栈算法？？不清楚

---

#### LRU、FIFO和Clock的比较

+ LRU算法和FIFO本质上都是先进先出的思路，只不过LRU是针对==页面的最近访问时间==来进行排序，所有需要在每一次页面访问的时候动态地调整各个页面之间的先后顺序（有一个页面的最近访问时间变了）；而FIFO是针对==页面进入内存的时间==来进行排序，这个时间是固定不变的，所以各页面之间的先后顺序是固定的，如果一个页面在进入内存后没有被访问，那么它的最近访问时间就是它进入内存的时间。换句话说，如果内存当中的所有页面都未曾访问过，那么LRU算法就退化为FIFO算法
+ 例如，给进程分配3个物理页面，逻辑页面的访问顺序为1,2,3,4,5,6,1,2,3...
+ 算法 + 局部性程序
+ LRU算法性能较好，但系统开销较大；FIFO算法系统开销较小，但可能会发生belady现象，因此，折中的办法就是Clock算法，在每一次页面访问时，它不必去动态地调整该页面在链表当中的顺序，而仅仅是做一个标记，然后等到发生缺页中断的时候，再把它移动到链表末尾。对于内存当中那些未被访问的页面，Clock算法的表现和LRU算法一样好；而对于那些曾经被访问过的页面，它不能像LRU算法那样，记住它们的准确位置

---

## 6.8局部页替换算法的问题、工作集模型

程序运行的阶段性，对物理页帧的需求是动态的

---

#### 工作集模型

前面介绍的各种页面置换算法，都是基于一个前提，即==程序的局部性原理==。但是此原理是否成立？

+ 如果局部性原理不成立，那么各种页面置换算法就没有什么分别，也没有什么意义。例如：假设进程对逻辑页面的访问顺序是1,2,3,4,5,6,7,8,9...，即单调递增，那么在物理页面数有限的前提下，不管采用何种置换算法，每次的页面访问都必然导致缺页中断
+ 如果局部性原理是成立的，那么如何来证明它的存在，如何来对它进行定量地分析？这就是==工作集模型==！

---

#### 工作集Working Set

工作集：一个进程==当前==正在使用的==逻辑页面==集合

可以用一个二元函数W(t, △)来表示

+ t是当前的执行时刻
+ △称为工作集窗口（working-set window），即一个定长的页面访问的时间窗口
+ W(t, △) = 在当前时刻t==之前==的△时间窗口当中的所有页面所组成的集合（随着t的变化，该集合也在不断地变化）
+ |W(t, △)|指工作集的大小，即页面数目

---

#### 工作集大小的变化

进程开始执行后，随着访问新页面逐步建立较稳定的工作集。当内存访问的局部性区域的位置大致稳定时，工作集大小也大致稳定；局部性区域的位置改变时，工作集快速扩张和收缩过渡到下一个稳定值

---

#### 常驻集

常驻集是指在当前时刻，进程实际驻留在内存当中的页面集合

+ 工作集是进程在运行过程中固有的性质，而常驻集取决于系统分配给进程的物理页面数目，以及所采用的页面置换算法；
+ 如果一个进程的整个工作集都在内存当中，即常驻集>=工作集，那么进程将很顺利地运行，而不会造成太多的缺页中断（直到工作集发生剧烈变动，从而过渡到另一个状态）
+ 当进程常驻集的大小达到某个数目之后，再给它分配更多的物理页面，缺页率也不会明显下降

---

## 6.9两个全局置换算法

#### 工作集页置换算法

追踪之前t个引用，在之前t个内存访问的页引用是工作集，t被称为窗口大小

==即使没有产生缺页==，某个页不在窗口中就要换出去

固定窗口，常驻集固定

---

#### 缺页率页面置换算法

+ ==可变分配策略==：常驻集大小可变。例如：每个进程在刚开始运行的时候，先根据程序大小给它分配一定数目的物理页面，然后在进程运行过程中，再动态地调整常驻集的大小
  + 可采用==全局页面置换==的方式，当发生一个缺页中断时，被置换的页面可以是在其他进程当中，各个并发进程竞争地使用物理页面
  + 优缺点：性能较好，但增加了系统开销
  + 具体实现：可以使用==缺页率算法==（PFF，page fault frequency）来动态调整常驻集的大小

---

#### 缺页率

缺页率表示“缺页次数/内存访问次数”（比率）或“缺页的平均时间间隔的倒数”

影响缺页率的因素：

+ 页面置换算法
+ 分配给进程的物理页面数目
+ 页面本身的大小
+ 程序的编写方法

---

#### 缺页率算法

若运行的程序的缺页率过高，则通过增加工作集来分配更多的物理页面；若运行的程序的缺页率过低，则通过减少工作集来减少它的物理页面数。使运行的每个程序的缺页率保持在一个合理的范围内

一个交替的工作集计算明确的试图最小化页缺失

+ 当缺页率高的时候-增加工作集
+ 当缺页率低的时候-减少工作集

#### 算法

保持追踪缺失发生概率

当缺失发生时，从上次页缺失起计算这个时间记录这个时间，tlast是上次的页缺失的时间

如果发生页缺失之间的时间是“大”的，之后减少工作集

如果tcurrent - tlast > T，之后从内存中移除所有在[tlast, tcurrent]时间内没有被引用的页

如果这个发生页缺失的时间是“小”的，之后增加工作集

如果tcurrent - tlast < T，之后增加缺失页到工作集中

---

## 6.10抖动问题

#### 抖动问题thrashing

+ 如果分配给一个进程的物理页面太少，不能包含整个的工作集，即常驻集<工作集，那么进程将会造成很多的缺页中断，需要频繁地在内存与外存之间替换页面，从而使进程的运行速度变得很慢，我们把这种状态称为“==抖动==”
+ 产生抖动的原因：随着驻留内存的进程数目增加，分配给每个进程的物理页面数不断减小，缺页率不断上升，所以OS要选择一个适当的进程数目和进程需要的帧数，以便在并发水平和缺页率之间达到一个平衡

---

## ==没懂==

抖动问题可能会被本地的页面置换改善

更好的规则为加载控制：调整MPL，所以：Better criteria for load control: Adjust MPL so that:

+ **平均页缺失时间mean time between page fault MTBF** = 页缺失服务时间 page fault service time PFST
+ ∑MSt = **内存的大小**

---

内存快用完时，大量换入换出，IO操作频繁，CPU时间基本被换入换出

根据CPU使用率来调整进程数目

MTBF/PFST = 1时是理想的，CPU利用率比峰值低一点点

通过OS的管理，进程数也多，对硬件资源充分利用，避免内存抖动

---




























































