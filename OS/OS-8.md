# 网易公开课——清华大学公开课：操作系统

---

+ 死锁
  + 死锁问题
  + 系统模型
  + 死锁特征
  + 死锁处理方法
    + Deadlock Prevention（死锁预防）
    + Deadlock Avoidance（死锁避免）
    + Deadlock Detection（死锁检测）
    + Recovery from Deadlock（死锁恢复）

---

## 11.1死锁问题

+ 流量只在一个方向
+ 桥的每个部分可以看作为一个资源
+ 如果死锁，可能通过一辆车倒退可以解决（抢占资源和回滚）
+ 如果发生死锁，可能几辆车必须都倒退
+ 可能发生饥饿

---

+ 一组阻塞的进程持有一种资源等待获取另一个进程所占有的一个资源
+ 例子
  + 系统有2个磁盘驱动器
  + P1和P2各有一个，都需要另外一个

---

## 11.2系统模型

+ 资源类型R1，R2，...，Rm
  CPU cycles, memory space, I/O devices
+ 每个资源类型Ri有Wi实例
+ 每个进程使用资源如下
  + request/get  <--  free resource
  + use/hold  <--  requested/used resource
  + release  <--  free resource

---

#### 可重复使用的资源

+ 在一个时间只能一个进程使用且不能被删除（没有特殊情况OS不应该暴力处理拥有资源的进程）
+ 进程获得资源，后来释放由其他进程重用
+ 处理器，I/O通道，主和副存储器，设备和数据结构，如果文件，数据库和信号量
+ 如果每个进程拥有一个资源并请求其他资源，死锁可能发生

#### 使用资源

+ 创建和销毁
+ 在I/O缓冲区的中断，信号，消息，信息都是资源
+ 如果接收消息阻塞可能会发生死锁
+ 可能少见的组合事件会引起死锁

---

#### 资源分配图

一组顶点V和边E的集合

+ V有两种类型
  + P = {P1，P2，...，Pn}，集合包括系统中是所有进程
  + R = {R1，R2，...，Rm}，集合包括系统中是所有资源类型
+ requesting/claiming edge —— directed edge: Pi --> Rj，进程需要资源
+ assignment/holding edge —— directed edge: Rj --> Pi，资源被进程使用

---

有向图产生环——>可能死锁，死锁——>环

#### 基本情况

+ 如果图中不包含循环————没有死锁
+ 如果图中包括循环
  + 如果每个资源类只有一个实例，那么死锁
  + 如果每个资源类有几个实例，可能死锁

---

## 11.3死锁特征

#### 如果四个条件同时成立死锁==可能==出现

+ ==互斥==：在一个时间只能有一个进程使用资源
+ ==持有并等待==：进程保持至少一个资源，正在等待获取其他进程持有的额外资源
+ ==无抢占==：一个资源只能被进程自愿释放，进程已经完成了它的任务之后
+ ==循环等待==：存在等待进程集合{P0，P1，...，PN}，P0正在等待P1所占用的资源，P1正在等待P2占用的资源，...，PN-1在等待PN所占用的资源，PN正在等待P0所占用的资源

#### 这四个条件是死锁的必要条件，非充分条件

---

## 11.4死锁处理办法

+ 确保系统永远不会进入死锁状态（需要很多约束，很多功能大大限制，功能无法充分运行）
+ 运行系统进入死锁状态，然后恢复（判断死锁存在的开销很大）
+ 忽略这个问题，假装系统中从来没有发生死锁；==用于大多数操作系统，包括UNIX==

---

## 11.5死锁预防和死锁避免

## 死锁预防

不让死锁出现，打破4个必要条件

#### 限制申请方式

+ 互斥——共享资源不是必须的，必须占用非共享资源
+ 占用并等待——必须保证当一个进程请求的资源，它不持有任何其他资源
  + 需要进程请求并分配其所有资源，它开始执行之前或允许进程请求资源仅当进程没有资源
  + 资源利用率低；可能发生饥饿
