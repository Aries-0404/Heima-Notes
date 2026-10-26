# Netty笔记

## 使用说明

对于netty这块感觉很懵，不管是看视频还是看黑马的笔记都是这种感觉，但是因为要做的api网关项目需要用到netty这块的知识所以不得以还是得学，回想起之前学习编程的方法就是一开始就去看视频，因为视频介绍的事无巨细你压根弄不清楚重点在哪，所以只能把所有的内容都当作是重点都希望弄懂，但是这种方法很低效：

1. 视频内容介绍的事无巨细，压根不清楚重点在哪里，如果将所有内容当作重点都弄懂就太耗时了
2. 视频中介绍的很多内容压根很少使用，甚至于你学过一次以后就都用不上了，但是在学习的过程中你会耗费大量时间，造成浪费
3. 很多时候你看视频压根听不懂它在说什么遇到不会的就在那死磕，一直不会一直卡在那里造成进度缓慢
4. 很多时候即使听了课程也还是一脸懵逼不清楚它在干什么，这时候又得重新回头一点点听课程一点点弄懂

要避免这种学习方式，现在我们不再是一味死磕视频，而是直接去netty官网查看使用指南，那里有很丰富的netty实际使用介绍，虽然它可能不如黑马的视频详细但是够用了，我们的目标是掌握整体框架再去追求细枝末节，我们所有的学习都要以实用作为第一前提。

在学习官网资料的同时记得补充笔记，同时遇到不会的记得询问gpt等把弄懂，官网的资料一定是最通用的框架知识，你只有将这个整体的框架弄懂才能去补充细枝末节。

![image-20250608152939296](./Netty%E7%AC%94%E8%AE%B0.assets/image-20250608152939296.png)





## 是什么：

Netty 是一个网络应用框架，它主要用于管理网络通信的信息输入输出（IO），主要用于开发网络服务器（处理请求）和客户端（发起请求）（注意一点：客户端和服务器都可以进行消息io处理）。它抽象和简化了 TCP、UDP 等传输层以及 HTTP、WebSocket 等应用层协议的编程，提供了强大的工具来处理网络连接的生命周期、数据的编解码（消息的发送与接收）以及各种网络事件。

## 为什么：

### （1）为什么要使用netty?



## 怎么办：

### 1、netty中重要的概念及其使用示例？

#### （1）Channel

channel：一个channel就代表一个节点和另一个节点构建了一个网络连接（TCP连接、UDP连接等）

##### ①使用示例1：

![image-20250608163920797](./Netty%E7%AC%94%E8%AE%B0.assets/image-20250608163920797.png)

问题汇总：

###### `ChannelHandlerContext`是什么？

![image-20250609135125626](./Netty%E7%AC%94%E8%AE%B0.assets/image-20250609135125626.png)

###### `channelActive()`方法是什么？

![image-20250609135219049](./Netty%E7%AC%94%E8%AE%B0.assets/image-20250609135219049.png)

###### `channel.writeAndFlush()`方法是什么？

![image-20250609135253240](./Netty%E7%AC%94%E8%AE%B0.assets/image-20250609135253240.png)



##### ②使用示例2：

![image-20250609111252990](./Netty%E7%AC%94%E8%AE%B0.assets/image-20250609111252990.png)

问题汇总：

###### 这里的`EventLoopGroup`是什么?

![image-20250609140125869](./Netty%E7%AC%94%E8%AE%B0.assets/image-20250609140125869.png)

###### 已经定义了`EventLoop`是用来处理一个`channel`上所有io事件的，既然如此为什么还要定义`EventLoopGroup`呢？

![image-20250609140835013](./Netty%E7%AC%94%E8%AE%B0.assets/image-20250609140835013.png)



###### 为什么要定义`bossGroup`和`workerGroup`？

`bossGroup`是服务端中专门用来监听客户端的连接请求的，`workerGroup`是服务端中用来监听客户端已建立连接的 I/O 操作（read、write、close 等）

![image-20250609140201823](./Netty%E7%AC%94%E8%AE%B0.assets/image-20250609140201823.png)

###### 这里的`childHandler()`方法是什么？

前面我们提过`EventLoop`是调度员的角色，它可以将io请求分配给特定的`ChannelHandler`或者说`ChannelPipeline`进行特定的逻辑处理，这里将什么io请求分配给哪个`ChannelHandler`的分配逻辑就是在`childHandler()`方法中定义的

![image-20250609140218938](./Netty%E7%AC%94%E8%AE%B0.assets/image-20250609140218938.png)

###### 这里的目的是为了要和服务器建立连接，你绑定了8080端口号但是并没有绑定ip地址啊，这样能建立连接吗？

因为示例中给出的是服务器端的构造过程，所以默认连接到本机的ip地址

![image-20250609140249783](./Netty%E7%AC%94%E8%AE%B0.assets/image-20250609140249783.png)







#### （2）EventLoop

`EventLoop`：一个`Channel`对应一个`EventLoop`（一个`EventLoop`绑定一个线程），`EventLoop`用来监听绑定的`channel`上的所有io事件

##### ①使用示例1：

![image-20250608164947468](./Netty%E7%AC%94%E8%AE%B0.assets/image-20250608164947468.png)

问题汇总：

###### 这里的`channelRead()`方法是什么？

![image-20250609143928690](./Netty%E7%AC%94%E8%AE%B0.assets/image-20250609143928690.png)

###### 为什么在channelRead()方法中又会调用eventLoop().execute()方法呢？

我们不是说过`EventLoop`在io操作中充当调度员的角色将具体的io操作交给特定的`ChannelHandler`进行逻辑处理吗？为什么这里在`特定的ChannelHandler即ChannelInboundHandler`中定义对于读取数据的io操作逻辑时又会调用`eventLoop().execute()`方法呢？

![image-20250609150154162](./Netty%E7%AC%94%E8%AE%B0.assets/image-20250609150154162.png)



##### ②使用示例2：

![image-20250609111704054](./Netty%E7%AC%94%E8%AE%B0.assets/image-20250609111704054.png)



#### （3）ChannelHandler

我们在`ChannelHandler`中定义对于各种io操作的处理逻辑，比如读取数据、写入数据库等，`netty`提供了各种`ChannelHandlerAdapter`（继承了`ChannelHandler`）用于对各种不同的io操作进行处理

##### ①使用示例1：

![image-20250608165309880](./Netty%E7%AC%94%E8%AE%B0.assets/image-20250608165309880.png)

##### notes：`EventLoop`和`ChannelHandler`有什么联系和区别？

因为`EventLoop`是用来监听绑定的`channel`上所有io操作的，而`ChannelHandler`是用来定义对于io操作的处理逻辑的，所以这二者之间关系密切。具体描述如下：`EventLoop`就像是调度员，它负责将io操作交给特定的`ChannelHander`进行处理，而`ChannelHandler`就像是具体执行工作的员工，它负责对于特定的io执行特定的处理逻辑。



##### ②使用示例2：

![image-20250609112014629](./Netty%E7%AC%94%E8%AE%B0.assets/image-20250609112014629.png)





#### （4）ChannelPipeline

`ChannelPipeline`：一组由`ChannelHandler`组成的双向链表，构成了对于io操作的多个处理逻辑的流水线

