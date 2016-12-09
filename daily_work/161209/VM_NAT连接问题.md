##虚拟机联网问题
###NAT模式虚拟机ubuntu无法上网（突然）之前还有用
（1）改桥接模式可以上网了，但不是共享的主机的网络。因此主机开了vpn虚拟机也无法连上国外网站    
（2）依然用NAT模式：    
打开运行--运行service.msc

![](http://i.imgur.com/W8gxB7m.jpg)

找到VMware NAT Service和VMware DHCP Service，先右击VMware DHCP Service，点击“停止”，然后开启“VMware NAT Service”，再开启“VMware DHCP Service”。

![](http://i.imgur.com/evKYEdS.jpg)

现在有用了
