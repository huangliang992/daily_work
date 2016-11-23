## 问题：
1. js代码测试不能像java一样，在eclipse中自动检测变量之类？
2. 代码封装成JS，现在几乎很少写js都是在后台action中写好，吧值传给页面，包括生成页面html代码
3. 现在会的页面向action传值的方法只会两个，一个是通过struts 的表单标签，传text值。另一个是用过超连接`<a href="xxx.action?attribure=value">`,传的value是字符串
4. 特别不能理解的是js中所有的类型都用var来定义。
5. action 向js中传值的方法是，js中把数据类型变为JSON字符串，在html body中通过hidden属性的input标签接受JSON，然后js在调用input中的内容，解析JSON向下面这样。 


		<input type="hidden" id="fp"  value="<s:property value="#session.filepath"/>"/>

		var glr = document.getElementById("gl").value;