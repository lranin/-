### 前言
在学习Java NIO的时候,遇到一个问题: NIO**是不是**比BIO快?**为什么**快?

书籍和知乎普遍的解释是,NIO线程少,上下文切换少.那么这种情况是怎么产生的呢?

本文将会从OS的IO开始,结合JAVA中IO模型,解析这个问题.

## Linux中的IO
首先,我们要分清楚底层操作系统的IO和Java IO的联系和区别.在展开下面的讨论之前,先明确一个定义: **Java IO只是将OS的IO包装抽象了一层,提供给开发人员使用.**

[Linux IO模式及 select、poll、epoll详解](https://segmentfault.com/a/1190000003063859)

[I/O中断原理](https://www.cnblogs.com/Jack-Blog/p/12038716.html)

[10分钟看懂， Java NIO 底层原理](https://www.cnblogs.com/crazymakercircle/p/10225159.html)

在OS层面上,有中断还是没有中断对于用户来说线程都是阻塞的,因此包装一层的java IO,调用它的线程也是会被阻塞挂起的.

*重要: java NIO（New IO） 不是IO模型中的NIO模型，而是另外的一种模型，叫做IO多路复用模型（ IO multiplexing ）。*

## JAVA中的IO

### 1. BIO模型(对应OS的BLOCKING-IO)
```Java
//以下代码均未处理异常和关闭连接
ServerSocket server = new ServerSocket(PORT);
log.info("BIO服务器开始监听:{}端口",PORT);
log.info("server info: "+server);
Socket socket = null;
while (true) {
    socket = server.accept();
    log.info("socket info: "+socket);
    //1.直接在当前阻塞式调用处理socket连接
    socketHandler(socket);
    //2.开启新线程处理新建立的socket连接
    new Thread(()->{
      socketHandler(socket); }).start();
}

//代码并未处理半包情况
private void socketHandler(Socket socket){
  BufferedReader in = null;
  PrintWriter out = null;
  try {
  in = new BufferedReader(new InputStreamReader(
          socket.getInputStream()));
  out = new PrintWriter(socket.getOutputStream(), true);
  String resp = null;
  String body = null;
  while (true) {
      body = in.readLine();
      if (body == null)
          break;
      log.info("服务器接到请求:{}",body);
      resp = "server`s response";
      out.println(resp);
    }
  }
}
```
BIO: 线程阻塞式调用OS的IO操作,进入IO操作时,线程一定会被block

1. **阻塞式调用:** main线程将会被挂起(BLOCKING),假如这时新的客户端连接进入,那么新连接将无法被处理,必须等待上一个连接处理完成,while循环才能继续
2. **新线程调用:** 解决了阻塞式调用面临的问题,每个连接都有一个线程处理,main线程不会被阻塞,新的连接进来后可以直接被处理.但这也带来的**新的问题**,加入连接的客户端过多,那么启动的线程将会是几千甚至是几万,这种情况下,线程切换和创建带来的负担,可能会直接打满CPU.
3. **线程池调用:** 解决了手动创建新线程带来的线程数过多的风险,也可以复用线程资源,减少系统内存和线程数负担,但是线程池的线程可能在大部分情况下都阻塞在等待IO读写的过程中,线程池的核心线程数总是会打到很高值.

* **改进点:**
  1. 将socket.accept();做成异步调用,在回调函数中处理连接成功/失败后的操作.
  2. 将IO操作和工作线程分离,IO线程池负责读写IO,worker线程池负责处理数据的业务逻辑.当读到数据时,IO线程将数据传递到worker线程中处理,worker线程处理完之后,将数据传递到write线程中处理.

* **改进点的局限性和解决方案:**
  1. 将IO操作分离之后,BIO仍旧是阻塞的,并没有从根本上解决IO线程池线程数多的问题.
  2. **那么有没有那么一种IO,可以不阻塞调用线程呢?**


### 2. NIO模型(对应epoll)
``` java
//初始化代码块
{
  private volatile boolean stop;
  Selector selector = Selector.open();
  serverSocketChannel = ServerSocketChannel.open();
  //配置为非阻塞模式
  serverSocketChannel.configureBlocking(false);
  serverSocketChannel.socket().bind(new InetSocketAddress(8080), 1024);
  serverSocketChannel.register(selector, SelectionKey.OP_ACCEPT);
  System.out.println("服务器开始监听端口: " + 8080);
}

//启动selector线程
new Thread(()->{selectorStart();},"服务器selector线程").start();

