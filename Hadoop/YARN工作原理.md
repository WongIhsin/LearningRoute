# YARN工作原理

Hadoop从1时代到2时代的版本演化过程中，最核心的变化是YARN的加入，弥补了经典Hadoop模型在扩展性、效率上和可用性等方面存在的明显不足，可以说它是Apache对Hadoop1进行的升级改造。

两个重要的变更：

+ HDFS的NameNode可以以集群的方式部署，增强了NameNode的水平扩展能力和高可用性，分别是HDFS Federation与HA
+ MapReduce将Hadoop1时代的JobTracker中的资源管理及任务生命周期管理（包括定时触发及监控），拆分成两个独立的组件（ResourceManager和ApplicationMaster），并更名为YARN（Yet Another Resource Negotiator）

---

#### YARN on HDFS的工作原理

HDFS是典型的master/slave结构，它是负责大数据存储的分布式系统。客户端Client、主节点NameNode和数据节点DataNode之间进行交互完成Hadoop集群数据的存储与读取等操作

在HDFS1时代，NameNode发生故障，需要Secondary NameNode节点手动将保存的元数据快照恢复到重新启动的NameNode。进行数据同步备份时，总会存在一定的延迟，如果NameNode失效时有部分数据还没有同步到Secondary NameNode上，就极有可能存在数据丢失的问题

---

HDFS1时代，Hadoop1.x中的NameNode只可能有一个，HDFS架构包含两层，即NameSpace和Block Storage Service

+ NameSpace层包含目录、文件及块的信息，它支持所有NameSpace相关文件系统的操作，如创建、删除、修改，以及文件和目录的列举
+ Block Storage Service又包含两个部分
  + Block Management（块管理）：通过registrations和心跳机制维护集群中DataNode的基本成员关系
  + Storage（存储）：存储实际的数据库并提供针对数据块的读/写服务

NameNode不可以扩展，当前的NameSpace只能被存放在单个NameNode上，而NameNode在内存中存储了整个分布式文件系统中的元数据信息，这限制了集群中数据块、文件和目录的数目

HDFS2的NameNode可以以集群的方式部署，增强了NameNode的水平扩展能力和高可用性，分别是HDFS Federation与HA（High Availability）

+ HDFS Federation在现有HDFS基础上添加了对多个NameNode/NameSpace的支持，可以同时部署多个NameNode，这些NameNode之间相互独立，彼此之间不需要协调，DataNode同时在所有NameNode中注册，作为他们的公共存储节点，定时向所有的这些NameNode发送心跳块使用情况的报告，并处理所有NameNode向其发送的指令
  + 该架构引入了两个新的概念——存储块池Block Pool和集群ID Cluster ID
  + Block Pool是啥不懂
  + HDFS Federation的好处
    + 命名空间的扩展，改进了对于集群HDFS上数据量增加时仍采用一个NameNode进行管理的弊端
    + 当NameNode所持有的数据量达到一个非常大的数量级的时候（如超过1亿个文件），单NameNode的处理压力过大，容易陷入繁忙状态，进而影响集群整体的吞吐量
    + 多个命名空间可以很好地隔离各自命名空间内的任务，除一些必要的关键任务处理外，许多本地特性的普通任务因得到屏蔽而互不干扰
+ HA通过主备NameNode解决单点故障的问题，在Hadoop HA中可以同时启动两个NameNode。其中一个处于工作（Active）状态，另一个处于随时待命（Standby）状态
  + 两个NameNode之间通过共享存储，同步edits信息，保证数据的状态一致。
  + NameNode之间通过Network File System（NFS）或者Quorum Journal Node（JN）共享数据



---

#### MapReduce on YARN的工作原理

可以通过Job对象上的submit()方法来调用、运行MapReduce作业，也可以通过调用waitForCompletion()方法提交以前没有提交过的作业，并等待它的完成

#### MapReduce1的工作机制

JobClient：基于MapReduce接口库编写的客户端程序，负责提交MapReduce作业

JobTracker：一个Hadoop集群中只有一个JobTracker，是NameNode节点上的守护进程。它是各个MapReduce模块之间的协调者，负责协调MapReduce作业执行，例如：需要处理哪些文件，分配任务的Map和Reduce执行节点，监控任务的执行，重新分配失败的任务等。

TaskTracker：执行由JobTracker分配的任务，每个TaskTracker可以启动一个或多个Map Task和Reduce Task，负责具体执行Map任务和Reduce任务的程序。同时TaskTracker与JobTracker之间通过心跳（HeartBeat）机制保持通信，以维护整个集群的运行状态

#### MapReduce作业的具体工作过程

+ 作业的提交
+ 作业的初始化
+ 作业任务的分配
+ 作业任务的执行
+ 作业的完成

---

#### MapReduce YARN的工作机制

MapReduce1很好地解决了基于HDFS平台大数据的计算功能，但它在模式设计上还不够灵活。

在大多数的MapReduce作业过程中，Map任务通常要明显地多于Reduce任务

通常Hadoop时代集群管理规模只能达到4000台左右，影响了Hadoop的可扩展性和稳定性。YARN的出现取消了Slot的概念，把JobTracker由一个守护进程分为ResourceManager和ApplicationMaster两个守护进程，将JobTracker所负责的资源管理与作业调度分离。

ResourceManager负责原来JobTracker管理的所有应用程序计算资源的分配、监控和管理；ApplicationMaster负责每一个具体应用程序的调度和协调

综上，YARN在执行Job过程中，将一个业务计算任务Job分解为若干个Task来执行，执行的载体在YARN内部被称为容器（Container，物理上是一个动态运行的JVM进程。在Task完成后，YARN会杀死Container，并重新分配容器，进行初始化，运行新的任务，其实这个过程有些耗费系统资源）

---

#### 容错机制

实现强同步复制时，主副本可以将操作日志并发发给所有备份副本并且等待回复，只要至少1个备份副本返回成功，就可以回复客户端操作成功

异步复制时，主副本不需要等待备份副本的回应，只需要本地修改成功，就可以告知客户端写操作成功。

总的来说，HDFS会对写入的数据计算校验和，并在读取数据时验证校验和。DataNode负责收到数据后存储该数据及其校验和。

每个DataNode也会在一个后台进程中运行一个DataBlockScanner，从而定期验证存储在这个DataNode的所有数据库。该措施是解决物理存储媒体尚未损坏的有力措施

HDFS会存储每个数据块的复本，可以通过数据复本来修复损坏的数据块。客户端在读取数据块时，如果检测到错误，首先向NameNode报告已损坏的数据块及其正在尝试读取操作的这个DataNode。NameNode会将这个数据块标记为已损坏，对这个数据块的请求会被NameNode安排到另一个副本上。之后，它安排将这个数据块的另一个副本复制到另一个DataNode上，如此，数据块副本因子又回到期望水平。此后，已损坏的数据块副本会被删除

#### 安全性

早期版本的Hadoop未对数据传输过程中的通信安全做出合理有效的防范措施。

在YARN时代，从安全认证角度来讲，客户端与NameNode和客户端与ResourceManager之间的初次通信均采用了Kerberos进行身份认证，之后便换用委托令牌认证以减少开销，而DataNode与NameNode和NodeManager与ResourceManager之间的认证始终采用Kerberos机制。

YARN的授权机制是通过访问控制列表ACL实现的