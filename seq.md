#### 常见方式

***

##### sql注入:

 	一般在用户进行提交请求的时候,会在其中夹杂着恶意输入,而数据库的管理端并未对此进行筛选或者说对此进行剔除,导致提交的请求成为了命令而运行.

​	当程序使用动态构造sql语句来访问数据库的时候容易出现此入侵.

尽量不要使用动态拼装sql库  可以开启PHP的magic_quote_gpc 来进行输入字符的转义处理

htmlspecialchars()和addslashes()函数。这两个函数都是对特殊字符进行转义。

addslashes 对传进来的post get cookie 等其中的单引号和双引号进行转移处理,在其前面添加\进行处理. 但是反斜杠并没有写到数据库当中,因此入股magic未开启,使用了addslashes进行转移,就不用在提取的时候使用stripslashes()来对字符进行去斜杠处理了.

而后者则是对特殊字符进行处理,将特殊字符用实体替

常见的:

 $sql="select * from users where username='$name' and password='$pwd'";

如果name为:’ or 1=1#

select * from users where username='' or 1=1#' and password=md5('')

等价于

elect * from users where username='' or 1=1

#是注释符

也就是嵌入了恶意表达式    or 1=1 来对其判断进行干扰使其丧失对主要身份验证信息的筛选和判断能力