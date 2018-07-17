#### 常见方式

***

##### sql注入:

 	一般在用户进行提交请求的时候,会在其中夹杂着恶意输入,而数据库的管理端并未对此进行筛选或者说对此进行剔除,导致提交的请求成为了命令而运行.

​	当程序使用动态构造sql语句来访问数据库的时候容易出现此入侵.

尽量不要使用动态拼装sql库  可以开启PHP的magic_quote_gpc 来进行输入字符的转义处理

htmlspecialchars()和addslashes()函数。这两个函数都是对特殊字符进行转义。

addslashes 对传进来的post get cookie 等其中的单引号和双引号进行转移处理,在其前面添加\进行处理. 但是反斜杠并没有写到数据库当中,因此入股magic未开启,使用了addslashes进行转移,就不用在提取的时候使用stripslashes()来对字符进行去斜杠处理了.

而后者则是对特殊字符进行处理,将特殊字符用实体替代

常见的:

 $sql="select * from users where username='$name' and password='$pwd'";

如果name为:’ or 1=1#

select * from users where username='' or 1=1#' and password=md5('')

等价于

elect * from users where username='' or 1=1

#是注释符

也就是嵌入了恶意表达式    or 1=1 来对其判断进行干扰使其丧失对主要身份验证信息的筛选和判断能力





***

防火墙是值得在本地和网路之间的对进出数据进行过滤和审查的一个软或者硬件

***
Cross Site Script 跨站脚本攻击 
一般是在web中插入恶意的html代码  xss可以盗取客户端的cookie 来实现对客户身份的冒充 实现欺诈性访问服务器
最一般的类型就是发送一个url+社会工程学来对客户进行攻击 为反射性xss
存储性xss 利用录入和修改数据的功能,将数据存入

一般利用输入功能 将一段js的代码插入到数据库之中.然后有客户进行访问的时候,系统会将代码段当做命令来执行,获取信息的脚本发送出来.
防御一般也和sql注入一样,采用字符转移进行,

服务器现将一个cookie发送给客户端,然后客户端在每次访问的时候都会带上这个标记
而一般的请记住我 这类选项就是基于cookie的机制进行实现的 就是在下次访问的时候 直接将cookie放在请求头 可以用来弥补HTTP无状态协议的缺点 使用cookie进行网站的跟踪.有两个http头部是专门负责设置以及发送cookie的,它们分别是Set-Cookie以及Cookie set一般是在客户端初次进行创建的时候使用的 之后再次进行访问的时候会自动将cookie的数据发送到服务端.

![Cookie Session 001](https://images.cnblogs.com/cnblogs_com/andy-zhou/811282/o_Cookie_Session001.png)

获取有cookie的内容的方式



Cookie具有不可跨域名性





