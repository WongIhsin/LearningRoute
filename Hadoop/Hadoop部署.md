#### Hadoop配置项

+ JDK安装：Java
+ Apache安装：需要管理远端Hadoop守护进程，通过SSH Secure Shell来启动和停止各个DataNode上的各种守护进程的。所以必须安装SSH，并且sshd必须正在运行
+ 如果Hadoop需要安装在集群中，即把整个集群当作一个整体来运行Hadoop平台，还需要：
  + 网络配置：保证集群中机器间的网络通信
  + 时间同步：保证集群中机器间的时间一致
  + 配置机器IP与机器名映射关系

---

#### Hadoop安装模式

Hadoop支持的运行模式有3种，分别是

+ 本地/独立模式（Local/Standalone Mode）
  + 所有程序都在同一个JVM上执行，开发阶段
+ 伪分布模式（PseudoDistributed Mode）
  + 对应Java守护进程都运行在一个物理机器上，模拟一个小规模集群的运行模式
+ 全分布模式（FullyDistributed Mode）
  + Java守护进程运行在一个集群上

---

