> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/m0_61588837/article/details/132694485?spm=1001.2014.3001.5506)

### 第四章 网络层

![](https://i-blog.csdnimg.cn/blog_migrate/2f6cfad7844d18d6a2ba5f85a5396cda.png)

#### 协议总览

![](https://i-blog.csdnimg.cn/blog_migrate/d42ab557843de1b25c2aa89ea53f8c7a.png)

**可靠传输，面向连接 (虚电路)；不可靠传输，无连接 (数据报网络)**

![](https://i-blog.csdnimg.cn/blog_migrate/fd39601aced6b2b128268d7a74630ed6.png)

**通过 TCP/IP 协议栈的网际层来学习网络层的理论知识和技术**

![](https://i-blog.csdnimg.cn/blog_migrate/df513239ef6213dac8241a1d5d8c5701.png)

#### 概念

*   从发送方主机传输**报文段**到接收方主机
*   发送方主机封装**报文段** (segments) 为**数据报 (datagrams)**
*   接收方主机递交**报文段**给传输层
*   在**每个**主机、路由器上都需要**运行网络层协议**
*   路由器会检查通过它的所有 **IP 数据报头部字段**，然后根据目的的 IP 地址对数据报进行转发

##### 两个主要的网络层功能

###### 转发 **(forwarding)** (数据平面)(硬件实现)

*   **将分组从路由器的输入端口转移到正确的路由器输出端口**

###### **路由** (routing)(控制平面)(软件实现)

*   **确定分组从发送方传输到接收方 (目的主机) 所经过的路径(或路由)**
    *   路由算法
    *   ![](https://i-blog.csdnimg.cn/blog_migrate/a4eb86f1ec13551db9bb7e7a64dbccb8.png)
*   路由是端到端的路线。整体路线
*   转发是局部具体怎么走
*   两者的相互作用
*   你要先路由确定路径我才能根据路径进行转发
    *   ![](https://i-blog.csdnimg.cn/blog_migrate/bf5adbe5f76b90896cb338c4c28769f8.png)
    *   **路由算法**确定通过网络的**端到端路径**
    *   转发表确定**本路由器**上的**本地转发**

#### 数据平面和控制平面

*   **数据平面**
    
    *   本地的，每个**路由器自身的功能**
        
    *   **决定抵达路由器输入端口的数据包如何转发到输出端口，进行路由器内部的数据报文从输入端口到输出端口的转发**
        
*   控制平面
    
    *   **整个网络范围**
    *   决定数据报在**端到端路径上的路由器之间**如何路由
    *   两种控制平面的实现方式
        *   传统的路由算法![](https://i-blog.csdnimg.cn/blog_migrate/9075aeb0c0d71e3f7b309bb3fc89f2e7.png)
        *   软件定义网络 SDN![](https://i-blog.csdnimg.cn/blog_migrate/49a58836c5f4c09fcb4f3bf99358c25e.png)

#### 虚电路和数据报网络

虚电路

**这个虚电路的建立是逻辑上的连接，和电路交换物理上的连接是不同的!!!**

![](https://i-blog.csdnimg.cn/blog_migrate/67b672f528fb283e03f4f62a5365bf42.png)

数据报网络

**采用这种设计，把可靠数据传输的实现交给了上层的运输层来实现，而网络层是负责存储转发数据报就 ok 了**

**网络的造价大大降低，运行方式灵活，能够适应多种应用**

![](https://i-blog.csdnimg.cn/blog_migrate/975247d4c78b96364f5f34114cdac346.png)

比较

![](https://i-blog.csdnimg.cn/blog_migrate/225aa58017c0079e6877434c4796c52e.png)

*   **数据报网络提供网络层的无连接服务**
    
*   **虚电路网络提供网络层的连接服务**
    
*   任何网络中的网络层只**提供两种服务之一，不会同时提供**
    
*   传输层：**面向连接服务在网络边缘的端系统中实现**
    
*   网络层：**面向连接服务在端系统及网络核心的路由器中实现**
    
*   数据报网络特点![](https://i-blog.csdnimg.cn/blog_migrate/ef4701cca0ee9b9b394215ddea6dc7e7.png)
    

##### 数据报转发表

*   采用地址范围建立表项![](https://i-blog.csdnimg.cn/blog_migrate/b59f8369de952667bedeb60402fa15c7.png)
*   对于给定的目的地址，使用**最长地址前缀匹配**来完成输出端口的查找
    *   ![](https://i-blog.csdnimg.cn/blog_migrate/186f35a1ce683727564b308151e0b660.png)
    *   路由器**转发表只维持转发状态信息**
    *   **转发表由选路算法修改；虚电路网络转发表随虚电路的建立和拆除更新**
    *   一个端系统发送给另一个端系统的**一批分组可能在网络中选择不同的路径，到达的顺序可能不一致**
*   数据报网络的特点
    *   **网络层服务模型简单**
    *   **端系统功能复杂**。**高层实现许多功能，如按序传送、可靠数据传输、拥塞控制和 DNS 名字解析等**
    *   带来的结果
        *   因特网**服务模型提供的服务保证最少**（可能没有！），对网络层的需求最小，使得互连使用各种不同**链路层技术的网络变得更加容易**
        *   许多应用都是在位于网络边缘的主机 (服务器) 上实现

#### 路由器的工作原理

![](https://i-blog.csdnimg.cn/blog_migrate/ef9c06f5037b2e20a48203eecd957ee8.png)

##### 结构及功能

*   两个核心功能
    *   **运行路由算法 / 协议 (OSPF,RIP,BGP)**
    *   将分组从路由器的**输入端传送到正确的输出链路**
*   路由器的体系结构
    *   ![](https://i-blog.csdnimg.cn/blog_migrate/f15442848202db0abb2eb77f1c608d07.png)

##### 输入端功能

*   **线路端接**模块
    *   将一条**物理链路端**接到**路由器的物理层**
*   **数据链路处理**模块
    *   实现路由器的**数据链路层功能**
*   **查找与转发**模块
    *   实现**查找与转发功能**，以便**分组通过路由器交换结构**转发到**适当的输出端口**
    *   确定将一个到达的分组**通过交换结构转发给哪个输出端口**。通过**查找转发表**实现，这里的转发表是存储在**输入端口的内存中**
    *   分布式交换
        *   选路处理器计算转发表，给每个输入端口存放一份转发表拷贝
        *   在每个输入端口本地作出交换决策，无需激活中央选路处理器
        *   可避免在路由器中某个单点产生转发处理瓶颈
        *   目的：**以线速完成输入端口的处理**
        *   排队：如果数据报到达输入端口的速度快于输入端口将数据报转发到交换结构的速度，就会发生排队
*   ![](https://i-blog.csdnimg.cn/blog_migrate/8f545f3c2964821ff828c82ef8bd3f79.png)

##### 三种交换结构![](https://i-blog.csdnimg.cn/blog_migrate/c58a96df83225d202515731adf70a33f.png)

*   经内存的交换结构
    
    *   输入端口与输出端口之间的交换**由 CPU(选路处理器) 控制完成**
        
    *   输入端口与输出端口类似 I/O 设备：
        
        *   当分组到达输入端口时，通过**中断**向选路处理器发出信号，将**分组拷贝到处理器内存**中；
            
        *   选路处理器根据分组中的目的地址查表找出适当的输出端口，将**该分组拷贝到输出端口的缓存**中
            
    *   **交换速度受总线带宽的速度限制 (每个分组穿过两次总线)**
        
        *   若总线带宽为每秒写入或读出 **B 个**分组，则总的转发吞吐量 (分组从输入端口被传送到输出端口的总速率) **小于 B/2**
        *   ![](https://i-blog.csdnimg.cn/blog_migrate/cb1d20055346093f254f08f10abcc760.png)
*   经总线的交换结构
    
    *   输入端口**通过一条共享总线将分组直接传送到输出端口**，不需要 CPU 选路处理器的干预
        *   每次**只能有一个分组**通过总线传送
        *   分组到达一个输入端口时，若总线正**忙**，会被暂时阻塞，在输入端口**排队**
        *   路由器交换带宽**受总线速率控制**
*   经交换矩阵的交换结构
    
    *   **纵横式交换机**：_由 2_n 条总线组成，_n_ 个输入端口与 _n_ 个输出端口连接
        
    *   到达输入端口的分组**沿水平总线**穿行，直至与所希望的输出端口的**垂直总线交叉点**
        
        *   若该条**垂直总线空闲**，则分组被**传送到输出端口**
        *   否则，该到达的分组被**阻塞**，必须在输入端口**排队**![](https://i-blog.csdnimg.cn/blog_migrate/b15ae9ed17b2f78a641ecb0d07dcc6fe.png)
    *   输出端口：
        
        *   取出存放在输出端口内存中的分组，并将其传输到输出链路上
        *   当交换结构将分组交付给**输出端口的速率超过输出链路速率**，就需要排队与缓存管理功能。当输出端口的缓冲区溢出时，就会出现延时和丢包![](https://i-blog.csdnimg.cn/blog_migrate/1775dd483d2c199213ee68958e993f88.png)
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/be8a9f909348303de27030d0793319ec.png)
        
    *   输入端口排队![](https://i-blog.csdnimg.cn/blog_migrate/e019b1db8d32f787caa8968af9c20b06.png)
        

##### 网络层转发分组的流程

相邻直接传，否则查表传，特定 > 到达 > 默认，否则出错

1、从数据报的首部提取目的主机的 IP 地址 D，得出目的网络地址 N

2、若网络 N 与此路由器直接相连，则把数据报直接交付给目的主机 D，这称为路由器的直接交付；否则是间接交付，执行步骤 3

3、若路由表中有目的地址为 D 的特定主机路由（对特定的目的主机指明一个特定的路由，通常是为了控制或测试网络，或出于安全考虑才采用的），

则把数据报传送给路由表中所指明的下一跳路由器；否则，执行步骤 4。

4、若路由表中有到达网络 N 的路由，则把数据报传送给路由表指明的下一跳路由器∶ 否则，执行步骤 5

5、路由表中有一个默认路由，则把数据报传送给路由表中所指明的默认路由器; 否则， 执行步骤 6

6、报告转发分组出错

#### IP 数据报

##### 发送过程

**IP 数据报发送可以直接发到本网络，也可以通过路由器转发到其他网络，如果发到本网络称作直接交付，发到其他网络称作间接交付；**

**IP 数据报中包含源 IP 地址和目的 IP 地址，IP 数据报到达路由器的位置，先检查首部看是否出错，如果出错发送 ICMP 差错报告报文，路由器查路由表，把目的地址和子网掩码相与，如果得到的目的网络不匹配，则查下一项；查到匹配的则根据吓一跳进行转发，如果查不到则丢弃并且给发送方返回信息**

**注意发送过程中源 IP 和目的 IP 地址是不变的，修改的是源 MAC 地址和目的 MAC 地址!!!**

![](https://i-blog.csdnimg.cn/blog_migrate/1e140f2dc03a7be3a3c2e29c44178d24.png)

例题

**中继器和集线器工作在物理层，既不隔离冲突域也不隔离广播域**

**网桥和交换机 (多端口网桥) 工作在数据链路层，可以隔离冲突域，不能隔离广播域**

**路由器工作在网络层，既隔离冲突域，也隔离广播域**

![](https://i-blog.csdnimg.cn/blog_migrate/cb7f232b92c94ecc78cb618897ba5984.png)

例题

注意要配置好默认网关

![](https://i-blog.csdnimg.cn/blog_migrate/00f2623d194b4b17f51b13710305c95f.png)

##### 首部

![](https://i-blog.csdnimg.cn/blog_migrate/98f5de1599d7459610ba5a17830e32f5.png)

###### 版本

![](https://i-blog.csdnimg.cn/blog_migrate/f20201739785c4d7b7c4b0c5425164e2.png)

###### 首部长度

![](https://i-blog.csdnimg.cn/blog_migrate/a460a65745dd39b60122ccd94557fad6.png)

###### 区分服务

只有使用区分服务该字段才有作用，一般情况不使用

![](https://i-blog.csdnimg.cn/blog_migrate/9ddbd53ec0328d10068185a04e353ada.png)

###### 总长度

表示总长度 (首部 + 数据载荷)

**IP 数据报是 16 位，理论上的最大长度是 2 的 16 次方 - 1，65535**

**首部长度以四字节为单位，总长度以字节为单位!!!**

![](https://i-blog.csdnimg.cn/blog_migrate/48e4b5bb9c4609629dca52e0d5fd8fd6.png)

###### 标识，标志，片偏移 (用于 IP 数据报分片)

![](https://i-blog.csdnimg.cn/blog_migrate/02e8e8f7302b24bbec7c6b7d7b22ff22.png)

**标识：标识各个分片数据报同一个数据报分片下来的**

**标志：DF 1 表示不允许分片，0 表示允许**

​ **MF 1 表示后面还有分片 0 表示最后一个分片**

​ **保留位 必须置 0**

**片偏移：指出分片数据包的数据载荷部分在原来的数据报上偏移了多少的位置，以 8 字节为单位 (写的时候注意要除以 8)，并且片偏移量必须为整数!!!**

![](https://i-blog.csdnimg.cn/blog_migrate/c6ba6fe722825be343acbf8988713529.png)

###### IP 分片举例

![](https://i-blog.csdnimg.cn/blog_migrate/987dcea30f3950225f86b8fca6ee3c7c.png)

![](https://i-blog.csdnimg.cn/blog_migrate/588617df3a30c845a276b3f565976bb8.png)

###### 生存时间 TTL(与路由环路相关)

![](https://i-blog.csdnimg.cn/blog_migrate/74f13287c07299b9162ffc57a67412fe.png)

举例：防止路由环路

![](https://i-blog.csdnimg.cn/blog_migrate/6cac59e42e66a17be915ebda596bef04.png)

###### 协议 (熟悉)

![](https://i-blog.csdnimg.cn/blog_migrate/3fef48d46052b73bfa35889b2cfd30e1.png)

###### 首部检验和 (如何计算)

**IP 只计算首部，tcp,udp 需要包含首部和数据**

![](https://i-blog.csdnimg.cn/blog_migrate/eb0406166b091ba6f31b3e3fe59dde9c.png)

![](https://i-blog.csdnimg.cn/blog_migrate/41ab873d6d5d7bf740a143612d698da8.png)

###### IP 地址

![](https://i-blog.csdnimg.cn/blog_migrate/1d95e5ea654646a643d92f76c78156f5.png)

##### 例题

注意：

**1. 首部 + 数据载荷 对应 MTU，记得要加上首部!!! 也就是数据链路层的数据部分最大值!!!**

**2. 片偏移量是以 8 字节为单位，并且一定是整数，所以数据载荷部分记得取 8 的整数倍数!!!**

![](https://i-blog.csdnimg.cn/blog_migrate/63d899b917fae4673727f6ac242ada0a.png)

###### 学校 PPT 所讲

![](https://i-blog.csdnimg.cn/blog_migrate/d69761a6458fe1cbbebce436f9f311d7.png)

*   各功能
    *   版本号：4 位。表示该 IP 数据报使用的 IP 协议版本。目前 Internet 中使用的主要是 TCP/IP 协议族中版本号为 4 的 IP 协议
    *   首部长度：4 位。此域指出**整个报头的长度**（包括选项）
        *   该长度是以 **32 位二进制数为一个计数单位**的
        *   **接收端**通过此域可以**计算出报头在何处结束及从何处开始读数据**
        *   普通 IP 数据报（没有任何选项）该字段的值是 **5（即 20 个字节的长度）**
    *   服务类型（TOS、type of service）：8 位。服务类型字段的 8 位分成了 5 个子域![](https://i-blog.csdnimg.cn/blog_migrate/b6776a8445f4a6e278d4f9fbbb7c3621.png)
    *   长度：16 位。总长度字段是指**整个 IP 数据报的长度（报头区 + 数据区）**，以字节位单位
        *   利用头部长度字段和总长度字段就可以计算出 IP 数据报中**数据内容的起始位置和长度**
    *   生存时间 (寿命 TTL，time to live)：占用 8 位二进制位
        *   指定了数据报可以在网络中传输的最长时间
        *   TTL 的初始值由源主机设置（通常为 32、64、128 或 256），一旦经过一个处理它的路由器，它的值就减 1。当该字段为 0 时，数据报就丢弃，并发送 **ICMP** 报文通知源主机，因此可以防止进入一个循环回路时，数据报无休止地传输下去
    *   高层：占用 8 位二进制位
        *   IP 协议可以承载各种上层协议，目标端根据协议标识就可以把收到的 IP 数据报送到 **TCP 或 UDP** 等处理此报文的上层协议了
    *   首部检查和
        *   占用 16 位二进制数
            *   用于协议**头数据有效性**的校验
            *   保证 IP **报头区在传输时的正确性和完整性**。头部检验和字段是根据 IP 协议头计算出的检验和，它**不对头部后面的数据进行计算**
            *   原理：发送端首先将**检验和字段置 0**，然后对头部中每 16 位二进制数进行**反码求和**的运算，并将结果存在校验和字段中。 由于接收方在计算过程中包含了发送方放在头部的校验和，因此，如果头部在传输过程中没有发生任何差错，那么**接收方计算的结果应该是全 1**。
    *   源 IP 地址：占用 32 位二进制数，表示发送端 IP 地址
    *   目的地址：占用 32 位二进制数，表述目的端 IP 地址

###### 分片和重组

![](https://i-blog.csdnimg.cn/blog_migrate/81b420dbb8c0dc44aab648114fbadf9d.png)

*   分片：
    *   把**一个数据报**为了适合网络传输而分**成多个数据报**的过程称为分片，被分片后的各个 IP 数据报可能**经过不同的路径**到达目标主机
    *   一个 IP 数据报在传输过程中可能被分片，也可能不被分片。如果被分片，分片后的 IP 数据报和原来没有分片的 IP 数据报结构是相同的，即**也是由 IP 头部和 IP 数据区两个部分组成**![](https://i-blog.csdnimg.cn/blog_migrate/1353c51b87310eb5c15352edbaea0303.png)
    *   分片后的 IP 数据报，**数据区是原 IP 数据报数据区的一个连续部分**，头部是原 IP 数据报**头部的复制**，但与原来未分片的 IP 数据报头部有两点主要不同：** 标志和片偏移 **
        *   标志：在 IP 数据报头部有一个叫标志的字段，用 3 位二进制数表示![](https://i-blog.csdnimg.cn/blog_migrate/e5e80249e763b21157c1f37bba9f6230.png)
    *   —片偏移：IP 数据报被分片后，各片数据区在原来 IP 数据区中的位置用 **13 位片偏移来表示**。上图中分片 1 的偏移为 0；分片 2 的偏移为 600；分片 3 的偏移为 1200 实际在 IP 地址中, 由于偏移是以 **8 个字节为单位**进行计算的, 因而在 IP 数据报中分片 1 的偏移是 0；分片 2 的偏移是 75；分片 3 的偏移是 150
*   重组
    *   当分了片的 IP 数据报到达最终目标主机时，目标主机对各分片进行组装，恢复成源主机发送时的 IP 数据报，这个过程叫做 IP 数据报的重组。
    *   IP 数据报头部中，**标识**用 16 位二进制数表示，它**唯一地标识主机发送的每一份数据报**
        *   在一个数据报被分片时，每个**分片仅把数据报 “标识” 字段的值原样复制一份**，所以一个数据报的所有分片具有相同的标识
    *   目标端主机重组数据报的原理:
        *   根据 “**标识**” 字段可以确定收到的分片属于原来哪个 IP 数据报
        *   根据 “**标志**” 字段的 “**片未完 MF**” 子字段可以确定分片是不是最后一个分片
        *   根据 “**偏移量**” 字段可以确定分片在原数据报中的位置

![](https://i-blog.csdnimg.cn/blog_migrate/7f37198717b3073812d41742f28ffc98.png)

##### ip 地址

###### 概述

**IPV4 地址就是给因特网上的每一台主机或者路由器的每一个接口分配一个全世界范围内唯一的 32 位 bit 的标识符**

![](https://i-blog.csdnimg.cn/blog_migrate/d25907154f7383081257c99b9ae56e34.png)

###### 分类编制的 IPV4

![](https://i-blog.csdnimg.cn/blog_migrate/77ddc2be8e641ab4e6056b517928fdf9.png)

###### D 类多播地址

![](https://i-blog.csdnimg.cn/blog_migrate/07cc3eccf4ff02ce488625471179a965.png)

![](https://i-blog.csdnimg.cn/blog_migrate/0464e0669073ad9f28f3834d1452a611.png)

###### A 类

![](https://i-blog.csdnimg.cn/blog_migrate/bea8c10d950b91c1619d529d8dfd143a.png)

###### B 类

![](https://i-blog.csdnimg.cn/blog_migrate/403c0ee216350953285158658c86f0ff.png)

###### C 类

![](https://i-blog.csdnimg.cn/blog_migrate/b586069d82ea4672a207cc52a4340383.png)

###### 特殊 IP 地址

![](https://i-blog.csdnimg.cn/blog_migrate/00c7d3d8be8b370598d33593e91466fa.png)

![](https://i-blog.csdnimg.cn/blog_migrate/d26bb15bf22353a0d24264e23cd13c79.png)

*   IPV4 编址
    
    *   ![](https://i-blog.csdnimg.cn/blog_migrate/e3b94af1cdd7938c058c45f9f579ece8.png)
        
    *   ![](https://i-blog.csdnimg.cn/blog_migrate/04d66ec9f192588252bd58bdb4608c96.png)
        
        *   分为主机号和网络号
    *   传统的 IP 地址分类![](https://i-blog.csdnimg.cn/blog_migrate/7c38a26c5058976eb81652b67e57ac81.png)
        
        *   A 类地址：第一个字节作为网络地址，**最高位为 0**，其余的三个字节作为主机地址。![](https://i-blog.csdnimg.cn/blog_migrate/da4897a904d0ac4fda48b79f831cbab7.png)
            
        *   B 类地址：l 两个字节作为网络地址，**最高位为 10**，其余的两个字节作为主机地址![](https://i-blog.csdnimg.cn/blog_migrate/c64cfed278dbcecd704b3a261daec417.png)
            
        *   C 类地址：利用 IP 地址的前三个字节作为网络地址，最高位为 110，最后一个字节作为主机地址![](https://i-blog.csdnimg.cn/blog_migrate/98e0c13fdc488c99f59c4eab95159afe.png)
            
        *   特殊 IP 地址段![](https://i-blog.csdnimg.cn/blog_migrate/cad7d76370d5a325d01050a1fa840ae5.png)
            
            ![](https://i-blog.csdnimg.cn/blog_migrate/e77de787d519e6f84f845698c466ffe9.png)
            
        *   同一局域网上的主机或路由器的 IP 地址中的网络号必须相同
            
        *   **交换机互连的网络仍然是一个局域网**，只能有一个网络号
            
        *   路由器总是具有两个或两个以上 IP 地址
            
        *   当两个路由器直接相连时，在连线两端的接口处，可以指明 IP 地址也可以不指明 IP 地址
            

###### 划分子网的 IPV4

**为什么要划分子网呢？**

**如果不划分子网，那么对于新的主机网络，就需要构建新的网络，申请新的网络号，如下弊端：**

![](https://i-blog.csdnimg.cn/blog_migrate/fee83c9f892143a171686231e8ffd986.png)

**用子网掩码可以表示借用了多少位主机号来充当子网号**

![](https://i-blog.csdnimg.cn/blog_migrate/2593125c17499c695ecfe796cd6d7d3f.png)

###### 例题

![](https://i-blog.csdnimg.cn/blog_migrate/051340669cd0b31d8b7135420e30d64b.png)

![](https://i-blog.csdnimg.cn/blog_migrate/ad9055267f30ffd8c590380ed734dc8c.png)

###### 子网号能否为全 0 或者全 1

**在 ABC 类 划分子网时，子网号不能为全 0 或全 1  
在 CIDR 划分子网时，子网号可以为全 0 或全 1**

![](https://i-blog.csdnimg.cn/blog_migrate/e797ede68371c88411dd4d1c94fec27e.png)

![](https://i-blog.csdnimg.cn/blog_migrate/cf96f6b3b1ee813efd719f78602809da.png)

###### 默认子网掩码

主机号不管划不划分，讲子网号和主机号全取 0

![](https://i-blog.csdnimg.cn/blog_migrate/2a8f4a534d8370f313160bff00aace79.png)

###### 无分类编制的 IPV4(CIDR)

###### 表示方法

![](https://i-blog.csdnimg.cn/blog_migrate/6cd0383ca9c4b150fe8d336cadba1840.png)

举例

**聚合 C 类网的数量是指这个地址块相当于多少个 C 类网络的数量**

**C 类网络主机号 8 位，因此 12-8，留出 4 位当子网号，就是 2 的 4 次方**

![](https://i-blog.csdnimg.cn/blog_migrate/d036272f0e27feb2ac6db85c25a330ac.png)

###### 路由聚合 (构造超网)

**减少路由表的占用，对 R1 的 5 条路由信息合成一条！**

**寻找共同的前缀，作为新的 IP 地址的网络前缀，然后主机号取 0 就得到了这个目的网络号**

**值得主要注意的是，如果出现了多个匹配项，选择网络前缀最长的那个，称为最长前缀匹配。这样的路由更加具体**

![](https://i-blog.csdnimg.cn/blog_migrate/1783a9742ae7822746c8e7da6ddc8878.png)

###### 例题

![](https://i-blog.csdnimg.cn/blog_migrate/109800997d608fc4982b6f61160967ef.png)

![](https://i-blog.csdnimg.cn/blog_migrate/a2cefab6e44c9180f97109eca037a8e1.png)

###### IP 地址的应用规划

###### 定长的子网掩码

![](https://i-blog.csdnimg.cn/blog_migrate/7a617efb09ddf9f56d84e01c04f60bb7.png)

**注意看题目给出的是主机还是总的 IP 地址需求个数，记得根据情况是否加上网络地址，广播地址和路由器接口地址!!!**

**注意，这些合计的 IP 地址数都是主机号决定的，而对于分类编址的 IPV4 的子网号不能取同 0 和同 1，这个和这些没关系**

**注意，分类编址子网号不能全 0 和全 1，CIDR 是可以全 0 和全 1 的**

![](https://i-blog.csdnimg.cn/blog_migrate/4fcb9a4a3f4b686884a835519eb039f8.png)

###### 变长的子网掩码 (CIDR 重点)

![](https://i-blog.csdnimg.cn/blog_migrate/c5bf1aada00d9f00b37ab5a96b2ee75e.png)

**注意分配原则：每个子块的起点位置不能随意选取，只能选取块大小整数倍的地址作为起点，建议从大的子块开始分配!!!**

**注意这里的子网所需地址数量普通情况下是大于主机数量 + 网络地址 + 广播地址 + 路由器接口地址，但是我们划分子网主机号位数得出的子网地址数是 2 的 n 次方倍，这样虽然划分的子网中有地址浪费，但是大大减小了；并且也能选取块大小整数倍的地址为起点**

![](https://i-blog.csdnimg.cn/blog_migrate/91818c9365e15056a2a6b915becdf28a.png)

###### 子网划分老师所讲

###### 划分子网

![](https://i-blog.csdnimg.cn/blog_migrate/31f79274adb310e6cfe4e9355d6d3325.png)

*   划分子网的方法是**从主机号借用若干个比特作为子网号**，剩下的主机位为主机号
*   子网特点：
    *   设备接口的 **IP 地址具有同样的网络部分**
    *   没有路由器的介入，**物理上能够相互到达**
*   子网掩码
    *   l 子网号字段长度是可变的，因此，为了确定子网地址，IP 协议提供了子网掩码的概念 。
    *   l 子网掩码用来**确定网络地址（包括网络号和子网号）**和**主机地址的长度**。子网掩码长为 **32 位比特**，其中的 **1 对应于 IP 地址中的网络号和子网号**，而子网掩码中的 **0 对应于主机号**。![](https://i-blog.csdnimg.cn/blog_migrate/781052946c08def957ceee767d11f1d7.png)

###### 子网划分技术

*   主机号**全 0 的地址不能用**，它被用做表示该子网的**子网号**；主机号**全 1 的也不能用**，它用于**本子网的广播**。因此每个子网所能容纳的主机数是 **2^N-2**，N 是主机号位数![](https://i-blog.csdnimg.cn/blog_migrate/2b029ddea3da9f337db217bb063c6aea.png)

###### 无分类域间路由 CIDR

*   Classless Inter-Domain Routing，CIDR
    
*   lCIDR 消除了传统的 A 类、B 类和 C 类地址的概念。
    
*   使用**斜线记法，**又称为 CIDR 记法来**区分网络前缀和主机号**，即在 IP 地址后面加上一个斜线 “/”，**斜线后面用一个数字指定网络前缀的长度**。
    
*   构造超网![](https://i-blog.csdnimg.cn/blog_migrate/9bc07fcfc3b0efb73e8f3c55392d330e.png)
    
*   ![](https://i-blog.csdnimg.cn/blog_migrate/15c856788d95c2c30ceeb2846d74b49a.png)
    
*   层次寻址，路由聚合![](https://i-blog.csdnimg.cn/blog_migrate/fae69868a78333848ae134ff1b58e1d9.png)
    
*   练习：![](https://i-blog.csdnimg.cn/blog_migrate/fdf7c810b5d2af0732dc8c9aa892a379.png)
    
    > （1）
    > 
    > 以最多台数的部门（60 台）为准，需要的最接近数为 2^6=64，故要从最后个字节借 8-6=2 位，
    > 
    > 子网分别为 202.1.1.0, 202.1.1.64, 202.1.1.128, 202.1.1.192，在这 4 个其中任选 3 个即可。掩码均为 255.255.255.192。
    > 
    > （2）
    > 
    > 若以最多台数的部门（120 台）为准，仅能分两个子网，无法满足。故应采用 CIDR 法：
    > 
    > 首先以最小需求台数部门为准（60 台），此时主机号位数需要 6 位（因为 60=<2^6-2），则子网号位数为 8-6=2 位，然后将子网划分出来。此时和（1）一样；
    > 
    > 接下来，部门 2、3 可以直接在 4 个子网中任选两个，部门 1 选剩下 2 个以满足 120 台的要求（但这两个子网要连续，以便用 CIDR 法合并之，做超网）。比如 202.1.1.128、202.1.1.192 分别给部门 2、3，部门 1 用 202.1.1.0、202.1.1.64。
    > 
    > （3）最后将各部门 IP 段用 CIDR 超网形式描述，以便对外发布：
    > 
    > 部门 1：202.1.1.0/25; (注意含义：表示前 25 位是网络号，且最后一个字节最高位为 0，后面 7 位是主机号)
    > 
    > 部门 2：202.1.1.128/26; (最后一个字节最高两位为 10，后面 6 位是主机号)
    > 
    > 部门 3：202.1.1.192/26; (最后一个字节最高两位为 11，后面 6 位是主机号)
    > 
    > 说明：将 202.1.1.128、202.1.1.192 给部门 1，202.1.1.0、202.1.1.64 分别给部门 2、3 亦可。此时答案为：
    > 
    > 部门 1：202.1.1.0/26;
    > 
    > 部门 2：202.1.1.64/26;
    > 
    > 部门 3：202.1.1.128/25
    

#### 静态路由配置

**静态路由是指网络管理员使用命令给路由器配置路由表**

**简单，开销小，但是不能适应网络拓扑的变化，一般只在小规模网络中使用**

![](https://i-blog.csdnimg.cn/blog_migrate/19abed32ef9a96bcc9d0fac0f07297b1.png)

静态路由配置：人为配置好

![](https://i-blog.csdnimg.cn/blog_migrate/c87dc430ee5e76311cb15cc1d24bda9d.png)

**默认路由：0.0.0.0(或者 0.0.0.0/0)，在其他的路由查不到的时候，就是默认走这里；网络前缀最短，路由最模糊**

**特定主机路由：给网络中的某个主机特定配置的一个路由，网络前缀最长，路由最具体**

**当有多个路由可选的时候，找最长前缀匹配的!!!**

![](https://i-blog.csdnimg.cn/blog_migrate/57dcfa1712088f4d1b20a2dbcd66d40a.png)

##### 路由环路

**静态路由配置错误导致路由环路**

配置错误后，R2 吓一跳给 R3，R3 查表又给 R2，死循环

**IP 首部设有生存时间 TTL 的字段，进入路由器后 - 1，若不为 0 则转发，否则丢弃，这样就很好的缓解了路由环路问题**

![](https://i-blog.csdnimg.cn/blog_migrate/f1b08c065414d48ac8adb210f24dbef0.png)

**聚合路由中有不存在的网络导致路由环路**

**为了节省空间，R2 的路由表将 R1 0 和 2 接口连接的网络聚合成了一个聚合路由存在了 R2 的路由表中，R2 发出目的网络地址 192.168.2.0/24，查 R2 的路由表发现 192.168.0.0/22 前缀匹配，则转发到 R1，R1 则正确接受并且转发给目的网络；**

**好，但是实际上 24 位的网络前缀在 22 位的基础上可以衍生出 4 个网络，其中两个网络是图中的网络拓扑没有的，所以就容易出现下列问题**

**发送 192.168.3.0/24，按照前缀匹配会发给 R1，然后 R1 找不到，就会找默认路由，然后又回到 R2 了, 形成环路**

**解决方法：针对这两个不存在的路由存储黑洞路由，下一跳设置为 NULL**

![](https://i-blog.csdnimg.cn/blog_migrate/11572c30a67ed2100fa81b786395658b.png)

**网络故障导致路由环路**

最开始正常，然后出故障，R2 不知道，仍发送到 R1，然后这个时候出故障之后从路由表中删除这个信息，然后就找到了默认路由，导致环路

解决：在消除之后设置黑洞路由，网络恢复之后将路由表恢复或者禁用黑洞路由

![](https://i-blog.csdnimg.cn/blog_migrate/b89e3d035028792101d5c08f899c21ac.png)

#### 动态主机配置协议 DHCP(应用层协议)

##### DHCP 发现 (客户发)

**源 IP：0.0.0.0，目的：255.255.255.255，因为不知道服务器地址所以广播**

**帧当中带有事务 ID 和客户的 MAC 地址**

![](https://i-blog.csdnimg.cn/blog_migrate/9884a4f0651724fd76128c8029972d09.png)

##### DHCP 提供 (服务器发)

**服务器收到后发送 DHCP 提供报文，源 IP：自己服务器 IP，目的 IP：广播**

**帧当中带有事务 ID，用来标识这个是刚才客户发送的 DHCP 报文的回复**

**还带有 IP 的配置信息，包括 IP 地址，子网源码，地址租期，默认网关，DNS 服务器等等**

**注意使用该 IP 的时候需要用 ARP 协议保证该 IP 地址未被其他主机使用**

![](https://i-blog.csdnimg.cn/blog_migrate/706c93606d50886fcaf66f9b1a4c5321.png)

##### DHCP 请求 (客户发)

**客户收到 DHCP 提供报文后，选择一个服务器的报文，想用它为自己提供的 IP，发送 DHCP 请求报文，目的地址仍为 0.0.0.0，因为还未正式使用这个 IP 地址，目的地址为广播，这样就不用单独给每一个 DHCP 服务器发送我是否同意用你的 IP 地址了**

**帧当中包含事务 ID，DHCP 客户的 MAC，DHCP 给分配的 IP 地址和 DHCP 服务器的 IP 地址**

![](https://i-blog.csdnimg.cn/blog_migrate/dada592e3f49a774c47bc617a527389e.png)

##### DHCP 确认 (服务器发)

**这时候服务器 1 收到请求，如果同意则返回确认报文**

**注意客户收到后，IP 地址仍需要用 ARP 协议检测是否被占用，如果在这个过程中有人捷足先登占用了，那么发送 DHCP 取消报文取消租约，并重新来过**

![](https://i-blog.csdnimg.cn/blog_migrate/fd4254b79b4a2c8e73ba6cca716556fe.png)

##### 关于租用期和补充

![](https://i-blog.csdnimg.cn/blog_migrate/dee2d51dad60a27dbf60a0fc43ce6cff.png)

##### 关于 DHCP 中继代理

**我们不愿意给每个网络都设置一个 DHCP 服务器，这样会导致 DHCP 服务器太多了**

**所以例如 DHCP 的发现报文，由于目的地址为全 1，为广播地址，这样的地址一般情况下是不会被路由器转发的，但是就是要通过路由器转发才能被 DHCP 服务器接受**

**那么只需要在路由器中配置 DHCP 服务器的 IP 地址就好了，这个路由器也就成为了 DHCP 中继代理**

**这个时候广播到达路由器的 DHCP 发现报文就会被路由器存储并且单播转发给 DHCP 服务器了**

![](https://i-blog.csdnimg.cn/blog_migrate/d138faae804234f575cd1f0a27316dd1.png)

##### 老师 PPT 所讲

用于给主机动态的分配 IP 地址，它提供了**即插即用的联网机制**，这种机制允许一台计算机加入新的网络和获取 IP 地址，而不用手工参与

**DHCP 是应用层协议，是基于 UDP 的**

*   DHCP: Dynamic Host Configuration Protocol
    
    *   自动从一个 DHCP 服务器得到 IP 地址
*   工作过程![](https://i-blog.csdnimg.cn/blog_migrate/fa87e9d2f50dfb0d7228b1cf61d70be3.png)
    
    > *   1：DHCP **服务器被动打开 UDP 端口 67**，等待客户端发来的报文
    >     
    > *   2：DHCP **客户从 UDP 端口 68** 发送 DHCP **发现报文**，以便从 DHCP 服务器获得一个 IP 地址
    >     
    > *   3：凡收到 DHCP 发现报文的 DHCP 服务器
    >     
    >     ​ **都发出 DHCP 提供报文**，因此 DHCP 客户
    >     
    >     ​ 可能**收到多个 DHCP 提供报文**。
    >     
    > *   4：DHCP 客户从几个 DHCP 服务器中**选择**
    >     
    >     ​ **其中的一个，并向所选择的 DHCP 服务**
    >     
    >     ​ **器发送 DHCP 请求报文**。
    >     
    > *   5：被选择的 DHCP 服务器发送**确认报文**
    >     
    >     ​ DHCPACK，客户进入已绑定状态，并可
    >     
    >     ​ 开始使用得到的**临时 IP 地址**了。
    >     
    > 
    > DHCP 客户现在要根据服务器提供的**租用期 T 设置两个计时器 T1 和 T2**，它们的超时时间分别是 **0.5T 和 0.875T**。当超时时间到就要请求更新租用期。
    > 
    > *   6：租用期过了一半（T1 时间到），DHCP 客户 发送
    >     
    >     ​ **请求报文 DHCPREQUEST** 要求**更新租用期**。
    >     
    > *   7： DHCP 服务器若不同意，则发回否认报文 DHCPNACK。这时 DHCP 客户必须**立即停止使用原来的 IP 地址，而必须重新申请 IP 地址**（回到步骤 2）
    >     
    > *   8： DHCP 服务器若同意，则发回确认报文 DHCPACK。DHCP 客户得到了**新的租用期，重新设置计时器。**
    >     
    > *   若 DHCP 服务器不响应步骤 6 的请求报文 DHCPREQUEST，则在租用期**过了 87.5%** 时，DHCP 客户必须**重新发送请求报文 DHCPREQUEST**（重复步骤 6），然后又继续后面的步骤。
    >     
    
*   服务场景![](https://i-blog.csdnimg.cn/blog_migrate/70f2238411ed2acd61dbdf8ad02d68b5.png)
    
*   DHCP 分配的不仅仅是 IP 地址，还可以分配
    
    *   客户的第一跳路由器的地址 (**网关**)
    *   DNS 服务器的 IP 地址或域名
    *   子网掩码
*   **DHCP 是应用层协议，基于 UDP**
    

#### NAT 网络地址转换

##### 私有 IP 地址

![](https://i-blog.csdnimg.cn/blog_migrate/d1c90de60c52069266f2edde4322a5d2.png)

本地网络只能用于局域网内部通信而不能被路由器进行转发，进入到公网当中

![](https://i-blog.csdnimg.cn/blog_migrate/966f54acc8276194c2b497676409d470.png)

##### NAT 过程

![](https://i-blog.csdnimg.cn/blog_migrate/4ed4697513b5a32ca1d70a68e1534384.png)

![](https://i-blog.csdnimg.cn/blog_migrate/653da794b59f5190345c7cfb5f6a0e3f.png)

**存在的问题：一个内网地址对应一个路由器上接口的外网地址，那么如果外网地址的数量不够的话会出问题**

![](https://i-blog.csdnimg.cn/blog_migrate/a313a62461e9b6e85de292f1666212b8.png)

##### NAPT

**把多个公网 IP 的问题用运输层的端口号来解决了，公网 IP 地址是相同的，但是端口号不同，所代表的进程服务就不同**

![](https://i-blog.csdnimg.cn/blog_migrate/b079f94d19e673ee51df4e05230161b9.png)

##### 补充

**使用 NAT 不能让外网主机优先向内网主机发送请求，因为这会导致在 NAT 表里面找不到记录而失败**

![](https://i-blog.csdnimg.cn/blog_migrate/509cb240f41bcb7b501a57df1311c390.png)

**NAT 对外网屏蔽了内网的 IP 地址**

![](https://i-blog.csdnimg.cn/blog_migrate/bcdd616f655db4db8f83b9c1efc2771c.png)

*   使本地网络分组离开时具有同样的 IP 地址![](https://i-blog.csdnimg.cn/blog_migrate/68f611794f06cc3be87936759564d5c1.png)
    
*   执行 NAT 时路由器要求：
    
    *   外出的分组: **替换每个外出的分组的 (源 IP 地址, 端口号) 为 (NAT IP 地址, 新端口号)**
    *   远程客户 / 服务器用 **(NAT IP 地址, 新端口号)**** 作为目的 ** 地来响应。
    *   (在 NAT 转换表中)记录每个 (源 IP 地址, 端口号) 到 (NAT IP 地址, 新端口号) 转换配对
    *   进来的分组: **对每个进来的分组，用保存在 NAT 表中的对应的 (源 IP 地址, 端口号) 替换分组中的目的域 (NAT IP 地址, 新端口号）**
*   地址转换过程![](https://i-blog.csdnimg.cn/blog_migrate/a89e95340e80c2a279018b6273770859.png)
    

#### 因特网控制报文协议 ICMP

![](https://i-blog.csdnimg.cn/blog_migrate/81ad158fb0ed3a33be352371636c8fda.png)

分为两种：**ICMP 差错报文和 ICMP 询问报文**

##### ICMP 差错报文

分为五种

###### 终点不可达

![](https://i-blog.csdnimg.cn/blog_migrate/9e5cd274a7e055107c28a335363d9f31.png)

###### 源点抑制

因为拥塞而丢弃

![](https://i-blog.csdnimg.cn/blog_migrate/5c052f8c1577c516c5ec58245803bf27.png)

###### 时间超过

![](https://i-blog.csdnimg.cn/blog_migrate/f18d0a9177d38e54000d17d394fd8123.png)

**当在规定时间内无法收取全部的数据报的时候也会发送 ICMP 差错报文 (时间超过) 来提醒发送方重发剩余的分片**

![](https://i-blog.csdnimg.cn/blog_migrate/0f18e7cab92dcc6e6da1f9e79b6fd443.png)

###### 参数问题

检测到误码

![](https://i-blog.csdnimg.cn/blog_migrate/1f56d1ec73c47682db66dfc11b6afee0.png)

###### 改变路由 (重定向)

**H1 默认网关是 R1，但是 H1 发送给 N2 的报文最佳路径是经过 R4，R1 收到这个报文后发现不是最佳路径，于是发送 ICMP 差错报告 (改变路由)，H1 添加路由项，发送报文给 N2 不经过默认网关 R1 而是 R4**

![](https://i-blog.csdnimg.cn/blog_migrate/5f7910161f84f1cd757ead7a5bd6f7ce.png)

###### 不应发送差错报文的情况

![](https://i-blog.csdnimg.cn/blog_migrate/6fef8770ebf6943bd789f7d294c59b36.png)

##### ICMP 询问报文

**回送请求和回答**

**时间戳请求和回答**

![](https://i-blog.csdnimg.cn/blog_migrate/cb86f04ddcf267b26ff63913cfb9dd66.png)

###### 应用

分组网间探测 PING

![](https://i-blog.csdnimg.cn/blog_migrate/e8214751c03c63175bcd524aced98f47.png)

跟踪路由

![](https://i-blog.csdnimg.cn/blog_migrate/efb30dff529cad161ab86a7a14401eee.png)

![](https://i-blog.csdnimg.cn/blog_migrate/8bb65fd3d5607ba09c2f422c808cc5d0.png)

##### 学校 PPT 所讲

**ICMP 差错报告报文：向源主机报告差错：包括终点不可达、拥塞丢弃、时间超过**

用于目标主机或到目标主机路径上的路由器向源主机报告差错和异常情况

*   终点不可达。当路由器或主机不能交付数据报时，就向源点发送终点不可达报文
*   源点抑制。当路由器或主机由于拥塞而丢弃数据报时，就向源点发送源点抑制报文
*   时间超过。当路由器收到生存时间为零的数据报时，除丢弃该数据报外还要向源点发送时间超过报文

**不应发送 ICMP 差错报告报文的几种情况如下**

*   对 ICMP 差错报告报文不再发送 ICMP 差错报告报文
*   第一个分片的数据报片的所有后续数据报片都不发送 ICMP 差错报告报文
*   对具有组播地址的数据报都不发送 ICMP 差错报告报文

*   Internet Control Message Protocol
*   用于**主机路由器之间**彼此**交流网络层信息**
    *   差错报告: 不可到达的主机, 网络, 端口, 协议
    *   请求 / 应答
*   位于 IP 之上
    *   因为 ICMP 消息是**装载在 IP 分组里的**

![](https://i-blog.csdnimg.cn/blog_migrate/5fc8bc80b2b6fd9163d9ec16232992fe.png)

#### IPV6

**更大的 IP 地址空间，从 32 位升到了 128 位**

**支持即插即用 (自动配置 IP 地址)**

**IPv6 首部长度必须是 8B 的整数倍，而 IPv4 首部是 4B 的整数倍，tcp 首部也是 4B 的整数倍!!!**

**从根本上解决了 IP 地址的耗尽问题**

*   IPv6 数据报格式
    *   **固定长度的 40 字节首部**
    *   不允许分片：**IPv6 只有在包的源结点才能分片，是端到端的，传输路径中的路由器不能分片**

##### IPV6 地址表示

*   冒号十六进制表示法
    *   ![](https://i-blog.csdnimg.cn/blog_migrate/cb25250eba0fcff44e8d5d23d8130cc7.png)

#### 选路算法

*   路由算法确定了通过网络的**端到端路径**
    
*   转发表确定了在路由器上的本地转发
    
*   默认路由器：
    
    *   与**主机直接相连的路由器**，又叫**第一跳路由器**
    *   每当主机发送一个分组时，都先传送给它的默认路由器
        *   * 源路由器：* 源主机的默认路由器
        *   * 目的路由器：* 目的主机的默认路由器
        *   从源主机到目的主机的选路归结为从源路由器到目的路由器的选路
    *   **路由算法**：是确定一个分组**从源路由器到目的路由器所经路径的算法**
    *   ![](https://i-blog.csdnimg.cn/blog_migrate/ccd18744261e7d618eac66dda00b1efe.png)

##### 网络的抽象图模型：费用 (cost)

![](https://i-blog.csdnimg.cn/blog_migrate/46e0fbf72cfaa84e8c89544e5234da6a.png)

##### 路由算法分类：

###### 全局路由算法

*   **所有路由器拥有完整的网络拓扑信息和链路费用信息。**
    
    **链路状态路由算法 LS**：**必须知道网络中每条链路的费用**
    

###### 分布式路由算法

*   以迭代的、分布式的方式计算最低费用路径
    *   节点只有**与其直接相连链路的费用信息**：**不需拥有**所有网络链路费用的完整信息
    *   通过迭代计算，并与相邻节点 (邻居节点) 交换信息
    *   逐步计算出到达某目的节点或一组目的节点的最低费用路径
*   **距离向量路由算法 DV**：每个节点维护到**网络中所有其他节点的费用**（距离）的估计向量

###### **静态路由算法**

*   路由确定后基本不再变化。只有人工干预调整时，可能有一些变化

###### 动态路由算法

*   网络的流量负载或拓扑发生变化时，**路径可能发生改变**
*   可以周期性地或直接地响应拓扑或链路费用的变化
*   易受选路循环、路由振荡之类问题的影响

![](https://i-blog.csdnimg.cn/blog_migrate/c15a4a4bc3b2cfeb3e384bfb065960c8.png)

##### 链路状态路由算法 LS

###### Dijkstra 最低费用路径算法

所有节点都知道网络拓扑，以及每条线路的信息

*   通过链路状态广播来实现
    
*   所有节点拥有相同的信息
    
*   计算**任意一个节点**（源节点）到**所有其他节点的最低费用路径**
    

*   给出该节点的转发表
    
*   迭代：通过 k 次迭代后可以知道到达 k 个目的节点的最低费用路径
    
*   基本思想：
    
    *   以源节点为起点，**每次找出一个到源节点的费用最低的节点**，直到把所有的目的节点都找到为止![](https://i-blog.csdnimg.cn/blog_migrate/6a0cb0d8c350340fc493d8b0f8e67365.png)
*   算法描述：![](https://i-blog.csdnimg.cn/blog_migrate/8b7552e5b9c4777e9095a08cec7fbfc9.png)
    
    *   1、初始化
    *   2、找出一个到源节点的费用最低的节点 w，并以此**更新其它点** D(v) 值
    *   3、重复步骤 2
        *   直到所有的网络结点都在 N’中为止

###### 例题

eg：

*   计算从 _u_ 到所有可能目的节点的最低费用路径。
    
*   计算过程如表，表中的每一行表示一次迭代结束时的算法变量值。![](https://i-blog.csdnimg.cn/blog_migrate/414092708e74403f8461decc19e2a104.png)
    
*   构建从源节点到所有目的节点的路径
    
    *   对于每个节点，都得到**从源节点沿着它的最低费用路径的前驱节点**
    *   每个前驱节点，又可得到它的前驱节点；以此继续，可以得到到所有目的节点的完整路径![](https://i-blog.csdnimg.cn/blog_migrate/607877927abee64f488065c5f5e91612.png)
*   构建最低费用路径树
    
    *   根据**目的节点找出顺序和其费用以及前驱节点**，可以画出源节点 u 到所有目的节点的最低费用路径树
    *   根据得到的所有目的节点的**完整路径**，或最低费用路径树，可以生成源节点的**转发表**
    *   转发表
        *   存放从源节点到每个目的节点的最低费用**路径上的下一跳节点**
        *   即指出对于发往某个目的节点的分组，**从该节点发出后的下一个节点![](https://i-blog.csdnimg.cn/blog_migrate/49a973eb199d3bd20287136c9de0a3a6.png)**

###### 转发表

*   构建转发表![](https://i-blog.csdnimg.cn/blog_migrate/b53eee86582aebac96ab128947e2d195.png)

###### dijkstra 复杂度

![](https://i-blog.csdnimg.cn/blog_migrate/4174d4af342a53d371592a9a24a6e041.png)

##### 距离向量路由算法 DV

*   距离向量路由算法是一种迭代的、异步的和分布式的算法
    *   分布式：每个节点都**从其直接相连邻居接收信息**，进行计算，再将计算结果**分发给邻居**
    *   迭代：计算过程**一直持续**到**邻居之间无更多信息交换为止**
    *   异步：不要求所有节点相互之间步伐一致地操作
    *   自我终结：算法能自行停止

###### 最低费用表示

![](https://i-blog.csdnimg.cn/blog_migrate/e0d42e019f0234776198be7b9ce7e10d.png)

*   dx(y)：**节点 x 到节点 y 的最低费用路径的费用**
    
*   v: 节点 **x 的邻居**节点
    
*   c(x,v)+ dv(y)：x 与某个邻居 v 之间的直接链路费用 c(x,v) 加上邻居 v 到 y 的最小费用。即 **x 经 v 到节点 y 的最小的路径费用**
    
*   minv ：从所有经直接相连邻居节点到节点 y 的费用中选取的**最小路径费用**
    
*   B-F 方程举例![](https://i-blog.csdnimg.cn/blog_migrate/728778f5eae2456ec24c21f6eae0296b.png)
    
*   对每个结点 x
    
    *   初始化
    *   更新自己的距离向量
    *   重复执行（2），直到没有更新的距离向量发出
    *   ![](https://i-blog.csdnimg.cn/blog_migrate/b427e20b880b89d4f53d401502ca1190.png)

###### 距离向量表

*   行：该节点的距离向量 Dx 和其邻居的距离向量 Dv
    
*   列：所有目的节点
    
*   节点 x 的距离向量 Dx ，即节点 x 到每个目的节点 y 的估计费用； Dx = [Dx(y)：y 在 N 中]
    
*   节点 x 每个邻居的距离向量 Dv ，即 x 的邻居 v 到每个目的节点 y 的估计费用，Dv = [Dv(y)：y 在 N 中]
    
*   ![](https://i-blog.csdnimg.cn/blog_migrate/0fdb59f3ba481d5cd6f5350dd4b18ada.png)
    
*   更新其距离向量
    
    *   ![](https://i-blog.csdnimg.cn/blog_migrate/743caf111f9e5653f672fb9a61838f22.png)
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/d92c5c32a16d49f3d545364016d5bfc8.png)
        

###### 链路费用改变与链路故障

*   当一个节点检测到从它到邻居的链路费用发生变化时，就更新其距离向量，如果最低费用路径的费用发生变化，通知其邻居
    
*   **某链路费用减少时情况**
    
    *   ![](https://i-blog.csdnimg.cn/blog_migrate/48caa30c0b481f90ea3f41f6ea4df695.png)
    *   ![](https://i-blog.csdnimg.cn/blog_migrate/8c2295ac804a8228e1051bfaa0e82a62.png)
    *   好消息在链路中能迅速传播
*   **某链路费用增加时情况**
    
    *   ![](https://i-blog.csdnimg.cn/blog_migrate/c40eda0aa492ed6e6463b77b1b953e22.png)
        
    *   ![](https://i-blog.csdnimg.cn/blog_migrate/c661098767021a02ecfc050553b0e1fd.png)
        
    *   ![](https://i-blog.csdnimg.cn/blog_migrate/c64445b12efce6851aae5acce55ecfd4.png)
        
    *   链路费用增加的 “坏消息” 传播很慢
        
    *   ![](https://i-blog.csdnimg.cn/blog_migrate/021c6890292bf3768d15147e5fe35ab0.png)
        
    *   不知道坏消息已经影响他了，局部形成环路
        

##### LS 算法与 DV 算法的比较

![](https://i-blog.csdnimg.cn/blog_migrate/215e8f75eb863706ecaf4094c75501c6.png)

*   LS 算法：
    
    *   知道网络**每条链路的费用**，需发送 O(nE) 个报文；**当一条链路的费用变化时，必须通知所有节点**
    *   需要 O(nE) 个报文和 O(n**2**) 的搜寻，可能会振荡
    
*   DV 算法：
    
    *   迭代时，仅在**两个直接相连邻居之间交换报文**
    *   当链路费用改变时，只有**该链路相连的节点的最低费用路径发生改变时**，**才传播已改变的链路费用**
    *   收敛较慢。可能会遇到选路回环，或计数到无穷的问题。
*   健壮性
    
    *   LS：
        *   路由器向其连接的一条链路广播不正确费用，路由计算基本独立（仅计算自己的转发表），**有一定健壮性**
    *   DV：
        *   个节点可向任意或所有目的节点发布其不正确的最低费用路径，一个节点的计算值会传递给它的邻居，并间接地传递给邻居的邻居。**一个不正确的计算值会扩散到整个网络**
        *   无健壮性

#### 分层次的路由选择协议

##### 域内路由协议 IGP

![](https://i-blog.csdnimg.cn/blog_migrate/6c4b143b2aa1a3f98834aaee15c67428.png)

*   域（自治系统）内路由选择
*   内部网关协议 IGP ：AS(**自治系统**) 内使用的
    *   RIP，OSPF
*   外部网关协议 EGP：AS 之间使用的
    *   BGP

###### 路由信息协议 RIP(自治系统内部)

*   基于**距离向量**的路由选择协议
    
*   距离：**定义为路径中经过的路由器个数**
    
    *   通常为跳数，即从源端口到目的端口所经过的路由器个数，经过一个路由器跳数 + 1
    *   **特别的，从一路由器到直接的网络距离为 1**
*   **RIP 允许路由最多包含 15 个路由器**
    
    *   **距离 16 表示不可达，而且就是用 16 来表示距离不可达到!!!**
    *   **RIP 适用于小的互联网**
    
    等价负载平衡
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/5d15a3dc508ba8ec52c0a92206d183b7.png)

三个要点

*   **和谁交换信息 和相邻的路由器**
*   **交换什么信息 自己的路由表**
*   **何时交换 周期性的交换**

###### 交换信息 (路由表更新问题)

![](https://i-blog.csdnimg.cn/blog_migrate/e6052e5d254593d456851dd1ce224607.png)

注意这里有的资料认为距离相等但是下一跳不相等的时候不进行路由表更新，也就是图中的 N8 4 C 没有，这个看情况来定

*   仅和**相邻的路由器**交换信息
*   路由器交换的信息是**自己的路由表**
*   ![](https://i-blog.csdnimg.cn/blog_migrate/c5c7b3af8587af11346921693abbb6a2.png)
*   eg
    *   修改发来的路由更新信息，距离 + 1，下一跳路由器改为发送来的路由器
    *   **更新距离**![](https://i-blog.csdnimg.cn/blog_migrate/fdef02f737ed20eafed8555c41c4c152.png)
*   eg2：![](https://i-blog.csdnimg.cn/blog_migrate/a61a49f185a6f0f287fbc2b21ea7f0cc.png)

###### RIP 存在 “坏消息传的慢” 的问题

**假如某一时刻 R1 收到 N1 故障，将自己的路由表修改为 N1 16 直连，然后到达周期时刻准备发给 R2，然后 R2 这时候不知道，也发送给 R1，但是 R2 的消息先到了，这个时候 R1 的消息尚未发出，收到 R2 的更新报文之后修改自己的路由表然后发出，这时候变为了 N1 3 R2，传过去 R2 变成了 N1 4 R1，以此类推，知道都更新为 16 的时候才知道不可到达，那么这个中间的过程就浪费了很长的时间，长达数分钟，形成了路由环路**

![](https://i-blog.csdnimg.cn/blog_migrate/6335bd32e52ad106b0f6cfa4052ff1dd.png)

这些措施并不能彻底解决这个问题，根本上是距离向量算法决定的

###### 开放最短路径优先 OSPF(自治系统内部)

**OSPF 信息直接通过 IP 传输 （不是 TCP 或 UDP），因此必须自己实现可靠报文传输、链路状态广播等功能。**

**为了克服 RIP 协议的缺点开发出来的，开放表示不是受某一家厂商控制而是公开发表的**

**OSPF 基于链路状态，RIP 基于距离向量**

*   **使用了 dijkstra 最短路径算法**
*   ![](https://i-blog.csdnimg.cn/blog_migrate/7c6f10bcb18e0c633020df48d46e0c7d.png)

###### 交换信息

**OSPF 相邻路由器之间通过 Hello 分组来建立和维护邻居关系**

![](https://i-blog.csdnimg.cn/blog_migrate/71c3426bcdbe7181acf4313872c8fcba.png)

**每个路由器都有链路状态公告 LSA**

![](https://i-blog.csdnimg.cn/blog_migrate/0f968ca1130166a5925535bf74254ef9.png)

每个路由器都有一个链路状态数据库 LSDB，用来存储每个数据库的 LSA

![](https://i-blog.csdnimg.cn/blog_migrate/262a271c30cedb645f151f7ea9c4c7b4.png)

工作过程

![](https://i-blog.csdnimg.cn/blog_migrate/146e4c941e9f70b3426c99e4addcee06.png)

五种分组类型

![](https://i-blog.csdnimg.cn/blog_migrate/c10e26d735c9a27010524819388f0215.png)

工作过程

![](https://i-blog.csdnimg.cn/blog_migrate/4e0ec4dd684e2d5a61a4091393c101d8.png)

**建立关系 (指定路由器)**

**由于当链路状态更新的时候路由器需要通过广播的形式向系统中其他的路由器发送链路状态请求分组，所以在网络中就会存在大量的广播分组的传递，为了减少这个问题，引入了指定路由器 DR 和备用指定路由器 BDR，规定其他的路由器只能和他们交换信息，减少了路由器之间相连的个数，但是增加了这两个路由器的负担**

![](https://i-blog.csdnimg.cn/blog_migrate/7890c722c77acd2b2a097727272dda94.png)

关于区域

将自治系统 AS 划分为若干个区域，为了减少区域内部的通信量，使得能用于大规模的网络系统

![](https://i-blog.csdnimg.cn/blog_migrate/49a91146e03119e3de27a7d3a005f58d.png)

*   通过洪泛法 (广播) 向自制系统内所有的路由器发送信息
    *   **广播**
*   交换本路由器**相邻的所有路由器的链路状态**
*   当**链路状态发生变化时**交换信息![](https://i-blog.csdnimg.cn/blog_migrate/d28e992266a2f3de0e02728cb9dba2e5.png)

##### 边界网关协议 BGP(自治系统之间)

不同的自治系统之间所使用的代价度量可能不相同，**这使得在自治系统之间没有一个相对统一的标准**

**自治系统之间的路由选择必须考虑相关的策略，例如：政治，经济，安全等等** (比如有的自治系统不让我们的 TCP 报文通过)

**BGP 协议为了找到一条尽可能快的路径 (避免兜圈圈)，而不是找到最佳路径**

###### 工作原理

**BGP 发言人之间通信需要建立 TCP 连接，端口号为 179**

![](https://i-blog.csdnimg.cn/blog_migrate/643e0a33dbefe4394f6abcf1147dde5a.png)

![](https://i-blog.csdnimg.cn/blog_migrate/2ee86ba5765b6e1c33682315aaf218ea.png)

![](https://i-blog.csdnimg.cn/blog_migrate/1149ba92005919f6fcfb44ffbdcd8eb9.png)

*   从相邻 AS 获取子网可达信息
*   向该 AS 内部的所有路由器传播这些可达性信息
*   域间寻路任务：![](https://i-blog.csdnimg.cn/blog_migrate/80c647f324bbf6691e3e3b5d44c2e0da.png)
*   基于距离矢量算法

##### 三个封装关系

![](https://i-blog.csdnimg.cn/blog_migrate/2af51cd12381eaf5f680588e5826749b.png)

### 第五章 链路层

#### 关于可靠数据传输

![](https://i-blog.csdnimg.cn/blog_migrate/0c59514e5e995110545cb2506277eac0.png)

地位

![](https://i-blog.csdnimg.cn/blog_migrate/13ba428be097e2cc24ea13b78f908b8a.png)

**链路加上通信协议就称为数据链路**

![](https://i-blog.csdnimg.cn/blog_migrate/f1a9f9da42dbb8ebc680b35c5e78e0a9.png)

#### 三个重要问题 (点对点传输)

**封装成帧；差错检测；可靠传输**

封装成帧

**应用层封装好应用层协议数据单元，然后交付给运输层添加运输层协议首部，然后交给网络层添加网络层协议首部，然后交给数据链路层添加帧头帧尾，最后交给物理层把这一段帧视作比特流进行传输，到达接收方后进行一层一层的解包得到数据**

![](https://i-blog.csdnimg.cn/blog_migrate/76cb7d3139398a66c84605d7b4aff6c3.png)

差错检测

![](https://i-blog.csdnimg.cn/blog_migrate/1bae9c9e9c75510f64adc2a458939365.png)

需要有一定的措施来判断传输过程的数据是否发生错误

可靠传输

![](https://i-blog.csdnimg.cn/blog_migrate/2ac1a44cbba4639a0dbab294b332c1b4.png)

尽管可能发生差错，但发送方发送什么，接收方都能相应的接受到什么，这个就叫做可靠传输

#### 其他的

![](https://i-blog.csdnimg.cn/blog_migrate/4b86ed5fb47b2e833d98a2b069259705.png)

##### 概述

*   主要术语
    
    *   链路层的节点包括了主机和路由器；
        
    *   我们将沿通信路径，将相邻节点连接起来的通信信道称为链路，这里的链路可以将主机与路由器，或者路由器与路由器连接起来。
        
    *   为了将一个数据报从源主机传输到目的主机，数据报必须通过沿端到端路径上的各段链路传输。
        
    *   链路主要包括：**有线链路和无线链路**。在实际链路上传输时，传输节点需要**将数据报封装成数据帧**进行传输。
        
    *   数据链路层的职责是将数据报从一个节点传送到与该节点**直接有物理链路相连**的另一个节点。
        
*   类比![](https://i-blog.csdnimg.cn/blog_migrate/5e60c6d6f97e7c18196c9a61d5f99393.png)
    
*   链路层提供的服务
    
    *   封装成**帧**，链路接入
        *   封装数据报为数据帧，**增加头部尾部信息**
        *   若是共享链路，接入链路
        *   在**数据帧头部**中，用 MAC 地址来表示源目的 MAC 地址
            *   不同于 IP 地址
    *   在相邻节点之间**可靠传输数据帧**
    *   流量控制 **(flow control)😗*
        *   用于控制发送节点向**直接相连的接收节点**发送**数据帧的频率**
    *   差错检测 **(error detection)😗*
        *   差错可能由信号衰减、噪声引入
        *   接收方检测是否出现错误
            *   通知发送方重传或丢弃数据帧
    *   错误纠正 **(error correction)😗*
        *   **接收方标识和纠正比特错误**，而不需要请求重传
    *   **半双工和全双工 (half-duplex and full-duplex):**
        *   • 在半双工模式，链路的两个节点都可以发送数据，但是不能同时发送
*   链路层实现的位置
    
    *   在主机和网络设备 (路由器) 上实现
    *   在主机上，链路层的主体部分是在**网络适配器**上实现的 (称为**网卡**)![](https://i-blog.csdnimg.cn/blog_migrate/f803e865a243dffdf39dbca76559d756.png)

#### 封装成帧

![](https://i-blog.csdnimg.cn/blog_migrate/047ff8d5ad529a465f95508803de8f0f.png)

**添加帧头和帧尾使之成为帧；包含重要的控制信息；帧头帧尾重要的作用之一就是帧定界**

![](https://i-blog.csdnimg.cn/blog_migrate/5d32870175cac1e47ccf3817dc552bf5.png)

##### 透明传输

透明传输是指数据链路层对上层交付的数据没有任何限制，就好像数据链路层不存在一样

** 理解下来就是说：我的帧头帧尾里面的定界符是一串 01，在数据当中很可能也存在一样的 01，那么不加处理的话就会导致接收方把数据里面的 01 认为是定界符而导致数据出错。** 怎么解决呢？

**面向字节的物理链路**，可以在数据当中的 flag 前面加上一个转义字符，表示这是数据不是定界符，接受方接收到之后把转义字符剔除就好了

![](https://i-blog.csdnimg.cn/blog_migrate/f790e2369f530b5ac3e48a2d5793f3a7.png)

如果数据中也包含转义字符形式的数据那么在前面也加上转义字符，接收方接受并且剔除就好了

![](https://i-blog.csdnimg.cn/blog_migrate/0616db668e2e2ce548ffb93686246da1.png)

那么面向比特的物理链路呢？

**帧定界码：01111110**

**在数据当中的没五个连续的 1 后面加一个 0，接收方接受识别并剔除就好了**

![](https://i-blog.csdnimg.cn/blog_migrate/d34105bef5d18f1c2968a69683915d2d.png)

##### 补充

帧数据部分尽可能大，提高利用率

**最大传送单元 MTU**

![](https://i-blog.csdnimg.cn/blog_migrate/ac9ad804c24dc0dfa25605f4fa6ca9fe.png)

#### 差错检测和纠错

![](https://i-blog.csdnimg.cn/blog_migrate/262b77a2d5af46a28c37a46ab8070c25.png)

*   发送节点![](https://i-blog.csdnimg.cn/blog_migrate/f826f3758505966d9609fddac1f67f20.png)
    
*   接收节点![](https://i-blog.csdnimg.cn/blog_migrate/2c4d7849c5d03f7a21d8f73980a7a852.png)
    
*   差错检测和纠正技术不能保证接收方检测到所有的比特差错，即**可能出现未检测到的比特差错**，而接收方并未发现
    
    *   选择一个合适的差错检测方案使未检测到的情况发生的概率很小即可。
        
        差错检测和纠错技术越好，越复杂，开销更大
        

##### 奇偶校验

**奇校验添加 1 保证 1 个数为奇数，如果 1 数量变为偶数就发生差错，但是如果 1 的奇性不变那么就无法检测出来，例如发生两位的误码**

![](https://i-blog.csdnimg.cn/blog_migrate/964e32316a409daa16ec2754658c6817.png)

*   ![](https://i-blog.csdnimg.cn/blog_migrate/916ac0869a8f07d74929c1bcea781c84.png)
    
*   二维奇偶校验![](https://i-blog.csdnimg.cn/blog_migrate/32b89614b5395e636830312b7089f26c.png)
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/af0aabd911d4ad88b486f6ee69963b13.png)
    
    > 第一行有 3 个 1，因此第一行的校验位为 1，
    > 
    > 第二行有 4 个 1，因此第二行的校验位为 0，
    > 
    > 第三行有 3 个 1，因此第三行的校验位为 1，
    > 
    > 最终得到得到左边这个图。
    > 
    > 假设在传输过程中，有一个比特错误，例如第二行第二列的 1 发生了翻转，变为了 0，
    > 
    > 在接收方进行检验时，会发现第二行的校验位不对，以及第二列的校验位不对，这样一交叉就会发现，第二行第二列的比特发生错误。
    > 
    > 因此二维奇偶校验可以检测并纠正单个比特差错；能够检测分组中任意两个比特的差错。
    

##### Internet 校验和

![](https://i-blog.csdnimg.cn/blog_migrate/19fe602f3f8c56ab835a9d4990613784.png)

> 在**发送方：**
> 
> 首先，将数据的每两个字节当作一个 16 位的整数，可分成若干整数；
> 
> 其次，将所有 16 位的整数求和；
> 
> 最后，对得到的和逐位取反，作为检查和，放在报文段首部，一起发送。
> 
> 在接收方：对接收到的信息 (包括检查和项) 按与发送方相同的方法求和。
> 
> 如果结果为全 “1”：则收到的数据无差错；
> 
> 如果结果中有 “0”：则收到的数据出现差错。

*   特点：
    *   分组开销小，检查和位数比较少
    *   差错检测能力弱
    *   适用于**运输层**
    *   链路层的差错检测由适配器中专用的硬件实现，采用更强的 CRC 方法

##### 循环冗余 (CRC) 检测

**算法要求多项式必须包含最低此项!!!**

![](https://i-blog.csdnimg.cn/blog_migrate/c744cd965fdde7f1d4683f495b5126d5.png)

*   即**多项式编码**，把要发送的比特串看作为**系数是 0 或 1** 的一个多项式，对比特串的操作看作为多项式运算
    
*   基本思想
    
    *   设发送节点要把数据 _D_（_d_ 比特）发送给接收节点
    *   发送方和接收方先共**同选定一个生成多项式 G**（r+1 比特），**最高有效位 (最左边) 是 1**。

> 把要发送的比特串看作为系数是 0 或 1 的一个多项式，对比特串的操作看作为多项式运算。
> 
> 这里我们来看这样一个例子，10111 这个比特序列，如果用多项式表示，每个比特作为多项式的系数，因此 10111 等价于 x 的 4 次方 + x 的 2 次方 + x 的 1 次方 + 1

*   基本思想![](https://i-blog.csdnimg.cn/blog_migrate/98232d43d2f1e14e0123d52bee199d31.png)
    
    *   模 2 运算
        
        *   加法不进位，减法不借位![](https://i-blog.csdnimg.cn/blog_migrate/f41f8adbe75a2212439199eaa377d894.png)
            
        *   乘以 2_r_，即比特模式左移 _r_ 个位置。
            
            ​ _D_×2_r_ XOR _R_ = _D_ 00…00 XOR _R_
            
            ​ = _DR_ (_d+r_ 比特)·
            
    *   > 如何计算 CRC 码。
        > 
        > 首先将**数据 D 称为 2 的 r 次方**，这里 r 表示 CRC 编码的位数，
        > 
        > 然后将其除以给定的生成多项式 G，
        > 
        > 所得的余数为 CRC 编码
        
*   ![](https://i-blog.csdnimg.cn/blog_migrate/953e6390718942f29b29bbb19b398dc1.png)
    
    > 我们通过一个具体的例子来说明，CRC 编码的计算过程。
    > 
    > 假设数据 D 为 101110，生成多项式 G 为 1001，CRC 编码为 3 比特，是生成多项式的位数减 1.
    > 
    > 这里用模 2 运输，计算 CRC 编码。
    > 
    > 首先将 D 乘以 2 的 3 次方，即可得 101110000；
    > 
    > 然后将 101110000 除以生成多项式 1001，最后可得余数为 0011，因为 CRC 编码只取 3 比特，因此为 011。
    > 
    > 因此发送方最终发送的数据为 101110011。
    
*   例题![](https://i-blog.csdnimg.cn/blog_migrate/01b7dad23c29879b301aee7d68ea9a83.png)
    
*   CRC 特点
    
    *   生成多项式 G 的选择：常见的有 8、12、16 和 32 比特生成多项式 G
    *   能检测**小于 r+1 位的突发差错、任何奇数个差错**。![](https://i-blog.csdnimg.cn/blog_migrate/3e58a60181da82a33d3042e06badab16.png)

补充

![](https://i-blog.csdnimg.cn/blog_migrate/439a1804704785ef72b8b28a20f06514.png)

**注意：CRC 具有纠错能力，但是数据链路层一般只用他的检错能力，关于纠错看第四条**

##### 三种方法的比较

![](https://i-blog.csdnimg.cn/blog_migrate/cf0480a1e23148d5844902593fe83cd7.png)

#### 多路访问链路和协议

![](https://i-blog.csdnimg.cn/blog_migrate/348265ce6126115d60ba518f467b6678.png)

![](https://i-blog.csdnimg.cn/blog_migrate/065f49a726bf32ad85cffd20ab336573.png)

##### 两种网络链路

*   **点对点链路**
    *   链路两端各一个节点。一个发送和一个接收
    *   如点对点协议 ppp
*   **广播链路**
    *   多个节点连接到**一个共享的广播信道**
    *   广播：
        *   **任何一个节点传输一帧**时，信号在信道上广播，**其他节点都可以收到一个拷贝**

##### PPP(point to point protocol) 协议（了解)

封装成帧，链路控制协议 LCP，网络控制协议 NCPs

![](https://i-blog.csdnimg.cn/blog_migrate/537b34bc61d41ec319cecb2dcb45d66e.png)

###### 帧格式

![](https://i-blog.csdnimg.cn/blog_migrate/c8ea0fa82fbd68a8f1cbfa3f4a94778b.png)

![](https://i-blog.csdnimg.cn/blog_migrate/c0aceaa6b63318ca56c08f375ddb2cf2.png)

###### 关于封装帧的透明传输

**面对字节：**

三种数据，定界符 7E，转义字符 7D 和 ASCII 码控制符的处理

![](https://i-blog.csdnimg.cn/blog_migrate/5c9d24f0942905570d1a6d287b47ae05.png)

**面对比特：**

![](https://i-blog.csdnimg.cn/blog_migrate/53e41a42ca2f0cf46fae69ff141e164e.png)

###### 差错检测

计算范围如下，计算完之后填入 FCS

![](https://i-blog.csdnimg.cn/blog_migrate/508aa7fbdb8284aeae7db5df0c99b9fb.png)

##### 广播信道

*   ##### 存在问题![](https://i-blog.csdnimg.cn/blog_migrate/13a1ca249c724db16ccb0ac3add3ff2d.png)
    

#### 多路访问协议

*   目的：
    
    *   避免多个节点同时使用信道，发生冲突，产生互相干扰
*   冲突（collide）：
    
    *   **两个以上的节点同时传输帧**，使接收方收不到正确的帧 (所有冲突的帧都受损失)
        *   造成广播信道时间的浪费
        *   多路访问协议可用于许多不同的网络环境，如有线和无线局域网、卫星网等
*   理想的多地址访问协议![](https://i-blog.csdnimg.cn/blog_migrate/bab2d033e6fc9ce0722cb5b09fe9976d.png)
    

![](https://i-blog.csdnimg.cn/blog_migrate/f19964630d3005bcbc17c8747654423f.png)

##### 信道划分 (静态划分信道) 协议

*   把信道划分为小片 (时隙)
*   给节点分配专用的小片
*   主要有 **TDMA、FDMA**、**CDMA** 三种

###### TDMA 时分复用

![](https://i-blog.csdnimg.cn/blog_migrate/f5d0f4eb7b14db90c607ae2d6ca70066.png)

*   设信道支持 _N_ 个节点，传输速率是 _R_ b/s
    
    *   **时分多路访问** TDMA **(time division multiple access)**
        
        *   将时间划分为_**时间帧**_，每个时间帧再划分为 _**N 个时隙**_（长度保证发送一个分组），分别分配给 _N_ 个节点。**每个节点只在固定分配的时隙中传输**。
            
            ​ 例：6 个站点的 LAN, 时隙 1、3、4 有分组, 时隙 2、5、6 空闲
            
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/edba7cc6dd55563fb1d07dcf46b40fb9.png)
        
*   频率相同，每个用户占用不同时隙
    
*   TDMA 特点：
    
    *   避免冲突、公平
        *   每个节点专用速率 R/N b/s
    *   节点速率有限：R/N b/s
    *   效率不高：节点必须等待它的传输时隙![](https://i-blog.csdnimg.cn/blog_migrate/9d16a950448c65b5af6436b70d801f76.png)

###### FDMA 频分复用

**不同用户的数据使用不用的频率段**

![](https://i-blog.csdnimg.cn/blog_migrate/3185157cd133953493901497b754ebbb.png)

*   **频分多路访问** FDMA **(frequency division multiple access)**
*   ![](https://i-blog.csdnimg.cn/blog_migrate/e69c10ac945ec511b2da2729b3f55d15.png)
*   每个用户占一个频段 ![](https://i-blog.csdnimg.cn/blog_migrate/39f16902d82b5fb57095f06779081309.png)

###### CDMA 码分复用

与前面两个不同，**码分复用允许用户在同样的时间使用同样的频带进行通信**

**由于各用户使用经过特殊挑选的不同码型，各用户之间不会造成干扰**

**每一个用户有标识性的唯一的编码，可以用这个编码对自己的数据进行编码然后进行发送**

*   **码分多路访问** CDMA (code division multiple access)
    
*   ![](https://i-blog.csdnimg.cn/blog_migrate/c8b7bed3e39a4e27905063ffa8e78cd3.png)
    

##### 随机访问协议

*   基本思想
    *   发送节点**以信道全部速率（_R_ b/s）发送**
    *   发生冲突时，**冲突的每个节点分别等待一个随机时间**，再重发，**直到帧 (分组) 发送成功**
    *   节点间没有协调者
*   典型随机访问协议：
    *   ALOHA 协议 (纯 ALOHA，时隙 ALOHA)
    *   载波监听多路访问 CSMA 协议
    *   带冲突检测的载波监听多路访问 CSMA/CD
    *   带冲突避免的载波监听多路访问 CSMA/CA

###### ALOHA

*   ![](https://i-blog.csdnimg.cn/blog_migrate/f7af8e67806e9f1a732f286f0fe6d08f.png)

> 首先介绍 ALOHA 协议。
> 
> ALOHA 是夏威夷大学研制的一个无线电广播通信网，采用星型拓扑结构，使**地理上分散的用户通过无线电来使用中心主机**。
> 
> 中心主机通过下行信道向二级主机广播分组；
> 
> 二级主机通过上行信道向中心主机发送分组（可能会冲突，无线电信道是一个公用信道）。如图所示。
> 
> ALOHA 有两种形式：时隙 ALOHA 和纯 ALOHA。我们将分别进行介绍。

###### 纯 ALOHA

*   非时隙 ALOHA：简单，不需同步
*   **帧一到达，立即传输**
*   如果与其他帧产生冲突，在该冲突帧传完之后：
    *   **以概率 p** 立即重传该帧
    *   **或等待一个帧的传输时间**，**再以概率 p 传输该帧**，或者**以概率 1-p 等待另一个帧的时间**![](https://i-blog.csdnimg.cn/blog_migrate/1738247bd1514cdab0e2dc1839b73aa6.png)
*   纯 ALOHA 效率![](https://i-blog.csdnimg.cn/blog_migrate/fe69c9d5294225f350ec56c6b88227ba.png)
*   ![](https://i-blog.csdnimg.cn/blog_migrate/4306567ca29048e4eee7e2d7c1c2b457.png)

###### 时隙 ALOHA

*   ![](https://i-blog.csdnimg.cn/blog_migrate/9a31b6d480bec9dbc1320c355e2635eb.png)
    
*   控制发送的时间
    
*   比纯 ALOHA 效率更高
    
*   效率
    
    *   当有很多节点，每个节点有很多帧要发送时，成功时隙所占的百分比
    *   ![](https://i-blog.csdnimg.cn/blog_migrate/d585acda086d35da9fa926b057d9ee11.png)

##### CSMA（载波侦听多路访问）

动态接入控制

协调总线上各个主机的工作，尽量避免产生碰撞，是我们要研究的问题

*   载波侦听
    
    *   某个节点在发送之前，先监听信道
    *   信道忙：有其他节点正往信道发送帧，该节点随机等待（回退）一段时间，然后再侦听信道
    *   信道空：该节点开始传输整个数据帧
*   CSMA 特点：
    
    *   发前监听，可减少冲突
    *   由于传播时延的存在，仍有可能出现冲突，并造成信道浪费
*   CSMA 发送冲突的例子
    
    *   ![](https://i-blog.csdnimg.cn/blog_migrate/7aaaa133325d72f03c764ed2376581d1.png)
    *   ![](https://i-blog.csdnimg.cn/blog_migrate/a330717272779151f22515169730cb58.png)
    
    > 在时间 t0：节点 B 侦听到信道空，开始传输帧，沿着媒体传播比特。
    > 
    > 在时间 t1：节点 D 有帧要发送。B 的传输信号未到达 D，因此 D 检测到信道空，开始传输数据帧。很快，B 的传输开始在 D 节点干扰 D 的传输（冲突）
    > 
    > 我们可以看到传播时延越长，节点不能侦听到另一个节点已经开始传输的可能性越大。
    
*   带来的问题：信道浪费
    
    *   **节点没有进行冲突检测**，既使发生了冲突，节点仍继续传输它们的帧。但**该帧已经被破坏、是无用的帧，信道传输时间被浪费**

###### 总线局域网协议 CSMA/CD

![](https://i-blog.csdnimg.cn/blog_migrate/8bd56ce5bd4391e55024a065c70679e4.png)

多址接入 MA：多个主机连在一条总线上，竞争使用总线

载波监听 CS：发送前检测是否有其他站点正在发送，有则等待 (**96 比特时间**)

碰撞检测 CD：**发送的过程**中检测是否出现碰撞，有则停止发送，退避一段时间再次发送 **（这个和发送完帧的传播不一样！发送完很有可能帧仍在传播)**

![](https://i-blog.csdnimg.cn/blog_migrate/d8e62cb071439ba352aa7c09a6cb48cb.png)

**载波监听多址接入 / 碰撞检测**

CS: 载波侦听 / 监听，每一个站在发送数据之前以及发送数据时都要检查总线上是否有其他计算机在发送数据

MA：多点接入，表示许多计算机以多点接入的方式连接在一根总线上

CD：碰撞检测 (冲突检测)，**边发送边监听**，判断自己在发送数据时其他站是否也在发送数据

*   基本原理：传送前侦听
    
    *   信道忙：延迟传送
    *   信道闲：传送整个帧
*   发送同时进行冲突检测：一旦检测到冲突就立即停止传输， 尽快重发
    
*   目的：缩短无效传送时间，提高信道的利用率
    
*   CSMA/CD 举例![](https://i-blog.csdnimg.cn/blog_migrate/dc0883c99447b4eb98e643d4c124e44b.png)
    
    > 可以看到 CSMA/CD 中虽然也会发生碰撞，但是两个节点 B、D 在检测到冲突之后很短的时间内都放弃传输。
    > 
    > （备注）
    > 
    > 黄色表示 B 节点的传播物理范围，红色表示 D 节点的物理传播范围
    > 
    > 怎么看时空图，横轴表示四个节点在空间中的位置，纵轴表示时间
    > 
    > 在时刻 t0，节点 B 侦听到信道是空闲的，因为当前没有其他节点在传输，于是节点 B 开始传输
    > 
    > 在 t1 时刻，由于 B 节点的电磁波还没有传播到 D 能检测的范围，因此 D 节点发现信道空闲，开始传输，这个时候就会产生相互干扰
    > 
    > D 节点检测到干扰后，会**立即放弃**传输，B 节点也会放弃
    
*   以太网 CSMA/CD 的运行机制
    
    > 我们说以太网也是采用 CSMA/CD 机制来访问共享信道。
    > 
    > 其基本思想如下：
    > 
    > 首先，适配器从网络层获得一个数据报，封装成帧，准备发送；
    > 
    > 第二，如果适配器侦听到信道空闲，开始传输帧；如果检测到信道繁忙，将等待一段时间，直到侦听到信道空闲，开始传输帧；
    > 
    > 第三，在传输过程中，适配器会同时监听是否有其他适配器的信号能量；
    > 
    > 如果适配器在整个帧的传输过程中，没有监听到其他信号，则完成该帧的传输；如果监听到来自其他适配器的信号，则中止传输帧；
    > 
    > 中止传输后，**适配器会等待一个随机时间**，重新执行步骤 2
    
    *   ![](https://i-blog.csdnimg.cn/blog_migrate/84cd1c926fa61403ceaafe8f48f30e46.png)

关于碰撞

![](https://i-blog.csdnimg.cn/blog_migrate/614e3b3fc5c1e70b58fdc72d4fc69c0f.png)

###### 以太网 CSMA/CD 的运行机制讨论

*   拥塞信号
    
    *   48 比特，确保所有传送者知道冲突发生
*   比特时间
    
    *   • 对于 10 Mbps Ethernet 为 0.1 微秒，  
        当 K=1023，等待时间大约 50 毫秒
*   二进制指数回退算法
    
    *   • 目标：适配器依据当前负载情况重传，重负载时等待时间变长
        
        • 第一次冲突： 在 {0,1} 中选 k 值；延迟 Kx512 比特时间传送
        
        • 第二次冲突：在 {0,1,2,3} 中选 k 值…
        
        •10 次以后，在 {0,1,2,3,4,…,1023} 中选 k 值。
        
        •K 是等概率选择
        

###### 争用期

*   最迟要多久才能检测到碰撞？![](https://i-blog.csdnimg.cn/blog_migrate/c082f9b2d3ea2637f9f9f0629a1c57a2.png)

引入**争用期**的概念

**我们需要尽量做到的就是在数据发送完成之前，就是最后一个比特发送出去之前，我们的第一个比特已经到达目的主机了，如果帧太短没到达的话，这段信号就没有人监听了，意思是发生了碰撞也没人管**

![](https://i-blog.csdnimg.cn/blog_migrate/d42aeb8c793ef1fc21d5b159bcfd9ac4.png)

###### 最小帧长

**如果发送的帧太短，那么发送帧的时间就很短，完了之后可能帧还没传到地方，这个时候并没有进行碰撞检测，所以这个时候发生碰撞就没有办法处理了**

![](https://i-blog.csdnimg.cn/blog_migrate/edabf1e72f8fc3224d7120e2ef37498d.png)

**如果接收方接收到的帧长度小于 64 字节，那么一定是发生了碰撞，这个数据为垃圾数据，应该直接丢弃**

**最小帧长 = 争用期 * 数据传输速率**

**这个是在传播时延和发送时延相同的极限情况下得出的，也就是第一个 bit 刚好传到然后最后一个 bit 刚好发送完的极限情况，保证了刚好在发送过程中能一直监听是否发生碰撞，免得发送完毕之后第一个字节还在传播就可能碰撞**

![](https://i-blog.csdnimg.cn/blog_migrate/37f82d030302b7f35f16aa1f58bf5a13.png)

###### 最大帧长

![](https://i-blog.csdnimg.cn/blog_migrate/40bc5f760081433d2d81b55e4921ff6f.png)

###### 截断二进制指数规避算法

**用来计算发生碰撞之后的与下一次重传的退避 (间隔时间)**

![](https://i-blog.csdnimg.cn/blog_migrate/dafafc2b443fcb39f87c410deb75fead.png)

![](https://i-blog.csdnimg.cn/blog_migrate/c7fef9f401bb683315cf9b4e67697942.png)

###### 信道利用率

![](https://i-blog.csdnimg.cn/blog_migrate/2f67413f29232e9da0be354945b811ee.png)

提高利用率：**以太网端到端的距离受到限制，以太网帧的长度应该尽可能长一些**

接受方的流程

需要通过三个检查：最短帧长；地址正确；CRC 检测出现误码

![](https://i-blog.csdnimg.cn/blog_migrate/d22ca1bf6ad5ce2f9d3eab77c53ac71e.png)

###### 无线局域网协议 CSMA/CA

CA：避免碰撞

**在无线局域网当中不能使用碰撞检测 CD，应该使用碰撞避免 CA！！！**

原因：

![](https://i-blog.csdnimg.cn/blog_migrate/d76e6e13db566f1e103d3772784716fe.png)

*   **可以适用于无线局域网**
    
    *   这是 CSMA/CD 无法实现的
*   工作原理
    
    *   预约信道
    *   ACK 帧
    *   RTS/CTS 帧
*   两者区别![](https://i-blog.csdnimg.cn/blog_migrate/9d7986b7ed5da1212a69996f1e3522d7.png)
    

##### 介质访问控制协议

*   MAC 协议：
*   ![](https://i-blog.csdnimg.cn/blog_migrate/00473aef7712cbc87a8e8cc36abe0625.png)

###### 轮询协议

*   轮询开销
*   等待延迟
*   单点故障

![](https://i-blog.csdnimg.cn/blog_migrate/3d9a7f55d9d1d6e33c806993a9bef69e.png)

###### 令牌传递协议

*   令牌：
    *   一个特殊格式的 MAC 控制帧，不含任何信息
    *   确保同一时刻只有一个节点独占信道
*   ![](https://i-blog.csdnimg.cn/blog_migrate/c5c1af557b031167894d4d4200a5a0ec.png)

#### MAC 地址，IP 地址和 ARP 协议

![](https://i-blog.csdnimg.cn/blog_migrate/35ffdb4a53d72456d369cd8cb110a868.png)

##### Mac 地址介绍

**Mac 地址又称为物理地址，用来全球性唯一标识网络接口，属于数据链路层，不属于物理层**

![](https://i-blog.csdnimg.cn/blog_migrate/dd1b9536a86b188dae15a74351c7f12e.png)

###### 单播，广播，多播 MAC 地址举例

![](https://i-blog.csdnimg.cn/blog_migrate/b0c88e573b33c2a900447565fee87512.png)

![](https://i-blog.csdnimg.cn/blog_migrate/ec4fb822c6262172212e2952bcee5bd7.png)

**多播和广播的区别：多播是一对多，不一定是全部；而广播就是一对全部**

**判断是否为多播地址，多播地址的第一个字节的第一个 bit 是 1，也就是图中的第二个数字是奇数，这里就是 7**

![](https://i-blog.csdnimg.cn/blog_migrate/d4f5b23f6e66aebeebb7118373f8093b.png)

**给主机配置多播组列表进行私有应用时，不得使用公有的标准多播地址**

##### IP 地址

![](https://i-blog.csdnimg.cn/blog_migrate/d604dccbca7a17595b145a2714bb49bd.png)

数据的封装过程

![](https://i-blog.csdnimg.cn/blog_migrate/728dbda5fa98ba4e7d8175c58518ea0a.png)

我看不懂也不需要看懂，加上首部 (尾部) 交给下一层

**在网络层首部中应该封装有源 IP 地址和目的 IP 地址；链路层首部中应该封装有源 MAC 地址和目的 MAC 地址**

###### 数据包转发过程中的 IP 和 MAC

**在数据包的转发过程中，源 IP 地址和目的 IP 地址始终保持不变，而源 MAC 地址和目的 MAC 地址逐段链路 (或逐个网络) 改变！！**

![](https://i-blog.csdnimg.cn/blog_migrate/d5d6e489181231c24392f16f7f4d72ab.png)

##### ARP 协议

**通过 IP 地址找到其对应的 MAC 地址**

**发送广播帧，目的 MAC 地址是全 1(FF-FF-FF-FF-FF-FF)**

**对应的主机收到并且将发送方的 IP 和 MAC 存到自己的 ARP 缓存表中记录**

![](https://i-blog.csdnimg.cn/blog_migrate/e5469e51ae9d3d127d3e20514981900f.png)

**返回的帧源 IP 和 MAC 是自己的，目的是发送方的；发送方收到返回帧之后解析，得到 MAC 地址并且存到 ARP 缓存表中**

![](https://i-blog.csdnimg.cn/blog_migrate/4d2ae4fa882f93d2951343dece6c0577.png)

**注意：ARP 协议只能用于一个链路或者网络当中；跨网路需要逐个网络使用**

![](https://i-blog.csdnimg.cn/blog_migrate/d27f40a597dd3b414ee7438e723c2c2f.png)

#### 交换局域网

##### 概述

*   局域网：Local Area Network (LAN)
    
*   多址访问协议广泛应用于局域网
    
*   基于随机访问的 CSMA/CD 广泛应用于局域网
    
*   局域网**按拓扑结构**进行分类：
    
    *   星形网、环形网、总线网、树形网和网状网
*   计算机与局域网通过**网络接口板进行连接**
    
    *   网络接口板又称**通信适配器**（Adapter）或**网络接口卡 NIC**（Network Interface Card），通常我们称为 “**网卡**”

##### 链路层寻址和 ARP

*   每个节点有网络层地址和链路层地址
    *   网络层地址
        *   节点在**网络中**分配的一个**唯一地址（IP 地址）**。用于把分组送到目的 IP 网络。长度为 32 比特（IPv4）
    *   链路层地址
        *   又叫做 **MAC 地址**或物理地址、局域网地址
*   MAC 地址
    *   LAN 地址、物理地址
    *   节点 “**网卡” 本身所带的地址（唯一**）
    *   MAC 地址长度通常为 **6 字节 (48 比特**)，共 **248** 个
    *   6 字节地址用 16 进制表示，每个字节表示为一对 16 进制数
        *   1A-2F-BB-76-09-AD
    *   **网卡的 MAC 地址是永久的**（生产时固化在其 ROM 里）

###### MAC 地址分配

*   ![](https://i-blog.csdnimg.cn/blog_migrate/63cd4f7ea653e7c1c11e610d4d47a0c2.png)
*   MAC 地址是**平面结构**
    *   带有同一网卡的节点，在任何网络中都有同样的 MAC 地址

###### AMC 地址识别

*   广播信道的局域网中，一个节点发送的帧，在信道上**广播传输，其他节点都可能收到该帧**
*   大多数情况，一个节点只向某个特定的节点发送
*   由 “**网卡**” 负责 MAC 地址的封装和识别
*   **发送适配器**：
    *   将目的 MAC 地址封装到帧中，并发送。**所有其他适配器都会收到这个帧**。
*   **接收适配器**：
    *   检查**帧的目的 MAC 地址是否与自己 MAC** 地址相匹配：
    *   匹配：**接收该帧，取出数据报，并传递给上层。不匹配：丢弃该帧**
*   广播帧：发送给所有节点的帧 全 1 地址：FF-FF-FF-FF-FF-FF

###### 回顾：节点的三种不同地址

![](https://i-blog.csdnimg.cn/blog_migrate/5460eaf69fb68f067a2bfe69acbb4dab.png)

> 刚刚我们讲到在单条链路上，数据帧的接收是根据目的 MAC 地址。
> 
> 这里我们简单回顾下，一个节点的三种不同的地址表示。
> 
> 第一种表示是：**应用层的主机名，例如域名**；
> 
> 第二种表示是：**网络层的 IP 地址**，用于**在网络层寻址**，即路由器根据目的 IP 地址来进行转发；主机也根据目的 IP 地址是否是本机地址来判断是否接收。
> 
> 第三章表示是：**链路层的 MAC 地址**，用于在链路层寻址，即实际链路传输时，每个主机**网卡根据目的 MAC 地址是否是本机的 MAC 地址来判断是否接收**。
> 
> 在本页的例子中，标出了每个主机的 IP 地址和每个网卡的 MAC 地址。

###### 回顾：地址之间的转换

![](https://i-blog.csdnimg.cn/blog_migrate/da663fc19c454cc9a63269ff5e44dc49.png)

> 那么，我们说实际的**数据帧发送**，**需要目的主机的 IP 地址和 MAC 地址**，这就需要提供地址之间的转化服务。
> 
> 例如已知主机名，怎么查找该**主机名对应的 IP 地址**呢？这就需要用到 **DNS 域名解析服务**；
> 
> 那么如果已知目的主机的 IP 地址，怎么查找它在同一个局域网中的 MAC 地址呢？这就需要用到 **ARP 协议**。ip->MAC
> 
> 我们说 ARP 协议的主要作用是**将 IP 地址解析为其对应的 MAC 地址。**
> 
> 需要说明的是 ARP 协议**只为在同一个局域网内部的节点进行 IP 地址解析**。

##### ARP 地址地址解析协议

![](https://i-blog.csdnimg.cn/blog_migrate/112e2f48264ff9f63b09a31fb15c5c73.png)

*   _ARP **表**:_ 局域网上的每个节点 (主机、路由器) 都有这个表
    
    • 为某些局域网节点进行 IP/MAC 地址映射：
    
    **<IP address; MAC address; TTL>**
    
    •TTL (存活时间): **地址映射将被删除的时间**（通常为 20 分钟）
    

###### 两个主机位于同一个 LAN

*   1. 主机 A 希望发送数据报给主机 B
    *   B 的 MAC 地址**不在 A 的 ARP 映射表中**
*   2. 主机 A **广播 ARP 查询分组, 其中包含 B 的 IP 地址**
    *   目的 MAC 地址 = FF-FF-FF-FF-FF-FF
    *   局域网中**所有节点收到 ARP 查询分组**
*   3. 主机 B 收到 ARP 查询分组，**返回 B 的 MAC 地址给主机 A**
    *   包含有 B 的 MAC 地址的帧发送给主机 A(单播)
*   4. 主机 A 在它的 ARP 表中缓存 **IP-to-MAC** ** 地址对，** 直到信息
    *   软状态：信息超时会被删除，除非有新的更新消息
    *   ARP 是即插即用的：
        *   节点创建 ARP 表不需要网络管理员的干预

###### 发送数据报到子网以外

![](https://i-blog.csdnimg.cn/blog_migrate/e254e6596bdf53069e88d5f5799b3256.png)

*   主机 A 构建 IP 数据报，**源地址是 A 的 IP 地址，目的地址是 B 的 IP 地址**
    
*   主机 A 构建链路层数据帧，**目的 MAC 地址是路由器左边端口的 MAC 地址**
    
*   1.![](https://i-blog.csdnimg.cn/blog_migrate/63ee097a5512ae0a9b3814fb1d127736.png)
    
    > 首先主机 A 构建 IP 数据报，数据报的源 IP 地址是自己的 111.111.111.111，目的 IP 地址是主机 B 的 IP 地址 222.222.222.222。
    > 
    > 那么该数据报封装成数据帧的源 MAC 地址是自己的 MAC 地址，74-29-9C-E8-FF-55，目的 MAC 地址是谁的呢？是 B 主机的 MAC 地址，还是路由器左边端口的 MAC 地址呢？
    > 
    > 我们首先分析这个数据帧的发送路径。我们说当主机 A 判断该数据报的接收主机与自己不在同一个局域网时，它会首先将数据报发送到自己的第一跳路由器，即这里的路由器 R，
    > 
    > 因此该数据帧的目的 MAC 地址应该是 R 的左边端口的 MAC 地址，E6-E9-00-17-BB-4B。
    
*   2.![](https://i-blog.csdnimg.cn/blog_migrate/b68dc616da343b93c03fb3824cd298c4.png)
    
    > 该数据帧到达路由器 R 后，路由器 R 接收数据帧，然后抽取出数据报递交给网络层，网络层根据目的 IP 地址，判断该数据报要往右边的端口转发，且接收主机 B 与自己右边端口属于同一个局域网。
    
*   3.![](https://i-blog.csdnimg.cn/blog_migrate/6272fdcbdfce4a5424aa7fd81b1080a2.png)
    
    > 因此向右边端口输出数据帧时，数据帧的源 MAC 地址是路由器右边端口的 MAC 地址 1A-23-F9-CD-06-9B，
    > 
    > 数据帧的目的 MAC 地址是主机 B 的 MAC 地址 49-BD-D2-C7-56-2A。
    
*   这里需要强调的是，在整个传输过程中，**数据报的源 IP 地址和目的 IP 地址是不会发生改变的，改变的只是数据帧的源和目的 MAC 地址。**
    

##### 以太网

###### 以太网的帧结构

*   发送方：
    *   发送适配器**将 IP 数据报封装成以太网帧**，并传递到物理层
*   接收方：
    *   接收适配器**从物理层收到该帧，取出 IP 数据报**，并传递给网络层
*   ![](https://i-blog.csdnimg.cn/blog_migrate/8f86a880824f885aa88a6b1cd9b8f76a.png)

> 接着，我们再介绍以太网的帧结构，如图所示，包括了前同步码、目的 MAC 地址、源 MAC 地址、类型、数据和 CRC 校验码。
> 
> 注意这里 CRC 检验的数据只包含目的 MAC 地址、源 MAC 地址、类型、数据四部分。
> 
> 首先发送方的发送适配器将 IP 数据报封装成以太网帧，并传递到物理层。
> 
> _接收方的_接收适配器从物理层收到该数据帧，抽取出 IP 数据报，并传递给网络层。
> 
> Ref : https://blog.csdn.net/eliot_shao/article/details/123473548

*   前同步码 (8 字节)![](https://i-blog.csdnimg.cn/blog_migrate/5aa0bbea9ea73e3a5183da19b3aea742.png)
    
    *   > 前同步码有 8 个字节，其中前 7 字节是 “10101010”，最后一个字节是 “10101011”。
        > 
        > 前同步码的作用是使接收方和发送方的时钟同步，接收方一旦**收到连续的 8 字节**前同步码，可确定有帧传过来。
        > 
        > 注意：前同步码是 “无效信号”，接收方收到后删除，不向上层传。CRC 的校验范围不包括前同步码。
        
*   源、目的 MAC 地址 (各 6 字节)![](https://i-blog.csdnimg.cn/blog_migrate/64ab6a90049de13f6e4b3cd72249e5dc.png)
    
    > 然后是各 6 个字节的源、目的 MAC 地址。
    > 
    > 如果主机 A 向主机 B 发送一个 IP 数据报，**主机 B 只接收目的 MAC 地址与自己 MAC 地址匹配的数据帧或广播地址的数据帧**，并将数据字段的内容传递给网络层。否则，丢弃该帧。
    
*   类型字段 (2 字节)
    
    *   ![](https://i-blog.csdnimg.cn/blog_migrate/c862a3dde8f8d758eaa2e8e2f99f146f.png)
        
    *   > 帧结构中类型字段主要是支持以太网中的多种[网络层协议](https://so.csdn.net/so/search?q=%E7%BD%91%E7%BB%9C%E5%B1%82%E5%8D%8F%E8%AE%AE&spm=1001.2101.3001.7020)的**复用的**，绝大多数这里的类型是指 IP 协议。
        
*   数据字段 (46-1500 字节)![](https://i-blog.csdnimg.cn/blog_migrate/d1c9205ffa9ca33b108cfe3e83a6b92a.png)
    
    *   然后是数据字段，_以太网的最大传输单元 **MTU** 是 **1500** 字节，最小长度是 **46** 字节。_
*   循环冗余检测 CRC(4 字节)![](https://i-blog.csdnimg.cn/blog_migrate/2d5eed342817fa4bfbd3dd180135e61f.png)
    
    帧结构的尾部**是 CRC 循环冗余校验码**。它主要是用于帮助接收主机检测数据帧中是否出现比特差错。
    

###### 不可靠的无连接服务

*   以太网向网络层提供的服务：
    *   无连接服务：
        *   通信时，**发送方适配器不需要先和接收方适配器 "握手"**
    *   不可靠服务：
        *   接收到的帧可能含有比特差错
            *   收到正确帧，不发确认帧
            *   收到出错帧，丢弃该帧，**不发否定帧**
            *   发送适配器不会重发出错帧
            *   丢弃数据的恢复是通过终端**传输层的可靠数据传输机制**来实现的

##### 交换机 SWITCH

###### 集线器 HUB

**集线器连接下的网络虽然物理上看起来不是总线型的，但是逻辑上仍然是总线型的**

**使用 CSMA/CD 协议**

**集线器只工作在物理层，他的每个接口只是简单的转发比特流，碰撞检测的事情交给各站的网卡进行检测**

![](https://i-blog.csdnimg.cn/blog_migrate/6773976f626e891ef9ed8eb091bb45f9.png)

使用集线器 HUB 在物理层扩展以太网

![](https://i-blog.csdnimg.cn/blog_migrate/98c79f6ed5435016324ed273a736b2a0.png)

**使用集线器连接的以太网就相当于是把总线型的给画的好看了点，其实没什么区别，用的还是 CSMA/CD 协议调节各个主机使用总线，还要进行碰撞检测，只能工作在半双工模式，收发帧不能同时进行**

###### 交换机与集线器的对比

###### 比较

![](https://i-blog.csdnimg.cn/blog_migrate/84300bd62edbe8abe78539b2e542dfc7.png)

![](https://i-blog.csdnimg.cn/blog_migrate/f14b6f7f0f6e3150f785dc8e23ae948c.png)

**某站的信号经过集线器会转发给广播域中的所有其他主机，但是经过交换机之后，由于内部的设置会只转发给目的主机**

![](https://i-blog.csdnimg.cn/blog_migrate/cf1c733a5d55a0f670ef9799b8b63dc2.png)

**以太网交换机有多个接口，一般工作在全双工模式，不用考虑碰撞检测 (不适用 CSMA/CD 协议)，能同时连通多个接口!**

![](https://i-blog.csdnimg.cn/blog_migrate/1784f71c403fca0c7274f92d6865d15f.png)

**关于碰撞的问题，集线器收到帧之后就会直接向其他所有主机进行传递，这里必然就会涉及到碰撞的问题**

**而交换机，当交换机收到多个帧的时候，为了避免碰撞会将这些帧进行缓存，之后再进行逐一转发，避免了碰撞的问题**

###### 交换机和集线器扩展以太网

单播帧

![](https://i-blog.csdnimg.cn/blog_migrate/44ca766a96fdc930aad9a952b51ebd31.png)

广播帧

![](https://i-blog.csdnimg.cn/blog_migrate/1a1075d2d735c94a16e3f97531887125.png)

可能发生碰撞的时候

![](https://i-blog.csdnimg.cn/blog_migrate/f13081b80f09b699f86b944256c43718.png)

**因此，集线器扩大了广播域，也扩大了碰撞域；而交换机扩大了广播域，但是隔离了碰撞域!!!**

![](https://i-blog.csdnimg.cn/blog_migrate/0748a33c6ea2006a1b3e44fffaaadd2c.png)

*   **链路层设备**
    *   **存储转发数据帧**
    *   **检查**到达的数据帧的 MAC 地址，有选择的转发数据帧到一个或多个输出链路，当数据帧被转发到一个共享网段时，使用 CSMA/CD 来访问共享链路
*   **透明**
    *   主机不关心是否存在交换机
*   **即插即用和自学习**
    *   交换机不需要手工配置
*   ![](https://i-blog.csdnimg.cn/blog_migrate/08225f02834df6f99d84ceb3d3722b01.png)

> 本次课将详细讲解链路层交换机的工作原理。
> 
> 首先交换机是属于链路层的设备，其主要作用是**存储转发数据帧。**
> 
> 对于到达交换机的数据帧，交换机首先**检查其目的 MAC 地址**，然后根据 MAC 地址，有选择的将数据帧转发到一个或多个输出链路；
> 
> 如果输出链路是一个共享网段，将使用 CSMA/CD 来访问共享链路。我们这里给出了一幅图来帮助大家理解后面这句话。
> 
> 在这幅图中 A、B、C 主机和交换机的端口 1 由集线器设备互连，因此这里 **，A、B、C 主机和交换机的端口 1 构成了一个共享网段 **，在共享链路发送数据帧，需要用到我们前面学习到的以太网链路访问协议 CSMA/CD。
> 
> 链路层交换机的第二个特点是透明，这里的透明是指的当一个主机向另一个主机发送数据帧时，它并不会知道某个交换机会收到这个数据帧，并将其转发到另一个节点。
> 
> 交换机的第三个特点是即插即用和自学习，也就是说交换机是不需要手工配置的，插上就可以用。

###### 支持多节点同时传输

![](https://i-blog.csdnimg.cn/blog_migrate/1598caff903f087b87a28e448aa67c1f.png)

> 在组网时加入交换机后，可以支持多个节点同时传输数据帧。
> 
> 可以看上边这幅图，每个主机由单独的链路与交换机端口相连，因此交换机每个端口对应的链路和主机是一个**独立的碰撞域。**
> 
> 又因为交换机对于收到的数据帧可进行缓存，因此在上边这幅图中，A 主机和 B 主机同时发送数据帧，将不会发生冲突。
> 
> 因此交换机具有较高的转发率。

###### 以太网自学习和转发帧的过程 (自学习算法)

###### 考虑以下过程

![](https://i-blog.csdnimg.cn/blog_migrate/147a2cc93b49482558e132fdf6b4c6c1.png)

A 给 B 发送帧，进入交换机 1 中，先进行登记，记录 A 的 MAC 地址和接口 1，然后找 B 的 MAC 地址，找不到，进行广播，接口 3 的 B 收到发现正确，接受；C 则丢弃；到 4 号接口转发到交换机 2 中，帧进入交换机 2 中先进行登记，登记 A 的 MAC 地址和接口 2，然后进行广播，都不匹配，则都丢弃，这就是一个完整的过程

后面还有两个过程

![](https://i-blog.csdnimg.cn/blog_migrate/0c47c872b918abab4be3747a46e406f3.png)

丢弃帧的例子

![](https://i-blog.csdnimg.cn/blog_migrate/585af44a9c646cb7cd4f919226abc66b.png)

**G 发送帧到 A，这个帧在分岔口两边传播，给 A 并接受，到交换机的时候先进行登记，然后查找，发现 A 1，意思是从哪里来回哪里去，这显然是没必要转发的，于是丢弃**

每条记录的持续时间不是永久的，会定期删除!!!

![](https://i-blog.csdnimg.cn/blog_migrate/801bd9b7f1709a8e799588323cb8d59b.png)

###### 转发表

![](https://i-blog.csdnimg.cn/blog_migrate/0b4c3ddf26243ecaddf652b156c1047f.png)

> 在右边这幅图中，我们有这样一个疑问：交换机是怎么知道 A’可通过端口 4 达到， B’ 可通过端口 5 到达呢？
> 
> **这是因为每个交换机都有一个转发表，其中的条目如下：**(主机的 MAC 地址，到达主机的端口，时戳)；
> 
> 因此当数据帧的目的 MAC 地址为 A’主机时，我们通过查找相应的转发表项，就可获知该主机对应的端口。
> 
> 那么转发表中的条目是怎么建立的呢？我们说是通过自学习机制。

*   自学习![](https://i-blog.csdnimg.cn/blog_migrate/ed7998652e1b173c2a913b3a0491e3bf.png)

> 下面我们就来介绍交换机的自学习机制。非常简单。
> 
> 每当交换机收到一个数据帧时，交换机会学习发送主机的位置：**及进入的局域网段和到达端口，并在转发表中进行记录**。
> 
> 如这里的例子所示，主机 A 发送数据帧给主机 A’，数据帧到达交换机时，交换机会在转发表中记录这样一个表项：
> 
> A 主机的 MAC 地址和可达端口号 1，生存时间 60 秒，这就表示通过端口 1 可达到 A 主机。

###### 数据帧的过滤 / 转发

*   ![](https://i-blog.csdnimg.cn/blog_migrate/374844028292eb209887829ab47bf563.png)

> 下面我们来学习交换机收到数据帧后，是如何进行查表转发的。我们说根据如下规则：
> 
> 首先，**记录**到达链路和发送主机的 MAC 地址；
> 
> 第二步，使用数据帧的目的 MAC 地址，在转**发表中进行检索：**
> 
> 如果在转发表条目中找到对应的 MAC 地址，则执行：
> 
> 如果**目的 MAC 地址对应的端口**与**数据帧的到达端口相同**，说明接收主机属于同一个共享网段，则**直接将该数据帧丢弃**。**因为接收主机也会收到该数据帧**；
> 
> 否则，转发端口与达到端口不一致，则将该数据帧转发到指定端口
> 
> 如果数据帧的目的 MAC 地址在转发表中没有找到，则交换机将该数据帧向除到达端口之外的所有端口转发，也就是**泛洪**。

###### 自学习 / 转发例子![](https://i-blog.csdnimg.cn/blog_migrate/346b19b3bb0e430d46e427306a17fb7b.png)

> 我们来看一下自学习和转发的例子。
> 
> 这里初始时转发表是空的，当主机 A 发送数据帧给主机 A’时，该数据帧到达交换机，
> 
> 交换机首先根据收到数据帧的端口和该数据帧的源 MAC 地址，建立表项 1，说明通过端口 1 可达到主机 A，这就是自学习。
> 
> 然后交换机根据数据帧的目的 MAC 地址（A’）在转发表中查找，由于没有找到 A’的 MAC 地址，则交换机将该数据帧向除 1 端口之外的所有端口转发，
> 
> 只有 A’主机会发现数据帧的目的 MAC 地址与自己的 MAC 地址一致，接收该数据帧。
> 
> 注意，这里主机 A’发往主机 A 的数据帧，在到达交换机后，由于转发表中已经有目的 MAC 地址对应的表项，因此交换机将只向 1 端口转发数据帧。

###### 交换机互连

*   ![](https://i-blog.csdnimg.cn/blog_migrate/b8c5fb066cbb54cbd645fc8e32a04643.png)

> 交换机也可以进行**互连，以组成更大的局域网**。如本页的图所示。
> 
> 那么请大家思考，这样一个问题，如果主机 A 发送数据帧给主机 G, 那么交换机 S1 是怎么知道需要先把数据转发到 S4 和 S3 的？
> 
> 我们说仍然是通过自学习。当数据帧到达 S1 时，可能 S1 的转发表中没有 G 主机的 MAC 地址的表项，于是 S1 将该数据帧泛洪，那么 S4 的端口也会收到这个泛洪的数据帧，
> 
> 如果 S4 的转发表也没有 G 主机的 MAC 地址对应的表项，则 S4 会继续泛洪，于是 S3 也会收到数据帧，如果 S3 的转发表仍然没有 G 主机的 MAC 地址，则 S3 会继续向它的端口泛洪，直到数据帧到达 G 主机。

*   多个交换机自学习的例子![](https://i-blog.csdnimg.cn/blog_migrate/e9d919d12155129555e987ab1eef7802.png)

###### 交换机交换特点和方式

*   特点
    
    *   识别目的 MAC 地址，根据交换表进行端口选择
    *   识别源 MAC 地址更新交换表
*   在识别目的 MAC 地址和源 MAC 地址的过程中是否需要接收并缓存完整的帧呢？
    
    *   在识别目的 MAC 地址和源 MAC 地址的过程中，**需要接收并缓存完整的帧**。这是因为 MAC 地址是位于数据链路层的帧的**头部**中，用于标识帧的源和目的设备。
        
        当一个设备接收到一个帧时，它需要读取帧头部中的目的 MAC 地址和源 MAC 地址字段来确定该帧是否是针对自己的，或者是从哪个设备发送过来的。因此，设备需要接收并缓存完整的帧，以便能够从帧头部提取 MAC 地址信息进行判断和处理
        
*   交换方式
    
    *   **存储转发** (缓存整个帧后再转发)
    *   **快速分组** (直通交换)
        *   识别出目的地址直接转发

存储转发交换方式

![](https://i-blog.csdnimg.cn/blog_migrate/ed10231df556eedc78d89775bca1c07c.png)

快速分组交换方式

![](https://i-blog.csdnimg.cn/blog_migrate/dafd57b75b580e8de9b812ffadf2427f.png)

> 存储转发：具有**差错检测**功能，**转发时延较大**，适用于**出错率高的链路**。
> 
> 快速分组又称直通交换：**不具有差错检测功能，转发时延较小**，适用于时延要求高，**出错率低的链路**。

###### 路由器和交换机的区别

![](https://i-blog.csdnimg.cn/blog_migrate/ac5d2e117e973268ceb737600b29dc53.png)

> 最后我们将交换机和路由器进行简单的对比。
> 
> 首先，路由器和交换机都是存储转发设备（中转设备），其中路由器是**网络层设备**，交换机是**链路层设备**。
> 
> 第二，路由器和交换机**都需要维护转发表**，其中路由器使用**路由算法**来计算转发表，**基于 IP 地址转发**；而交换机是通过泛**洪和自学习**来建立转发表，**基于 MAC 地址**进行数据帧转发。

##### 虚拟局域网 VLAN

以太网的规模变大之后，随之而来的就是广播域不断增大，广播会导致很大的弊端!!!

![](https://i-blog.csdnimg.cn/blog_migrate/cfcee0e6ff117f5cd0056ff5d8140755.png)

如何缓解这个问题呢？

可以使用路由器，** 路由器默认情况下不对广播数据包进行转发，** 所以可以隔绝广播域

![](https://i-blog.csdnimg.cn/blog_migrate/18a2ee36b145c3dc9ac5ae78e43ada65.png)

但是路由器成本太高了，在局域网内部尽量少的使用路由器，所以这个时候可以用虚拟局域网 VLAN 技术

![](https://i-blog.csdnimg.cn/blog_migrate/0605a3139e48c75ac161a7a9f436a0e4.png)

**同一个 VLAN 内部可以广播通信，不同的不可以广播通信；VLAN 的划分与物理位置无关，只与人为的设置相关!!!**

### 第六章 网络编程

考试只会考写步骤，代码在实验里就写过了

#### 步骤

![](https://i-blog.csdnimg.cn/blog_migrate/ea8457c6196969a4b9fd0fa85261de7f.png)

![](https://i-blog.csdnimg.cn/blog_migrate/8891b053d679a4f53e61fb2099ffe7a9.png)

123