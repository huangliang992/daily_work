## js代码自动查错的插件？

目前没有找到

## 一个页面中多个表单提交到同一个Action类
设置用不同的action名字和方法处理就行了

	<action name="GetBig" class="com.hainu.cs.action.GetBigPictureAction" method="execute"> 
	<result name="SUCCESS">getbigpicture.jsp</result>
	</action>
	
		<action name="GetBig1" class="com.hainu.cs.action.GetBigPictureAction" method="executeb">
	<result name="SUCCESS">getbigpicture.jsp</result>
	</action>
	
			<action name="GetBig2" class="com.hainu.cs.action.GetBigPictureAction" method="executea">
	<result name="SUCCESS">getbigpicture.jsp</result>
	</action>
获取 big picture的效果

![](http://i.imgur.com/i4uz16r.png)