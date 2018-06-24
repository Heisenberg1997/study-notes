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