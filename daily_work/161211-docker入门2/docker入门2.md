#Docker安装Tomcat和Nginx
##1.安装tomcat
使用docker pull tomcat,从docker hub 中拉取tomcat镜像，注意好像Tomcat8要求角度看、版本在jdk8以上。
使用docker run -p 8080:8080 --name mytomcat -d tomcat:latest 来运行容器。
![2016-12-12 15:29:41屏幕截图.png](/home/hl/图片/2016-12-12 15:29:41屏幕截图.png)
在浏览器输入localhost:8080查看
![2016-12-12 15:30:52屏幕截图.png](/home/hl/图片/2016-12-12 15:30:52屏幕截图.png)


##2.安装nginx
同样docker pull nginx。使用docker run -p 80:80 --name mynginx -d nginx:latest 运行容器
![2016-12-12 15:25:07屏幕截图.png](/home/hl/图片/2016-12-12 15:25:07屏幕截图.png)
在浏览器输入localhost:80可看到
![2016-12-12 15:26:33屏幕截图.png](/home/hl/图片/2016-12-12 15:26:33屏幕截图.png)

##3.查看当前容器镜像和启动情况
安装完后可查看当前所有镜像
![2016-12-12 15:20:10屏幕截图.png](/home/hl/图片/2016-12-12 15:20:10屏幕截图.png)
使用docker ps查看容器运行情况

![2016-12-12 15:33:39屏幕截图.png](/home/hl/图片/2016-12-12 15:33:39屏幕截图.png)

##4.通过Eclipse管理容器
Eclipse的Docker tool可以管理docker的容器和镜像，包括启动容器，重启容器等。 但目前并不支持run as + docker 里的Tomcat一键设置。
![2016-12-12 15:48:25屏幕截图.png](/home/hl/图片/2016-12-12 15:48:25屏幕截图.png)
