## 什么是Netty，为什么要用Netty

### 什么是Netty

1. Netty是一个机遇NIO的客户端服务器框架，使用它可以快速简单地开发网络应用程序
2. 极大的简化并优化了TCP和UDP套接字服务器等网络编程，并且性能以及安全性等高很多方面甚至要更好
3. 支持多种协议。FTP，SMTP，HTTP以及各种二进制和基于文本的传统协议

Netty成功地找到了一种在不妥协可维护性和性能的情况下实现易于开发，性能，稳定性和灵活性的方法。

### 为什么用Netty，Netty优点

1. 统一的API，支持多种传输类型，阻塞和非阻塞的
2. 简单而强大的线程模型
3. 自带各种协议栈
4. 真正的无连接数据包套接字支持
5. 比直接用Java核心API有更高的吞吐量、更低的延迟、更低的资源消耗和更少的内存复制
6. 安全性不错，有完整的SSL/TLS以及StartTLS支持
7. 社区活跃
8. 成熟稳定

### Netty应用场景

NIO可以做的事，Netty都可以做而且可以做的更好，而主要用来做网络通信

1. 作为RPC框架的网络通信工具
2. 实现一个自己的HTTP服务器
3. 实现一个即时通讯系统
4. 实现消息推送系统

## Netty的核心组件

### 核心组件及作用

#### Channel

Channel接口是Netty对网络操作的抽象类，包括了基本的I/O操作。

比较常用的是NioServerSocketChannel(服务端)和NioSocketChannel(客户端)。Netty中Channel接口提供的API，大大降低了直接使用Socket类的复杂性

#### EventLoop

事件循环是Netty最核心的概念。EventLoop定义了Netty的核心抽象类，用于处理连接的生命周期中所发生的事件。

说白了，主要作用实际上是负责监听网络事件并调用事件处理器进行相关IO操作。

Channel为Netty网络操作（读写等）抽象类，EventLoop负责处理注册到其上的Channel处理IO操作。

#### ChannelFuture

Netty是异步非阻塞的，所有IO操作都是异步的

因此不能立刻得到操作是否成功。可以通过ChannelFuture接口的addListener()方法注册一个ChannelFutureListener，当执行操作成功或者失败时，监听就会自动触发返回结果。

可以通过channel()方法获取关联的Channel。也可以使用sync()方法让异步操作变成同步的

#### ChannelHandler和ChannelPipeline

ChannelHandler是消息的具体处理器，负责处理读写操作、客户端连接等事情

ChannelPipeline为ChannelHandler的链，提供了一个容易并定义了用于演着链传播入站和出站事件流的API。当Channel被创建时，自动分配到它专属的ChannelPipeline

## EventLoopGroup

EventLoopGroup包含了多个EventLoop，EventLoop主要作用实际是负责监听网络事件并调用事件处理器进行相关IO操作的处理。

EventLoop处理IO事件将会在专有的Thread上处理，从而保证Thread和EventLoop1：1，以保证线程安全。

EventLoopGroup分为两部分

1. Boss EventLoopGroup用于接收连接
2. Worker EventLoopGroup用于具体处理

客户端通过connect方法连接服务端，Boss EventLoopGroup处理客户端连接请求，客户端处理完后，会将这个连接交给Worker EventLoopGroup来处理，并负责相关IO操作

### Bootstrap和ServerBootstrap

Bootstrap是客户端的启动引导类/辅助类；ServerBootstrap是服务器端启动引导类/辅助类

Bootstrap通常使用connect()方法连接到远程主机和端口，作为一个Netty TCP协议通信中的客户端；另外Bootstrap也可以通过bind()方法绑定一个本地端口，作为UDP协议通信中的一段。

ServerBootstrap通常使用bind()方法绑定到本地端口上，然后等待客户端的连接

Bootstrap需要配置一个线程组EventLoopGroup，而ServerBootstrap需要两个，一个用于接收，一个用于处理

### NioEventLoopGroup默认的构造函数会起多少线程

NioEventLoopGroup默认构造函数会起CPU核心数*2的线程数

## Netty线程模型

大部分网络框架都基于Reactor模式（Reactor模式基于事件驱动，采用多路复用将事件分发给相应的Handler处理，适合处理海量IO场景）

Netty靠NioEventLoopGroup线程池来实现具体的线程模型

### 单线程模型