+ 无抢占——（可以kill掉其他进程？）
  + 如果进程占有某些资源，并请求其他不能被立即分配的资源，则释放当前正占有的资源
  + 被抢占资源添加到资源列表中
  + 只有当它能够获得旧的资源以及它请求新的资源，进程可以得到执行
+ 循环等待——对所有资源类型进行排序，并要求每个进程按照资源的顺序进行申请（通用OS中用的不多，嵌入式OS中常用，因为它资源类型有限）

---

## 死锁避免

申请资源时候判断，如果可能出现死锁，就不分配资源，有效的方法

#### 需要系统具有一些额外的先验信息提供

+ 最简单和最有效的模式是要求每个进程声明它可能需要的每个类型资源的==最大数目==
+ 资源的分配状态是通过限定==提供==与==分配==的资源数量，和进程的==最大==需求
+ 死锁避免算法==动态检查==的资源分配状态，以确保永远不会有一个环形等待状态（不安全状态）
  + 不安全状态包含死锁状态，约束较大

---

+ 当一个进程请求可用资源，系统必须判断立即分配是否能使系统处于安全状态
+ 系统处于安全状态指：针对所有进程，存在安全序列
  + ==序列（P1，P2，...，PN）是安全的==：针对每个Pi，Pi要求的资源能够由当前可用的资源和所有的Pj持有的资源来满足，其中j<i
    + 如果Pi资源的需求不是立即可用，那么Pi可以等到所有Pj完成
    + 当Pi完成后，Pi+1可以得到所需要的资源，执行，返回所分配的资源，并终止
    + 用同样的方法，Pi+2，Pi+3和Pn能获得其所需的资源

+ 如果系统处于安全状态——无死锁
+ 如果系统处于不安全状态——可能死锁
+ ==避免死锁==：确保系统永远不会进入不安全状态

+ Claim edge Pi —> Rj表示进程Pj请求资源的Rj：由虚线表示
+ 当一个进程请求资源，Claim edge转换为request edge
+ 当资源被进程释放，assignment edge转换为claim edge
+ 在系统中资源必须要求先验

---

## 11.6银行家算法

Banker's Algorithm银行家算法是一个死锁避免的著名算法，是由艾滋格·迪杰斯特拉在1965年为T.H.E系统设计的一种避免死锁产生的算法。他以银行借贷系统的分配策略为基础，判断并保证系统的安全运行

#### 背景

在银行系统中，客户完成项目需要申请贷款的数量是有限的，每个客户在第一次申请贷款时要声明完成该项目所需的最大资金量，在满足所有贷款要求并完成项目时，客户应及时归还

银行家在客户申请的贷款数量不超过自己拥有的最大值时，都应尽量满足客户的需要

在这样的描述中，银行家就好比操作系统，资金就是资源，客户就相当于要申请资源的进程

#### 前提条件

+ 多个实例
+ 每个进程都必须能最大限度地利用资源
+ 当一个进程请求一个资源，就不得不等待
+ 当一个进程获得所有的资源就必须在一段有限的时间释放它们

基于上述前提条件，银行家算法通过尝试寻找允许每个进程获得的最大资源并结束（把资源返还给系统）的进程请求的一个理想执行时许，来决定一个状态是否是安全的。不存在这满足要求的执行时序的状态都是不安全的

#### 银行家算法的数据结构

n = 进程数量，m = 资源类型数量

+ Max（总需求量）：n x m矩阵。如果Max[i, j] = k，表示进程Pi最多请求资源类型Rj的k个实例
+ Available（剩余空闲量）：长度为m的向量。如果Available[j] = k，有k个类型Rj的资源实例可用
+ Allocation（已分配量）：n x m矩阵。如果Allocation[i, j] = k，则Pi当前分配了k给Rj的实例
+ Need（未来需要量）：n x m矩阵。如果Need[i, j] = k，则Pi可能需要至少k个Rj实例完成任务
+ Need[i, j] = Max[i, j] - Allocation[i, j]

#### Safety State Estimating Algorithm

