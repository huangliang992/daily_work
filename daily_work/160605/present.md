### 1. 控制<s:property value="xxx"/>输出，比如xxx中间有换行符号		
<s:property value="cirresult" escape="false"/> 加上escape属性设置成false    
xxx中是"\n\r"的话， String result=result.replaceAll("\n\r",<br>);替换

### 2. 控制输出显示的字体，颜色之类的
使用bootstrap    
<s:form action="FindPath" theme="simple">主题要设置成simple    
<s:textfield name="begin" cssClass=""/>cssClass和styleClass都不行

以findpaths为例

	<body>
	<h2>Find the Paths between the Two Given Classes</h2>
	<s:form action="FindPath" theme="simple">
		<table class="table table-bordered">
			<tr class="info">
				<td>Input the begin class in the class diagram Class 1
				<s:textfield name="begin" styleClass="input-small"/></td>
			</tr>
			<tr class="info">
				<td>Input the end class in the class diagram Class 2 
				<s:textfield name="end" /></td>
			</tr>
			<tr class="info">
			<td><s:submit value="submit" /></td>
			</tr>
		</table>
	</s:form>
	<h4 class="text-primary">The paths between the begin class and end class are:</h4>
	<p class="text-success">
		<s:property value="result" escape="false"></s:property>
	</p>
	<br><hr>
	</body>

效果

![](http://i.imgur.com/3v0Sr3o.png)