一个线程需要执行处理所有的accept, read, decode, process, encode, send事件。对于高负载，高并发并且对性能要求比较高的场景不适用。

### 多线程模型

一个Acceptor线程只负责监听客户端的连接，一个NIO线程池负责具体处理。

满足绝大部分应用场景，并发连接量大的时候可能会出现问题。

### 主从多线程模型

从一个主线程NIO线程池中选择一个线程作为Acceptor线程，绑定监听端口，接收客户端连接，其他线程负责后续的接入认证等工作。连接建立完成后，Sub NIO线程池负责具体处理IO读写。如果多线程模型无法满足需求，可以考虑主从多线程模型。

## Netty服务端和客户端的启动过程

### 服务端创建过程：

1. 创建两个NioEventLoopGroup实例，bossGroup(用于处理客户端的TCP连接请求，线程一般为1)和workerGroup(负责每一条连接的具体读写数据的处理逻辑，真正负责IO操作交给对应的Handler处理，线程数一般为CPU核心*2)
2. 创建服务器启动引导类ServerBootstrap
3. 通过`.group()`方法给引导类配置两大线程组，确定线程模型
4. 通过channel()方法给引导类指定IO模型为NIO
5. 通过`.childHandler()`给引导类创建一个`ChannelInitializer`，然后指定了服务端消息的业务处理逻辑`HelloServerHandler`对象
6. 调用ServerBootstrap类的bind()方法绑定端口

### 客户端创建过程

1. 创建一个NioEventLoopGroup对象实例
2. 创建客户端启动引导类Bootstrap
3. 通过`.group()`给Bootstrap配置一个线程组
4. 通过channel()方法给Bootstrap指定IO模型为NIO
5. 通过`.childHandler()`给引导类创建一个`ChannelInitializer`，然后指定了客户端消息的业务处理逻辑`HelloServerHandler`对象
6. 调用Bootstrap类的connect()方法进行连接，该方法指定inetHost(ip地址)和inetPort(端口号)

## TCP粘包/拆包以及解决办法

TCP粘包/拆包：基于TCP发送数据时，出现了多个字符串粘在一起或者一个字符串被拆开的问题

解决方法

1. Netty自带的解码器
   * LineBasedFrameDecoder：发送端发送数据包时，每个数据报纸剪以换行符作为分隔。工作原理是依次遍历ByteBuf中可读字节，判断是否由换行符，然后进行相应的截取
   * DelimiterBasedFrameDecoder：可自定义分隔符解码器。LineBasedFrameDecoder实际上就是特殊的这个
   * FixedLengthFrameDecoder：固定长度解码器，能够按照指定的长度对消息进行相应的拆包
   * LengthFieldBasedFrameDecoder：自定义长度解码器，能够在消息头定义字段长度进行相应的拆包
2. 自定义序列化编解码器。
   * 使用Java自带的有实现Serializable接口来实现序列化，但由于性能安全性低所以一般不使用
   * 通常情况下使用Protostuff，Hessian2，json序列化方式

## Netty的长连接、长心跳机制

在TCP保持长连接过程中，可能出现断网等网络异常，异常发生时，如果客户端和服务器端没有进行交互，他们是无法发现对方已经掉线，为了解决这个问题，就引入了心跳机制。

心跳机制工作原理：

在服务器端和客户端之间，一定时间内没有数据交互时，客户端或服务器就会发送一个特殊的数据包给对方，当接收方接收到这个数据报文之后，也立即发送一个特殊的数据报文回应发送方，此即一个PING-PONG交互，可以确保TCP连接的有效性。

## Netty的零拷贝

维基百科：

Zero-copy：指计算机执行操作时，CPU不需要先将数据从某处内存复制到另一个特定的区域。这种技术通常用于通过网络传输文件时节省CPU周期和内存带宽。

在OS层面：

Zero-copy通常指避免在用户态和内核态之间来回拷贝数据。

Netty层面：

主要体现在对于数据操作的优化

1. 使用Netty提供的CompositByteBuf类，可以将多个ByteBuf合并为一个逻辑上的ByteBuf，避免了各个ByteBuf之间的拷贝
2. ByteBuf支持slice操作，因此可以将ByteBuf分解为多个共享同一个存储区域的ByteBuf，避免了内存的拷贝
3. 通过FileRegion包装的FileChannel.transferTo实现文件传输，可以直接将文件缓冲区的数据发送到目标Channel，避免传统通过循环write方式导致的内存拷贝问题

