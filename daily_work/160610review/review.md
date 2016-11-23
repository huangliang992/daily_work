## UML网站建站过程回顾
### 1. 从文件上传功能开始
因为采用了struts 2 框架， 所以jsp页面中引入`<%@ taglib prefix="s" uri="/struts-tags"%>`taglib 是标签库 prefix是前缀 uri是同一资源标识符
文件上传采用表单提交的方式

	<s:form action="xx" method="post" type="multipart/form-data" theme="simple">
		<s:file name="xx"></s:file>
		<s:submit value="submit"></s:submit>
	</s:form>

采用post方式提交，multipart是多种部分的意思，表示表单中的数据类型是多种类型的，theme是表单的风格，不加simple会有他的默认风格，不好看。

action会被struts拦截，struts 根据action生成相应的action代理类，里面会有提交的数据，然后根据struts 2 中设置的class="xx.action"把，数据传递给它，action中要有相应的get和set方法才行，属性的名称也得相同。然后再action类中队文件进行存储。

	属性： File f;

	方法：execute(){
		FileInputStream fs=new FileInputStream(f);
		//获取上传的路径,服务器upload文件夹的路径
		String path=ServletActionContext.getServletContext.getRealPath("/WEB-INF/upload");
		//上传后文件的路径,用File.serperator替代"/"是为了window和linux系统路径不同问题
		String filepath=path+File.serperator+path;
		//获取session 将filepath存到session里
		ActionContext ac=ActionContext.getContext()
		Map session=ac.getSession();
		session.put("filepath",filepath)
		//别的类中调用是session.get("filepath")
		//将文件上传写到上传的文件中
		FileOutputStream fo=New FileOutputStream(filepath);
		byte[]b = new byte[1024];  
        int len = 0;  
		//一行一行读取文件，一行是1024字节
        while((len=in.read(b))>0){  
            out.write(b,0,len);  
        }  
        out.close();  
	}

上传完了后得对文件进行解析，展示成图形，这里用到了jsUML这个插件，插件就不介绍了。通过XMLManagerFull类解析出文件中的类，各种关系，并保存成了各自的HashMap，在FileUploadAction类中将HashMap转为JSON字符串，然后传递给jsp页面中的js供调用。传递采用了
hiden的方法，因为直接在js中通过 `var t='<s:propert value="JSONStr" escape="false">;`
不知道为啥有时候获取不到，但在body中能`<s:property value="JSONStr" escae="false">`打印出来。
**hiden 方法：**
	
	<input id="str" type="hiden" value="<s:property value="JSONStr" >">
	
	js中调用如下
	
	var t=document.getElementById("str").value;
	//JSON字符串转JSON对象,必须加括号因为直接来{会被当成函数
	var json=eval("("+t+")")
	for(var k in json){}//可遍历JSON中所有键值，key是键
	var v=json[k];
	var v1=json.k;//都可以获取出k对应的值

### 2.其他部分
其他部分无非就是传值，处理，返回给jsp了，和upload其实都差不多，以findpath为例。
首先，在jsp页面中要输入begin和end类采用`<s:textfield name="begin">`传值，action类中接受并处理。返回`<s:property value="result" escape="false">`.result是字符串，可以包含html中的DOM元素，加上escape="false"是为了能让字符串中这些元素显示出来。
在action类中可以将`"\n"`替换成`"<br>"`,方法是replaceAll("\n","<br>");

### 3.文件下载部分
文件下载部分是将upload文件夹下所有的文件以列表的形式展示在页面上，并提供下载。
首先谈一个文件的下载，用户点击一个链接，`<a href="xxAction?filename=<%="xx"%>">xx</a>`实现了将filename=xx传递给了xxAction类.接下来就是action类中返回对应的这个文件了。

	public InputStream getFileDownload() {
			//this.fileName = "example.uml" ;
			
			System.out.println(this.fileName);
			String fp="/WEB-INF/upload/"+this.fileName;
		return ServletActionContext.getServletContext().getResourceAsStream(fp) ;
		}


struts 中设置如下，type="stream"是文件下载用的，下面几个param 中name也是对应的stream类中的属性，fileName是指显示的的下载框中文件的名字，attachment是显示下载框。fileDownload对应action类中的getFileDownload方法，名称要一致。action中也要有fileDownload方法。

	<action name="Download" class="com.hainu.cs.action.DownloadAction">
	<result name="SUCCESS" type="stream">
		<param name="contentType">text/plain</param>
		<param name="contentDisposition">attachment;fileName="${fileName}"</param>
		<param name="inputName">fileDownload</param>
		<param name="bufferSize">1024</param>
	</result>
	</action>	

我采用的返回值是带DOM标签的字符串result，在result中生成了表格。

		@Override
		public String execute() throws Exception {
			String path = ServletActionContext.getServletContext().getRealPath("/WEB-INF/upload");
			ReadDirectory rd=new ReadDirectory();
			rd.readFile(path);
			fileListJSON=rd.getFileStr();
			System.out.println(fileListJSON);
			result="<table class=\"table\" border=\"1\">"+"<tr><td>File Name</td><td>File Path</td>"
					+ "<td>Download File</td></tr>";
			HashMap<String,String> l=rd.getList();
			for(Map.Entry<String, String>entry:l.entrySet()){
				String key=entry.getKey();
				String value=entry.getValue();
				//<a href="Download.action?fileName=<%="test1.uml"%>">test</a>
				String down="";
				System.out.println(key);
				down=down+"<a href=\"Download.action?fileName="+key+"\"> Download</a>";
				System.out.println(down);
				result=result+"<tr><td>"+key+"</td><td>"+value+"</td><td>"+down+"</td></tr>";
			}
			result=result+"</table>";
			return "SUCCESS";
		}