1. Work和Finish分别是长度为m和n的向量
   初始化：

   ```c
   Work = Available;                    //当前资源剩余空间量
   Finish[i] = false for i = 1,2,...,n  //线程i没结束
   ```

2. 找这样的i：
   //接下来找出Need比Work小的进程i
   （a） Finish[i] = false
   （b）Needi <= Work

3. ```c
   Work = Work + Allocationi;//进程i的资源需求量小于当前剩余空闲资源量，所以配置给它再回收
   Finish[i] = true;
   //转到2
   ```

4. ```c
   If Finish[i] == true for all i:    //所有进程的Finish为True，表明系统处于安全状态
       then the system is in a safe state.
   ```

---

#### Banker's Algorithm

Initial: Request = request vector for process Pi. If Request[j] = k then Process Pi wants k instances of resource type Rj.

While:

1. 如果Requesti <= Needi转到步骤2，否则，提出错误条件，因为进程已经超过了其==最大要求==
2. 如果Requesti <= Available，转到步骤1，否则Pi必须==等待==，因为资源不可用
3. 假装给Pi分配它需要的资源：//生成一个需要判断状态是否安全的资源分配环境
   Available = Available - Requesti;
   Allocationi = Allocationj + Requesti;
   Needi = Needi - Requesti;

CALL Safety State Estimating Algorithm

+ 如果返回safe，将资源分配给Pi
+ 如果返回unsafe，Pi必须等待，旧的资源分配状态被恢复

---

公开课话音不同步，没看懂

#### 下面是恐龙书的银行家算法的介绍

This algorithm is commonly known as the *banker's algorithm*. The name was chosen because the algorithm could be used in a banking system to ensure that the bank never allocated its available cash in such a way that it could no longer satisfy the needs of all its customers. 一些废话

When a new process enters the system, it must declare the maximum number of instances of each resource type that it may need. This number may not exceed the total number of resources in the system. When a user requests a set of resources, the system must determine whether the allocation of these resources will leave the system in a safe state. 进程需要提前声明自己需要各类资源的最大值，当一个进程请求一个资源，系统必须决定是否分配资源后系统能保持安全状态。

If it will, the resources are allocated; otherwise, the process must wait until some other process releases enough resources. 如果可以满足，资源会被分配，如果不能满足，进程将会等待直到其他进程释放足够的资源

Let *n* be the number of processes in the system and *m* be the number of resource types. We need the following data structures:

+ Available. A vector of length *m* indicates the number of available resources of each type.
+ Max. An *n* x *m* matrix defines the maximum demand of each process.
+ Allocation. An *n* x *m* matrix defines the number of resources of each type currently allocated to each process.
+ Need. An *n* x *m* matrix indicates the remaining resource need of each process.

The vector *Allocationi* specifies the resources currently allocated to process Pi; the vector *Needi* specifies the additional resources that process Pi may still request to complete its task.

7.5.3.1 Safety Algorithm

This algorithm can be described as follows:

1. Let *Work* and *Finish* be vectors of length *m* and *n*, respectively. Initialize *Work = Available* and *Finish[i] = false* for *i* = 0,1,...,n-1
2. Find an *i* such that both
   a. Finish[i] == false
   b. Needi <= Work
   If no such i exists, go to step 4.
3. Work = Work + Allocationi
   Finish[i] = true
   Go to step 2.
4. If Finish[i] == true for all i, then the system is in a safe state.

This algorithm may require an order of m x n^2 operations to determine whether a state is safe.

7.5.3.2 Resource-Request Algorithm

When a request for resources is made by process Pi, the following actions are taken:

1. If *Requesti* <= *Needi*, go to step 2.
2. If *Requesti* <= *Available*, go to step 3. Otherwise, Pi must wait, since the resources are not available.
3. Have the system pretend to have allocated the requested resources to process Pi by modifying the state as follows:
   *Available = Available -Requesti;*
   *Allocationi = Allocationi + Requesti;*
   *Needi = Needi - Requesti;*
   If the resulting resource-allocation state is safe, the transaction is completed, and process Pi is allocated its resources. However if the new state is unsafe, the Pi must wait for *Requesti*, and the old resource-allocation state is restored.

