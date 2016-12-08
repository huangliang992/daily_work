##搭建linux开发环境
（1）安装vmware虚拟机    
（2）安装ubuntu    
下载镜像，装上去就可以了。然后换语言，更改桌面环境KDE（sudo apt-get install --no-install-recommends kubuntu-desktop）。关机的指令sudo shutdown -r now。看上去其他桌面环境并不好用。安装搜狗输入法。    
（3）安装Eclipse（STS）    
说个问题，STS 400M大小左右，从官网下每秒几k的速度不知道下到何年何月。用网上方法，将连接用百度云下载到网盘，再从网盘下载要块好多。
如果装的OpenJDK删掉装OrecalJdk，sudo add-apt-repository ppa:webupd8team/java，sudo apt-get update，sudo apt-get install oracle-java7-installer。STS自带有maven插件，不需要额外安装。TOmcat需要自己下载，然后添加到STS就行了。    
（4）安装Docker    
Eclipse自带有docker的插件叫Docker Tool可以管理docker。
（5）安装Tomcat
（6）安装Mysql
（7）安装Redis