public void selectorStart(){
  while (!stop) {
      try {
          selector.select(1000);
          Set<SelectionKey> selectedKeys = selector.selectedKeys();
          Iterator<SelectionKey> it = selectedKeys.iterator();
          SelectionKey key = null;
          while (it.hasNext()) {
              key = it.next();
              it.remove();
              try {
                  dispatch(key);
              } catch (Exception e) {
                  if (key != null) {
                      key.cancel();
                      if (key.channel() != null)
                          key.channel().close();
                  }
              }
          }
      } catch (Throwable t) {
          t.printStackTrace();
      }
  }
}

//代码并未处理半包情况
private void dispatch(SelectionKey key) throws IOException {
    if (key.isValid()) {
        // 处理新接入的请求消息
        if (key.isAcceptable()) {
            // Accept the new connection
            ServerSocketChannel ssc = (ServerSocketChannel) key.channel();
            SocketChannel sc = ssc.accept();
            sc.configureBlocking(false);
            // Add the new connection to the selector
            sc.register(selector, SelectionKey.OP_READ);
        }
        if (key.isReadable()) {
            // Read the data
            SocketChannel sc = (SocketChannel) key.channel();
            ByteBuffer readBuffer = ByteBuffer.allocate(1024);
            int readBytes = sc.read(readBuffer);
            if (readBytes > 0) {
                readBuffer.flip();
                byte[] bytes = new byte[readBuffer.remaining()];
                readBuffer.get(bytes);
                //开启工作线程处理读入的数据
                new Thread(()->{this.workHandler(bytes);});
            } else if (readBytes < 0) {
                // 对端链路关闭
                key.cancel();
                sc.close();
            } else
                ; // 读到0字节，忽略
        }
        if (key.isWritable()){
                SocketChannel sc = (SocketChannel) key.channel();
                doWrite(sc, "响应成功!");
        }
    }

    private void workHandler(byte[] readBytes){
      String body = new String(readBytes, "UTF-8");
      System.out.println("服务端接收到请求: "
              + body);
      //向selector中注册写事件
      sc.register(selector, SelectionKey.OP_WRITE);
    }
}
```
NIO: NIO并不为连接开启线程,而只是将连接对象注册到selector(事件监听器)上,selector.select()方法实际调用了OS层面的epoll机制,更新selector中注册对象的状态

* NIO巧妙的使用Reactor模型,在单线程中分发请求到对应的Handler上处理

  * 基于事件驱动-> selector() 调用epoll更新注册在其上对象的状态

  * 统一的事件分派中心-> dispatch(); 分发事件

  * 事件处理服务-> connect() & read() & write()
* 在NIO模型中,整体就只有一个selector线程对注册在它之上的**ServerSocketChannel**和**SocketChannel**对象根据OS的epoll返回值进行状态的变更,然后进行对应的事件分发
* **ServerSocketChannel**和**SocketChannel**的状态变更是OS底层回调变更的,例如在windows中sun.nio.ch.WindowsSelectorImpl.SubSelector#poll0 这个native方法会更新socket持有的FD,将状态写入

### 3. NIO的channel的ZERO-COPY特性

[NIO的ZERO-COPY特性](java学习笔记\RPC\NIO的ZERO-COPY特性.md)

### 总结
现在回答开头的问题:

**是不是NIO更快?为什么更快?**

在连接数较少时,NIO和BIO实际上是没有什么速度差异的,并且因为NIO还需要很多额外的代码工作(文中未提及的解半包,网络闪断,安全认证,编码及解码等),因此优先选择BIO.

连接数较多时,NIO因为单线程处理所有连接的IO,在线程上的极大优势,将会比BIO的性能高出很多.

所以,NIO在连接数多时,优于BIO.

并且NIO还具有了ZERO-COPY特性,在数据流小的情况下,数据传输

当然,还是**没有银弹**这个软件设计原则,根据内部情况和外部环境才能做出最优抉择.


> 参考及引用内容

[参考书籍:<<Netty权威指南>>](https://www.amazon.cn/dp/B00WFSXDRM)

[怎样理解阻塞非阻塞与同步异步的区别？](https://www.zhihu.com/question/19732473/answer/241673170)

[Linux IO模式及 select、poll、epoll详解](https://segmentfault.com/a/1190000003063859)

[用户空间与内核空间，进程上下文与中断上下文](https://www.cnblogs.com/Anker/p/3269106.html)

[Linux 内核空间与用户空间](https://www.cnblogs.com/sparkdev/p/8410350.html)

[I/O中断原理](https://www.cnblogs.com/Jack-Blog/p/12038716.html)

[反应器模式(reactor)](https://zh.wikipedia.org/wiki/%E5%8F%8D%E5%BA%94%E5%99%A8%E6%A8%A1%E5%BC%8F)