7.5.3.3 An Illustrative Example

...

We claim that the system is currently in a safe state. 

Suppose now that process Pi requests one  additional instance of Request1 = (1,0,2). To decide whether this request can be immediately granted.

...

You should be able to see, however, that when the system is in this state, a request for (3,3,0) by P4 cannot be granted, since the resources are not available. Furthermore, a request for (0,2,0) by P0 cannot be granted, even though the resources are available, since the resulting state is unsafe.

还是没太懂

---

## 11.7死锁检测和死锁恢复

+ 允许系统进入死锁状态
+ 死锁检测算法
+ 恢复机制

---

资源分配图——>等待图

+ Maintain *wait-for* graph
  + 节点是进程
  + Pi → Pj：Pi等待Pj
+ 定期调用检测算法来搜索图中是否存在循环
+ 算法需要n^2次操作，n是图中顶点的数目

---

+ Available：长度为M的向量表示每种类型可用资源的数量
+ Allocation：一个nxm矩阵定义了当前分配给各个进程每种类型资源的数量
+ Request：一个nxm矩阵表示各进程的当前请求

#### 死锁检测算法

和死锁避免算法很像，==定期去执行，看系统是否有环==

1. Work和Finish分别是长度为m与n的向量，初始化
   a. Work = Available    //work为当前空闲资源量
   b. For i = 1,2,...,n, If Allocationi > 0, then Finish[i] = false //Finish为线程是否结束
2. 找出这样的索引
   a. Finish[i] = false
   b. Requesti <= Work
   //线程没有结束的线程，且此线程将需要的资源量小于当前空闲资源量
   如果没有找到，转到4
3. Work = Work + Allocationi
   Finish[i] = true
   转到2    //把找到的线程拥有的资源释放回当前空闲资源中
4. If Finish[i] == false, for some i，系统处于死锁状态，此外，If Finish[i] == false, Pi死锁    //如果有Finish[i]等于false，这表示系统处于死锁状态

算法需要O(m x n^2)操作检测是否系统处于死锁状态

---

#### 操作系统很少使用检测算法、银行家算法等的原因

开销比较大，而且银行家算法需要提前知道每个进程需要的最大资源个数，该信息很难获得

只用在调试阶段

---

#### 检测算法使用

+ 何时、使用什么样的频率来检测依赖于：
  + 死锁多久可能会发生
  + 多少进程需要被回滚
+ 如果检测算法多次被调用，有可能是资源图有多个循环，所以我们无法分辨出多个可能死锁进程中的哪些“造成”死锁
+ 很难去使用，更多的是在开发阶段去判断系统是否正确工作

---

## 死锁恢复

+ 终止所有的死锁进程
+ 在一个时间内终止一个进程直到死锁解除
  + 终止进程的顺序应该是
    + 进程的优先级
    + 进程运行了多久以及需要多少时间才能完成
    + 进程占用的资源
    + 进程完成需要的资源
    + 多少进程需要被终止
    + 进程是交互还是批处理
  + 选择一个受害者——最小的成本
  + 回滚——返回到一些安全状态，重启进程到安全状态
  + 饥饿——同一进程可能一直被选作受害者，包括回滚的数量

---

#### 目前死锁还未很好解决，OS采用鸵鸟策略，一直跑，出现了再死锁恢复

---

## 11.8IPC概述

+ IPC进程间通信 Inter Process Communication
  + 概述
    + 通信模型
    + 直接及间接通信
    + 阻塞与非阻塞
    + 通信链路缓冲
  + 信号
  + 管道
  + 消息队列
  + 共享内存

---

+ 进程通信的机制及同步
+ 不使用共享变量的进程通信
+ IPC facility提供2个操作：
  + send(message)——消息大小固定或者可变
  + receive(message)
+ 如果P和Q想通信，需要
  + 在它们之间建立通信链路
  + 通过send/receive交换消息
+ 通信链路的实现
  + 物理（例如，共享内存，硬件总线）
  + 逻辑（例如，逻辑属性）

