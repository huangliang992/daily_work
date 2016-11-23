# 如何改项目的facet与compiler不匹配问题和web module 版本不匹配问题

## 1. 先进project facet查看配置进行更改
## 2. 如果不行，进项目的.setting文件夹下找到project.facet.xml修改里面相对应的版本

	<?xml version="1.0" encoding="UTF-8"?>
	<faceted-project>
 	 <installed facet="java" version="1.8"/>
  	<installed facet="jst.web" version="2.4"/>
  	<installed facet="wst.jsdt.web" version="1.0"/>
  	<installed facet="jst.jsf" version="2.1"/>
  	<installed facet="jst.jaxrs" version="1.1"/>
	</faceted-project>
