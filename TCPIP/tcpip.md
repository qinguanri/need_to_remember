#TCP/IP详解

####第3章 IP：网际协议

IP是TCP/IP协议族中最为核心的协议。所有的TCP、UDP、ICMP及IGMP数据都已IP数据包格式传输。
IP协议不能保证IP数据报能成功地到达目的地。IP仅提供尽力而为的服务。如果发生某种错误，如某个路由器暂时用完了缓冲区，IP有一个简单的错误处理算法：丢弃该数据包，然后发送ICMP消息报给信源端。任何要求的可靠性必须由上层来提供。
无连接表示IP并不维护任何关于后续数据报的状态信息。每个数据报的处理是相互独立的。这也说明，IP数据报可以不按发送顺序接收。
IP数据报格式中，总长度字段指整个IP数据报的长度，以字节为单位。由于该字段长16比特，所以IP数据报最长可达65535字节。
TTL生存时间字段设置了数据报可以经过的最多路由数。它指定了数据报的生存时间。TTL的初始值由源主机设置（通常为32或64），一旦经过一个处理它的路由器，它的值就减1。当字段的值为0时，数据报就被丢弃，并发送ICMP报文通知源主机。

1.1	IP路由选择
如果目的主机与源主机直接相连或都在一个共享网络上，那么IP数据报就直接送到目的主机上。否则，主机把数据报发往一默认的路由器上，由路由器来转发该数据报。
IP可以从TCP、UDP、ICMP和IGMP接收数据报并进行发送，或者从一个网络接口接收数据报并进行发送。IP层在内存中有一个路由表。当收到一份数据报并进行发送时，它都要对该表搜索一次。
路由表的每一项都包含下面这些信息：

	目的IP地址
	下一跳路由器的IP地址
	标志。其中一个标志指明目的IP地址是网络地址还是主机地址，另一个标志指明下一站路由器是否为真正的下一站路由器。
	为数据报的传输指定一个网络接口

从路由表信息可以看出，IP并不知道到达任何目的的完整路径。所有的IP路由选择只为数据报传输提供下一站路由器的IP地址。
IP路由选择主要完成以下功能：

（1）	搜索路由表，寻找能与目的IP地址完全匹配的表目
（2）	搜索路由表，寻找能与目的网络号相匹配的表目。
（3）	搜索路由表，寻找标为“默认”的表目。

####第4章 ARP：地址解析协议
当一台主机把以太网数据帧发送到位于同一局域网上的另一台主机时，是根据48bit的以太网地址来确定目的接口的。设备驱动程序从不检查IP数据报中的目的IP地址。
地址解析为这两种不同的地址形式提供映射：32bit的IP地址和数据链路层使用的任何类型的地址。
ARP为IP地址到对应的硬件地址之间提供动态映射。
 
图.地址解析协议：ARP和RARP

2.1	一个例子（FTP）
 
图.当用户输入“ftp主机名”时ARP的操作
在ARP背后有一个基本概念，那就是网络接口有一个硬件地址（一个48bit的值，标识不同的以太网或令牌环网络接口）。在硬件层次上进行的数据帧交换必须有正确的接口地址。但是，TCP/IP有自己的地址：32bit的IP地址。知道主机的IP地址并不能让内核发送一帧数据给主机。内核必须知道目的端的硬件地址才能发送数据。ARP的功能是在32bit的IP地址和采用不同网络技术的硬件地址之间提供动态映射。
2.2	ARP高速缓存
ARP高效运行的关键是由于每个主机上都有一个ARP高速缓存。这个高速缓存存放了最近Internet地址到硬件地址之间的映射记录。每一项的生存时间是20分钟。
2.3	ARP代理
如果ARP请求时从一个网络的主机发往另一个网络的主机，那么连接这两个网络的路由器就可以回答该请求，这个过程称作委托ARP或ARP代理（Proxy ARP）。这样可以欺骗发起ARP请求的发送端，使它误以为路由器就是目的主机，而事实上目的主机是在路由器的“另一边”。路由器的功能相当于目的主机的代理，把分组从其他主机转发给它。
在大多数TCP/IP实现中，ARP是一个基础协议，但是它的运行对应于应用程序或系统管理员来说一般是透明的。

####第6章 ICMP：Internet控制报文协议
ICMP是IP层的一个组成部分。它传递差错报文以及其他需要注意的信息。ICMP报文通常被IP层或更高层（TCP或UDP）协议使用。一些ICMP报文把差错报文返回给用户进程。
ICMP报文是在IP数据报内部被传输的。如图所示：
 
下面各种情况都不会产生ICMP差错报文：
（1）	ICMP差错报文
（2）	目的地址是广播地址或多播地址的IP数据报
（3）	作为链路层广播的数据报
（4）	不是IP分片的第一片
（5）	源地址不是单个主机的数据报，例如源地址是零地址、环回地址、广播地址。
这些规则时为了防止过去允许ICMP差错报文对广播分组响应带来的广播风暴。

####第17章 TCP传输控制协议

4.1	TCP的服务

