##Docker 入门
###1.docker安装（ubuntu安装方法）
ubuntu安装docker可以通过wget方法，也可以通过新力德包管理器安装（我采用这种方法）选中docker-engine安装即可。建议够买vpn否则下载速度非常慢（我使用的vpn是LOCO VPN加速器）

![](http://i.imgur.com/pn6QhcI.jpg)

安装成功后用dockers version 查看

![](http://i.imgur.com/uSCYbHk.jpg)

如果出现server 部分信息没有，那是因为不是root用户执行命令`sudo usermod -aG docker （你的用户名）` 然后重新登陆ubuntu系统。即可。

###2.运行hello-world实例

![](http://i.imgur.com/PeYm2KA.jpg)

###3.Eclipse Docker tool 插件
Eclipse可以安装管理docker的插件，当本地docker安装成功后，eclipse可以直接连接上去。如果机器上docker没安装配置启动的话，连接不上。

![](http://i.imgur.com/J3jiiwJ.jpg)

###4.Docker 的hello world
运行命令 `docker ubuntu：16.04 /bin/echo "hello world"`，其中 docker run 表示运行一个容器， ubuntu：16.04 是镜像的名称，如果本地没有则会从远程的docker仓库中下载。因此非常建议用VPN保证下载的速度。/bin/echo "hello world"是在启动的容器里面执行的命令。

![](http://i.imgur.com/ZDIAH6p.jpg)

###5.docker 镜像的使用
执行`docker images`查看本机上docker镜像

![](http://i.imgur.com/wyY9mBr.jpg)

我们可以从docker hub网上找我们需要的镜像，也可以用命令来搜索，例如我要找mysql输入 `docker search mysql`即可

![](http://i.imgur.com/raFc8xe.jpg)

现在用`docker pull` 来下载mysql， `docker pull mysql`

![](http://i.imgur.com/lF3zg87.jpg)

下载完后，运行容器使用这个镜像 `docker run mysql`

![](http://i.imgur.com/isfqHx0.jpg)

通过docker ps 查看

![](http://i.imgur.com/ZxHCpFQ.jpg)

可是我用mysql -u root -p root 登陆不上去

![](http://i.imgur.com/UiIsRL8.jpg)