### 4.struts 2 标签的介绍
**s:include** :一个页面包含另一个页面或者servlet

**s:form** ：表单

**s:textfield** ：文本

**s:bean** ：javabean 没有用过

**s:file** ：文件

**s:submit** ：提交

**s:iterator** ：迭代，接收action传递的数组，以后不会用这个了，直接在action类中创造字符串带标记的

		<s:iterator value="tranmatrix" status="rowstatus">
				<tr>
					<s:iterator value="node[#rowstatus.index]">
						<td><s:property /></td>
					</s:iterator>
					<s:iterator value="tranmatrix[#rowstatus.index]">
						<td><s:property /></td>
					</s:iterator>
				</tr>
			</s:iterator>

**s:property** ：获取action或session中的值

**s:url** 和 **s:a** ：

            //第一种方式,在标签内使用标签时用%
         <s:a href="userAction!loadUser.action?user.id=%{id}">编辑</s:a> 
	  
           //第二种方式:使用<s:url>标签解决  
 	    <s:url id="idUrl" action="userAction!delUser.action">
		   <s:param name="user.id" value="%{id}"></s:param>
		</s:url> 
		<s:a href="%{idUrl}">删除</s:a>

               //第三种:直接加入
		<a href="<s:url action="userAction!delUser.action">
                    <s:param name="user.id" value="id"/>
                 </s:url>">删除2
        </a>		



**s:param** ：参数设置 `<s:param name="xx" value="xxx">`

### 5.EL和OGNL

EL表达式： 
>>单纯在jsp页面中出现，是在四个作用域中取值,page,request,session,application.
>>如果在struts环境中，它除了有在上面的四个作用域的取值功能外，还能从值栈(valuestack)中取值.
>>特点1：${name}，name在值栈中的查找顺序是：先从对象栈中取，取到终止，否则,向map中取。    
>>特点2：在对象栈的查找顺序是，先从model中找是否有name这个属性，找到终止，否则，找action中是否有name这个全局变量。    
>>特点3：${#name}，里面的是不带#号的。    
>>特点4：如果放在对象栈中的是一个自定义的对象，那么${property}里面可以直接去该对象的属性值，不用这样${object.property}


OGNL表达式:
>>1：读取从后台传递的值    
%{#name}:表示从值栈的map中取值    
%{name}:表示从值栈的对象栈中取值  
%{#request.name}:表示从request域中取值

>>2：自己构建数据    
  a，构建Map<s:iterator var="map" value="#{'key1':'value1','key2':'value2'}"/>    
  b，构建List<s:iterator var="list" value="{'one','two','three'}">


用法区别：OGNL是通常要结合Struts 2的标志一起使用，如<s:property value="#xx" /> struts页面中不能单独使用，el可以单独使用 ${sessionScope.username} ${requestScope.user.username}

![](http://i.imgur.com/qiskZNH.png)
# ognl讲解

OGNL是Struts 2默认的表达式语言。是Object Graphic Navigation Language（对象图导航语言）的缩写，它是一个开源项目。

 1．#符号的用途一般有三种。    
1)访问非根对象属性，例如示例中的#session.msg表达式，由于Struts 2中值栈被视为根对象，所以访问其他非根对象时，需要加#前缀。实际上，#相当于ActionContext.getContext();#session.msg表达式相当于ActionContext.getContext().getSession(). getAttribute(”msg”) 。     
2)用于过滤和投影（projecting）集合，如示例中的persons.{?#this.age>20}。    
3)用来构造Map，例如示例中的#{’foo1′:’bar1′, ’foo2′:’bar2′}。

 

2．%符号 %符号的用途是在标志的属性为字符串类型时，计算OGNL表达式的值。如下面的代码所示： 构造Map


	<s:set name=”foobar” value=”#{’foo1′:’bar1′, ‘foo2′:’bar2′}” />  
	<p>The value of key “foo1″ is <s:property value=”#foobar['foo1']” /></p>  
	<p>不使用％：<s:url value=”#foobar['foo1']” /></p>  
	<p>使用％：<s:url value=”%{#foobar['foo1']}” /></p>  

	<s:set name=”foobar” value=”#{’foo1′:’bar1′, ‘foo2′:’bar2′}” />  
	<p>The value of key “foo1″ is <s:property value=”#foobar['foo1']” /></p>  
	<p>不使用％：<s:url value=”#foobar['foo1']” /></p>  
	<p>使用％：<s:url value=”%{#foobar['foo1']}” /></p>

3．$符号

$符号主要有两个方面的用途。    
在国际化资源文件中，引用OGNL表达式，例如国际化资源文件中的代码：reg.agerange=国际化资源信息：年龄必须在${min}同${max}之间。     
在Struts 2框架的配置文件中引用OGNL表达式，例如下面的代码片断所示：


	<validators>  
	    <field name=”intb”>  
	            <field-validator type=”int”>  
	            <param name=”min”>10</param>  
	            <param name=”max”>100</param>  
	           <message>BAction-test校验：数字必须为${min}为${max}之间！</message>  
	       </field-validator>  
	    </field>  
	</validators>

### 6.遇到的还没解决的问题

1.js封装与调试