---

#### 直接通信

+ 进程必须正确的命名对方：
  + send(P, message)——发送信息到进程P
  + receive(Q, message)——从进程Q接受消息
+ 通信链路的属性
  + 自动建立链路
  + 一条链路恰好对应一对通信进程
  + 每对进程之间只有一个链接存在
  + 链接可以是单向的，但通常为双向的

---

#### 间接通信

+ 定向从消息队列接收消息
  + 每个消息队列都有一个唯一的ID
  + 只有它们共享了一个消息队列，进程才能够通信
+ 通信链路的属性
  + 只有进程共享一个共同的消息队列，才建立链路
  + 链接可以与许多进程相关联
  + 每对进程可以共享多个通信链路
  + 链接可以是单向或双向
+ 操作
  + 创建一个新的消息队列
  + 通过消息队列发送和接收消息
  + 销毁消息队列
+ 原语的定义如下：
  + send(A, message)——发送消息到队列A
  + receive(A, message)——从队列A接受消息

---

+ 消息传递可以是阻塞或非阻塞
+ 阻塞被认为是同步的
  + **Blocking send** has the sender block until the message is received
  + **Blocking receive** has the receiver block until a message is available
+ 非阻塞被认为是异步的
  + Non-blocking send has the sender send the message and continue
  + Non-blocking receive has the receiver receive a valid message or null

---

+ 队列的消息被附加到链路；可以是以下3种方式之一
  + 0容量——0 message
    + 发送方必须等待接收方（rendezvous）
  + 有限容量——n messages的有限长度
    + 发送方必须等待，如果队列满
  + 无限容量——无限长度（理论上的理解）
    + 发送方不需要等待

---

## 11.9信号、管道、消息队列和共享内存

#### 信号

+ Signal（信号）
  + 软件中断通知事件处理
  + Examples：SIGFPE，SIGKILL，SIGUSRI，SIGSTOP，SIGCONT
+ 接收到信号时会发生什么
  + Catch：指定信号处理函数被调用
  + Ignore：依靠操作系统的默认操作
    + Examples：Abort，memory damp，suspend or restime process
  + Mask：闭塞信号因此不会传送
    + 可能是暂时的（当处理同样类型的信号）
+ 不足
  + 不能传输要交换的任何数据

---

+ OS在内核态接收到信号产生，OS通过修改堆栈，OS从内核态返回用户态，会根据栈信息跳到信号处理函数入口执行，执行完毕之后继续返回到被打断位置
+ 木马病毒就是使用类似的方式
+ 流程
  + 注册信号处理函数register handles
  + 派遣给处理器dispatch to handler
  + 信号程序处理栈signal handler stack

---

#### 管道

+ 子进程从父进程继承文件描述符，==父进程帮子进程建立好的通道==，传输的是字节流，传递的不是有意义的数据结构
+ 进程不知道（或不关心）从键盘，文件，程序读取或写入到终端，文件，程序
+ 例如% ls | more
+ 管道其实是内存中的一块buffer，buffer是有大小的
+ shell
  + 创建管道
  + 为ls创建一个进程，设置stdout为管道写端
  + 为more创建一个进程，设置stdin为管道读端

---

#### 消息队列

+ 消息队列按FIFO来管理消息
  + Message：作为一个字节序列存储
  + Message Queues：消息数组
  + FIFO & FILO configuration

---

#### 共享内存

相对于管道、消息队列是间接通信方式，共享内存是一种直接通信的方式

+ 进程
  + 每个进程都有私有地址空间
  + 在每个地址空间内，明确地设置了共享内存段
+ 优点
  + 快速、方便地共享数据
+ 不足
  + 必须同步数据访问

同一块物理内存映射到不同的进程的地址空间

+ 最快的方法
+ 一个进程写另外一个进程立即可见
+ 没有系统调用干预
+ 没有数据复制
+ 不提供同步
  + 由程序员提供同步

---

#### 还有Socket方式，但是其属于网络通信不属于OS，这里不介绍了










