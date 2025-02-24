> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/m0_61588837/article/details/132694345?spm=1001.2014.3001.5506)

计网期末复习
------

### 第一章 计算机网络和因特网

#### 计算机网络的两大功能

*   连通性
*   共享性

#### Internet 具体构成

*   数以亿计的**计算互连设备、通信链路、分组交换**：路由器和交换机
    
    *   主机（host）= 端系统（end system）
        
        *   运行网络应用程序
    *   通信链路 (link)
        
        双绞线, 光纤, 无线电频谱, 卫星  
        传输速率 = 带宽 (bandwidth)  
        带宽单位为 bps，bit per second 每秒传输的位数，例如百兆光纤就是 100M bps，实际传输使用的单位为 BPS，1B=8bit，所以传输速率为 100/8=12.5M BPS，就是 12.5MB/s  
        通信链路只有极限带宽，例如 5 号线极限带宽为 300M，不能接千兆网卡，只能接百兆网卡  
        网卡有传输的能力，传输速率取决于网卡能传输的能力，例如百兆，千兆网卡
        
    *   分组 (packet) 交换：
        
        *   路由器（Router）和交换机（Switch）
        *   交换机：形成局域网
        *   路由器：链接互联网

#### 协议的基本要素

*   **语法：语法即是用户数据与控制信息的结构和格式**
*   **语义：语义即是需要发出控制信息，以及完成的动作与做出的响应**
*   **同步：同步可以理解为时序，即是对事件实现顺序的详细说明**

#### 网络核心

![](https://i-blog.csdnimg.cn/blog_migrate/612e3d1199d3c2a34d17186730d0f3e2.png)

##### 电路交换

电话交换机接通电话线的方式成为电路交换

![](https://i-blog.csdnimg.cn/blog_migrate/840400517ace873478593f5f0f64314c.png)

*   **每次会话预留沿其路径（线路）所需的独占资源**－－电话网
    
*   从主机 A 到主机 B 经一个电路交换网络需要多长时间发送一个 640Kb 的文件?  
    **电路交换的频分和时分**
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/71312a7c1d0265edb92be24cb488284c.png)
    
    举例子：
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/102b29cb0bb12904855a1671e10d546a.png)
    
*   过程
    
    *   建立连接
        *   会一直霸占着这条路径
    *   传输数据
    *   释放连接
*   优点
    
    *   传输速度快、高效
    *   实时
*   缺点
    
    *   资源利用率低
    *   新建连接需要占据一定的时间，甚至比通话的时间还长

##### 多路复用

*   数据以离散的数据块通过网络来发送
    *   分片分配到会话
    *   分片没有被会话使用的情况下，分片空载 (不共享)
    *   电路级性能（有保证）
    *   要求呼叫建立－－建立一个专门的端到端线路 (意味着每个链路上预留一个线路)
    *   链路带宽分片

频分－frequency division

时分－time division

##### 分组交换

![](https://i-blog.csdnimg.cn/blog_migrate/53e61cc51a54e45082e7359ff2d8d9b1.png)

每个端到端的**数据流被划分成分组**

*   **所有分组共享网络资源**
    
*   **每个分组使用全部链路带宽**
    
*   资源按需使用
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/b91feab6444fee19a8d584260e1a3f9c.png)
    

##### 报文交换

