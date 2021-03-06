2019.5.4

#### 写在前面（勉强理解）

一般使用的都是在一个局域网中的一台机器

其中路由器可以认为是192.168.199.1，登录管理界面也是这个地址

路由器同时是该局域网网络内的***DHCP服务器、网关、DNS服务器***

同时路由器提供了一个局域网192.168.199.2-254，子网掩码为255.255.255.0，其中192.168.199.1已经给路由器使用

该局域网中的其中一台机器的IP地址就为192.168.199.11/24

路由器提供一个局域网的维持管理，路由器本身有一个网卡，并连接一个上层分配的网络，即公网IP或者是校园网分配的IP

国内的运营商ISP提供的一般为PPPoE宽带上网，所以会看到子网掩码可能是255.255.255.255，节省IP地址

###### PPPoE拨号上网（个人理解）

以前的话需要点击**宽带连接**拨号，输入用户名和密码，才能上网，现在的话家庭生活中一般都是连接路由器就可以直接上网了

现在家里有电话线、光纤、或者入户网线

（看到电话线的猫叫ADSL Modem）

**猫**和**光猫**，其实是叫**调制解调器Modem**，电话线上网和光纤入户都差不多，都是提供一个调制解调器，需要将调制解调器连接到路由器的，入户网线也是连接到路由器

其实第一次安装网络的时候，需要网线或猫连接路由器，然后路由器连接一台电脑，路由器厂商会提供一个操作流程，此时可以通过电脑操作路由器，这时设置PPPoE（ADSL虚拟拨号），需要填写运营商提供的账号和密码，这时也可以设置无线网等，然后重启生效，路由器就会自动拨号上网

这个拨号是按需连接

需要补充一下PPPoE知识

#### IP基本知识

1. Internet中每台主机或路由器都有一个 **32bit** 全球唯一标识的IP地址，32为IP地址差不多有40多亿个可用IP地址

2. IP地址编码方式

   1. 传统分类的IP地址

      传统的IP地址分为**A**、**B**、**C**、**D**、**E**五类

      由***网络号***与***主机号***两部分组成：网络号为主机所连接到的网络；主机号为该主机或路由器在该网络中的地址

      其中的

      ***ABC三类***为0+, 10+, 110+开头的地址

      ***D类地址为1110+开头，多目的广播地址(28位)***    没懂...

      ***E类地址为11110+开头***，保留用于实验室和将来使用

      只有两级结构，某些情况下比较浪费资源

   2. 划分子网的三级结构

      三级结构为**网络号**-**子网号**-**主机号**

      提出了子网掩码的概念，各类地址的默认子网掩码

      A类：255.0.0.0

      B类：255.255.0.0

      C类：255.255.255.0

      比如一个子网长度为6的B类网，可以表示为139.29.246.0/22

   3. 无分类编址CIDR

      使用**网络前缀**来代替子网的概念，IP地址表示为网络前缀和主机号

      将三类地址加子网的划分方式变成一个一个的CIDR块，便于划分和管理，使用者影响不大

      使用 **/** 来表示网络前缀所占比特数

#### 特殊地址

主机号全为0表示网络本身

主机号全为1表示网络的广播地址

127.0.0.1网络保留作为环路自测地址，此地址表示任意主机本身

32bit全为0，表示整个TCP/IP网络

32bit全为1，表示整个TCP/IP网络的广播地址

#### NAT技术

NAT，网络地址转换技术，将专用网络地址（如：企业内部网络）转换为公用地址，对外隐藏了内部管理的IP地址，减少IP地址注册的费用，也节省了IPv4地址。

私有IP地址网段如下

***10.0.0.0~10.255.255.255***

***172.16.0.0~172.31.255.255***

***192.168.0.0~192.168.255.255***

即在Internet中的路由器对私有地址不进行转发

NAT路由器至少有一个有效的外部全球地址，**通过映射转换，将内网的IP和端口，映射到网关的某一个端口**，在路由器的映射表转化成为全局IP使用

网关即是路由器的公网地址



