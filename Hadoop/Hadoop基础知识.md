Hadoop版本更新很快，实际上，当前Hadoop从宏观上只有两个时代——Hadoop 1时代和Hadoop 2时代

#### Hadoop2时代多出一层资源管理层Yet Another Resource Negotiator, YARN，使HDFS上存储的内容供更多的框架使用成为可能，提高了资源利用率，节省了项目成本投入

HDFS Hadoop Distributed File System，Hadoop分布式文件系统

HDFS实现多台机器上的数据存储，MapReduce实现多台机器上的数据计算，YARN实现资源调度与管理。由于YARN的引入，使原来不能直接应用于HDFS框架的一些组件如Spark等可以通过YARN实现与HDFS的交互

---

Apache官网2018年2月公布的信息，Apache Hadoop项目目前包括Hadoop Common、HDFS、Hadoop YARN和Hadoop MR共计4个模块

#### Hadoop Common公共模块

为Hadoop其他模块提供支持实用程序，是整体Hadoop项目的核心，主要包括一组分布式文件系统、通用I/O组件与接口（序列化、远程过程调用RPC、持久化数据结构）

#### HDFS（Hadoop Distributed File System，Hadoop分布式文件系统）

提供对应用程序数据的高吞吐量访问，是Google GFS的开源实现。HDFS客户端会将大小不一的众多文件切成等大小的数据块（Block，默认为128MB），以多副本的形式存在不同的集群节点中

#### Hadoop YARN（Yet Another Resource Negotiator，另一种资源协调者）

作业调度和集群资源管理的框架，Hadoop2新增系统，主要作用是负责集群的资源管理和统一调度

#### Hadoop MR（MapReduce，分布式计算框架）

从Hadoop2开始，MR演进成基于YARN系统的大型数据集的并行处理技术。首先从HDFS中取出数据块Block，进行分片Split，每个分片对应一个Mapper任务（即把一个大文件的任务分解成n个小任务），将Mapper计算的结果通过Reducer进行汇总计算，得出最终结构并进行输出

#### 此外还有11个Apache的其他Hadoop项目的相关配置

HBase：在Hadoop上提供了类似于Bigtable的能力，支持大型表的结构化数据存储。HBase底层基于HDFS存储数据，故具有良好的横向扩展能力，可以动态增加服务器以达到扩容的目的

Hive：是基于Hadoop的一个提供数据汇总和特定查询的数据库基础架构

Pig：是一个基于Hadoop的用于并行计算的高级数据流语言和执行框架，其中包括用于表达数据分析程序的高级语言，以及用于评估这些程序的基础结构

Spark：是以Hadoop数据处理而设计的快速、通用的计算引擎

ZooKeeper：是一个用于分布式应用的高性能协调服务框架，Hadoop和HBase的重要组件，是为分布式应用提供一致性服务的软件。ZooKeeper提供的功能包括配置维护、域名服务、分布式同步、组服务等

---

## 核心

Common为Hadoop整体框架提供支撑性功能
HDFS负责存储数据
MapReduce负责数据计算

---

## Hadoop1时代

Hadoop Common提供基础支持，主要包括文件系统（File System），远程过程调用（RPC）和数据串行化库（Serialization Libraries）

HDFS由一个NameNode和多个DataNode组成，其中NameNode是HDFS的管理者，负责管理文件系统命名空间，维护文件系统的文件树，以及所有的文件、目录的元数据。这些信息存储在NameNode维护的两个本地的磁盘文件中——**命名空间镜像文件**FSImage和**编辑日志文件**Edit logs。DataNode是HDFS中保存数据的节点。DataNode定期向NameNode报告其存储的数据块列表，以备使用者通过直接访问DataNode获得相应的数据

MapReduce是一个基于计算框架的编程模型，适用于在大规模计算机集群上编写离线的、大数据量的、相对快速处理的并行化程序，它由一个JobTracker和多个TaskTracker组成。其中JobTracker是应用于MapReduce模块之间的控制协调者，负责协调MapReduce作业的执行。当一个MapReduce作业提交到集群中，JobTracker负责确定后续执行计划。每个Hadoop集群中只有一个JobTracker。TaskTracker负责执行由JobTracker分配的任务，每个TaskTracker可以启动一个或多个Mapper/Reducer任务。同时TaskTracker与JobTracker之间通过心跳HeartBeat机制保持通信，以维护整个集群的运行状态。Mapper/Reducer任务有TaskTracker启动，负责具体执行哪些任务程序

---

## Hadoop2时代

首先针对单NameNode制约HDFS的扩展性问题，提出了HDFS Federation，让多个NameNode分管不同的目录，进而实现访问隔离和横向扩展，彻底解决了NameNode单点故障的问题