前面我们提到过`EventLoop`就像是调度员它负责监听绑定的`channel`上的所有io操作，然后将特定的io操作交给特定的`ChannelHandler`进行处理，但是得注意往往对于一个io操作会定义多个处理逻辑，这就意味着一个IO事件会被多个`Handler`顺序处理，比如先解码再业务处理最后编码。

##### ①使用示例1：

![image-20250608165453325](./Netty%E7%AC%94%E8%AE%B0.assets/image-20250608165453325.png)



##### ②使用示例2：

![image-20250609112132247](./Netty%E7%AC%94%E8%AE%B0.assets/image-20250609112132247.png)



#### （5）ByteBuf

前面我们提到的`Channel`、`EventLoop`都是传输层的模型，但是`ByteBuf`不是传输层模型也不是物理层模型==而是应用层模型==，它主要的作用就是：消息发送者将应用层的实体类对象序列化成字节流以后存储在其中，后续消息接收者从其中获取字节流数据然后通过反序列化成实体类对象进行解析

##### ①使用示例1：

![image-20250608165805012](./Netty%E7%AC%94%E8%AE%B0.assets/image-20250608165805012.png)



##### ②使用示例2：

![image-20250609112230685](./Netty%E7%AC%94%E8%AE%B0.assets/image-20250609112230685.png)



#### notes：数据在网络传输中各个层级的变化

| OSI层             | 发送端处理（封装）                                           | PDU名称                          | 接收端处理（解封装）                                         |
| :---------------- | :----------------------------------------------------------- | :------------------------------- | :----------------------------------------------------------- |
| **7. 应用层**     | 用户数据（如：HTTP请求、FTP命令、Email正文）生成。           | **数据**                         | 将用户数据传递给相应的应用程序（如浏览器、邮件客户端）。     |
| **6. 表示层**     | 将数据转换为网络标准格式（编码、加密、压缩）。例如：将文本转为ASCII/Unicode，图像转为JPEG，数据加密。 | **数据**                         | 将数据从网络标准格式转换回应用层需要的格式（解码、解密、解压缩）。 |
| **5. 会话层**     | 建立、管理、终止应用程序之间的会话（连接）。添加会话控制信息（如会话ID、同步点）。 | **数据**                         | 管理会话（如恢复中断的传输），移除会话层头部，将数据传递给表示层。 |
| **4. 传输层**     | **关键变化点：** 将上层数据**分段**（如果需要）。添加**传输层头部**（如TCP/UDP头部），包含**源端口、目的端口**、序列号、确认号（TCP）、长度、校验和等。确保端到端的可靠或不可靠传输。 | **段** (TCP) 或 **数据报** (UDP) | 根据端口号将数据传递给正确的上层应用程序进程。进行错误检测（校验和）。如果是TCP，进行流量控制、可靠传输（确认、重传）、重组分段。移除传输层头部。 |
| **3. 网络层**     | **关键变化点：** 添加**网络层头部**（如IP头部），包含**源IP地址、目的IP地址**、TTL（生存时间）、协议标识符（指示上层是TCP还是UDP）、校验和等。负责逻辑寻址和路由（选择最佳路径）。将传输层PDU封装成**分组/包**。 | **分组** 或 **包**               | 根据目的IP地址进行路由决策（选择下一跳）。进行错误检测（IP校验和）。移除网络层头部，将数据传递给传输层。 |
| **2. 数据链路层** | **关键变化点：** 添加**数据链路层头部和尾部**（如以太网头部/尾部）。头部包含**源MAC地址、目的MAC地址**、类型（指示上层是IP还是ARP等）。尾部通常包含**帧校验序列**用于错误检测（如CRC）。将网络层分组封装成**帧**。负责在**同一物理网络段内**通过MAC地址进行节点到节点的可靠传输。 | **帧**                           | 检查目的MAC地址是否匹配本机或广播/组播地址。进行强大的错误检测（FCS），丢弃错误帧。移除数据链路层头部和尾部，将分组传递给网络层。 |
| **1. 物理层**     | **关键变化点：** 将数据链路层的**帧**转换为适合在物理介质（电缆、光纤、无线电波）上传输的**比特流**。处理电压电平、光脉冲、射频调制、时序、物理连接器等。不添加头部/尾部，处理的是原始比特。 | **比特**                         | 从物理介质上检测信号（电压、光、无线电波），将其还原为**比特流**。将比特流传递给数据链路层进行帧的组装。 |



#### notes：关于在网络中进行数据传输的补充说明

我们知道计算机网络细分为7层架构，由底层到高层分别是：物理层（数据以比特流的形式存在）、数据链路层（数据以数据帧的形式存在）、网络层（数据以数据包的形式存在）、传输层（数据以数据段或数据报文的形式存在）、会话层、表示层、应用层（数据以实体对象的形式存在），其中每一层都有它对应的功能。其中应用层一般存储的数据就是我们使用的数据，比如说创建的一个实体类Person对象，但是我们又提到应用层的ByteBuf存储的是字节数据（字节数据就是一个个01的二进制位），这二者矛盾吗？或者说实体类对象在应用层是如何存储的呢？

这二者其实并不矛盾，具体见下图介绍：

![image-20250608224248235](./Netty%E7%AC%94%E8%AE%B0.assets/image-20250608224248235.png)

==总结一下：数据传输的过程是==：

消息发送者==将应用层的实体类对象数据序列化成字节流存入ByteBuf中==，然后在网络中进行传输，后续接收者==从ByteBuf中获取字节流反序列化成应用层的实体类对象==



#### notes：关于序列化和反序列化的介绍

序列化：将一个实体类对象转换成一连串的01字节码

反序列化：将一连串的01字节码转换成对应的实体类对象



#### notes：从应用层以下数据都是以01形式存储那么不同层级之间的数据划分是否还有意义？

我的问题是既然应用层使用的实体类对象被序列化成01字节流存入ByteBuf中，后续各个层级依次在此01数据上进行补充请求头请求地址这些内容，但是它们依然是01数据，那么各个层级的数据彼此之间有区别吗？

当然有区别，虽然都是01数据但是在各个层级中这些01数据代表不同的意义：在物理层中数据以比特流的形式存在这些01数据彼此间是独立的无意义的，物理层只负责传输这些01数据并不会去计较这些数据的意义，但是在数据链路层数据以数据帧的形式存在，数据帧虽然也是01数据但是这些数据不是彼此独立毫无意义的，这些数据中包含了物理地址（如MAC地址）和错误检测等信息，同理对于传输层的01数据也是如此，传输层处理的是数据段（segments，对于TCP）或数据报（datagrams，对于UDP），负责端到端的通信。





### 2、使用netty编写服务端、客户端程序

#### （1）编写Discard服务端（服务端舍弃一切接收到的消息）

##### ①创建服务端处理器（用于处理服务端的处理逻辑：这里的逻辑就是舍弃一切消息）

```java
package io.netty.example.discard;

import io.netty.buffer.ByteBuf;

import io.netty.channel.ChannelHandlerContext;
import io.netty.channel.ChannelInboundHandlerAdapter;

/**
 * Handles a server-side channel.
 */
public class DiscardServerHandler extends ChannelInboundHandlerAdapter { // (1)

    @Override
    public void channelRead(ChannelHandlerContext ctx, Object msg) { // (2)
        // Discard the received data silently.
        ((ByteBuf) msg).release(); // (3)
    }

    @Override
    public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) { // (4)
        // Close the connection when an exception is raised.
        cause.printStackTrace();
        ctx.close();
    }
}
```

