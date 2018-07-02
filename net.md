#### TCP/IP协议

***

ip包经过ip协议加工，通过路由器进行转发，特点是按块发送，经过多个路由设备，不保证送达也不保证顺序到达。



TCP协议建立在IP协议之上，负责在两台计算机之间建立可靠的连接，保证数据包的顺序到达，通过握手建立连接，对每个IP包进行编号，保证能够顺序收到，掉包就重新发送。



TCP连接 需要一个客户端一个服务端



#### socket编程

***

- Python实现

  ***

  ​	客户端：

  ​		基本步骤：

  ​				建立一个socket ，建立连接，发送数据，接受数据，关闭连接

  ```python
  import socket
  
  s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
  # 建立连接:
  s.connect(('127.0.0.1', 9999))
  # 接收欢迎消息:
  print s.recv(1024)
  for data in ['Michael', 'Tracy', 'Sarah']:
      # 发送数据:
      s.send(data)
      print s.recv(1024)
  s.send('exit')
  s.close()
  ```

  

  ​	服务端;

  ```python
  #!/usr/bin/env python
  # -*- coding: utf-8 -*-
  
  
  import time, socket, threading
  
  def tcplink(sock, addr):
      print 'Accept new connection from %s:%s...' % addr
      sock.send('Welcome!')
      while True:
          data = sock.recv(1024)
          time.sleep(1)
          if data == 'exit' or not data:
              break
          sock.send('Hello, %s!' % data)
      sock.close()
      print 'Connection from %s:%s closed.' % addr
  
  s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
  # 监听端口:
  s.bind(('127.0.0.1', 9999))
  s.listen(5)
  print 'Waiting for connection...'
  while True:
      # 接受一个新连接:
      sock, addr = s.accept()
      # 创建新线程来处理TCP连接:
      t = threading.Thread(target=tcplink, args=(sock, addr))
  t.start()
  ```

   

  bind函数一般在connect和listen之前使用，socket建立套接口之后，就存在一个地址族之中，但是并没有赋名，主要给其分配本地的端口给变量。

 listen将一个主动的进程变成一个服务端的被动的接受进程。一般在绑定端口后和accept之前调用。

所以服务器上有两个套接字，一个用于不停的监听一个用于接受和发送。



TCP/IP协议集 一般为**四层** 第一层为物理通讯的接口，第二层为网络层，多个网络协议，用于实现物理地址和逻辑地址的转换，就是说能够识别计算机的地址。第三层是传输层TCP，用于数据的传递。第五层是应用层，用于进一步的提升。

ip地分配原则 为同一网络内的不同主句分配相同的网络号，不同的主机号

ip分配下来后 通过子网掩码进行分割，**将ip地址分为网络号和主机号**

![IP与子网掩码的关系](https://img-blog.csdn.net/20160427105221679)

网络层提供主机间的逻辑通信，运输层提供端口间的逻辑通信。

TCP建立连接需要三次握手，也就是客户端额服务端一共需要发送三个包才能确认建立的连接。

​	第一次握手：客户端发送**SYN包，SYN = 1，ack = 0，随机产生一个值给seg=x**，给服务发送请求，客户端进入SYN_SENT等待服务器确认。发送**SYN包**　ＳＹＮ为请求的作用

​	第二次握手，服务器就收到SYN包，知道了客户端在请求连接，发送**SYN+ACK=1 ，ack = x+1，seg=Y**的包给客户端，服务器进入SYN_RCVD状态.发送**ＳＹＮ＋ＡＣＫ包**

​		第二次握手的时候发送的包包括确认包也包括一个请求包．

​	第三次握手：客户端收到包，检查ack是不是x+1 ACK的标示是不是1，如果正确发送**ACK包 **ACK=1，ack=Y+1，将ACK发送给服务器，服务器进行判断，如果正确，则进入ESTABLISHED状态病并开始传输数据

一般的ＡＣＫ包是用来进行数据传输的确认的．

TCP的断开 四次挥手

ＴＣＰ的连接是全双工的，每个方向都需要进行单独的关闭，就是发送一个ＦＩＮ的包，收到Ｆ包则这个方向上不能收到数据了　但是自己可以发送数据．

第一次挥手：客户端发送一个ＦＩＮ包给服务器，

第二次挥手：服务器收到后发送一个ＡＣＫ进行确认

第三次挥手：服务器发送一个ＦＩＮ包给客户端

第四次挥手：客户端进行数据位的确认并发送一个ＡＣＫ给服务器



![img](https://img-blog.csdn.net/20160219154603439?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

为什么需要三次握手四次挥手????少几次可不可以？？？？

三次握手可以防止已经失效的请求进行连接，比如一个已经逗留的很长时间的请求进入服务器，那么服务器就直接返回一个包，说自己可以建立连接，但是客户端没有请求建立连接，两次握手的话，客户端无法给服务器一个提示说自己没有请求建立连接．





应用层;

​	ＤＮＳ域名解析　ＦＴＰ　ＷＷＷ　等

应用层协议

P2P的模式可以使得计算机既是客户端又是服务端

套接字是应用层到传输层的一个通用的接口



可靠数据传输

吞吐量  **带宽敏感传输 ** 一般的多媒体





***

### 计算机网络自顶向下方法

***

路由器也就是进行数据包的转发的作用.

协议一般控制着因特网中的信息的接受和传送

socket是应用程序进程和运输层协议之间的接口.



选择协议的方式:

​	可靠数据传输   一般属于对数据比较敏感的 比如银行 邮件等等  而像视频音频等可以容忍丢失 

​	吞吐量  可用吞吐量 就是发送进程能够像接受进程交付比特的速率

​	定时和安全性

HTTP是应用层的协议

常用的应用程序协议:

​	UDP 和TCP协议  

TCP包括面向连接服务和可靠数据传输服务

- 面向连接的服务在进行数据传输之前需要先建立连接,通过握手建立连接,之后全双工发送数据,结束之后拆除连接.
- 可靠数据传输 **能够无差错的按顺序的进行数据的交付**

TCP还有拥塞控制机制

TCP和UDP都没有任何的加密机制  因此在应用层上进行的数据的加密 SSL加密



UDP是无连接的  因此在进行数据传输之前是不存在任何的握手过程的.

UDP不会保证数据的传输顺序和是否到达



***

电子邮件的主要应用层协议是SMTP

***

web的应用层协议是超文本传输协议HTTP   使用的是TCP作为运输协议   HTTP属于一个无状态协议 就是不会记住客户端以前是否提交过请求



***

HTTP的报文有两种 第一种是请求报文 第二个是相应报文

???????????????????????????????????????????????????????????

GET  请求行

HOST 首部行

Connection

User-agent

accept-language

***

web在进行连接的时候 首先进行连接的是WEB的缓存器 同时会发送cookie进行用户身份的判别