其次，取消了Map Slots与Reducer Slots的概念，并将JobTracker的功能一分为二，即全局资源管理ResourceManager（RM）和管理每个应用程序的ApplicationMaster（AM），应用程序是单个作业或作业的DAG（有向无环图）任务。用RM来管理节点资源，负责所有应用程序的资源分配，用AM来监控与调度作业。AM中每个Application都有一个单独的示例，Application是用户提交的一组任务，他可以由一个或多个Job任务组成，进而诞生了全新的通用资源管理框架YARN

基于YARN，用户可以运行各种类型的应用程序，不再像Hadoop1那样仅局限于MapReduce一类应用

---

# RPC工作原理

Hadoop的远程过程调用（Remote Procedure Call，RPC）是Hadoop中的核心通信机制，RPC主要用于所有Hadoop的组件元数据交换，如MapReduce、Hadoop分布式文件系统HDFS和Hadoop的数据库HBase

RPC是一种通过网络从远程计算机程序上请求服务，而不需要了解底层网络技术的协议，RPC假定某些传输协议（如TCP和UDP）存在，为通信程序之间携带信息数据

#### RPC的特点

- 透明性：远程调用其他机器上的程序，对用户来说就像调用本地方法一样
- 高性能：RPC Server能够并发处理多个来自Client的请求
- 可控性：JDK中已经提供了一个RPC框架——RMI，但是该RPC框架过于重要级并且可控之处比较少，所以Hadoop RPC实现了自定义的RPC框架

Caller——Network——Callee

#### Caller：

User——User-stub——RPCRuntime

#### Callee：

RPCRuntime——Server-stub——Server

------

#### RPC运行机制

- 序列化层：Client和Server端通信传递的信息采用了Hadoop里提供的序列化类或自定义的Writable类型
- 函数调用层：Hadoop RPC通过动态代理及Java反射实现函数调用
- 网络传输层：Hadoop RPC采用了基于TCP/IP的Socket机制
- 服务器端框架层：RPC Server利用Java NIO及采用了事件驱动的I/O模型，提高自己的并发处理能力

------

#### Hadoop RPC默认的设计是基于Java套接字通信，基于高性能网络的通信并不能达到最大的性能，会是一个性能瓶颈

------

Hadoop中RPC的设计技术

- 动态代理
  - 动态代理可以提供对另一个对象的访问，同时隐藏实际对象的具体事实，因为代理对象对客户隐藏了实际对象
- 反射——动态加载类
  - 反射机制是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意一个方法和属性
- 序列化
  - 序列化是把对象转化为字节序列的过程，反过来说就是反序列化。两方面的应用：一是存储对象，可以是永久地存储在硬盘的一个文件上，也可以是存储在redis支持序列化存储的容器中；而是网络上远程传输对象
- 非阻塞异步IO(NIO)
  - 非阻塞异步IO指的是用户调用读写方法是不阻塞的，立刻返回，而且用户不需要关注读写，只需要提供回调操作，内核线程在完成读写后回调用户提供的callback

#### 使用Hadoop RPC的步骤

- 定义RPC协议
  - 是客户端和服务器端之间的通信接口，定义了服务器端对外提供的服务接口
- 实现RPC协议
  - Hadoop RPC协议通常是一个Java接口，用户需要实现该接口
- 构造和启动RPC Server
  - 直接使用静态类Builder构造一个RPC Server，并调用函数start()启动该Server
- 构造RPC Client并发送请求
  - 使用静态方法getProxy构造客户端代理对象，直接通过代理对象调用远程端的方法

------

1. Hadoop中所有自定义的RPC接口都需要继承VersionedProtocol接口，它描述了协议的版本信息。
2. 默认情况下，不同版本号的RPC Client和Server之间不能相互通信，因此客户端和服务器端通过版本号标识

```java
public interface IProxyProtocol extends VersionProtocol {
    static final long VERSION = 23234L;
    int Add(int number1, int number2);
}
```

```java
public class MyProxy implements IProxyProtocol {
    public int Add(int number1, int number2) {
        System.out.println("我被调用了！");
        int result = number1 + number2;
        return result;
    }
    
    public long getProtocolVersion(String protocol, long clientVersion)
        throws IOException {
        System.out.println("MyProxy.ProtocolVersion=" + IProxyProtocol.VERSION);
        return IProxyProtocol.VERSION;
    }
}
```

构造RPC Server并启动服务

```java
public class MyServer {
    public static int PORT = 5432;
    public static String IPAddress = "127.0.0.1";
    
    public static void main(String[] args) throws Exception {
        MyProxy proxy = new MyProxy();
        final Server server = RPC.getServer(proxy, IPAddress, PORT, new Configuration());
        server.start();
    }
}
```

#### 核心RPC.getServer()方法

4个参数：被调用的Java对象，服务器地址，服务器端口，获取平台环境参数的对象。