1. `DiscardServerHandler`继承了[`ChannelInboundHandlerAdapter`](https://netty.io/4.1/api/io/netty/channel/ChannelInboundHandlerAdapter.html)，它是[`ChannelInboundHandler`](https://netty.io/4.1/api/io/netty/channel/ChannelInboundHandler.html) 的一个实现。[`ChannelInboundHandler`](https://netty.io/4.1/api/io/netty/channel/ChannelInboundHandler.html)提供了各种可供重写的事件处理方法。目前，只需扩展即可，[`ChannelInboundHandlerAdapter`](https://netty.io/4.1/api/io/netty/channel/ChannelInboundHandlerAdapter.html)无需自行实现处理程序接口。
2. 我们在`channelRead()`方法这里重写了事件处理程序。每当从客户端接收到新数据时都会自动调用此方法。在此示例中，收到的消息类型为[`ByteBuf`](https://netty.io/4.1/api/io/netty/buffer/ByteBuf.html)。
3. 为了实现该`DISCARD`协议，处理程序必须忽略接收到的消息。 [`ByteBuf`](https://netty.io/4.1/api/io/netty/buffer/ByteBuf.html)是一个引用计数对象，必须通过`release()`方法显式释放。请记住，释放传递给处理程序的任何引用计数对象是处理程序的责任。上述`channelRead()`方法属于简化版本，通常`channelRead()`方法的实现如下：

```java
@Override
public void channelRead(ChannelHandlerContext ctx, Object msg) {
    try {
        // Do something with msg
    } finally {
        ReferenceCountUtil.release(msg);
    }
}
```

4. 当 Netty 因 I/O 错误引发异常或处理程序在处理事件引发异常时，会使用 Throwable 调用事件处理程序方法`exceptionCaught()`。大多数情况下，捕获的异常应该被记录下来，并在此关闭其关联的通道，但此方法的实现可能会有所不同，具体取决于您想要如何处理异常情况。例如，您可能希望在关闭连接之前发送一条包含错误代码的响应消息。



##### ②创建服务端

```java
package io.netty.example.discard;
    
import io.netty.bootstrap.ServerBootstrap;

import io.netty.channel.ChannelFuture;
import io.netty.channel.ChannelInitializer;
import io.netty.channel.ChannelOption;
import io.netty.channel.EventLoopGroup;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.SocketChannel;
import io.netty.channel.socket.nio.NioServerSocketChannel;
    
/**
 * Discards any incoming data.
 */
public class DiscardServer {
    
    private int port;
    
    public DiscardServer(int port) {
        this.port = port;
    }
    
    public void run() throws Exception {
        EventLoopGroup bossGroup = new NioEventLoopGroup(); // (1)
        EventLoopGroup workerGroup = new NioEventLoopGroup();
        try {
            ServerBootstrap b = new ServerBootstrap(); // (2)
            b.group(bossGroup, workerGroup)
             .channel(NioServerSocketChannel.class) // (3)
             .childHandler(new ChannelInitializer<SocketChannel>() { // (4)
                 @Override
                 public void initChannel(SocketChannel ch) throws Exception {
                     ch.pipeline().addLast(new DiscardServerHandler());
                 }
             })
             .option(ChannelOption.SO_BACKLOG, 128)          // (5)
             .childOption(ChannelOption.SO_KEEPALIVE, true); // (6)
    
            // Bind and start to accept incoming connections.
            ChannelFuture f = b.bind(port).sync(); // (7)
    
            // Wait until the server socket is closed.
            // In this example, this does not happen, but you can do that to gracefully
            // shut down your server.
            f.channel().closeFuture().sync();
        } finally {
            workerGroup.shutdownGracefully();
            bossGroup.shutdownGracefully();
        }
    }
    
    public static void main(String[] args) throws Exception {
        int port = 8080;
        if (args.length > 0) {
            port = Integer.parseInt(args[0]);
        }

        new DiscardServer(port).run();
    }
}
```

1. NioEventLoopGroup 是一个处理 I/O 操作的多线程事件循环
   NioEventLoopGroup 主要用于处理服务器和客户端的 I/O 操作。它通过多线程的方式，管理不同的连接和数据流。每个线程称为 EventLoop，它负责执行连接的处理。

   在这个例子中：

​	一个线程被称为 boss，负责接收客户端的连接。

​	另一个线程称为 worker，在 boss 接收连接后，处理数据传输和读写操作。

2. ServerBootstrap 是一个用于设置服务器的辅助类
ServerBootstrap 是 Netty 提供的一个工具类，用来简化服务器的设置过程。你可以直接使用它来启动服务端，而不需要手动设置每个细节。它通过一种方便的方式将服务器的各项参数配置好，帮助启动服务。

​	例如：设置使用 NioServerSocketChannel 来接受客户端连接。

3. 在这里，我们指定使用 NioServerSocketChannel 用于实例化新类 Channel 来接受传入连接的类
NioServerSocketChannel 是一个具体的实现类，用于处理 TCP 连接。它专门负责在服务器端监听并接收来自客户端的连接请求。我们指定使用这个类来让服务器监听端口，等待客户端连接。

4. 此时指定的处理程序将始终由新接受的 Channel 进行评估
ChannelInitializer 是一个特别的处理器，用来配置新的连接（每个客户端连接会对应一个新的 Channel）。当有新连接到来时，ChannelInitializer 会配置这个连接的处理逻辑，并加入到处理管道中（ChannelPipeline），决定如何处理接收到的数据。

​	这就是为什么我们使用 ChannelDiscardServerHandler 作为处理器，它会丢弃接收到的所有数据。

5. 您可以设置特定于 Channel 实现的参数
可以为 Channel 配置一些与具体实现相关的选项，例如：

​	tcpNoDelay：是否禁用 Nagle 算法（即减少小包发送的延迟）。

​	keepAlive：是否保持连接处于活动状态（例如，在空闲时发送心跳包）。

​	这些设置会影响到底层的 TCP/IP 连接行为。

6. 您是否注意到 option() 和 childOption()？
option() 用于设置 NioServerSocketChannel（即负责接受连接的通道）的配置选项。

​	childOption() 用于设置 NioSocketChannel（即每个接受连接后的实际数据传输通道）的配置选项。

​	这些选项分别控制接收连接的通道和已连接通道的不同参数，帮助我们管理和优化连接的行为。

7. 最后一步是绑定端口并启动服务器
通过 bind() 方法，将服务器绑定到指定的端口（例如 8080），这样服务器就可以开始接收客户端的连接请求。

​	绑定端口后，服务器会开始监听来自客户端的连接，并建立起通信通道。



##### ③校验服务端是否正常工作

现在我们已经编写好了第一个服务器，我们需要测试它是否真的能正常工作。最简单的测试方法是使用*telnet*命令。例如，你可以`telnet localhost 8080`在命令行中输入 telnet 并输入一些内容。

但是，我们能说服务器工作正常吗？我们无法确定，因为它是一个丢弃服务器。你不会收到任何响应。为了证明它确实正常工作，让我们修改服务器，让它打印接收到的内容。

我们已经知道，`channelRead()`只要收到数据，就会调用该方法。让我们在该`channelRead()`方法中添加一些代码`DiscardServerHandler`：

```java
@Override
public void channelRead(ChannelHandlerContext ctx, Object msg) {
    ByteBuf in = (ByteBuf) msg;
    try {
        while (in.isReadable()) { // (1)
            System.out.print((char) in.readByte());
            System.out.flush();
        }
    } finally {
        ReferenceCountUtil.release(msg); // (2)
    }
}
```

如果您再次运行*telnet*命令，您将看到服务器打印它收到的内容。





#### （2）编写Echo服务端（服务端将接收到的消息原封不动的返回给客户端）

到目前为止，我们一直在消费数据，但没有任何响应。然而，服务器通常应该响应请求。让我们学习如何通过实现[`ECHO`](http://tools.ietf.org/html/rfc862)协议来向客户端发送响应消息，该协议会将接收到的任何数据发送回客户端。

与前几节中实现的丢弃服务器唯一的区别是，它将接收到的数据发送回去，而不是将接收到的数据打印到控制台。因此，只需再次修改该`channelRead()`方法即可：

```java
    @Override
    public void channelRead(ChannelHandlerContext ctx, Object msg) {
        ctx.write(msg); // (1)
        ctx.flush(); // (2)
    }
```

1. **`ChannelHandlerContext`** 是 Netty 中的一个上下文对象，它提供了多种方法，帮助你处理各种 I/O 操作（如读取、写入数据等）。在这里，我们使用 **`write(Object)`** 方法将接收到的消息逐字节写入到缓冲区，缓冲区的数据不会立即发送到网络，除非调用`flush()`方法将消息刷新到网络中，如果觉得麻烦的话也可以调用`writeAndFlush(object)`方法。
2. 另外值得注意的是，在上述的`Discard`服务端代码中我们调用`release()`方法手动释放消息，但是在很多情况下（比如这里往缓冲区写入数据的情况）`netty`会自动管理内存，也就是说它会自动进行资源的释放。

如果您再次运行*telnet*命令，您将看到服务器发回您发送给它的任何内容。



#### （3）编写Time服务端（在客户端进行连接时服务端向客户端发送当前的时间戳）

本节要实现的协议是[`TIME`](http://tools.ietf.org/html/rfc868)协议。它与前面的示例不同，它并不接收任何请求，仅仅是发送一条包含 32 位整数的消息并在消息发送后关闭连接。在本例中，您将学习如何构造和发送消息，以及如何在完成后关闭连接。

因为我们要忽略任何接收到的数据，要做的仅仅是在连接建立后立即发送消息，所以这次我们不能使用`channelRead()`方法，而是使用`channelActive()`方法。以下是它的实现：

```java
package io.netty.example.time;

public class TimeServerHandler extends ChannelInboundHandlerAdapter {

    @Override
    public void channelActive(final ChannelHandlerContext ctx) { // (1)
        final ByteBuf time = ctx.alloc().buffer(4); // (2)
        /**
        将当前时间转换为 NTP 时间戳，并写入到 ByteBuf 中。具体解释如下：
        1、System.currentTimeMillis()：Java 中的一个方法，返回自 1970年1月1日 00:00:00 UTC 到当前时间的
        毫秒数，称为 Unix 时间戳 或 Unix 时间。
        2、System.currentTimeMillis() / 1000L：/ 1000L 表示将毫秒数转化为秒数，L 是为了确保操作数是 long 类型。
        由于 System.currentTimeMillis() 返回的是毫秒数，而 NTP 时间戳使用的是秒为单位，因此需要将其除以 
        1000（即将毫秒转换为秒）。
        3、System.currentTimeMillis() / 1000L + 2208988800L：2208988800L 是一个常量，它表示 1970年1月1日
        00:00:00 UTC 到 1900年1月1日 00:00:00 UTC 之间的秒数。由于NTP 时间戳（网络时间协议时间戳）使用的起始时间
        是 1900年1月1日 00:00:00 UTC，而 Unix 时间戳使用的是 1970年1月1日 00:00:00 UTC。这两个时间点之间的差距
        是 2208988800 秒。所以，加上 2208988800L 的目的是将当前的 Unix 时间戳转换为 NTP 时间戳。
        4、(int) (System.currentTimeMillis() / 1000L + 2208988800L)：由于 time.writeInt() 方法要求传入
        一个 32 位整数，而 (System.currentTimeMillis() / 1000L + 2208988800L) 的结果是一个 long 类型的
        数字（64 位），因此需要将其强制转换为 int 类型（32 位）。
        */
        time.writeInt((int) (System.currentTimeMillis() / 1000L + 2208988800L));
        
        final ChannelFuture f = ctx.writeAndFlush(time); // (3)
        f.addListener(new ChannelFutureListener() {
            @Override
            public void operationComplete(ChannelFuture future) {
                assert f == future;
                ctx.close();
            }
        }); // (4)
    }
    
    @Override
    public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) {
        cause.printStackTrace();
        ctx.close();
    }
}
```

1. 当客户端与服务器的连接建立并准备好进行数据传输时，**`channelActive`** 方法会被触发。在这个方法中，服务端会写入一个 **32 位整数**，这个整数表示当前的时间，通常这个整数是 **时间戳**，表示自某个时间点以来的秒数。

2. **`ByteBuf`** 是 Netty 中用于处理字节数据的类。为了存储消息（在这里是 32 位整数），我们需要为它分配一个 **`ByteBuf` 缓冲区**，因为一个 32 位整数占 4 个字节。

3. 这里涉及到两个知识点：

   - netty与传统的NIO（Java 的传统 I/O）进行输入输出处理方式有些区别，NIO 使用 **缓冲区**（ByteBuffer）来存储数据，但它没有提供直接的方法来标记数据的开始和结束位置，除非显式调用 **flip()** 方法来切换缓冲区的读写状态。如果忘记调用 `flip()`，数据可能会错误地处理，导致发送的消息出错或根本不发送数据。**Netty** 避免了这个问题，**Netty** 为不同的操作类型提供了不同的指针来管理数据的位置，因此开发者不需要担心这个麻烦。

   - 当你调用 **`write()`** 或 **`writeAndFlush()`** 时，它们会 **异步** 执行数据发送的操作。也就是说，发送操作不会阻塞当前线程，线程可以继续执行其他操作，而不会等待数据发送完成。例如，以下代码甚至可能在消息发送之前就关闭连接：

     ```Java
     Channel ch = ...;
     ch.writeAndFlush(message);
     ch.close();
     ```

     但这样的情况是我们不希望的，所以我们就得跟踪数据发送是否完成，只有在它完成的时候才去关闭连接。

4. 为了跟踪数据发送是否完成，Netty 返回一个 **`ChannelFuture`** 对象。`ChannelFuture` 是一个表示“尚未完成的操作”的对象。它的作用就是 **告诉你操作是否完成**，比如数据是否已经成功发送到网络上。 **`ChannelFutureListener` 的作用**：用来监听 `ChannelFuture` 操作的监听器。它会在数据发送完成后触发回调函数（即 `operationComplete()` 方法），允许你在数据发送完成后执行一些后续操作。





#### （4）编写Time客户端（接收服务端传来的时间戳并将它转换为对应的日期）

与 **DISCARD** 或 **ECHO** 协议不同的是它们只需要一个服务端即可实现功能（因为我们可以直接通过telnet命令向服务端发出请求并看到它们的响应结果，所以不需要客户端），但是**TIME协议**不仅需要服务端还需要客户端，这是因为服务端响应的 **32 位的二进制时间戳**（例如从 1900 年到当前的秒数）对于人类来说难以理解无法直接转换成一个日历上的日期（比如我们看到 `1234567890` 这个数字，但我们不容易知道它代表的是哪个日期），所以我们才需要客户端，它能够接收到这个二进制数据并将其转换为我们能理解的日期格式，例如 "2023年8月23日"。

**服务器和客户端的区别**：

- 在 **Netty** 中，服务器和客户端的主要区别在于 **使用不同的 Bootstrap 和 Channel 实现**。
  - **ServerBootstrap** 用于配置和启动 **服务器端**。
  - **Bootstrap** 用于配置和启动 **客户端**。
- 这两种类型的应用程序使用不同的配置方法，因为它们的目标是不同的：服务器的目的是等待并接收客户端的连接，而客户端的目的是发起连接并与服务器进行通信。

##### ①创建客户端的处理器（它的处理逻辑是将服务端传来的二进制时间戳转换成对应的日期）

```java
package io.netty.example.time;

import java.util.Date;

import io.netty.buffer.ByteBuf;
import io.netty.channel.ChannelHandlerContext;
import io.netty.channel.ChannelInboundHandlerAdapter;

public class TimeClientHandler extends ChannelInboundHandlerAdapter {
    @Override
    public void channelRead(ChannelHandlerContext ctx, Object msg) {
        ByteBuf m = (ByteBuf) msg; // (1)
        try {
            /**
            客户端的处理逻辑是当收到服务端的ntp时间戳以后，首先将它转换为标准UNIX时间戳，然后通过标准时间戳转换成
            对应的日期，具体介绍看上述Time服务端的代码解释
            */
            long currentTimeMillis = (m.readUnsignedInt() - 2208988800L) * 1000L;
            System.out.println(new Date(currentTimeMillis));
            ctx.close();
        } finally {
            m.release();
        }
    }

    @Override
    public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) {
        cause.printStackTrace();
        ctx.close();
    }
}
```

##### ②创建客户端

```java
package io.netty.example.time;

public class TimeClient {
    public static void main(String[] args) throws Exception {
        String host = args[0];
        int port = Integer.parseInt(args[1]);
        EventLoopGroup workerGroup = new NioEventLoopGroup();
        
        try {
            Bootstrap b = new Bootstrap(); // (1)
            b.group(workerGroup); // (2)
            b.channel(NioSocketChannel.class); // (3)
            b.option(ChannelOption.SO_KEEPALIVE, true); // (4)
            b.handler(new ChannelInitializer<SocketChannel>() {
                @Override
                public void initChannel(SocketChannel ch) throws Exception {
                    ch.pipeline().addLast(new TimeClientHandler());
                }
            });
            
            // Start the client.
            ChannelFuture f = b.connect(host, port).sync(); // (5)

            // Wait until the connection is closed.
            f.channel().closeFuture().sync();
        } finally {
            workerGroup.shutdownGracefully();
        }
    }
}
```

1. `Bootstrap` 是用于 **客户端** 的配置和启动工具类，而 `ServerBootstrap` 是用于 **服务器端** 的配置和启动工具类。它们的功能类似，但分别适用于客户端和服务器端。`ServerBootstrap` 是专门为 **ServerChannel**（例如 `NioServerSocketChannel`）设计的，而 `Bootstrap` 是为 **客户端通道**（例如 `NioSocketChannel`）设计的。
2. `EventLoopGroup` 是用来管理和调度线程的对象。在 Netty 中，**`boss` 组** 负责接收客户端的连接请求，而 **`worker` 组** 负责处理已建立连接的数据传输。如果只提供一个 `EventLoopGroup`，那么这个 `EventLoopGroup` 就会同时负责 **`boss` 组** 和 **`worker` 组**，即同一个 `EventLoopGroup` 会负责接受连接和处理数据传输。但通常在服务器端，`boss` 和 `worker` 组会分开使用，以便更好地控制。
3.  `NioSocketChannel` 是用于 **客户端** 的通道，用来与服务器建立连接，而`NioServerSocketChannel` 是专门用于 **服务器端** 接收连接的通道。
4. 与服务端的 **`ServerBootstrap`** 的用法不同，在 **客户端的代码** 中，我们没有使用 `childOption()`。在服务端的 **`ServerBootstrap`** 中，`childOption()` 用于配置子通道（即服务器端的 `SocketChannel`）。但在 **客户端** 使用的 **`SocketChannel`** 并没有父级通道，因此我们不需要使用 `childOption()`。
5. `bind()` 方法是用于 **服务器端** 绑定端口并监听客户端的连接；而 **`connect()`** 方法是用于 **客户端** 发起与服务器的连接。



#### （5）在服务端、客户端的处理器中处理基于流的传输

##### ①接收端（这里的接收端可以是服务端也可以是客户端）的数据碎片化问题

1. **流协议与数据缓冲区**

   - 在 **流协议**（如 TCP/IP）中，接收到的数据被存储在 **socket 接收缓冲区** 中。

   - 不幸的是，这个缓冲区并不是一个 **数据包队列**，而是一个 **字节队列**。这意味着即使你发送了两个消息，网络协议栈不会将它们当作两个独立的消息来处理，而是将它们视为一连串字节的集合。

2. **数据接收的碎片化问题**

   - 因为 **TCP/IP 协议栈** 是流式的，它不会保证收到的数据会按照发送顺序、分割成消息的完整格式传递。例如，你发送了三个独立的包：

     ```nginx
     ABC DEF GHI
     ```

     但是在接收端，由于缓冲区的字节流特性，接收到的可能是：

     ```nginx
     AB CDEF GHI
     ```

3. **为什么会发生这种情况？**
   - 由于流协议的特性，数据在传输过程中可能被分割成碎片，因此接收方可能在接收到字节流时，并不会直接按消息顺序接收到数据。换句话说，接收端可能会把数据分成多个碎片，导致接收到的内容并不完整，或者不符合应用逻辑的期望格式。

4. **接收端需要做的事情**

   - 无论是在 **服务器端** 还是 **客户端**，接收方都需要 **重组接收到的碎片**，将它们整合成符合应用逻辑的消息格式。这一过程通常称为 **数据重组（defragmentation）** 或 **数据组装**。

   - 例如，尽管接收到的碎片是：

     ```nginx
     AB CDEF GHI
     ```

     但是它应该被重新组合成：

     ```nginx
     ABC DEF GHI
     ```



##### ②解决接收端数据碎片化问题的方式1：在处理器中利用内部缓冲区解决数据碎片化问题

就拿上述的Time服务端的情况进行举例：

1. **背景问题**

   - 在 **TIME 协议** 中，服务器返回的 **32 位整数**（即 NTP 时间戳）是一个 **4 字节** 的数据块。

   - 由于 **流协议**（如 TCP）会将数据作为字节流进行传输，数据可能会被 **分割成碎片**。例如，如果一个 32 位的整数被分为 2 个 2 字节的碎片传输，接收端无法直接知道它是一个完整的消息。

   - **问题**：虽然 32 位整数的数据量很小，不容易碎片化，但随着流量增大，数据的碎片化可能会变得更加明显。因此，接收方需要在接收到完整的 4 字节数据后再进行处理。

2. **解决方案：内部缓冲区**

   为了解决这个问题，解决方案是通过一个 **内部缓冲区**（`buf`）来累积接收到的数据，直到 4 字节数据完全接收完毕，再进行后续处理。

3. **代码解析**

   ```java
   public class TimeClientHandler extends ChannelInboundHandlerAdapter {
       private ByteBuf buf; // (1)
   
       @Override
       public void handlerAdded(ChannelHandlerContext ctx) { // (2)
           buf = ctx.alloc().buffer(4); 
       }
   
       @Override
       public void handlerRemoved(ChannelHandlerContext ctx) {// (3)
           buf.release(); 
           buf = null;
       }
   
       @Override
       public void channelRead(ChannelHandlerContext ctx, Object msg) {// (4)
           ByteBuf m = (ByteBuf) msg; 
           buf.writeBytes(m); 
   
           if (buf.readableBytes() >= 4) { 
               long currentTimeMillis = (buf.readUnsignedInt() - 2208988800L) * 1000L;
               System.out.println(new Date(currentTimeMillis));
               ctx.close();
           }
           m.release();
       }
   
       @Override
       public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) { // (5)
           cause.printStackTrace();
           ctx.close();
       }
   }
   ```

4. **逐行解析：**

   1. **`private ByteBuf buf;`**
      - 声明一个 `ByteBuf` 类型的变量 `buf`，用于存储接收到的数据。

   2. **`handlerAdded()` 方法：**
      - 当 `ChannelHandler` 被添加到管道中时，调用这个方法。此时分配一个 4 字节大小的缓冲区 `buf`，用于存储收到的数据。

   3. **`handlerRemoved()` 方法：**
      - 当 `ChannelHandler` 被移除时，释放 `buf` 资源，并将其置为 `null`。

   4. **`channelRead()` 方法：**
      - 这是接收数据并进行处理的关键方法：
        - **`ByteBuf m = (ByteBuf) msg;`**：接收到的消息是一个 `ByteBuf` 类型，包含从服务器返回的字节数据。
        - **`buf.writeBytes(m);`**：将接收到的字节数据 `m` 写入到 `buf` 中。
        - **`if (buf.readableBytes() >= 4)`**：检查 `buf` 中是否已经有足够的字节（至少 4 个字节）。如果接收到的数据已经完整（4 字节），则执行后续处理。
        - **`buf.readUnsignedInt() - 2208988800L`**：从 `buf` 中读取 4 字节（32 位整数），并将其转换为 Unix 时间戳（通过减去 `2208988800L` 进行转换）。然后将其转换为毫秒。
        - **`System.out.println(new Date(currentTimeMillis));`**：将时间戳转换为可读的日期并打印出来。
        - **`ctx.close();`**：处理完数据后，关闭连接。
        - **`m.release();`**：释放接收到的数据 `m`，避免内存泄漏。

   5. **`exceptionCaught()` 方法：**
      - 用于捕获和处理异常。如果发生异常，则打印堆栈信息并关闭连接。



##### ③解决接收端数据碎片化问题的方式2：创建一个解码器（也属于处理器的一种）专门解决数据碎片化问题

1. **第一种方案与第二种方案的对比**

   - **第一种方案**：在处理器中使用一个累积缓冲区来处理接收到的数据，然后进行业务代码的处理。虽然这种方法解决了 **TIME 客户端** 的问题，但它的==实现过程中将处理数据碎片化问题的代码和业务逻辑代码混合在一起这样不太优雅==。
   - **第二种方案**：==专门创建一个解码器 **`ByteToMessageDecoder`** （解码器本身也属于处理器的一种）来处理数据的碎片化问题==，然后将处理好的数据传给流水线上后一个处理器，后一个处理器接收到数据后专门用来处理业务逻辑，==通过两个功能不同的处理器实现原先一个处理器的功能以此实现功能的解耦==，这样的代码更加优雅。

2. **模块化 `ChannelHandler`**

   - 通过将一个单一的 **`ChannelHandler`** 分解成多个模块化的部分，你可以减少应用程序的复杂度。例如这里就可以将方式1中的`TimeClientHandler`的处理逻辑拆分成两部分：

     - **`TimeDecoder`**：专门负责处理 **数据的碎片化问题**（即解决半包和粘包问题）。
     - **`TimeClientHandler`**：处理简单的业务逻辑。

     通过将不同的逻辑拆分到不同的处理器中，可以使代码结构更清晰、更易于维护。

3. **Netty 提供的扩展类 `ByteToMessageDecoder`**
   - **`ByteToMessageDecoder`** 是 Netty 提供的一个抽象类，专门用于解码从 **字节流** 中接收到的数据。它帮助我们解决了 **数据碎片化** 问题。
     - **`decode()`** 方法：用于从输入缓冲区中提取数据并将其转换为应用程序所需要的消息格式。
     - 该类维护了一个 **内部的累积缓冲区**，每次调用 **`decode()`** 方法时，如果接收到的数据不足以组成完整的消息，解码器会等待更多数据的到来。

4. **`TimeDecoder` 的实现**

   ```java
   public class TimeDecoder extends ByteToMessageDecoder { // (1)
       @Override
       protected void decode(ChannelHandlerContext ctx, ByteBuf in, List<Object> out) { // (2)
           if (in.readableBytes() < 4) { // (3)
               return;
           }
           out.add(in.readBytes(4)); // (4)
       }
   }
   ```
   
   - 关于上述代码的解析：
   
     1. **继承 `ByteToMessageDecoder`**：
        - `TimeDecoder` 类继承了 `ByteToMessageDecoder`，这意味着它能够处理接收到的字节流，并将其解码成具体的消息格式。
   
     2. **`decode()` 方法**：
        - `decode()` 是解码的核心方法。每次数据到达时，`decode()` 会被调用。它接受三个参数：
          - **`ctx`**：`ChannelHandlerContext`，提供了对当前通道的访问。
          - **`in`**：接收到的字节流（`ByteBuf`）。
          - **`out`**：解码后的数据将被添加到 `out` 中，这些数据将被传递到下一个处理器。
   
     3. **检查可读字节数**：
        - **`if (in.readableBytes() < 4)`**：这行代码检查接收到的字节是否足够。如果不足 4 个字节（即不够一个 32 位整数），则 **返回**，等待更多的数据到达。只有在接收到至少 4 字节数据时，才会继续解码操作。（注意：这里解码器的底层依然采用的缓冲区的思想，这点和方式1是相同的，并且==这里依然没有解决发送端发送变长度数据的问题==(都是解决发送方发送固定大小的数据：4个字节)，==要想解决变长度数据的话有几种方式：在每条消息的结尾添加分隔符或者在每条消息开头添加一个字段表示该消息的长度==）
   
     4. **读取并添加解码后的数据**：
        - **`out.add(in.readBytes(4))`**：当接收到 4 个字节时，将这 4 个字节从 `ByteBuf` 中读取出来，并将其添加到 `out` 中。`out` 中的数据将传递给下一个处理器（例如，`TimeClientHandler`）进行进一步处理。
   
     5. **`ByteToMessageDecoder` 的优势**
        - **`ByteToMessageDecoder`** 可以确保接收到的字节数据只有在完整的消息被接收时才会解码，避免了半包和粘包的问题。它内部维护了一个缓冲区，能够等待数据的完整性，直到收到足够的数据来解码完整的消息。
   

5. 将多个处理器（TimeDecoder解码器、TimeClientHandler）加入到客户端TimeClient的流水线中

   ```java
   package io.netty.example.time;
   
   public class TimeClient {
       public static void main(String[] args) throws Exception {
           String host = args[0];
           int port = Integer.parseInt(args[1]);
           EventLoopGroup workerGroup = new NioEventLoopGroup();
           
           try {
               Bootstrap b = new Bootstrap(); // (1)
               b.group(workerGroup); // (2)
               b.channel(NioSocketChannel.class); // (3)
               b.option(ChannelOption.SO_KEEPALIVE, true); // (4)
               //将多个处理逻辑加入到流水线中
               b.handler(new ChannelInitializer<SocketChannel>() {
                   @Override
                   public void initChannel(SocketChannel ch) throws Exception {
                       ch.pipeline().addLast(new TimeDecoder(), new TimeClientHandler());
                   }
               });
               
               // Start the client.
               ChannelFuture f = b.connect(host, port).sync(); // (5)
   
               // Wait until the connection is closed.
               f.channel().closeFuture().sync();
           } finally {
               workerGroup.shutdownGracefully();
           }
       }
   }
   ```





#### （6）通过解码器将ByteBuf数据反序列化成Pojo实体类，从而在业务代码中使用POJO代替ByteBuf进行消息处理

##### 是什么：

首先得明确一点就是：因为规定了网络消息传递的数据格式就是`ByteBuf`类型，所以它的作用不是`Pojo`实体类可以代替的，那么这里用`pojo`代替`ByteBuf`是什么意思呢？

在网络消息传递的过程中接收方在收到消息后，首先得经过反序列化将`ByteBuf`数据转换成`pojo`实体类，然后再业务逻辑中处理该`pojo`类，现在我们将原本接收方的处理器拆分成两部分，一部分完成原本的反序列化过程这部分称为解码器，另一部分完成原本的业务逻辑处理这部分就是原本的业务逻辑处理器，由于在进行业务逻辑处理前消息已经经过了解码器的反序列化所以在业务代码中就可以直接使用`pojo`实体类了，这就是我们所说的在业务代码中用`pojo`代替`ByteBuf`的过程。

##### 为什么：

###### （1）为什么要用pojo来代替ByteBuf呢？

1. **使用 POJO 的优点**
   - **可维护性**：当你在 `ChannelHandler` 中使用 POJO 时，代码变得更容易维护和重用。你可以将从数据流中提取信息的逻辑和协议消息的处理逻辑分开，使代码更加清晰。
   - **可重用性**：POJO 提供了一种标准的方式来组织数据，方便在不同的地方进行复用。而 **`ByteBuf`** 主要用于处理字节流，它的功能更多地集中在数据的读写上，不太适合直接传递业务数据。

2. **`ByteBuf` 适用于简单场景**，而POJO适用于复杂场景

   - 在上述 **TIME 协议** 客户端和服务器的示例中，只处理了一个 **32 位整数**，这个数字并不复杂，使用 **`ByteBuf`** 来直接操作也不会造成很大的问题。

   - 使用 **POJO** 可以使你的代码更加分离和模块化，尤其是在 **复杂协议** 的设计中，你将需要不同类型的数据结构来处理消息，因此 **POJO** 使得这些数据结构更加直观、易于管理和维护。

##### 怎么办：

###### （1）如何使用`pojo`代替`ByteBuf`呢？

还是拿上述的`Time`服务端和客户端的使用举例：

1. **自己创建一个 `UnixTime` 的`POJO`实体类**

   ```java
   public class UnixTime {
       private final long value;
   
       public UnixTime() {
           this(System.currentTimeMillis() / 1000L + 2208988800L);  // 计算 NTP 时间戳
       }
   
       public UnixTime(long value) {
           this.value = value;
       }
   
       public long value() {
           return value;
       }
   
       @Override
       public String toString() {
           return new Date((value - 2208988800L) * 1000L).toString();
       }
   }
   ```

   - **UnixTime POJO**：定义了一个 `UnixTime` 类，用于存储和表示 **Unix 时间戳**。它有两个构造函数，一个默认构造函数会计算当前的 Unix 时间戳，另一个构造函数可以用传入的值初始化。

   - **`toString()`** 方法将时间戳转换为 `Date` 对象并返回可读的日期字符串。

2. **在接收方创建`TimeDecoder` 解码器，将ByteBuf类型的数据反序列化成POJO实体类，并传递给流水线中的下一个处理器**

   ```java
   /**
   之前使用ByteBuf的代码：
   
   public class TimeDecoder extends ByteToMessageDecoder { // (1)
       @Override
       protected void decode(ChannelHandlerContext ctx, ByteBuf in, List<Object> out) { // (2)
           if (in.readableBytes() < 4) { // (3)
               return;
           }
           out.add(in.readBytes(4)); // (4)
       }
   }
   */
   
   public class TimeDecoder extends ByteToMessageDecoder { // (1)
       @Override
       protected void decode(ChannelHandlerContext ctx, ByteBuf in, List<Object> out) {
           if (in.readableBytes() < 4) {
               return;
           }
           out.add(new UnixTime(in.readUnsignedInt()));  // 使用 UnixTime 类而不是 ByteBuf
       }
   }
   ```

   - 在 **`TimeDecoder`** 中，`decode()` 方法处理接收到的数据流。在这个修改版本中，`ByteBuf` 中的 4 字节数据被读取后，直接封装成 `UnixTime` 对象并添加到输出列表 `out` 中。

   - **`in.readUnsignedInt()`**：读取 `ByteBuf` 中的无符号 32 位整数，并将其传递给 `UnixTime` 的构造函数，创建一个 `UnixTime` 对象。

3. **在接收方的处理器`TimeClientHandler`中修改 消息的处理逻辑 ，用`POJO`代替`ByteBuf`**

   这里有个问题：客户端想要直接使用`pojo`代替`ByteBuf`，但是客户端接收到的消息直接就是`pojo`类型嘛？假如接收到的消息就是`ByteBuf`类型的话它还需要经过反序列化将`ByteBuf`类型转换成`pojo`实体类才可以使用啊？

   我们知道在网络通信中消息接收方接收到的消息统一都是`ByteBuf`类型（不管它是客户端还是服务端只要它作为接收方都是如此），那么如果不经过反序列化的话它又如何使用`pojo`实体类代替`ByteBuf`呢？因为这里客户端的流水线中有多个逻辑处理器，在之前的解码器当中就已经将`ByteBuf`类型的数据反序列化成了`pojo`实体类然后传给流水线后面的处理器，所以这里可以直接使用`pojo`实体类。

   ```java
   /**
   之前客户端的处理器中使用ByteBuf的代码，需要先将ByteBuf数据处理成Pojo类型：
   	@Override
       public void channelRead(ChannelHandlerContext ctx, Object msg) {// (4)
           ByteBuf m = (ByteBuf) msg; 
           buf.writeBytes(m); 
   
           if (buf.readableBytes() >= 4) { 
               long currentTimeMillis = (buf.readUnsignedInt() - 2208988800L) * 1000L;
               System.out.println(new Date(currentTimeMillis));
               ctx.close();
           }
           m.release();
       }
       
   */    
   
   
   @Override
   public void channelRead(ChannelHandlerContext ctx, Object msg) {
       UnixTime m = (UnixTime) msg;  // 使用 UnixTime 对象
       System.out.println(m);
       ctx.close();
   }
   ```

   - 在 **`TimeClientHandler`** 中，接收到的消息不再是 `ByteBuf`，而是 `UnixTime` 对象。代码直接将其转换为 `UnixTime` 对象并打印出来。

4. **在发送方的处理器`TimeServerHandler`中修改消息的发送逻辑 **，直接使用pojo代替ByteBuf

   ```java
   /**
   原先发送端的处理器中消息发送的代码使用ByteBuf:
   
   public class TimeServerHandler extends ChannelInboundHandlerAdapter {
   
       @Override
       public void channelActive(final ChannelHandlerContext ctx) { // (1)
           final ByteBuf time = ctx.alloc().buffer(4); // (2)
           time.writeInt((int) (System.currentTimeMillis() / 1000L + 2208988800L));
           
           final ChannelFuture f = ctx.writeAndFlush(time); // (3)
           f.addListener(new ChannelFutureListener() {
               @Override
               public void operationComplete(ChannelFuture future) {
                   assert f == future;
                   ctx.close();
               }
           }); // (4)
       }
   }
   */
   
   
   public class TimeServerHandler extends ChannelInboundHandlerAdapter {
   
       @Override
       public void channelActive(ChannelHandlerContext ctx) {
           ChannelFuture f = ctx.writeAndFlush(new UnixTime());  // 发送 UnixTime 对象
           f.addListener(ChannelFutureListener.CLOSE);
       }
   }
   ```

   - **`TimeServerHandler`** 中，服务器端会创建一个新的 `UnixTime` 对象并通过 `writeAndFlush()` 发送给流水线上的下一个处理器。

   ==notes：注意这里发送方调用`ctx.writeAndFlush(new UnixTime())`方法后并不是说直接将消息通过网络发送给了接收方而是将消息发送给了流水线的下一个处理器，等到所有处理器处理完毕后由最后一个处理器将消息通过网络发送给接收方==

   

5. **在发送方创建编码器`TimeEncoder`**，用于将pojo类序列化成ByteBuf对象

   ```java
   public class TimeEncoder extends ChannelOutboundHandlerAdapter {
       @Override
       public void write(ChannelHandlerContext ctx, Object msg, ChannelPromise promise) {
           UnixTime m = (UnixTime) msg;
           ByteBuf encoded = ctx.alloc().buffer(4);
           encoded.writeInt((int)m.value());
           ctx.write(encoded, promise);  // 将 UnixTime 编码为字节流
       }
   }
   ```

   - **编码器**：`TimeEncoder` 负责将 `UnixTime` 对象编码为 `ByteBuf`。它通过 `ctx.alloc().buffer(4)` 创建一个 4 字节的缓冲区，并将 `UnixTime` 中的值转换为字节流写入缓冲区。

6. **简化编码：使用 `MessageToByteEncoder`**

   如果觉得上述发送端编码器的代码麻烦的话还可以通过直接继承`MessageToByteEncoder`类简化编码器的编写过程

   ```java
   public class TimeEncoder extends MessageToByteEncoder<UnixTime> {
       @Override
       protected void encode(ChannelHandlerContext ctx, UnixTime msg, ByteBuf out) {
           out.writeInt((int)msg.value());
       }
   }
   ```

   - 通过继承 **`MessageToByteEncoder`**，可以进一步简化编码过程。`encode()` 方法直接将 `UnixTime` 的值写入 `ByteBuf` 中。

7. **在发送方的流水线中添加编码器、逻辑处理器**

   ```java
   ch.pipeline().addLast(new TimeServerHandler());  // 在管道中添加 TimeServerHandler
   ch.pipeline().addLast(new TimeEncoder());  // 在管道中添加 TimeEncoder
   ```
   
   - 在 发送端的流水线中，先添加 `TimeServerHandler` 处理器，进行业务逻辑处理。
   - 接着在 发送端的流水线中，添加 `TimeEncoder` 编码器，将 `UnixTime` 对象编码为字节流发送。

##### notes：关于发送方的流水线中各个处理器的逻辑顺序问题？

![image-20250618160609369](./Netty%E7%AC%94%E8%AE%B0.assets/image-20250618160609369.png)





##### notes：关于发送方通过`write(object)、writeAndFlush(object)`方法进行消息发送的补充说明

作为发送方当通过`write(object)、writeAndFlush(object)`方法进行消息发送的时候，并不是说直接就将消息发送给了接收方，而是将消息发送到了流水线的下一个处理器，得经过流水线上所有处理器的处理后才会在网络上进行发送；对于接收方来说也是同理必须经过流水线上所有处理器的处理后才会被接收方最终接收。





#### （7）关闭一个netty服务端或客户端

在Netty 应用程序中一般我们使用 `eventLoopGroup.shutdownGracefully()` 方法来优雅地关闭应用。

下述示例为关闭一个netty服务端的代码：

```java
import io.netty.bootstrap.ServerBootstrap;
import io.netty.channel.*;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.SocketChannel;
import io.netty.channel.socket.nio.NioServerSocketChannel;

public class NettyServer {
    
    public static void main(String[] args) {
        // 创建两个 EventLoopGroup：一个用于接受客户端连接，另一个用于处理网络事件
        EventLoopGroup bossGroup = new NioEventLoopGroup(1); // boss group 负责接受连接
        EventLoopGroup workerGroup = new NioEventLoopGroup(); // worker group 负责处理连接后的事件
        
        try {
            // 创建 ServerBootstrap 并配置服务器
            ServerBootstrap b = new ServerBootstrap();
            b.group(bossGroup, workerGroup)
                .channel(NioServerSocketChannel.class)
                .childHandler(new ChannelInitializer<SocketChannel>() {
                    @Override
                    protected void initChannel(SocketChannel ch) throws Exception {
                        ch.pipeline().addLast(new SimpleChannelInboundHandler<String>() {
                            @Override
                            protected void channelRead0(ChannelHandlerContext ctx, String msg) throws Exception {
                                System.out.println("Received: " + msg);
                            }
                        });
                    }
                });
            
            // 绑定端口并启动服务器
            ChannelFuture f = b.bind(8080).sync();
            
            // 等待直到服务器 socket 被关闭
            f.channel().closeFuture().sync();
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            // 关闭 EventLoopGroup
            // 优雅关闭 workerGroup 和 bossGroup
            bossGroup.shutdownGracefully();
            workerGroup.shutdownGracefully();
        }
    }
}
```

