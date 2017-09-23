# A map of the Networking Code in Linux Kernel 2.4.20

## 由浅入深Linux Kernel 2.4.20网络

### 内容

#### 引言

当我们开始在 DataTAG testbed 研究gigabit 网络以及终端主机的性能时，我们很快意识到有很多丢包发生在终端主机上，并且不清楚这种丢包发生在哪里。为了更好的理解丢包以及缓冲区溢出，我们绘制了有关Linux内核中网络相关代码的流程图，方便了解网络是如何在linux中实现的，并且可以方便我们检查哪一部分代码是导致丢包的始作俑者，而我们从未注意到。

这份报告阐述了我们对于网络模块在Linux Kernel 2.4.20 下是如何运行的理解。我们选择2.4.20发行版是因为在我们开始书写这份报告的时候，2.4.20是最新最稳定的内核版本(2.6还未正式发布)，并且因为这是第一份支持新应用编程接口(NAPI, New Application Programming Interface)。该API支持了网络中断缓及由此介绍数据包在内核中被处理的一种主要的改变。NAPI是开发版分支2.5的主要新尝试并且应该在2.6版本中出现，直到2.4.20被正式发行。关于NAPI的更多介绍以及相关网络新功能可以查看Cooperstein的在线入门教程。

在这份文档中，我们将通过跟踪主机收发数据包的形式来一览内核。我们不会考虑其他类似于X.25的协议。在相对低层，比如在sub-IP层，我们只关心以太网协议而忽略其他比如ATM(异步传输模式)。最终，在IP相关的代码中，我们在此只描述IPV4，并让IPV6位以后的工作做准备。点明一点，IPV4和IPV6其实没有太大的不同，就网络而言(IPV6 有较大的地址空间，没有数据碎片等等)。

这份报告适合那些对IP网络熟悉的人。想要深入理解IP协议和传输控制协议的可以去看Stevens的书籍。Linux kernel 2.4.20 植入了一个针对于Reno(RFC 2581)的变体，叫做NewReno，另外还有选择性确认(the selective acknowledgment, SACK)操作，其在RFC2018 及2883中有详细说明。

在剩下的报告中，我们自低向上的来研究Linux内核。在第二章，我们给出了Linux中关于网络的代码框架。//todo

In the rest of this report, we follow a bottom-up approach to investigate the Linux kernel. In Section 2, we give the big picture of the way the networking code is structured in Linux. A brief introduction to the most relevant data structures is given in Section 3. In Section 4, the sub-IP layer is described. In Section 5, we investigate the network layer (IP unicast, IP multicast, ARP, ICMP). TCP is studied in Section 6 and UDP in Section 7. The socket Application Programming Interface (API) is described in Section 8. Finally, we present some concluding remarks in Section 9.

#### 关于网络的代码: 概览

图一描绘了关于网络的代码在Linux Kernel中的分布。大多数的代码在net/ipv4下。剩下的相关代码在net/core及net/sched下。头文件可以在include/linux及include/net下找到。

图一

关于内核的网络代码，开发者可以通过将自己的代码挂在netfilter的钩子(hooks)上，用来分析或者改变数据包。在这份文档中，我们使用HOOK符号在图表中进行标记。

#### 通用数据结构

##### Socket buffers

##### sock

##### 有关TCP的操作

#### Sub-IP 层

##### 内存管理机制

##### 包接收机制

##### 包传输机制

##### 监控网络队列输入输出的指令

#### 网络层

##### IP单播

##### ARP

##### ICMP

#### TCP

##### TCP输入

##### SACKs

##### 快速响应 QuickACKs

##### 超时机制 Timeous

##### ECN

##### TCP 输出

##### 转换阻塞窗口

#### UDP

####套接字API

##### socket()

##### bind()

##### listen()

##### accept() 和 connect()

##### write()

##### close()

#### 结论



