### web

***

一般的步骤，首先前端发送一个请求给服务器，一般用get（url）来访问网址，之后服务器进行回应，并给出访问后的信息。之间通过协议进行通讯，也就是HTTP协议进行通讯，后端一般得到的请求就是一组数据，url+协议+hostname+端口

MVC设计模式

![](/home/heisenberg/下载/17122850-e60fd55ef7ed48679f6785aa3ff80082.png)

最上面的一层直接面向用户的视图层（V），给用户一个操作的界面，是程序的外壳

最下面的一层，是核心的数据层（M）,也就是层序需要操作的数据和信息，

最中间的一层就是控制层，就是承接的作用，将用户层的需求转换成数据层的数据的显示，并将数据反向到用户层。



**MVC是一个做WEB的模式！！！！**

浏览器和服务器之间的传输协议是HTTP

***

#### socket编程

***

最典型的socket编程就是web的网络编程，浏览器获得用户输入的url 然后通过socket向服务器发送请求，服务器将浏览器的内容在通过socket返回到浏览器，浏览器再对其进行解析渲染等操作实现本地数据的显示。

常用的数据传输方式有两个，第一个是SOCK_STREAM 和SOCK_DGRAM

**SOCK_STREAM** 面向连接的传输，数据不会出现丢失，如果出现了丢失和数据的损坏，则会重新发送，但是效率很低，常见的http协议就是使用SOCK_STREAM传输数据。



**SOCKET_DGRAM** 无连接的传输数据，一方只管传输数据，一方只管接受，不管数据的丢失和损坏，常见的是就是QQ视频和语音等保证通讯效率尽可能小的延迟，因为尽管流失了一部分数据，等多声音有噪点或者有杂音。



***

```
int socket(int af, int type, int protocol);
```

**af**为地址族 就是IP地址 常用有AF_INET 和AF_INET6 表示ipv4与ipv6  127.0.0.1表示本机地址

**type**为传输协议就是上面的两种

**protocol**为传输协议，常用IPPROTO_TCP IPPTOTO_UDP 就是TCP协议和UDP协议

确定了套接字的各种属性之后，服务器要用bind将套接字与IP和端口绑定，这样流经此接口的数据才能给套接字使用，而客户端使用connect函数与服务器进行连接

bind（）函数原型

​	int bind（int socket ，struct sockaddr *addr，socklen_t addrlen）;

sock为socket文件描述符 

sock进行创建之后需要自己对实际的端口进行指配  也就是用来将虚拟变量和实际的向关联起来.



***

![Servlet 架构](https://www.runoob.com/wp-content/uploads/2014/07/servlet-arch.jpg)

servelt的位置

读取客户端发送的各种数据有隐形和显性,之后通过处理和访问数据库进行数据的交换的访问,之后将结果返回到客户端上去进行数据的显示.

包:javax.servlet  javax.servlet.http

创建接口的方法有三个:

```java
//Servlet的生命周期:从Servlet被创建到Servlet被销毁的过程
//一次创建，到处服务
//一个Servlet只会有一个对象，服务所有的请求
/*
 * 1.实例化（使用构造方法创建对象）
 * 2.初始化  执行init方法 一般只执行一次
 * 3.服务     执行service方法
 * 4.销毁    执行destroy方法
 */
public class ServletDemo1 implements Servlet {

    //public ServletDemo1(){}

     //生命周期方法:当Servlet第一次被创建对象时执行该方法,该方法在整个生命周期中只执行一次
    public void init(ServletConfig arg0) throws ServletException {
                System.out.println("=======init=========");
        }

    //生命周期方法:对客户端响应的方法,该方法会被执行多次，每次请求该servlet都会执行该方法
    public void service(ServletRequest arg0, ServletResponse arg1)
            throws ServletException, IOException {
        System.out.println("hehe");

    }

    //生命周期方法:当Servlet被销毁时执行该方法
    public void destroy() {
        System.out.println("******destroy**********");
    }
//当停止tomcat时也就销毁的servlet。
    public ServletConfig getServletConfig() {

        return null;
    }

    public String getServletInfo() {

        return null;
    }
}
```

servlet是servlet的配置文件,可以用一个或者多个init-param标签对servlet配置进行初始化,web在进行创建servlet实例对象的时候会将这些参数进行封装到servletCnfig对象中并在调用的时候servlet的init的函数的时候传给servlet,从而通过servlet获得当前的初始化的内容.

日常将笔记坐上,就是为了green all the days