尽管TCP和UDP都是用相同的网络层（IP成），TCP却向应用层提供和UDP完全不同的服务。TCP提供一种面向连接的、可靠的字节流服务。
TCP通过下列方式来提供可靠性：
	应用数据被分割成TCP认为最合适发送的数据块。
	TCP发出一个段后，它启动一个定时器，等待目的端确认收到这个报文段。
	当TCP收到另一端的数据时，它将发送一个确认。
	TCP将保持它首部和数据的校验和。
	如果必要，TCP将收到的数据进行重新排序，将收到的数据以正确的顺序交给应用层。
	TCP的接收端必须丢弃重复的数据。
	TCP能提供流量控制。防止较快的主机致使较慢主机的缓冲区溢出。
TCP不在字节流中插入标识符。TCP不知道传输的数据字节流是二进制数据，还是ASCII字符或者其他类型数据。对字节流的解释由TCP连接双方的应用层解释。

4.2	TCP的首部

TCP数据被封装在一个IP数据报中。
 
TCP首部的数据格式如下，大部分情况下有20字节。首部长度的单位是32bit，用4位来表示，因此TCP最多有2^4*32bit = 60字节的首部：
 
	每个TCP段都包含源端和目的端的端口号，用于寻找发端和收端应用程序。这两个值加上IP首部中的源端IP地址和目的端IP地址唯一确定一个TCP连接。
	TCP为应用层提供全双工服务。这意味着数据能在两个方向上独立地进行传输。因此，连接的每一端都必须保持每个方向上的传输数据序号。
	检验和覆盖了整个的TCP报文段：TCP首部和TCP数据。

####第18章 TCP连接的建立与终止

TCP是一个面向连接的协议，无论哪一方向另一方发送数据之前，都必须先在双发之间建立一条连接。
5.1	建立连接协议
（1）	客户端发送一个SYN报文段指明客户打算连接的服务端的端口，以及初始序号（ISN）。
（2）	服务端发回包含服务器的初始序号的SYN报文段作为应答。同时，将确认序号设置为客户的ISN加1以对客户的SYN报文段进行确认。
（3）	客户必须将确认序号设置为服务器的ISN加1以对服务器的SYN报文段进行确认。
这三个报文段完成连接的建立。这个过程也称为三次握手。
 
注意：如果两端同时打开连接，需要交换4个报文段。
5.2	连接终止协议
建立一个连接需要三次握手，而终止一个连接要经过4次握手。这由TCP的半关闭（half-close）造成的。既然一个TCP连接是全双工（即数据在两个方向上都能同时传递），因此每个方向必须单独地进行关闭。这原则就是当一方完成它的数据发送任务后就能发送一个FIN来终止这个方向连接。
收到一个FIN只意味着在这一方向上没有数据流动。一个TCP连接在收到一个FIN后仍能发送数据。这对利用半关闭的应用来说是可能的。
5.3	连接建立的超时
有很多情况导致无法建立连接。客户端在放弃建立连接尝试前进行SYN重传的时间。第一次超时时间为6秒左右，第二次超时时间为24.00秒。
5.4	最大报文长度
MSS = MTU –  40B
5.5	TCP的状态变迁
图.TCP正常连接和终止所对应的状态
当一个TCP执行一个主动关闭，并发回最后一个ACK，该连接必须在TIME_WAIT状态停留的时间为2倍的MSL。这样可以让TCP再次发送最后一个ACK以防这个ACK丢失（这时另一端会超时并重发FIN）。这种2MSL等待的另一个结果是这个TCP连接在2MSL等待期间，定义这个连接的套接字不能再被使用。在连接处于2MSL等待时，任何迟到的报文段将被丢弃。
5.6	检测半打开连接
半打开连接的另一个常见原因是当客户端主机突然掉电而不是正常的结束客户应用程序后再关机。
6	第20章 TCP的成块数据流
滑动窗口协议允许发送方在停止等待确认前可以连续发送多个分组。由于发送方不必每发一个分组就停下来等待确认，因此该协议和停等协议相比可以加速数据的传输。
6.1	滑动窗口
6.2	窗口大小
BSD默认的窗口大小为4096B。socketAPI允许进程设置发送和接收缓存的大小。接收缓存的大小是该连接上所能够通告的最大窗口大小。有一些应用程序通过修改缓存大小来增加性能。
6.3	慢启动

####第21章 TCP的超时与重传

对每个连接，TCP管理4个不同的定时器。
（1）	重传定时器使用与当希望收到另一端的确认。
（2）	坚持定时器使窗口大小信息保持不断流动。
（3）	保活定时器可检测到一个空闲连接的另一端何时崩溃或重启。
（4）	2MSL定时器测量一个连接处于TIME_WAIT状态的时间。
7.1	往返时间测量
TCP超时与重传中最重要的部分就是对一个给定连接的往返时间（RTT）的测量。由于路由器和网络流量均会变化，因此我们认为RTT可能经常会发生变化，TCP应该跟踪这些变化并相应地改变其超时时间。
RTT的计算有专门的计算公式及算法。
7.2	拥塞避免算法