![](https://i-blog.csdnimg.cn/blog_migrate/b09a84d9cb60f0aeda22cf5fd929881f.png)

#### 比较分组交换与电路交换：

![](https://i-blog.csdnimg.cn/blog_migrate/5561b89c539870209c4249e89c636f26.png)

*   分组交换允许更多的用户使用网络
*   ![](https://i-blog.csdnimg.cn/blog_migrate/569e39e1e2b5261306e1ead38849cf66.png)
*   **优势**
    *   **适合大量的突发数据传输**
    *   **资源共享**
    *   **简单，无需连接**
*   **缺点**
    *   **过渡竞争导致分组延迟与丢失**
    *   **需要可靠数据传输、拥塞控制协议**

#### 网络的分类

![](https://i-blog.csdnimg.cn/blog_migrate/a70d7f722f77c47342f4722f773d9b0c.png)

*   图中 FDM 频分，TDM 时分
*   **虚电路网络一定是面向连接的**
*   数据报网络既可以提供面向连接的服务，也可以提供无连接的服务

#### 四种时延

1. 节点**处理时延** nodal processing delay：分析地址部分，进行差错检验等花费的时间

​ 检查错误位  
​ 选择输出链路  
​ 高速路由器处理延迟－微秒级

2.  **排队时延** queueing delay：在进入路由器之后等待处理的时间

等待被发送到输出链路上的时间  
取决于路由器的拥塞程度

3.  **传输 (发送) 时延** Transmission delay：**从发送分组的第一个 bit 算起，到该分组最后一个分组被发送完毕需要的时间**

R = 链路带宽 (bps)  
L = 分组长度 (bits)  
发送分组比特流的时间 = L/R

4.  **传播时延** Propagation delay：一个比特从链路一端传播到另一端所需要的时间

d = 物理链路的长度  
s = 介质的信号传播速度 (~2x108 m/sec)  
传播延迟 = d/s  
注意: s 和 R 是两个完全不同的速度参量!

![](https://i-blog.csdnimg.cn/blog_migrate/ff4eb93fb83ac6ddce6c9e860e51774c.png)

##### 总的节点延迟

![](https://i-blog.csdnimg.cn/blog_migrate/eeb1c9f61d00a49433611f2d41e400a7.png)

*   dproc = **处理时延**
*   dqueue = **排队时延**
*   dtrans = **传输时延**
*   dprop = **传播时延**

![](https://i-blog.csdnimg.cn/blog_migrate/b2290fcd6e673a0e3fd0b097eecb5a7e.png)

#### 流量强度

##### 流量强度 = 分组长度 L * 平均分组到达率 a / 链路带宽 R

L = 分组长度 (bits)

a = 平均分组到达率 average packet arrival rate

R = 链路带宽 (bps)

**流量强度：traffic intensity = La/R**

##### 流量强度与延迟关系

La/R ~ 0: 分组稀疏到达, 无队列, 平均排队延迟极小接近于 0

La/R -> 1: 分组猝发到达, 形成队列, 队列长度迅速增加, 排队延迟大幅增大

La/R > 1: 输出队列平均位到达速率超过送走这些位的极限速率，输出队列持续增长，排队延迟趋于无穷大

![](https://i-blog.csdnimg.cn/blog_migrate/034a710204385747af5680929097052a.png)

#### 分组丢失

*   **路由器输入链路和输出链路的缓冲区容量有限**
*   当分组到达路由器**输入链路**发现缓冲区已满，则路由器只好丢弃分组
*   当分组在路由器内部要**转发到输出链路**时发现输出缓冲区队列已满，路由器只好丢弃分组
*   丢失的分组可能被前路由节点、源节点重传，或不重传
*   丢包率或分组丢失率（packet loss rate/ratio）

#### 吞吐量

*   网络吞吐量：
    
    *   单位时间内**整个网络传输数据的速率或分组数**
*   bps 或 data packets per second
    
*   吞吐量: 接收端接收到数据的比特速率 (bps)
    
    瞬时吞吐量: 某一瞬间的吞吐量
    
    平均吞吐量: 一段时间内的吞吐量均值
    

#### 各层次常用协议

##### 分层次结构

![](https://i-blog.csdnimg.cn/blog_migrate/e0a0523631316e1cf42cf8d4a3d1b8f4.png)

##### 应用层

*   支持网络应用，报文传
*   FTP, SMTP, STTP

##### 传输层

*   主机**进程间**的数据段传送
*   TCP、UDP

##### 网络层

*   **主机与主机**间分组传送
    
*   IP、路由协议
    

##### 链路层

*   **相邻网络节点间**的数据帧传送
*   PPP(Point to Point Protocol)，Ethernet

##### 物理层

*   物理介质上的比特传送
    
*   对等实体:
    
    *   两台计算机上**同一层**所属的程序、进程或实体称为该层的对等程序、对等进程或对等实体

![](https://i-blog.csdnimg.cn/blog_migrate/ff95bf55bd86ae59ab535a1209e44f7e.png)

### 第二章 应用层

![](https://i-blog.csdnimg.cn/blog_migrate/432f1a49a602c99e6302f9a450d83cde.png)

#### 两种方式

##### 客户机 / 服务器 CS

![](https://i-blog.csdnimg.cn/blog_migrate/60069886acc56bdeb1d833575e969c00.png)

*   服务器：具有客户资源
    
    *   **总是打开的主机**
    *   具有固定的、**众所周知的 IP 地址**
    *   主机群集常被用于创建强大的虚拟服务器
*   客户机：发出请求和接受数据
    
    *   同服务器端通信
    *   可以间断的同服务器连接
    *   可以拥有**动态 IP 地址**
    *   **客户机相互之间不直接通信**

##### 对等 P2P 方式

![](https://i-blog.csdnimg.cn/blog_migrate/f18abf95c6ed83208ea5c85a30f04ce4.png)

#### 进程通信

*   进程
    *   运行在端系统中的程序
*   同一主机上的两个进程通过**内部进程通信机制**进行通信
*   不同主机上的进程通过**交换报文**相互通信

#### 套接字

*   又叫应用程序编程接口 API

#### 进程寻址

**IP 地址 + 端口号**

#### web 应用和 HTTP

*   网页
    
*   由许多**对象**组成
    
*   **对象就是文件**，可以是 HTML 文件, JPEG 图像, Java applet, 音频文件…
    
    **多数网页由单个基本 HTML 文件和若干个所引用的对象构成**
    
    *   每一个对象对应一个 URL

##### 使用 TCP：HTTP 属于应用层，使用运输层的 TCP 进行传输

*   1 客户初始化一个与 **HTTP 服务器 80 端口**的 TCP 连接 (创建套接字)
*   2 HTTP 服务器接受来自客户的 TCP 连接请求, 建立连接
*   3 Browser (HTTP client) 和 Web 服务器 (HTTP server) **交换 HTTP 消息** (应用层协议消息) 包括 **HTTP 请求和响应消息**
*   4 最后结束，关闭 TCP 连接

##### HTTP 是无状态协议

*   **HTTP 服务器不维护客户先前的状态信息**

![](https://i-blog.csdnimg.cn/blog_migrate/14b62d8485c4e209d8f4b20983cc2f21.png)

##### 非持久的 HTTP 连接

![](https://i-blog.csdnimg.cn/blog_migrate/d5086e95e2fa23ae64f84423ab63d123.png)

*   **每个 TCP 连接上只传送一个对象**，下载多个对象需要建立多个 TCP 连接
    
*   **HTTP/1.0 使用非持久 HTTP 连接**
    
*   **非持久 HTTP** 详解
    
    *   假设用户输入 URL http://www.someSchool.edu/someDepartment/home.index 网页由 1 个 HTML 文件, 和 10 个 jpeg 图像构成
    *   解析步骤
    
    > 第一步：**建立 TCP 连接**
    > 
    > *   HTTP 客户机初始化与服务器主机中 HTTP 服务器的 **TCP 连接**
    > *   服务器中 HTTP 服务器**在 80 端口监听 TCP 连接**。收到请求连接，接受，建立连接，通知客户
    > 
    > 第二步：**HTTP 请求连接**
    > 
    > *   HTTP 客户发送 HTTP 请求消息，包含 **URL 到 TCP 连接套接字**，消息指出客户所需要的 **web 对象**
    > *   HTTP 服务器接受请求消息，产生一个**响应消息**，包含**被请求对象**，并发送这个消息到自身 TCP 连接套接字
    > *   **HTTP 服务器结束 TCP 连接**
    > *   HTTP **客户接收**包含 html 文件的响应消息, 显示 html. **解析** html 文件，**找出 10 个 jpeg 对象**
    > *   **对十个引用对象重复上述操作 (图中是并行的，串行的话需要一个一个按顺序来)**
    
*   在非持久 HTTP 连接下，上述只需 4RTT
    
*   **由于是非持续的，那么建立一次连接服务器返回消息之后就会断开连接，所以先建立 TCP 连接，发 HTTP 请求得到 HTML 对象，然后解析得到 10 个图片对象然后重新建立连接发送请求就得到对象了**
    
*   ![](https://i-blog.csdnimg.cn/blog_migrate/c59765431a1ed4d474e8ba5926bc0051.png)
    
*   缺点：
    
    *   每个对象都需要 2 个 RTT
    *   OS 必须为每个 TCP 连接分配主机资源
    *   **大量客户的并发 TCP 连接形成服务器的严重负担**

##### 持久 HTTP 连接

![](https://i-blog.csdnimg.cn/blog_migrate/f30346ca95f501ec8dc7ab7707b277bc.png)

*   **一个 TCP 连接上可以传送多个对象，也就是说建立 TCP 连接之后正常状态下等整个过程结束之后再断开，保证了持续性**
    
*   HTTP/1.1 默认使用持久 HTTP 连接
    
*   例题
    
*   ![](https://i-blog.csdnimg.cn/blog_migrate/2c85122e56c4a02f36e9f6775a085e6d.png)
    

###### **不带流水线** (可以理解为串行) 的持久 HTTP 连接

*   客户先前响应消息收到, 才发出新的请求消息
*   每个引用对象经历 1 个 RTT
*   ![](https://i-blog.csdnimg.cn/blog_migrate/51d4b34b494701d54e1445b79c393b2a.png)

###### 带流水线 (可以理解为并行) 的持久 HTTP 连接：最优解决方案

*   **HTTP/1.1 默认使用**
*   客户遇到 1 个引用对象就发送请求消息
*   所有**引用对象只经历 1 个 RTT**
*   ![](https://i-blog.csdnimg.cn/blog_migrate/d5c5f1bd4b3776433b9647fe5ff445e5.png)

例子：

![](https://i-blog.csdnimg.cn/blog_migrate/32e5509a5958b22b65beba025460eca8.png)

![](https://i-blog.csdnimg.cn/blog_migrate/dbbbd33505e06e36c94b09ede4f73dff.png)

##### 响应时间模型

*   定义往返时间 RTT(Round-Trip Time):
    *   响应时间:
        *   **1 个 RTT 用于建立 TCP 连接**
        *   **1 个 RTT 用于 HTTP 请求 / 响应消息的交互**
        *   **Html 文件传输时间**
    *   **total = 2RTT+transmit time**
*   ![](https://i-blog.csdnimg.cn/blog_migrate/81f81feea0f30624e939411a8a1771e9.png)

#### HTTP 报文格式

*   请求报文（ASCII 文本：易于人读的格式）![](https://i-blog.csdnimg.cn/blog_migrate/e36fc86eacfb811e01e5153481be1c8b.png)

![](https://i-blog.csdnimg.cn/blog_migrate/c73668035eb8368cfebbce616778980a.png)

*   响应报文![](https://i-blog.csdnimg.cn/blog_migrate/c3a4c084ae3a8e18b7a99cbdc7023330.png)

#### cookie：跟踪用户

**cookie 文件是存在用户本地的!!!**

**把无状态的 Http 协议进行状态化!!!**

![](https://i-blog.csdnimg.cn/blog_migrate/3b20cd4b298a826063f0440137ef0898.png)

![](https://i-blog.csdnimg.cn/blog_migrate/91a7f12ec1251c390227b784d2ac3dd0.png)

![](https://i-blog.csdnimg.cn/blog_migrate/0a94a7f922f8858372211eabe77ac06e.png)

Cookie 和隐私：

*   Cookies 允许网站更加了解你
*   你可以给网站提供名字和 e-mail 给网站
*   广告公司通过网站获得信息
*   **Cookies 不适合游动用户**

#### web 缓存 (代理服务器)

![](https://i-blog.csdnimg.cn/blog_migrate/665084fa4d3106eced82be7c09e54cf9.png)

![](https://i-blog.csdnimg.cn/blog_migrate/87e90a81dcff8ea2490ecc8daa72face.png)

*   **目标：在不访问服务器的情况下满足用户的 Http 请求** (代表**起始服务器**满足 HTTP 请求)
*   一般有 ISP(Internet 服务提供商) 架设

**所有 HTTP 请求指向缓存**

*   **对象在缓存中：缓存器返回对象**
*   **否则缓存器向起始服务器发出请求，接收对象后转发给客户机**
*   这么看起来缓存既可以充当服务器给用户返回对象，也可以充当客户端向起始服务器发送请求

**为什么需要 Web 服务器？**

*   减少对客户机请求的响应时间
*   减少内部网络与接入链路上的通信量
*   能从整体上大大降低因特网上的 Web 流量

**条件 get 方法**

**确认缓存服务器中的对象是否为最新的**，使用户拿到最新的数据

![](https://i-blog.csdnimg.cn/blog_migrate/5568325b82fd6fdf66c9cf80d1a7511f.png)

web 缓存中的文件都有两个附加字段，last-modified 最近一次修改时间和 expires 有效时间

先请求 web 缓存，如果未过期，则直接返回；如果过期了，则请求原始服务器

![](https://i-blog.csdnimg.cn/blog_migrate/2f4fe952038d73b17cc52f0a07d3602e.png)

**web 缓存向原始服务器发送请求，包含 last-modified，和自己的进行比对，如果相同则返回 304 not modified，不附加其他数据，如果已更改，就返回已更改的对象**

![](https://i-blog.csdnimg.cn/blog_migrate/c30bd4999f8eff123e359897eb328b55.png)

#### FTP 文件传送协议

##### 概念

![](https://i-blog.csdnimg.cn/blog_migrate/2b1f68d21e90b5513fbbb52c928b2152.png)

##### 主动模式

![](https://i-blog.csdnimg.cn/blog_migrate/fa11cd9563042f79e79ec6283fd77826.png)

##### 被动模式

![](https://i-blog.csdnimg.cn/blog_migrate/8cb4a627b6ca31123295b61ea7b2bf23.png)

##### 例题

#### 电子邮件

![](https://i-blog.csdnimg.cn/blog_migrate/015f52b4d49f3af3f593da3736ab2271.png)

结构：**用户代理，邮件服务器，电子邮件使用的协议**

![](https://i-blog.csdnimg.cn/blog_migrate/adfc4f68abb5d63d80fddc3104466e86.png)

![](https://i-blog.csdnimg.cn/blog_migrate/9ad8d699fcaa00667748d0e59123c5b5.png)

过程

![](https://i-blog.csdnimg.cn/blog_migrate/029fd00b68a8d9d259c434b9746335ba.png)

**用户代理 (UA)：**

用户和电子邮件系统的接口

用户代理向用户提供一个很友好的接口来发送和接收邮件，用户代理至少应当具有**撰写、显示和邮件处理**的功能

**邮件服务器：**

它的功能是发送和接收邮件，同时还要向发信人报告邮件传送的情况（已交付、被拒绝、丢失等）。

**邮件服务器采用客户 / 服务器方式工作，**

但它必须能够同时充当客户和服务器邮件发送协议和读取协议∶

邮件发送协议用于用户代理向邮件服务器发送邮件或在邮件服务器之间发送邮件，如 SMTP;

邮件读取协议用于用户代理从邮件服务器读取邮件，如 POP3

**SMTP 用的是 “推”（Push）的通信方式，POP3 用的是 “拉”（Pull）的通信方式**

**电子邮件格式与 MIME**

![](https://i-blog.csdnimg.cn/blog_migrate/727c452e71488e40801db5e25a3a9d4a.png)

**多用途网际邮件扩充（MIME）**

**由于 SMTP 只能传送一定长度的 ASCII 码，**

许多其他**非英语国家的文字**（如中文、俄文，甚至带重音符号的法文或德文）就**无法传送**，

且**无法传送可执行文件及其他二进制对象**，因此提出了用途网络邮件扩充

##### SMTP

*   Simple Mail Transfer Protocol
    
*   客户使用 **TCP** 来可靠传输邮件消息到 **服务器端口号 25**
    
*   **直接传送**: 发送服务器到接收服务器
    
*   传输的 3 个阶段
    
    *   **连接建立 (握手)**
        
        发件人的邮件发送到发送方的邮件服务器的缓存中；
        
        SMTP 客户每隔一段时间就对邮件缓存扫描一次，如果发现有邮件就建立 TCP 连接
        
    *   **邮件消息的传输**
        
        连接建立后，就可开始传送邮件。邮件的传送从 **MAIL 命令开始**，MAIL 命令后面有发件人的地址
        
    *   **连接释放 (结束)**
        
        邮件发送完毕后，SMTP 客户应**发送 QUIT 命令**。
        
        SMTP 服务器**返回的信息是 221（服务关闭）**，表示 SMTP 同意释放 TCP 连接
        
*   命令 / 应答的交互
    
    *   命令: ASCII 文本格式
    *   应答: 状态码及其短语
*   ![](https://i-blog.csdnimg.cn/blog_migrate/66cff415d00c371e690a8fa26690337c.png)
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/0c3043dbb05cd1054b89892ab63b6d91.png)
    
*   SMTP 总结
    
    *   SMTP 使用**持久连接**
    *   SMTP 要求邮件消息 (header & body) 必须是 **7-bit ASCII**、
*   SMTP 与 HTTP 的比较
    
    *   HTTP：**拉**协议
        
    *   SMTP：**推**协议
        
    *   HTTP: **每个对象封装在它各自的 HTTP 响应消息中发送**
        
    *   SMTP: **一个邮件内各个对象置于同一个邮件消息的多目部分发送**
        
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/28741786b42572d1a6498b8fa20e8f15.png)
    

###### Mime

**SMTP 协议只能传送 7bit 的 ASCII 码文本数据，不能传可执行文件或者其他二进制对象**

**SMTP 不能传送多媒体文件，以及其他非英语国家的文字**

**解决不能传送 ASCII 码数据的问题，一处多用途因特网邮件扩展 MIME**

**MIME 还可以用于面向非 ASCII 码的 Http！！！**

![](https://i-blog.csdnimg.cn/blog_migrate/c7d60faca90a102db86a8c45021edcfe.png)

##### POP3 和 IMAP

POP 也使用**客户 / 服务器的工作方式，在传输层使用 TCP，端口号为 110**。**IMAP4 使用 143**

POP 有两种工作方式：

**“下载并保留 "和" 下载并删除”**

![](https://i-blog.csdnimg.cn/blog_migrate/db371611e6c53f127d437774fcf973ab.png)

##### 基于万维网的电子邮件

![](https://i-blog.csdnimg.cn/blog_migrate/9018168df58259fb0481df3e88f206ae.png)

##### 邮件消息的格式

*   SMTP: 用来交换邮件消息的协议
    
*   RFC 822: **文本邮件消息格式标准**
    
*   信头－头部行。如：
    
    To:
    
    From:
    
    Subject:
    
    这些头部不同于 SMTP 命令!
    
    信体
    
    邮件消息也必须是 **ASCII 字符**
    
    **![](https://i-blog.csdnimg.cn/blog_migrate/93400a13f66ec3fee0072db5cd87baa4.png)**
    

#### DNS

##### 关于主机名和域名

![](https://i-blog.csdnimg.cn/blog_migrate/4a4fd0ea21d65904ba7f1901261df490.png)

##### 概述

![](https://i-blog.csdnimg.cn/blog_migrate/e756ae9b01d4e02f167b5fca4fccc66f.png)

##### 域名系统

![](https://i-blog.csdnimg.cn/blog_migrate/dc5ffdddb170fed0793ad0898ffd28a3.png)

*   功能
    
    *   **主机名到 IP 地址的转换**
    *   主机别名：**一个主机可以有一个规范主机名和多个主机别名**
    *   **邮件服务器别名**
    *   **负载分配**：DNS 实现冗余服务器：一个 IP 地址集合可以对应于同一个规范主机名
*   特点：
    
    *   分布式数据库：一个由分层 DNS 服务器实现的分布式数据库
    *   应用层协议：DNS 服务器实现域名转换 (域名 / 地址转换)
*   为什么不集中式 DNS？
    
    *   单点故障
    *   巨大访问量
    *   远距离集中式数据库
    *   维护
    *   难以扩展
*   DNS 分类![](https://i-blog.csdnimg.cn/blog_migrate/01194854ec68d8c2968c6345bddb268d.png)
    

![](https://i-blog.csdnimg.cn/blog_migrate/42cb1cddd760164aab30c81df757e9f6.png)

或者这么划分

![](https://i-blog.csdnimg.cn/blog_migrate/65ea62bccee8639a3e9a2bdef8d6dd00.png)

**根名字服务器**

*   根名字服务器负责**记录顶级域名服务器的信息**

**顶级域名服务器**

*   **负责顶级域名** com, org, net, edu, etc, 和**所有国家的顶级域名** uk, fr, ca, jp.

**权威 DNS 服务器**

在因特网上具有公共可访问主机（如 Web 服务器和邮件服务器）的每个组织机构必须提供公共可访问的 DNS 记录，

这些记录将这些主机的名字映射为 IP 地址。

组织机构的权威 DNS 服务器负责保存这些 DNS 记录。

**多数大学和公司维护它们的基本权威 DNS 服务器**

**本地 DNS 服务器**

*   严格来说不属于该服务器的层次结构
    
*   **每个 ISP（如居民区 ISP、公司、大学）都有一个本地 DNS 也叫默认服务器**
    
    当**主机发出 DNS 请求时，该请求被发往本地 DNS 服务器**。
    
    起着**代理的作用**，转发请求到层次结构中。
    

##### 递归查询

![](https://i-blog.csdnimg.cn/blog_migrate/9bbc0133011b7d318f02f94cdf59f468.png)

##### 迭代查询

![](https://i-blog.csdnimg.cn/blog_migrate/b3dd4197116b05590c97e05ffcb62375.png)

总结

**递归查询：我帮你查了，妈的；查不到我让你帮我查，查到了告诉我**

**迭代查询：我不知道，但是我可以让你去找别人**

**注意，本地域名服务器查不到直接向跟域名服务器请求，然后才是接下来的操作**

**本地主机中也有 DNS 高速缓存，当高速缓存有的时候就不需要进入本地域名服务器了**

![](https://i-blog.csdnimg.cn/blog_migrate/f7089404b1acb0208247ccacba49abc0.png)

###### 例题

![](https://i-blog.csdnimg.cn/blog_migrate/41898d7078656c65bda26db17b444e53.png)

##### DNS 缓存和权威 DNS 记录更新

![](https://i-blog.csdnimg.cn/blog_migrate/9c04e433c0b11c8d610b52d9ab640025.png)

*   **一旦名字服务器获得 DNS 映射, 它将缓存该映射到局部内存**
*   **服务器在一定时间后将丢弃缓存信息**
*   **本地 DNS 服务器可以缓存 TLD 服务器的 IP 地址**

##### DNS 记录

RR 格式: (name, value, type,ttl)

*   Type=A（Address 地址）
    
    *   name = 主机名  
        value = IP 地址
*   Type=CNAME（canonical 别名）
    
    *   name = 主机别名：www.ibm.com 的真名为 servereast.backup2.ibm.com  
        value = 真实的规范主机名
*   Type=NS（ name server ）
    
    *   name = 域名（如 foo.com）  
        value = 该域权威名字服务器的主机名
*   Type=MX（mail exchange）
    
    *   name = 邮件服务器的主机别名  
        value = 邮件服务器的真实规范主机名

![](https://i-blog.csdnimg.cn/blog_migrate/6de5eb5f5cb0a86d36de2e81dd9ec925.png)

##### DNS 协议

*   **查询报文与应答报文** , 但具有**同样的报文格式**
    
*   报文头部
    
    *   **标识符: 16 位**，查询和应答报文使用相同的标识符
        
    *   标志: 有若干个标志构成，分别标识不同的功能
        
        查询 / 应答－0/ 1
        
        查询希望是 / 非递归查询－1/0
        
        应答可 / 否获得 (支持) 递归查询－1/0
        
        应答是 / 否来自权威名字服务器－1/ 0
        

![](https://i-blog.csdnimg.cn/blog_migrate/5c49159ae32dd32689a578363d97854b.png)

#### 应用层协议小节

![](https://i-blog.csdnimg.cn/blog_migrate/4d8a3823b8736f1a7338911f8c965647.png)

![](https://i-blog.csdnimg.cn/blog_migrate/da3f086de946b90b3e5d0b913fb6c350.png)

### 第三章 运输层

**运输层通信的双方是进程到进程，而网络层是主机到主机**

![](https://i-blog.csdnimg.cn/blog_migrate/811476bc7889f0f5cb028fd0d91844f5.png)

**运输层屏蔽了下层网络核心的细节，使应用进程看起来就好像是两个运输层实体之间有一条端到端的逻辑通信通道!!!**

**运输层使用端口号来区分不同的应用进程！！！**

![](https://i-blog.csdnimg.cn/blog_migrate/b443e30ec5620fe4656c3e446c93bb51.png)

#### 概念

**从通信和信息处理的角度看，传输层向它上面的应用层提供通信服务，它属于面向通信部分的最高层，同时也是用户功能中的最低层**

**传输层位于网络层之上，它为运行在不同主机上的进程之间提供了逻辑通信，而网络层提供主机之间的逻辑通信。**

**显然，即使网络层协议不可靠（网络层协议使分组丢失、混乱或重复），传输层同样能为应用程序提供可靠的服务**

**传输层提供应用进程之间的逻辑通信（即端到端的通信）**

**与网络层的区别是，网络层提供的是主机之间的逻辑通信复用和分用。**

在两个不同的**主机**上运行的**应用程序之间提供逻辑通信**

*   传输层协议运行在**端系统**
    
    *   发送方: 将**应用程序报文**分成**数据段**传递给网络层
        
    *   接受方: 将**数据段重新组装成报文**传递到应用层
        
*   **两个进程**之间的逻辑通信
    
*   可靠, 增强的网络层服务
    

#### 端口号

**引入端口号的原因：不同操作系统对进程 PID 的表示不同，为了统一，就是用端口号来在网络中表示应用层的不同进程**

![](https://i-blog.csdnimg.cn/blog_migrate/0d913db408a73c68abfc6afc36d8ee95.png)

#### 多路复用和多路分解

![](https://i-blog.csdnimg.cn/blog_migrate/8e1bf0040a6896f4b737aa3ba4c3ccf6.png)

![](https://i-blog.csdnimg.cn/blog_migrate/32d8597068644f797a1ce8c1a4d9d89b.png)

**应用层把应用层数据协议单元交给运输层，运输层判断采用 udp 还是 tcp，分别封装为 udp 对应的用户数据报和 tcp 对应的 tcp 报文段，这称作复用，将对应数据交给网络层封装成 IP 数据报，这称作 IP 复用，接收方采取相反的策略即可**

![](https://i-blog.csdnimg.cn/blog_migrate/ad76045586ba6919a1063b7419450b00.png)

*   多路复用
    *   多个**套接字收集数据**，用首部封装数据，然后将报文段传递到网络层
*   多路分解
    *   将接收到的**数据段传递到正确的套接字**

![](https://i-blog.csdnimg.cn/blog_migrate/2d921fce6ffe7ffe04ab643191e97520.png)

*   多路分解工作过程
    *   主机收到 **IP 数据报**
        *   每个**数据报**有**源 IP 地址，目的 IP 地址**
        *   每个**数据报搬运一个数据段**
        *   每个**数据段**有源和目的端口号
        *   主机用 IP 地址和端口号指明数据段属于哪个合适的套接字
        *   ![](https://i-blog.csdnimg.cn/blog_migrate/fe3594cf3cd758de1281081ca07e5045.png)

![](https://i-blog.csdnimg.cn/blog_migrate/835cf13c9fec3bdfbed11226167fb532.png)

![](https://i-blog.csdnimg.cn/blog_migrate/f0674471e28b97aacf7167719fd78fa1.png)

**应用层的协议对应的运输层协议以及端口号**

![](https://i-blog.csdnimg.cn/blog_migrate/3912787e50cec8e62afe92780cefe609.png)

#### TCP 和 UDP 的对比

**udp 无连接，tcp 有连接**

![](https://i-blog.csdnimg.cn/blog_migrate/e34c8345e84e975a4bcb48ea6fa49c6a.png)

**udp 支持单播，多播和广播，而 tcp 只支持单播 (建立连接)**

![](https://i-blog.csdnimg.cn/blog_migrate/87b5b194abaa2f2693f1e7a4af0e164a.png)

**udp 面向应用报文，tcp 面向字节流**

**udp 仅加上一个 udp 数据首部；**

**tcp 将应用层协议数据单元视作连续的字节流，以字节为基础进行发送，然后在接受方重新拼接**

![](https://i-blog.csdnimg.cn/blog_migrate/6387145a35e7639f28ecdcf1bbd1bb1b.png)

**udp 提供无连接不可靠的服务，tcp 提供面向连接的可靠服务**

![](https://i-blog.csdnimg.cn/blog_migrate/f9ef3aff6728258135260de035623b3c.png)

**udp 和 tcp 的首部不同**

![](https://i-blog.csdnimg.cn/blog_migrate/ac214a99fbbc35033a0678529ab62078.png)

#### UDP

**UDP 使用应用层协议提供可靠性!!!**

##### 概述

*   提供**尽力而为**的服务
*   数据报可能**丢失**、**传递失序的报文到应用程序**
*   **无连接**
*   **UDP 是面向报文的。发送方 UDP 对应用程序交下来的报文，在添加首部后就向下交付**

##### UDP 存在原因

*   无需建立连接，**减少延迟**
*   **简单**，在发送者和接受者之间不需要连接状态
*   **很小的数据段首部**。首部开销小，只有 **8 个字节**
*   **没有拥塞控制**，UDP 能够用尽可能快的速度传递
*   UDP 支持一对一、一对多、多对一和多对多的交互通信

![](https://i-blog.csdnimg.cn/blog_migrate/f8c297f66d902395ac17957c3cc5f03a.png)

##### 首部

*   UDP 首部格式
*   ![](https://i-blog.csdnimg.cn/blog_migrate/159b23fc1c42f6e9c7cca957bae48dc8.png)
*   ![](https://i-blog.csdnimg.cn/blog_migrate/b9e4fc8348948f1562f3f43a976cc84b.png)
    *   首部字段 8 字节，4 个字段，每个字段 2 字节
    *   长度是首部和数据的总长度
    *   源端口： 源端口号，**需要对方回信时选用**，不需要时全部置 0
    *   长度：UDP 的数据报的长度（包括首部和数据）其最小值为 8（只有首部）
    *   校验和：检测 UDP 数据报在传输中是否有错，有错则丢弃。该字段是可选的，当源主机不想计算校验和，则直接令该字段全为 0
*   校验和的计算![](https://i-blog.csdnimg.cn/blog_migrate/53a0686cd22dcb8136c1035b70ed83dd.png)
    *   **17 是协议代号，表示 UDP**
    *   伪首部的作用：
        *   **伪首部既不向下传送也不向上递交，而只是为了计算校验和**
        *   通过**伪首部的 IP 地址检验**，UDP 可以确认该数据报是不是发送给本机 IP 地址的
        *   通过伪首部的**协议字段检验**，UDP 可以确认 IP 有没有把不应该传给 UDP 而应该传给别的高层的数据报传给了 UDP
        *   伪首部的 UDP 长度 = UDP 数据包的 UDP 包长度字段值
        *   识别一个通信应用需要 **5 个因素**。" **源 IP 地址 "、“目标 IP 地址”、“源端口”、“目标端口”、“协议号”**。UDP 首部只包含了（源端口和目标端口），用此来校验，如果其他三项信息被破坏，极有可能导致应收包应用收不到，不应该收包的应用收到。为此，有必要在通信中，验证这 5 项的识别码是否正确，就引入了伪首部的概念。

##### 校验和

###### 伪首部 (12 个字节)

![](https://i-blog.csdnimg.cn/blog_migrate/ba552f98eb7a559b0c4e9ec5dd70d48c.png)

**在计算校验和的时候，UDP 数据报之前需要加上 12B 的伪首部，伪首部并不是 UDP 的真正首部，只是在计算校验和的时候，加在前面构成一个临时的数据报，校验和既不向下传送也不向上递交，只是为了计算校验和**

![](https://i-blog.csdnimg.cn/blog_migrate/675d19f5aa11e58f7f23d0be82a0c35f.png)

*   校验和例子  
    ![](https://i-blog.csdnimg.cn/blog_migrate/e9501517c23d5252063f19a1b6918128.png)
    
    *   求和时产生的进位必须回卷到结果上![](https://i-blog.csdnimg.cn/blog_migrate/4d42fd2085dd9d3bdb00e4a509239f01.png)
    *   **累加和按位取反得到校验和**
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/b1e172304e363c317dc31307725c3afd.png)
    

#### TCP

##### MSS

*   **MSS: TCP 报文段中数据字段的最大长度，而 MSS 通常根据 MTU（最大链路层帧）来设置。**

**熟知端口号：0-1023；可提供 2^16 个 不同端口**

**TCP 是在不可靠的 IP 层之上实现的可靠的 (rdt 服务) 数据传输协议，它主要解决传输的可靠、有序、无丢失和不重复问题。**

##### 特点

*   点到点：
    
    *   面向连接的运输层协议
    *   一个发送者，一个接收者
    *   可靠按序的**字节流**
*   可靠：
    
    *   提供可靠的交付服务
    *   传输的数据无差错，不丢失，不重复且有序
*   面向字节流：
    
    *   虽然应用程序和 TCP 的交互是一次一个数据块（大小不等）
    *   但 TCP 把应用程序交下来的数据仅视为一连串的**无结构的字节流**
*   全双工数据：
    
    *   允许通信双方的应用进程在任何时候都能发送数据
    *   发送和接受双方都设有发送缓存和接收缓存，临时存放双方通信的数据
    *   同一个连接上的**双向数据流**
    *   **MSS: 最大报文段长**
*   没有信息边界
    
    *   流水线
*   TCP **拥塞控制**设置窗口大小
    
    *   收发缓冲区
*   面向连接
    
    *   在数据交换前握手
    *   初始化发送方和接收方数据
*   **流量控制**
    
    *   **发送方不会淹没接收方**

##### 报文段结构

报文段分为首部和数据两个部分

###### **MSS：TCP 允许的最大数据段长度!!! 不包含 TCP 首部!!!**

###### 首部 (20 固定长度 + 40 扩展首部)

###### 序号

**32bit，指出本报文段数据载荷的第一个字节的序号**

![](https://i-blog.csdnimg.cn/blog_migrate/7f82486896274b80ff52b0a60839b191.png)

###### 确认号

代表希望收到的下一个数据载荷第一个字节的序号，页是最前面所有数据的确认

![](https://i-blog.csdnimg.cn/blog_migrate/a245db2b549bde45e4a3ada393d4d065.png)

**在建立连接之后所有的报文段都设置 ACK=1，ACK=0 的数据无效**

![](https://i-blog.csdnimg.cn/blog_migrate/47a01551188ba658d2b1afb20f5a3213.png)

###### 数据偏移

**4bit，并且以 4 字节为单位，实际上指出了 TCP 首部的长度，实际大小取决于扩展首部**![](https://i-blog.csdnimg.cn/blog_migrate/6e485ca96e649f4f9aad0f51174bf0ff.png)

###### 保留

占 6bit，保留为今后使用，目前设置为 0

![](https://i-blog.csdnimg.cn/blog_migrate/dbcc9dc6d067cf3d1dc800b52478662b.png)

###### 窗口

**指出发送本报文一方的接收窗口**

![](https://i-blog.csdnimg.cn/blog_migrate/58f0fdda9f8392418be6cd1dc390f258.png)

###### 校验和

**计算检验和时需要加上 12 字节的伪首部，和 udp 一样**

![](https://i-blog.csdnimg.cn/blog_migrate/a2360a4b3cf71fc2d2bf6b498a6256df.png)

###### SYN 同步标志位

![](https://i-blog.csdnimg.cn/blog_migrate/b5b5e2e0ba41198906793d2e9a5f9c0e.png)

###### FIN 终止标志位

![](https://i-blog.csdnimg.cn/blog_migrate/b23ca228288a1224e0fe3479dc7f87f8.png)

###### RST 复位标志位

![](https://i-blog.csdnimg.cn/blog_migrate/f4f53f718856684db256587e7be9e069.png)

###### PSH 推送标志位

![](https://i-blog.csdnimg.cn/blog_migrate/82b355530ee2a2bf37bcb84bfe0e2f6b.png)

###### URG 紧急标志位

![](https://i-blog.csdnimg.cn/blog_migrate/602fc86e2cea83136a93ad414360fc06.png)

###### 选项

![](https://i-blog.csdnimg.cn/blog_migrate/284b288610c42ec288b420506e5564a1.png)

###### 填充

![](https://i-blog.csdnimg.cn/blog_migrate/29c955bf26b2ed67c4568efc14a7cfe7.png)

*   首部格式 **20 字节**

![](https://i-blog.csdnimg.cn/blog_migrate/7914d133b26d3be421544e677aae12ec.png)

> 源端口目的端口：2 字节
> 
> **序号 Seq**：**4 字节**，TCP 连接中传送的数据流中的每一个字节都编上一个序号。序号字段的值则指的是本报文段所发送的**数据的第一个字节的序号**
> 
> **确认号**：**期望接收**的下一个报文段的数据的第一个字节的序号；如果是 N，则表示到序号 N-1 为止，所有数据都正确收到
> 
> 数据偏移：**4 比特，以 32 为为单位，首指出 TCP 报文段的数据起始处距离 TCP 报文段的起始处有多远；因此当此字段的值为 15 时，达到 TCP 首部的最大长度 60B**
> 
> 紧急位 URG：URG=1 时，表明紧急指针字段有效。它告诉系统此报文段中有**紧急数据**，应尽快传送（相当于高优先级的数据）
> 
> **确认位 ACK：确认。只有当 ACK=1 时确认号字段才有效。当 ACK=0 时，确认号无效；TCP 规定，在连接建立之后所有传送的报文段都必须把 ACK 置为 1**
> 
> 推送位 PSH：接收方 TCP 收到 PSH=1 的报文段，**就尽快地交付给接收应用进程，而不再等到整个缓存都填满后再向上交付**
> 
> 同步位 SYN：**SYN=1 表示这是一个连接请求或连接接收报文**
> 
> 终止位 FIN：**用来释放一个连接**。当 FIN = 1 时，表明此报文段的发送方的数据已发送完毕，并要求释放传输连接。
> 
> RST：当 RST=1 时表示出现严重差错，必须释放连接重新建立连接
> 
> FIN：FIN=1，表明次报文段的发送端数据已发送完毕并要求释放连接
> 
> 窗口：2 字节，让对方设置发送窗口的依据。单位是字节
> 
> 检验和：2 字节，**检验首部和数据两个部分。计算检验和时，要在 TCP 报文段前面加上 12 字节的伪首部 (和 UDP 相同)**
> 
> 保留字段：6 位，全为 0

![](https://i-blog.csdnimg.cn/blog_migrate/7e6d0c5936c70cca8dbdd1a084bfa401.png)

##### 可靠数据传输

基本概念

![](https://i-blog.csdnimg.cn/blog_migrate/3502fbb9aa1d278d137eb1631ba2e506.png)

**有线链路误码率较低，一般交给运输层来进行处理；但是无线链路易受干扰，误码率比较高，要求数据链路层必须提供可靠数据传输服务**

![](https://i-blog.csdnimg.cn/blog_migrate/7650f8700fa5af88c69a294d0211708b.png)

###### 停等协议 SW

![](https://i-blog.csdnimg.cn/blog_migrate/ce6ba9332dac1413bb49198567ae10fe.png)

确认与否认：

**发送方发送一个数据分组之后，必须停止下来等待接收方对于该分组的确认 ACK，确认之后才能进行下一个分组的发送，如果发生误码，接收方通过 fcs 段检测到误码之后就会发送 NAK，发送方就重传该分组，所以在收到确认之前，发送方不能删除该分组的缓存**

超时重传：

**若分组发生丢失，这种情况在数据链路层的传递不太容易发生，但是在不同网络之间，通过路由器连接形成的复杂网络系统下就是经常发生的事情，这个时候需要设置一个超市定时器，如果超时了仍然没有收到确认，那么有可能发生了丢失，立即重传**

![](https://i-blog.csdnimg.cn/blog_migrate/6a5d2304a1093fb2fdde28fad804c133.png)

确认丢失：

确认 ACK 丢失了，超时进行重传，那么接收方如何判断收到的是相同的分组呢？

给发送数据的分组加上序号，**在停等协议中使用 01 即可，一个 bit 位**，当接收方发现两个数据相同则丢弃；

那么如果确认分组迟到了呢？第四个图

ACK0 迟到了，导致超时重传 DATA0，接收方收到 DATA0 之后将其丢弃并且发送 ACK，那么这个时候发送方就受到了两个一样的确认分组了，同理带上序号，ACK0 第二次收到就被丢弃；在前面收到 ACK0 的时候发送 DATA1，接收方收到并且发送 ACK1，这就和前面的不同了，所以可以接着发送下一个数据

**由此可见，序号的作用是用来判断该分组和上一个分组是否相同，所以用 01 就可以了，一个 bit 位**

信道利用率

![](https://i-blog.csdnimg.cn/blog_migrate/7fcdf624ab850913c33b01dfbcbb5fbb.png)

信道利用率一般比较低，如果超时重传那更低了，所以产生了另外两种协议

###### 回退 N 帧协议 GBN

**是一种连续 ARQ 协议**

无差错情况

**发送窗口和接收窗口，在回退 N 帧协议中，接收窗口大小固定为 1，发送方看情况**

**假设这里用 3 个 bit，也就是 0-7；发送窗口的大小取值范围为![](https://i-blog.csdnimg.cn/blog_migrate/bacd9d02cda55be95652075a6169dc62.png)，这里取 5**，取 1 就是停等协议

**发送方在发送的过程中，可以连续将发送窗口内的分组发给接收方，假设正确接收，每接受一个，接收方就返回一个 ACK 确认分组，并且把接收窗口后移，发送方接收到确认分组之后就把发送窗口后移，并且可以将确认完的分组将缓存中删除了**

**注意，接受并且后移的时候分组的序号要和窗口当前的序号相匹配**

![](https://i-blog.csdnimg.cn/blog_migrate/51a194d97cf22ede79d8651daf500bee.png)

累计确认

接收方可以不用一个一个发送确认，可以用累计确认，比如 ACK1 表示 1 及其以前的所有分组被正确接收，也就是 0 1

这里假设发送 ACK1 和 ACK4，ACK1 丢失了，但是可以通过 ACK4 得知已经正确接收，所以省掉了重传的步骤

**即使发生确认分组丢失，发送方也不一定就必须重传!**

并且还可以减少网络资源的占用，减小接收方的开销等等

![](https://i-blog.csdnimg.cn/blog_migrate/3b4ec368115f9a6bee6ebf094a877c88.png)

有差错情况

**这里 5 号分组发生了误码，被接受方丢弃了，剩余的分组与接收窗口不匹配，接收方无法接受，只能将这四个分组丢弃**

![](https://i-blog.csdnimg.cn/blog_migrate/1cc139a2cb943c63a791131fb5b1d798.png)

**丢弃之后，每丢弃一个序号不匹配的分组，就发送一个已经接受的 ACK，这里就是 ACK4，发送方接收到这些确认分组之后，就知道发送出现了问题，根据具体实现来确定收到几个重复确认来重传，总之，要么就是不等计时器重传，要么就是等待计时器结束然后进行重传**

**数据链路层的 ACK 和运输层的 ack 有区别，数据链路层的 ACKn 标识 n 及其以前的数据分组被正确接收**

![](https://i-blog.csdnimg.cn/blog_migrate/efe02b21a1404181d5155f8c72907283.png)

GO-Back-N

![](https://i-blog.csdnimg.cn/blog_migrate/b84532851f3f14b3ed5448a4aff80d42.png)

**发送窗口超过上限举例**

发送窗口在这里的范围显示是：1< Wt <=7，不能取 8，当取 8 时如下

发送方一次性发送 0-7，接收方正确接收并且发送累计确认 ACK7，但是确认分组丢失了，于是超时之后重传该分组，重复分组进来之后发现 0 和 0 匹配，可以接受，但是这个时候接受的是重复的分组，就出现了问题

**接收方无法分辨新旧分组!!!**

![](https://i-blog.csdnimg.cn/blog_migrate/d689411274ae14c125efe8be61765e56.png)

总结

![](https://i-blog.csdnimg.cn/blog_migrate/1e94954a25d05eb8c7b7f97d112fab8f.png)

###### 选择重传协议 SR

选择重传的接收窗口应该大于 1，接收方只能按序接受正确到达的数据分组

**选择重传应采用逐一确认而不是累计确认!!!**

![](https://i-blog.csdnimg.cn/blog_migrate/2bfe4423062a9ce0417fdec2bb956a96.png)

举例：

本例中 WtWr4

**发送方发送 0-3 号分组，接收方接受的时候 2 号分组丢失了，接收方先接受 01，发送确认分组，然后将接收窗口后移；接受 3 号分组，但是不能将接收窗口后移，因为接收到的是失序分组，然后返回 ACK3**

![](https://i-blog.csdnimg.cn/blog_migrate/293c243290314444fd948cf9c8be28d7.png)

**发送方接收到 01 分组确认之后就将发送窗口后移，这时候 45 在里面，可以发送 45 号数据分组；与此同时，发送方接收到了 3 号确认分组，将 3 号分组的定时器暂停避免超时重传，表示已经收到，但是这个时候不能移动发送窗口，因为发送方收到的是失序的确认分组，需要等待 2 号分组定时器超时，然后进行重传**

![](https://i-blog.csdnimg.cn/blog_migrate/a8a1eeac52d2466314840743332ca180.png)

**超时之后重传 2 号分组，在这之前也许 45 号分组已经被正确接受，但是由于失序，不能移动接收窗口，发送方接受到确认之后，由于确认分组失序，不能移动发送窗口，并且需要暂停 45 计时器，现在就只剩下 2 号分组的重传了**

![](https://i-blog.csdnimg.cn/blog_migrate/a48e62ed0325f3f7d798dde63dd855da.png)

**2 号分组被正确接收后终于有序，接受窗口后移；发送方接收到确认分组后终于有序，发送窗口后移；进而可以进行后续的传输**

![](https://i-blog.csdnimg.cn/blog_migrate/9c885d5d464017a14206e2184b2acfa2.png)

发送，接受窗口尺寸问题

![](https://i-blog.csdnimg.cn/blog_migrate/eea36ad2a5ee19cc95d121c05474af43.png)

**注意这里和回退 N 帧区别，一个是 2(n-1)，一个是 2n -1**

![](https://i-blog.csdnimg.cn/blog_migrate/e96c7cf9d0c466c2d361c9aaf349ef35.png)

![](https://i-blog.csdnimg.cn/blog_migrate/c95c850141fed562b5bfb392e435a46d.png)

如果超出界限举例

这个例子最大取 4，我们取 5

**发送方发送 0-4，接收方接受并且返回确认分组，但是 0 号确认丢失，发送窗口不能移动；但是接收窗口移动了，如图所示**

**一段时间过后重传 0 号分组，接收方窗口中 0 存在，可以接受，但是这个 0 并不是我们想要的 0，就导致了无法区分新旧数据分组；**

**如果大小合适接收窗口中应该没有 0，这个时候接收方知道确认分组丢失了，所以重新发送确认分组 ACK0，发送方接受，正常运作**

![](https://i-blog.csdnimg.cn/blog_migrate/f86f7d07d83095d8fb7fbdd3436449a0.png)

总结

![](https://i-blog.csdnimg.cn/blog_migrate/1b3db14911d2a8a076b826e2a9d4ccde.png)

###### Tcp 可靠数据传输的实现

###### 比较

![](https://i-blog.csdnimg.cn/blog_migrate/1733d0687cae5d2518c6192debb81069.png)

![](https://i-blog.csdnimg.cn/blog_migrate/005e5908aeebb898c0ba8f2e10b00ee6.png)

**是以上上面三种协议为基础实现的，并没有独立使用哪一种**

**TCP 以字节为单位的滑动窗口实现可靠数据传输**

![](https://i-blog.csdnimg.cn/blog_migrate/52275b74513b902953a5b8bc44ad658f.png)

补充：

![](https://i-blog.csdnimg.cn/blog_migrate/277fc455c7863e92d8a90bbdc75afe4f.png)

###### 利用率 (重要!!!)

**注意 RTT 是往返时延，L/R 就是指的是传输速率!!!**

![](https://i-blog.csdnimg.cn/blog_migrate/46fca9b9fff281d07c869c6ceb15dde6.png)

![](https://i-blog.csdnimg.cn/blog_migrate/86c13d4b8255700126ba1265dd8e58d8.png)

###### 例题：

![](https://i-blog.csdnimg.cn/blog_migrate/92d73c9d9650300ad4fc5b34fdae6b0b.png)

**注意这里细节：**

**序号 500 开始，再发送 500，最后一个字节是 999，ACK 是期望收到，所以是 ACK1000！！！**

**TCP 规定只能对按序到达的最高序号进行确认，所以这个 1000 实际上是累计确认!!!**

##### 可靠传输协议 rdt

![](https://i-blog.csdnimg.cn/blog_migrate/2a7d289bfa6007e4d21bc585460f439b.png)

![](https://i-blog.csdnimg.cn/blog_migrate/a334d99cc29aa30d37a401706f906412.png)

*   FSM：有限状态机
    *   来表示发送方和接收方

###### rdt1.0 完全可靠信道上的可靠数据传输

![](https://i-blog.csdnimg.cn/blog_migrate/8c0a27c0794d1395dc72d4e2fe35843f.png)

###### rdt2.0 具有 bit 错误的信道

![](https://i-blog.csdnimg.cn/blog_migrate/a458a9ac1399f91f31760737b9192e0c.png)

*   提供了差错检测和接收方反馈 (ACK,NAK)
    
*   ![](https://i-blog.csdnimg.cn/blog_migrate/1b8165ddbfc10aed853c5ddd4f5a1aed.png)
    
*   当没有错误时：![](https://i-blog.csdnimg.cn/blog_migrate/050ef18c81b07c2de24b5166d92e32a5.png)
    
*   当发生错误时：![](https://i-blog.csdnimg.cn/blog_migrate/4c15ffe083e81ac94d898574abd275e2.png)
    
*   停 - 等协议
    
    *   发送方发送一个报文，然后等待接受方的响应
*   Rdt2.0 的致命缺陷
    
    *   **如果 ACK/NAK 混淆了会发生什么？**
        
        *   发送方并不知道接收方发生了什么!
            
            万能做法：**重发**
            
            不能正确重发: 可能重复
            
*   **处理重复**:
    
    发送方给每个分组加一个序号
    
    在 ACK/NAK 混淆时发送方重发当前分组
    
    接收方丢弃重复的分组（并不向上传递）
    
    ——停等协议数据包需要多少序号？
    
    *   两个序号 (0,1) 就可以满足

###### rdt2.1 发送方处理 • 两个序号 (0,1) 就可以满足混乱的 ACK/NAKs

![](https://i-blog.csdnimg.cn/blog_migrate/053c1322b8c02cee89edde49652297d2.png)

rdt2.1 接收方处理混乱的 ACK/NAKs

![](https://i-blog.csdnimg.cn/blog_migrate/2ef329f560d872394eb60a2dfdd4614f.png)

*   ![](https://i-blog.csdnimg.cn/blog_migrate/cd02a9f08658e04d2575b54cfa681688.png)

###### rdt2.2 一个 NAK 不要的协议

![](https://i-blog.csdnimg.cn/blog_migrate/9df044b66ab2cbddb8a08a6ad130b76b.png)

![](https://i-blog.csdnimg.cn/blog_migrate/fd895261f48a72079d990061b2d62303.png)

###### rdt3.0 具有出错和丢失的信道

![](https://i-blog.csdnimg.cn/blog_migrate/a7ec993b0fbffe45fee522732035cbc2.png)

![](https://i-blog.csdnimg.cn/blog_migrate/eac5551354aff40d810f5a63827548b3.png)

![](https://i-blog.csdnimg.cn/blog_migrate/dd1dc6f66922b27f6c0a6d6f450db619.png)

![](https://i-blog.csdnimg.cn/blog_migrate/808727c6e68c14ca4223dadb90e853f4.png)

性能

![](https://i-blog.csdnimg.cn/blog_migrate/f41ab3f6dacd5b85ff105e20d49025d9.png)

![](https://i-blog.csdnimg.cn/blog_migrate/9f4e1a317213e58019e4223b5995d9b3.png)

![](https://i-blog.csdnimg.cn/blog_migrate/f91047908b641740239f87b7edc4d786.png)

*   算出来的是发送方的利用率

流水线技术 go-back-N

*   流水线: 发送方允许发送多个 “在路上的”, 还没有确认的报文
    
    *   序号数目的范围必须增加
    *   在发送方 / 接收方**必须有缓冲区**
*   增加了利用率![](https://i-blog.csdnimg.cn/blog_migrate/e3162c9776bffe02eebca99e63886515.png)
    

滑动窗口

*   ![](https://i-blog.csdnimg.cn/blog_migrate/85e663945a10994b16ea15108ef8bd97.png)
    
*   发送方:
    
    *   在分组头中规定一个 k 位的序号
        
    *   **窗口：允许的连续未确认的报文**
        
*   接收方：
    
    *   ACK-only: 总是为**正确接收的最高序号的分组发送 ACK**
        
    *   可能生成重复的 ACKs
        
    *   **只需要记住被期待接收的序号** expectedseqnum
        
    *   接收到失序分组:
        
        丢弃 (不缓冲) -> 没有接收缓冲区!
        
        重发最高序号分组的 ACK![](https://i-blog.csdnimg.cn/blog_migrate/51244a057b820386669765af072aa9c7.png)
        

选择性重传 **Selective Repeat, SR**

*   接收方**分别确认已经收到的分组**
    
    *   必要时，缓冲报文, 最后按序提交给上层
*   发送者**只重发没有收到确认的分组**
    
    *   对每个没有确认的报文发送者都要启动一个定时器 (每个**未被确认的报文都有一个定时器**)
*   **发送窗口**
    
    ​ N 个连续序号
    
    ​ **限制被发送的未确认的分组数量**
    
*   ![](https://i-blog.csdnimg.cn/blog_migrate/f523c149063e0d6075e5f625a6cceb9a.png)
    
*   ![](https://i-blog.csdnimg.cn/blog_migrate/fc70b7009ca6641053db53de4fb05c30.png)
    
    *   接收方收到窗口第一个 ACK 之后，向后移动一位
        *   收到非首位的 ACK，窗口不移动
    *   接收方收到窗口第一个报文之后，向后移动一位
        *   当失序接收时，窗口不移动，记下已接收的报文序号，当窗口中的序号全部接收时，**整体移动**

窗口大小和序号大小的关系

![](https://i-blog.csdnimg.cn/blog_migrate/d6d0a664c908288c6b6a169a47b23696.png)

*   窗口≤序号空间大小的一半
    
*   重发场景![](https://i-blog.csdnimg.cn/blog_migrate/64e632b644c762985a970ac8b7659150.png)
    

快速重传

*   超时触发重传存在问题: 超时周期往往太长——重传丢失报文之前要**等待很长时间**, 因此**增加了网络的时延**
    
    *   发送方可以在超时之前通过重复的 ACK 检测丢失报文段
        
    *   发送方常常一个接一个地发送很多报文段
        
    *   如果报文段丢失, 则**发送方**将可能接收到**很多重复的 ACKs**
        
    *   如果发送方收到一个确认后再收到 **3 个对同样报文段的确认**，发送方应意识到不对劲——生成三个重复 ACK，是因为接收方存在缺失报文段
        
        **启动快速重传**: 在定时器超时之前重发丢失的报文段
        

![](https://i-blog.csdnimg.cn/blog_migrate/2d5d6c2cb4ee56b5c5fe24a44e7d6481.png)

##### 流量控制

运输层的流量控制和数据链路层的流量控制的区别是：

**传输层**定义**端到端用户之间的流量控制**，

**数据链路**层定义两个中间的**相邻结点**的流量控制。

另外，数据链路层的**滑动窗口协议的窗口大小**不能动态变化，**传输层**的则可以**动态变化**

![](https://i-blog.csdnimg.cn/blog_migrate/a64289ef68c9838d35bb827d32a513a4.png)

*   TCP 连接的接收方有一个接收缓冲区
    
*   发送速率和接收应用程序的提取速率匹配
    
*   流量控制：**发送方不能发送的太多太快，让接收缓冲区溢出**
    
    *   **应用程序可能从这个缓冲区读出数据很慢**
*   工作机制（假设 TCP **接收方丢弃失序的报文段**）
    
    *   流量控制**使用接收窗口**: 接收缓冲区的剩余空间
    *   接收方在**报文段**中宣告接收窗口的**剩余空间**
    *   发送方限制没有确认的数据**不超过接收窗口**

###### 举例

前面的过程不再赘述

![](https://i-blog.csdnimg.cn/blog_migrate/bdbe4c748f61ad5053682601e39f2fba.png)

后续打破死锁

**发送窗口为 0 之后启动持续计时器，为什么要引入持续计时器呢？**

**如果等待接收方通知，如果这个报文丢失了那么两方就陷入了互相等待的死锁状态；**

**当持续计时器超时，那么发送零窗口探测报文，如果为 0 返回确认，然后再启动一个持续计时器；**

**值得一提的是，零窗口探测报文也是具有计时器的，当超时就重发**

**那么接收窗口满了还能接受报文吗？TCP 规定，像零窗口探测报文，确认报文，带有紧急事务的报文是必须被接受的!!!**

![](https://i-blog.csdnimg.cn/blog_migrate/5cc6245e5c2b7b540007be83d779dee5.png)

##### TCP 建立连接：三次握手

![](https://i-blog.csdnimg.cn/blog_migrate/bdb455e69709135e2c0c57ec61d515a1.png)

**第一步，设置 SYN=1, 给定一个初始序号 seq=x，发送给 Tcp 服务器；**

**第二步，服务器收到之后发送针对 TCP 连接请求的确认报文，SYN=1，ACK=1，seq=y，ack=x+1；TCP 服务器进入同步已接收状态，TCP 客户进入连接已建立状态**

**TCP 规定，SYN=1 的请求连接的报文不能携带数据，但是需要消耗一个序号**

**第三步，客户端发送针对 TCP 服务器确认的确认，ACK=1，seq=x+1，ack=y+1，这是一个普通的报文段，如果不携带数据不消耗序号，下一个序号从 x+1 开始!!! 这时 TCP 服务器才进入连接已建立阶段**

**为什么不用两次握手？**(面试常考)

![](https://i-blog.csdnimg.cn/blog_migrate/2501b4a43e16da1acc587d217183eb1f.png)

浪费资源，同时

![](https://i-blog.csdnimg.cn/blog_migrate/a783fac63c1db7f11563c5881530b803.png)

![](https://i-blog.csdnimg.cn/blog_migrate/35630f560f71413b375b0644e90e31f4.png)

*   用户发送 TCP **SYN 报文段到服务器**
    
    *   **客户机 TCP 向服务器的 TCP 发送请求连接的报文段**
        
        **这个报文段置同步位 SYN 为 1，同时指定一个初始的序号 seq = x**
        
        **TCP 规定，SYN 不能携带数据，并且要消耗掉一个序号**
        
    *   指定初始的序号
        
    *   没有数据
        
*   服务器接收 SYN，**回复 SYN/ACK 报文段**
    
    *   如同意建立连接，则向客户机发回确认，并为该 TCP 连接分配**缓存和变量**。
        
        在确认报文段中，把 **SYN 位和 ACK 位都置 1** ， **确认号是 ack=x +1** ，
        
        同时也为自己选择一个**初始序号 scq=y** 。 注 意 ， **确认报文段不能携带数据，但也要消耗掉一个序号**
        
    *   服务器分配缓冲区和变量
        
    *   指定服务器的初始序号
        
*   客户接收 SYN/ACK，回复 ACK 报文段
    
    *   当客户机收到确认报文段后，还要向服务器给出确 认，并为该 TCP 连接**分配缓存和变量** 。
        
        确认报文段的 **ACK 位置 1**，**确认号 ack=y+1** ，**序号 seq=x+1**。
        
        **该报文段可以携带数据， 若不携带数据则不消耗序号**
        
    *   可能包含数据
        
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/f9c3c75208ed2db490abb13037ad4472.png)
    

##### TCP 释放连接：四次挥手

![](https://i-blog.csdnimg.cn/blog_migrate/ebdb9ba433be4577e58dd85867768e4c.png)

**第一步，TCP 客户端向服务器发送终止报文，FIN=1，ACK=1,seq=u,ack=v，seq 和 ack 针对上一次发送和接受的数据进行设置**

**TCP 规定 FIN=1 的终止报文段即使不携带数据也要消耗一个序号**

**第二步，TCP 服务器向客户端发送普通确认报文，ACK=1，seq=v，ack=u+1，seq 和 ack 由上一条得出**

**这样 TCP 客户到服务器单方向的传送就关闭了**

**第三步，TCP 服务器还可能进行一些数据传输，最后发送终止报文，FIN=1,ACK=1，seq=w，ack=u+1,w 是根据自己这边数据传输得出的，u+1 是因为客户端没有发送报文了，这是对 u 前面的重复确认**

**第四步，TCP 客户端发送普通的确认报文，ACK=1,seq=u+1,ack=w+1**

注意一点：Tcp 发送确认报文之后还要经过 2MSL 才会完全关闭，下面探讨为什么

**如果最后一个报文段丢失了那么会导致服务器一直重发这条报文，所以需要设置一段等待时间**

![](https://i-blog.csdnimg.cn/blog_migrate/c56a3386843628f9a9a3737436dc6b98.png)

###### 保活计时器

![](https://i-blog.csdnimg.cn/blog_migrate/be23550dce8d65deeb05f55eae92b453.png)

**TCP 服务器为了避免 TCP 客户端出现问题而无法及时发送报文段，设置了一个保活计时器，每收到一个报文就重置一下该计时器**

**如果到了规定时间还未收到数据，那么 TCP 服务器进程会像 TCP 客户进程发送探测保温段，如果发送了很多 (10) 个仍无响应，则关闭连接**

![](https://i-blog.csdnimg.cn/blog_migrate/5a21e1dae4c73695d6a80aadfe91dc8d.png)

*   ** 客户关闭套接字: **clientSocket.close()
    
    Step 1: 客户发送 TCP **FIN 控制报文段到服务器**
    
    ​ 该报文段的终止位 **FIN 置 1 ， 序 号 seq=u** ，它等于**前面已传送过的数据的最后一个字节的序号加 1**，
    
    ​ FIN 报文段即使**不携带数据，也消耗掉一个序号**
    
    Step 2: 服务器接收 FIN, **回复 ACK. 进入半关闭连接状态**
    
    ​ **确认号 ack=u+1 ， 序 号 seq=w** ， 等于它**前面已传送过的数据的最后一个字节的序号加 1**
    
    ​ 此时，从客户机到服务器这个方向的连接就释放了
    
    Step 3: **服务器发送 FIN** 到客户，**客户接收 FIN, 回复 ACK**
    
    进入 “time wait” 状态等待结束时释放连接资源
    
    Step 4: **服务器接收 ACK. 连接关闭**.
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/adb60c9610b443e083746cd91e0e2373.png)

![](https://i-blog.csdnimg.cn/blog_migrate/e2a8440dc95498ebad514fe3831f51f4.png)

##### 拥塞控制

![](https://i-blog.csdnimg.cn/blog_migrate/899c31728424b5310522a3fc43738489.png)

拥塞控制是指**防止过多的数据注入网络**，**保证网络中的路由器或链路不至于过载**

拥塞控制和流量控制的区别：

拥塞控制是让**网络能够承受现有的网络负荷**，是一个全局性的过程，涉及所有的主机、所有的路由器，以及与降低网络传输性能有关的所有因素。强调全局

相反，流量控制往往是指点对点的通信量的控制，是个端到端的问题（接收端控制发送端），它所要做的是**抑制发送端发送数据的速率**，**以便使接收端来得及接收**。强调点对点

*   术语
    
    *   **MSS：最大报文段**
    *   rwnd：拥塞控制窗口
    *   **ssthresh：满开始门限**
*   拥塞：
    
    *   从信息系角度：**太多源主机发送太多的数据**，速度太快以至于网络来不及处理
        
    *   与流量控制不同
        
    *   表现:
        
        **丢失分组** (路由器的缓冲区溢出)
        
        **长延迟** (在路由器的缓冲区排队)
        

控制方法

*   端到端拥塞控制
    *   **没有从网络中得到明确的反馈**
    *   从**端系统观察到的丢失和延迟推断出拥塞**
    *   TCP 采用的方法
*   网络辅助的拥塞控制
    *   路由器给端系统提供反馈
    *   单 bit 指示拥塞 (SNA, DECnet, TCP/IP ECN, ATM)
    *   指明发送者应该发送的速率

###### TCP 拥塞控制

![](https://i-blog.csdnimg.cn/blog_migrate/14e401dd8f93a06583d265be0f99af96.png)

*   三个机制
    *   慢启动
    *   AIMD
    *   对拥塞事件做出反应

归纳

**只要网络没有出现拥塞，拥塞窗口就可以再大一些；只要出现拥塞，就减少一些**

**判断是否出现网络拥塞：没有及时收到按时到达的确认报文 (发生超时重传)**

**发送方将拥塞窗口作为发送窗口**

![](https://i-blog.csdnimg.cn/blog_migrate/010c238671f9271d37f4912bab709adb.png)

###### 慢启动

**提前设置一个慢开始门限值，这里是 16，然后采用指数的形式上涨，每次增加一倍；初值为 1**

![](https://i-blog.csdnimg.cn/blog_migrate/1c03c468ed0e88402c7f6d1a31cb6c01.png)

*   **连接开始时，CongWin=1MSS**
    *   **以 2 的指数方式增加速率**![](https://i-blog.csdnimg.cn/blog_migrate/76072f57865a9fbef1ca6ae19e5ecd8f.png)
*   ![](https://i-blog.csdnimg.cn/blog_migrate/ce5028efce3644f63be43fba061ce8e2.png)

###### 拥塞避免算法

**如果慢启动加倍后超过了门限值，那么发送窗口去门限值!!!**

**超过慢开始门限之后每次增加一倍了，而是固定的增加 1**

![](https://i-blog.csdnimg.cn/blog_migrate/fda0408d90bfac1f3557721265ff86f8.png)

**如果过程中丢失了几个报文，导致重传计时器超时，那么判断网络很可能出现了拥塞，那么如下操作：**

**更新门限值为当前拥塞窗口的一半，设置拥塞窗口为 1，重新开始慢启动**

![](https://i-blog.csdnimg.cn/blog_migrate/a6fd06faee212212c0c74b693a72b0ad.png)

**每经过一个往返时延 RTT 就把发送方的拥塞窗口 cwnd 加 1，而不是加倍**

使拥塞窗口 cwnd 按线性规律缓慢增长（即加法增大），这比慢开始算法的拥塞窗口增长速率要缓慢得多

###### 慢开始门限 ssthresh

*   **防止拥塞窗口 cwnd 增长过大引起网络拥塞，还需要设置慢开始门限 ssthresh 状态变量**
*   **当 cwnd < ssthresh 时，使用慢开始算法**
*   **当 cwnd == ssthresh 时，可以使用慢开始算法，也可以使用拥塞避免算法**
*   **当 cwnd > ssthresh 时，使用拥塞避免算法**
*   **无论使用哪种算法，只要发送方判断网络出现拥塞，就把慢开始门限 ssthresh 设置为出现拥塞时的发送方窗口值的一半，把拥塞窗口 cwnd 设置为 1，执行慢开始算法**
*   ![](https://i-blog.csdnimg.cn/blog_migrate/6acab5f1f98581b00dab3727e4a08859.png)

![](https://i-blog.csdnimg.cn/blog_migrate/102d04c11c4d1059bb6f35ce19be666f.png)

###### 快速重传和快速恢复

![](https://i-blog.csdnimg.cn/blog_migrate/4cd93b4b9b6dcda69cffde65337511ea.png)

上面的意思简单来说就是：当发送方连续收到**三个重复的 ACK 报文**时，直接重传对方尚未收到的报文段，而不必等待那个报文段设置的重传计时器超时

![](https://i-blog.csdnimg.cn/blog_migrate/b365c977bab99f9355042ecdeafda0df.png)

快速重传

![](https://i-blog.csdnimg.cn/blog_migrate/06d541d102118094d1ff238d7bedb00e.png)

注意细节 (大题考)

**确认 M2 的意思是：我现在确认了 M2，我想要的是 M3!!!**

**收到三个重复确认的时候就对相应的报文段立即重传，而不是等待计时器超时，超时了就可能认为网络拥塞，导致网络传输效率减慢**

![](https://i-blog.csdnimg.cn/blog_migrate/4202ff1c75b5b5e59b395f7a999ed427.png)

快恢复算法

**针对只是个别报文段进行丢失的情况，在收到三个重复确认的时候启动快恢复算法而不是慢启动算法**

**将慢开始门限和拥塞窗口都调为当前窗口的一半，开始拥塞避免算法；也有的把拥塞窗口调整为新的门限加 3**

**做题时记得加上 3!!!**

![](https://i-blog.csdnimg.cn/blog_migrate/8b28d51918c5ff4f2783de65b464452b.png)

###### 注意 (快速恢复阶段受到重复的 ACK)

![](https://i-blog.csdnimg.cn/blog_migrate/f8ebdfec8d6e8ec0310143ee7ee8d6b4.png)

**(在三个重复 ACK 之后) 这个时候调成发送窗口为门限值大小!!!**

![](https://i-blog.csdnimg.cn/blog_migrate/8cab294c9cb692a69d099e74731d92f4.png)

![](https://i-blog.csdnimg.cn/blog_migrate/d03c989b752661d05da37055c8c75550.png)

###### 总结

![](https://i-blog.csdnimg.cn/blog_migrate/a03393062cb6d1c58745db90f5d3a3ed.png)

![](https://i-blog.csdnimg.cn/blog_migrate/608b32e7ffc46ad360889dcaa492ca98.png)

*   当 CongWin 低于阀值, 发送方处于慢启动阶段, 窗口指数增长.
*   当 CongWin 高于阀值, 发送方处于拥塞避免阶段, 窗口线性增长.
*   当三个重复的 ACK 出现时, 阀值置为 CongWin/2 并且 CongWin 置为阀值加上 3 个 MSS 并进入**快速恢复阶段**，此时**每收到一个重复的 ACK 拥塞窗口增加 1MSS**，**如果收到新的 ACK 则拥塞窗口置成阀值**）.
*   当超时发生时 ，**阀值置为 CongWin/2 并且 CongWin 置为 1 MSS.**