到此为止，服务器就处于监听状态，不停地等待客户端的请求到达

#### 构造RPC Client并发出请求

这里使用静态方法getProxy或waitForProxy构造客户端代理对象，直接通过代理对象调用远程端的方法

```java
public class MyClient {
    public static void main(String[] args) {
        InetSocketAddress inetSocketAddress = new InetSocketAddress(
            MyServer.IPAddress, MyServer.PORT);
        try {
            IProxyProtocol proxy = (IProxyProtocol) RPC.waitForProxy(
                IProxyProtocol.class, IProxyProtocol.VERSION, inetSocketAddress, 
                new Configuration());
            int result = proxy.Add(10, 25);
            System.out.println("10+25=" + result);
            RPC.stopProxy(proxy);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

#### 核心RPC.waitForProxy()方法

返回的代理对象就是服务端对象的代理，内部就是使用java.lang.Proxy实现的

------

#### RPCoIB和JVM——旁路缓冲管理方案：在高性能网络InfiniBand上数据交换的改进

# （没懂）

InfiniBand架构是一种支持多并发链接的“转换线缆”技术，在这种技术中，每种链接都可以达到2.5Gbits/s的运行速度。InfiniBand是一个功能完善的网络通信系统

#### 1. 基本思想

RPCoIB使用与现有的基于套接字的Hadoop RPC相同的接口
目前的Hadoop RPC的设计是基于Java的InputStream/OutputStream（客户端）及SocketChannel（服务器端），他们之间基本的读写操作都相似。为了更好地权衡性能和向后兼容，要设计一套基于RDMA的和Java IO的接口兼容类，这些类包括RDMAInputStream/RDMAOutputStream及RDMAChannel

##### RDMA：Remote Direct Memory Access，远程直接数据存取

#### 2. JVM——旁路缓冲区管理

**基于历史两级缓冲池**：当一个缓冲区（具有适当尺寸）以处理当前调用时，缓冲区有较高的可能性利用与下一个调用相同的元组`<protocol, method>`，这种现象叫Message Size Locality。因此，设计了如下缓冲池设计

#### 3. RPCoIB的一体化设计

上层应用程序可以透明地使用这样的设计



------

## MapReduce工作原理

并行计算模型通常指从并行算法的设计和分析出发，将各种并行计算机（至少某一类并行计算机）的基本特征抽象出来，形成一个抽象的计算模型。从更广的意义上说，并行计算模型为并行计算提供了硬件和软件界面，在该界面的约定下，并行系统硬件设计者和软件设计者可以开发对并行性的支持机制，从而提高系统的性能。

有代表性的并行计算模型包括PRAM模型/BSP模型/LogP模型/C^3模型等

#### 并行计算Parallel Computing

指同时使用多种计算资源解决计算问题的过程，是提高计算机系统计算速度和处理能力的一种有效手段。基本思想是用多个处理器来协同求解同一问题，即将被求解的问题分解成若干个部分，各部分均由一个独立的处理机来并行计算。

并行计算系统既可以是专门设计的、含有多个处理器的超级计算机，也可以是以某种方式互连的若干台独立计算机构成的集群

#### 并行处理计算机系统Parall Computer System

指同时执行多个任务、多条指令或同时对多个数据项进行处理的计算机系统。1.能同时执行多条指令或同时处理多个数据项的单中央处理器计算机；2.多处理机系统

并行处理计算机的结构特点主要表现在两个方面：1.在单处理机内广泛采用各种并行措施；2.由单处理机发展成各种不同耦合度的多处理机系统

------

#### Hadoop MapReduce（分布式计算框架）

基于Hadoop MapReduce软件框架可以轻松编写应用程序，并且以可靠、容错的方式在由商用机器组成的数千个节点的大型集群上，并行处理TB量级的数据集

Google MapReduce是一个编程模型，也是一个处理和生成超大数据集的算法模型的相关实现。用户首先创建一个Map函数处理一个基于键值对的数据集合，输出中间的基于键值对key-value的数据集合；然后再创建一个Reduce函数来合并所有具有相同中间键值key的中间值value

在运行时只关心如下几点：如何分割输入数据，在大量计算机组成的集群上的调度，集群中计算机的错误处理，管理集群中计算机之间必要的通信

实现一个MapReduce框架模型的主要贡献是通过简单的接口来实现自动的并行化和大规模的分布式计算，通过使用MapReduce模型接口，实现在大量普通的PC上进行高性能的计算

#### 通常，计算节点和存储节点是在一起的，即MapReduce框架和分布式文件系统HDFS运行在相同的节点上

使整个集群的网络带宽被非常高效地利用

#### MapReduce计算模型

MapReduce编程框架适用于大数据计算，这里大数据计算主要包括大数据管理、大数据分析及大数据清洗等大数据预处理等操作

通俗来讲，MapReduce就是在HDFS将一个大文件切分成众多小文件分别存储在不同节点的基础上，尽量在数据所在的节点上完成小任务计算再合并成最终结果。其中这个大任务分解成小任务再合并的过程是一个典型的合并计算过程，以尽量快速地完成海量数据的计算

一个MapReduce作业job通常会把输入的数据集切分为若干独立的数据块Block，由Map任务task以完全并行的方式处理它们。框架会对Map的输出先进行排序，然后把结果输入给Reduce任务。典型的是作业的输入和输出都会被存储在文件系统中。整个框架负责任务的调度、监控和已经失败的任务的重新执行

#### MapReduce计算的几个阶段

- HDFS：存储了大文件切分后的数据块，图中Block大小为128MB

- 分片：在进行Map计算之前，MapReduce会根据输入文件计算输入分片input split，每个输入分片针对一个Map任务，输入分片存储的并非数据本身，而是一个分片长度和一个记录数据的位置的数组。分片树不一定等于Block数

  - 分片的具体大小可以通过如下公式计量（没懂）

    ```
    minSize = max{minSplitSize, mapred.min.split.size}
    maxSize = mapred.max.split.size
    splitSize = max{minSize, min{maxSize, blockSize}}
    ```

- Map任务：将输入键值对（key/value pair）映射到一组中间格式的键值对集合。Map的数目通常是由输入数据的大小决定的，一般就是所有输入文件的总块（Block）数。官网给出的参考值是Map正常的并行规模是每个节点10~100个Map，对于CPU消耗较小的Map任务可以设到300个左右。由于每个任务初始化需要一定的时间，所以比较合理的情况是Map执行的时间至少超过1分钟。这样10TB的输入数据，每个Block的大小为128MB的话，则需要大约82000个Map来完成任务

- Shuffle任务：Hadoop的核心思想是MapReduce，而Shuffle是MapReduce的核心，Shuffle的主要工作是从Map结束到Reduce开始之间的过程，Shuffle阶段完成了数据的分区/分组/排序的工作

- Reduce任务：将与一个key关联的一组中间数值集归约Reduce为一个更小的数值集。Reducer的输入就是Mapper已经排好序的输出。这个过程和排序两个阶段是同时进行的；Map的输出也是一边被取回一边被合并的。Reducer的输出是没有排序的。增加Reduce的数目会增加整个框架的开销，但可以改善负载均衡，降低由于执行失败带来的负面影响。（不懂）

  - 比例因子比整体数目稍小一些，是为了给框架中的推测性任务speculative-tasks或失败的任务预留一些Reduce的资源（不懂）

- 输出：记录MapReduce结果的输出

------

#### 从HDFS上获取指定的Block进行分片后，将分片结果输入Mapper进行Map处理，将结果输出给Shuffle完成分区、排序、分组的计算过程，之后计算结果会被Reducer按指定业务进一步处理，最终将计算出的结果输出到指定的位置，如HDFS上

MapReduce框架透明性强，易于编程、易扩展；对于宕机等原因导致的错误，容错性很强；能够处理PB级以上海量数据的离线处理。但由于他的工作机制有延时性，对于类似于MySQL这样在毫秒级或者秒级内返回结果的情况达不到要求。MapReduce自身的设计特点决定了数据源必须是静态的

# 故不能处理处理动态变化的数据，如流式计算等

华为在FusionInsight产品中对HDFS进行了文件块集中分布的加强，使需要做关联和汇总计算的两种表所在的文件FileA和FileB，通过指定同一个分布ID，使其所有的Block分布在一起，不再需要跨节点读取数据就能完成计算，极大提高MR Join的性能

------

## Hadoop改进

#### LATE算法：良好的适应异构性环境

Hadoop一个最大的优点是能够自动处理失败，如果一个节点坏了，Hadoop会将此节点的任务放到另一个节点上执行。同样地，工作但速度慢的节点被称为掉队节点，Hadoop会将此节点上的任务随机地复制到另一台机器上进行更快速的处理。这种行为称为推测执行

推测执行带来的几个难题

- 推测执行需要占用资源，如网络等
- 选择节点与选择任务同样重要
- 在异构性环境中，很难确定哪一个节点的速度慢

Hadoop被广泛应用于短小任务中，其中应答时间是很重要的一个因素。Hadoop的性能与任务调度程序有很大的关系，任务调度程序假定集群的节点均匀并且任务是线性执行的，根据这些假设决定何时重新执行掉队任务。事实上，集群并不一定是均匀的，如在虚拟数据中心中就不具备均匀性。Hadoop的任务调度程序在异构环境中可能引起严重的性能退化。

当一个节点空闲时，Hadoop从3种任务中选择一个执行：第一种是失败的任务，第二种是没被执行的任务，第三种是随机地选择一个任务。使用进度得分来衡量任务的进度，最小值为0，最大值为1。对于Map来说，进度得分就是输入数据中被读入的比例。对于Reduce来说，执行被分为3部分：任务读取Map结果，Map结果排序，以及Reduce任务执行，在每部分中，分数为被处理的数据比例

Hadoop根据每类任务的平均得分来定义一个阈值，每当一个任务的进度得分小于该类任务的平均得分减去0.2，并且该任务运行了至少一分钟，那此任务可被看作掉队任务。调度程序保证每个任务最多只有一个被推测执行（不懂）

#### LATE调度程序的基本思想

每次选取最晚结束的任务来推测执行，从而极大地减少响应时间。有很多方法能够估计任务的剩余时间。

假设任务是以恒定的速率执行的，每个任务的进度率根据公式ProgressScore/T来估计，即每分钟处理的任务进度是多少，剩余时间根据公式(1-ProgressScore)/ProgressRate来估计

每次把推测任务复制到快速节点上运行，并且通过一个简单的机制来实现，即不要把任务复制到进度总分在阈值以下的节点中，其中进度总分为该节点所有已经完成及正在运行的任务的进度得分相加

#### LATE算法的工作流程如下

当一个节点申请任务，并且当前执行的推测任务数小于上限，则：当此节点的总进程得分小于SlowNodeThreshold时，忽视请求；根据剩余时间将当前正在运行的非推测任务排序；将进度率低于SlowTaskThreshold的任务中排名最高的任务，复制到此节点

------

#### Mantri：MapReduce异常处理

MapReduce的作业经常会有无法预测的性能表现。一个作业由一个分阶段执行的任务集合构成。这些任务之间会有前后依赖关系——有的任务依赖于其他任务的计算结果，并且很多任务之间是并行执行的。如果一个任务的执行时间超出相似任务的时间，则那些依赖这个任务结果的任务就都会被延迟。在一个作业中，少数的几个这样的异常可以阻止作业剩余任务的执行，甚至可以使作业执行时间增加34%

Mantri是一个能够监视任务并且根据导致异常的原因来剔除异常的系统。它主要采取以下技术：1.重启已经认识到资源约束和工作不均衡的异常任务；2.根据集群网络情况安排任务；3.根据开销-收益分析结果来保护任务的输出结果

trem代表完成一个正在运行中的任务所需的剩余时间，tnew代表重新运行该任务所需的时间

计算trem的模型如下，trem = telaspsed·d/dread + twrapup，式中dread代表一个任务已经读取的数据量，d代表这个任务总共需要处理的数据量；telaspsed代表加工dread所需的时间，所以前半部分代表加工剩余数据所需的时间；twrapup代表所有数据已经读取进来之后需要的计算时间，这个时间是根据之前的任务进行估计的

计算tnew的模型如下，tnew=processRate x locationFactor x d + schedLag

#### Mantri的实现方法

实现重启的方法：Mantri使用两种不同的方法来实现重启。第一种杀死一个进行中的任务并在别的地方重新运行，第二种安排制作一个这个任务的副本。执行这两种方法均需要一个条件，即概论Ptnew<trem比较大

配置任务的方法：已知网络连接情况和输入数据的位置，使用一个中心调度机制可以最优地对所有任务进行配置，但是这要求掌握实时的网络信息和中心调度机，进行大量的协调运算。Mantri采用了一种近似最优解的算法，这种算法是根据集群网络情况配置任务的，既不需要知道实时的网络情况，也不需要进行作业间的协调工作

避免重新计算的方法：为了减轻重新计算造成的作业完成时间的增加，Mantri通过复制任务的输出结果来避免数据丢失的问题

Mantri最根本的优势在于它将静态的MapReduce作业的结构和在运行过程中动态获取的信息整合在一个完整的框架里。这个框架可以依据整合的信息发现异常，对可以采取的针对性措施、可以获得的收益和消耗进行衡量，如值得采取，就实施该针对性措施

------

#### SkewTune：MapReduce中数据偏斜处理

当数据偏斜发生时，一些分区的操作在处理输入数据时，显然比其他分区花费的时间长，从而造成了整个计算时间变长

一个MapReduce工作主要由Map阶段和Reduce阶段组成，在每个阶段，输入数据的一个子集被计算机集群进行分布式任务处理。Map任务完成后通知Reduce任务接收新的可用数据，这个转换过程被称为重分配，直到所有的Map任务完成后，Reduce阶段的重分配才能完成，然后开始Reduce操作，负载不均衡可以发生在Map阶段，也可以发生在Reduce阶段。数据偏斜会显著增加作业执行时间，以及降低吞吐率

#### MapReduce的4种数据偏斜类型

1. Map阶段：高代价记录。Map任务一个接着一个处理由键值对组成的一系列记录。理想情况下，处理时间在不同的记录之间差距不大。然而根据不同的应用，有些记录可能需要更多的CPU执行时间和内存。这些高代价记录可能比其他记录大，也可能Map算法的运行时间取决于记录的值
2. Map阶段：异构Map。MapReduce是一个一元运算符，但是也可以通过逻辑级联多个数据集，作为一个单一的输入来模拟实现n元运算，每个数据集可能需要不同的处理，从而导致运行时间的多峰分布
3. Reduce阶段：分区偏移。在MapReduce中，Map任务的输出会通过默认的散列分区或者用户自定义分区逻辑分布在不同的Reduce任务。默认散列分区通常是足够的均匀分布数据。然而，散列分区不保证均匀分布
4. Reduce阶段：高代价关键字组。在MapReduce中Reduce任务处理（关键字，值域）对序列，被称作关键字组。类似于Map任务中处理高代价记录一样，高代价关键字组会造成Reduce任务运行时间的偏移

SkewTune不要求来自用户的输入，它被广泛使用，因为它并不探究什么原因导致数据偏斜发生的，而是观察工作的执行，重新均衡负载，使资源变得可用，即当集群中的一个节点变成空闲时，SkewTune通过最大预期的剩余处理时间来标识任务，然后这个任务的未处理输入数据会主动地重新分区，重分区会充分利用集群中的节点，并且会保留输入数据的顺序，是原始输出可以通过串联来重建

SkewTune设计用于MapReduce的引擎，特征在于通过基于磁盘的处理和面向记录的数据模型。通过扩展Hadoop并行数据处理系统来实现SkewTune技术。SkewTune通过优化在减小数据偏斜的同时，能够保持容错和MapReduce的可扩展性

SkewTune设计理念：1.开发者透明；2.解决方案透明；3.最大的适用性；4.没有同步障碍

SkewTune实现方法简介：SkewTune以Hadoop任务作为输入，将任务Map和Reduce阶段看作是独立的，SkewTune的数据偏斜缓解技术被设计用于MapReduce的类型的数据处理引擎

这些引擎在数据偏斜处理时有以下重要的特征：1.一个协调——工作架构，其中协调节点做出调度决策，而工作节点运行分配给它们的任务。一个任务完成后，工作节点从协调节点那里获取新的任务；2.去耦执行，即一个操作符不会对它的前一个操作符产生反馈，二者彼此独立；3.独立处理记录，即执行一个UDO时每条记录是独立的；4.任务进度估计，即估计剩余时间，然后工作节点定期传给协调节点；5.任务统计，即跟踪一些基本的统计，例如处理过的或者未处理的数据大小或记录数

------

在没有SkewTune时，发生数据偏斜后的运行时间由最慢的任务决定

在使用了SkewTune后，在任务T1完成后检测到了数据偏斜，此时标记T2为落后者，然后重新分区T2中未处理完的数据。系统会将剩余的数据分给T1，T2，T3这3个节点而不是仅仅分给T1，T2两个节点，这样T3结束后也可以继续工作。重新分配的目的是保证任务是同时完成的。重新分配后的子任务T2a，T2b，T2c被称为mitigators。SkewTune重复这个检测-减少循环，当检测到T4是下一个落后者时，将T4的剩余未处理数据重新分区。检测时间太早会导致任务拆分，从而造成不必要的开销，太晚则会错过最佳时间，从而使减缓数据偏斜的效果变差。

SkewTune通过估计最大剩余时间来选择落后任务

标记数据偏斜的原则：剩余处理时间的一半大于重分区的开销，即tremain/2 > w
w是在30s的量级上的，也就是说任务剩余时间在1min以上才会触发重分区

SkewTune实现数据偏斜减缓的原则如下：尽量减少重分区次数以减少重分区开销（通过标识一个落后者）；尽量减少重分区的可见副作用以实现透明度（使用范围分区）；尽量减少总的开销（使用一种廉价的、线性时间的启发式算法）

#### 数据偏斜检测算法

算法步骤如下：

- 停止落后者的运算：协调者通知落后者停止运算，并且记录下最后处理的位置，然后对剩余数据重分区。如果发生落后者处在不能停下来或者很难停下来的状态时，协调者选择另外一个落后者，或者如果该落后者是最后一个任务，重分区该落后者的整个输入
- 扫描剩余数据：为了保证数据偏斜减缓的透明性，SkewTune使用范围分区分配工作给mitigator，这样能够保证数据顺序保持不变。如果用Hash函数的方式并增加一个额外的MapReduce任务来合并mitigator的输出，一方面会增加开销，另一方面Hash函数不能保证均匀分配。范围分配落后者的剩余输入数据需要收集剩余数据的一些信息，SkewTune采用了对输入数据的压缩摘要来收集，摘要使用了一系列关键字间隔的方式，每个间隔大致相等
- 选择间隔大小：|S|表示集群总的节点数，△表示未处理的字节数，由于SkewTune想要分配不均匀的工作量给不同的mitigator，所以生成了k x |S|个间隔，实现更细粒度的数据分配，但它们也通过增加数量增加了开销（不是很懂）

------

#### 间隔生成算法

------

SkewTune实现方法的规划减缓器：算法分为两个阶段，第一个阶段计算最快的完成时间opt（假设能够对剩余的工作完美分割），当一个节点的分配工作小于2 x w时，停止以防止产生任意小碎片。第二个阶段中，算法依次给最早可用的减缓器分配间隔值，不断重复，直到分配完所有的间隔。时间复杂度为O(|I| + |S|log|S|)，其中I是间隔的数目，S是集群中节点的个数（不是很懂）

#### SkewTune能够显著地减少作业在偏斜状态下的工作时间，并且对没有偏斜的开销增加很小。SkewTune同样能够保证输出和初始未优化时一样的顺序和分区特性，使平台和现有的代码兼容

------

#### 基于RDMA的MapReduce设计：提升大数据应用的性能和规模

因为由Map和Reduce过程得到的数据可能分布在不同聚类中，网络性能在考察使用Hadoop MapReduce框架实现的数据密集应用程序的性能上起到关键作用

默认的基于Socket通信模型的数据传输方式成为了系统性能的瓶颈。为了避免这种潜在的瓶颈，企业数据中心的开发者开始尝试使用高性能互联技术（如InfiniBand）来提升其大数据应用的性能和规模。

当前的Hadoop中间层组件还不能完全利用由InfinitiBand所提供的高性能通信功能。因此人们提出了一种在InfinitiBand网络上基于RDMA的Hadoop MapReduce的设计，这种InfinitiBand网络上基于RDMA的MapReduce新的设计方案及检索中间数据的高效的预取和高速缓存机制表现良好

------

通常的设计方案中，在默认的MapReduce工作流程中，映射输出被保存在本地磁盘上的文件中，可通过TaskTracker访问。在运行ReduceTask、Shuffle、Reduce的过程中，系统通过HTTP提供这些映射输出

基于RDMA的MapReduce设计，在Shuffle阶段，该设计修改了TaskTracker和ReduceTask两者，以充分发挥RDMA的好处，而不是使用HTTP协议

#### 各个模块的关键技术如下

1. 基于RDMA的Shuffle设计

   在重排阶段，为了实现RDMA的好处，修改了TaskTracker和ReduceTask

   TaskTracker增加以下新组件：RDMA监听器，RDMA接收器，DataRequest队列，RDMA响应器

   ReduceTask增加了RDMA备份器：在原来的MapReduce设计中，备份线程响应TaskTracker的请求数据并为Merge操作排序数据。同样的，RDMACopier发送请求给TaskTrackers，并把数据存储到DataToMerge队列等待合并操作

2. 快速Merge设计

   可以在一些键值对从Map输出到规约器的瞬间开始合并。因此，在设计中，RDMA响应器一侧的TaskTracker发送一定数量的键值对，其发生在Map输出的开始部分而不是整个Map。在接受来自所有Map位置的键值对的同时，一个ReduceTask合并所有这些数据，建立一个优先队列。然后，它不断从优先队列中提取按顺序排序的键值对，并把这些数据写入一个先入先出的结构体DataToReduce队列中。当每个Map输出文件已经在Map中排序时，ReduceTask的合并操作只用从优先队列中提取数据，直到来自特定Map的键值对数目减少到0。此时他需要从特定Map任务得到下一组键值对，并写入到优先队列中（不懂）

3. 中间数据预提取和缓存

   为了确保TaskTracker更快地响应，本地磁盘中的映射输出文件的中间数据要有一个高效的缓存机制。TaskTracker在缓存时添加了MapOutput预提取器，MapOutput预提取器是用于尽可能快地缓存中间映射输出的守护线程池。

   这个缓存新功能就是它可以根据数据获得的难易度和必要性调整缓存，可以更迅速地根据ReduceTask需求数据优先进行缓存

------

# HDFS工作原理

分布式文件系统，源自Google，2003年的GFS论文，是GFS克隆版

GFS（Google File System）是一个可扩展的分布式文件系统，用于大型的、分布式的、对大量数据进行访问的应用。它运行于廉价的普通硬件上并提供容错功能，可以给大量的用户提供总体性能较高的服务。

Google的GFS不仅仅是一个文件系统，还包含数据冗余、支持低成本的数据快照，除了提供常规的创建、删除、打开、关闭、读、写文件操作，GFS还提供附加记录的操作

附加记录操作是原子性的，多个客户端可以在不需要额外的同步锁定的情况下，同时对一个文件追加数据。根据Google应用的具体情况，对文件的随机写入操作几乎不存在，读操作也通常是按顺序的，绝大部分文件的修改是采用在文件尾部追加数据。

---

#### GFS架构

一个主服务器master server，多个数据服务器chunk server，多个客户client

+ client是一套类似于传统文件系统的API接口函数，是一组专用接口，应用程序通过访问这些接口来实现操作
+ chunk服务器负责具体的存储工作，它要做的是把拆分块chunk保存在本地硬盘上、读写块数据
+ master节点管理所有的文件系统元数据、管理系统范围内的活动。例如，管理整个系统内所有chunk的副本，决定chunk的存储位置，创建新chunk和它的副本，协调各种各样的系统活动以保证chunk被完全复制，在所有的chunk服务器之间进行负载均衡，回收不再使用的存储空间等

---

GFS中存储的文件都被分成固定64MB的chunk，每一个chunk在创建时会被master分配一个不变的、全球唯一的64位chunk标识，chunk服务器要根据这个标识和字节范围来读写数据块。chunk在不同的机器中复制多份，默认为3份。chunk的复制由master负责的

master服务器存储的元数据有3类，存放在内存中，保证操作的速度

+ 文件和chunk的命名空间
+ 文件和chunk的对应关系
+ 每个chunk副本的存放地点

前两种元数据会以记录变更日志的方式，记录在操作系统的系统日志文件中，第三种元数据chunk的存放地点不会被持久保存，只是在master服务启动或者有新的chunk服务器加入时，由master向各个chunk服务器轮询它们所存储的chunk信息

---

#### 应用程序读取数据的流程：

1. 应用指定读取某个文件的某段数据，因为数据块是定长的，client可以计算出这段数据跨越了几个数据块，client将文件名和需要的数据块索引发送给master
2. master根据文件名查找命名空间和文件——块映射表，得到需要的数据块副本所在的地址，将数据块的ID和其所有副本的地址反馈给client
3. client选择一个副本，联系chunk服务器索取需要的数据
4. chunk服务器返回数据给client。每个chunk以块为单位划分，每个块64KB，对应一个32bit的校验和，如果读取chunk时，数据和校验和不匹配，就返回错误，从而使client选择其他chunk服务器上的副本

---

#### 应用程序写数据的流程：

1. client首先发送请求（包括文件名）到GFS的master；
2. master通过查找返回相应的所有chunk服务器及chunk信息；
3. client根据这些信息给chunk服务器发送请求，去执行写操作。一次写入，必须在所有副本全部写入成功，才算成功写入

---

#### HDFS可以认为是GFS的简化版本

HDFS不同于其他分布式文件系统的地方：HDFS高度容错，可部署在低成本硬件上。HDFS提供对应用程序数据的高吞吐量访问，适用于具有大数据集的应用程序。HDFS放宽了一部分POSIX约束，来实现流式读取文件系统数据的目的

---

# HDFS

HDFS存储模式支持结构化、半结构化和非结构化数据的均匀存储。将文件切分成等大的数据块Block，默认为64MB或者128MB，再以多副本形式进行存储

整个系统把分块Block、分发、容错等过程比较难的地方都抽象掉，使用户感觉像在操作一块硬盘那样容易

#### HDFS具有以下特点：

1. 功能强大，操作简单、易用。数据文件的切分、容错、负载均衡过程透明化；
2. 良好的扩展性。可以动态增加服务器集群里的节点数，解决集群在线扩容的问题。另外，用户可以通过HDFS的一些工具，对已有的数据进行重新分布，以达到数据在新增的节点及老的节点上均衡分布的目的；
3. 高容错性；
4. 支持流式数据访问。有效地实现数据一次写入、多次读取的概念模型；
5. 适合PB量级以上海量数据的存储；目前DFS能在一个集群里扩展到成千上万个节点，10万个用户的并发访问量；
6. 异构软硬件平台间的可移植性。

---

#### HDFS采用master/slave架构

一个HDFS集群由一个NameNode和一定数目的DataNode组成

+ Client（客户端）：客户端是指需要访问HDFS文件服务的用户或应用。如命令行、API应用
+ Metadata（元数据）：方便集群及文件管理，存储的文件系统目录树信息（如文件名、目录名、文件和目录的从属关系、文件和目录的大小、创建及最后访问时间、权限）













#### 额外的

1. HTTP使用TCP而不是UDP的原因在于（打开）一个网页必须传送很多数据，而TCP提供传输控制，按顺序组织数据和纠正错误
2. 序列化：凡是需要进行“跨平台存储”和“网络传输”的数据，都需要进行序列化，把一个对象状态保存成一种跨平台识别的字节格式，然后其他的平台才可以通过字节信息解析还原对象信息