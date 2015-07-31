# Nginx入门

#### Nginx架构设计

Nginx采用了高度模块化的设计，这些模块的接口非常简单灵活，具有良好的扩展性和可重用性。官方Nginx共有五大类型的模块：
核心模块、配置模块、事件模块、HTTP模块、mail模块。

#### 事件驱动架构

Nginx采用完全的事件驱动架构来处理业务，这与传统的Web服务器（如Apache）是不同的。传统Web服务器中，每个请求从连接建立
到连接关闭，都需要始终占用系统资源（一般情况下是一个请求对应着一个进程或线程），有可能在这段时间内连接是空闲的，导致CPU、内存被浪费，影响并发连接数。
Nginx则和传统Web服务器不同，它不会使用进程或者线程来作为事件消费者，而是使用Nginx的模块。Nginx中，只有事件收集、分发器才有
资格占用进程资源。

传统Web服务器与Nginx间的重要差别：前者是每个事件消费者占一个进程资源，后者的事件消费者只是
被时间分发者进程短期调用而已。这种设计使得网络性能、响应时间都得到提升。
**但是，者会带来一个重要的弊端，即每个事件消费者都不能有阻塞行为，否则将会导致长时间占用时间分发者进程而导致
其他事件得不到及时响应。**