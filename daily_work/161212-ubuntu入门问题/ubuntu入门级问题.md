##修改当前用户为root权限
在ubuntu系统新建的时候就应该修改root的密码sudo root passwd
###1.输入sudo gedit /etc/passwd 进入如下页面
![](http://i.imgur.com/5OX8G0H.jpg)
###2.修改用户hl的两个1000为0，然后重新登陆
![](http://i.imgur.com/msJ2rhB.jpg)
###3.真的不要用这个方式瞎几把搞，被这百度经验帖害惨了
不要乱修改用户的权限
###4.改回来方式：
在登陆界面按shif+ctrl+f2进入终端，输入用户名密码，或者root的用户名和密码，得到root权限后。
输入vi /etc/lightdm/lightdm.conf
在该文件中输入：
    
	[SeatDefaults]
	greeter-session=unity-greeter
	user-session=ubuntu
	greeter-show-manual-login=true
	allow-guest=false    
注意不要输错了，[SeatsDefaults]不要输成了Set。   
重新启动ubuntu后，发现登陆界面有login选项，输入root用户名和root密码，进入，然后gedit /etc/passwd改回来就好了。
###5.普通用户和root用户切换的问题
（1）加sudo 都知道的
（2）sudo su 输入密码后就切换成root用户了，    
![](http://i.imgur.com/YkVlMsT.jpg)
（3）su user可以切换回普通用户，或者直接退出就行。

###6.删除用户
sudo userdel -r mysql 删除mysql用户

###7.终端修改文件的方法
进入文件后，按i插入和修改文件，按esc退出编辑，按q！退出，wq保存并退出。这是vim的方式，在之前按shift+：也可以输入命令退出，按esc不顶用，好像是没有vim的包。

###8.怎么设置.sh文件直接运行而非用编辑器打开
首先在新利德安装dconf-editor，然后在控制台输入dconf-editor，打开设置，将默认打开方式改为运行
![](http://i.imgur.com/lGpwTaX.jpg)

###9.安装.deb软件
sudo dpkg -i xx.deb 进行安装    
![](http://i.imgur.com/BZdAGfA.jpg)

安装完后，搜索软甲并添加桌面快捷方式    
![](http://i.imgur.com/rOBWDQg.jpg)