## 为什么用session
应为session在页面刷新后，里面的值还是保留的。避免了之前那样，action类中属性代码重复

## 获取session
<s:property value="#session.filepath"/> 

需要在页面获取session值当作一个JS参数，于是就在JS中写了这般代码

var a = '<s:porperty value="#session.xx"/>'

然而事实上在页面上并没有获取这个参数，检查发现是session没有取得。

并且奇怪的是，刷新页面即可获得session。

难道是S标签的问题？

但是当我在js中写下如此代码测试

var b = <%=session.getAttribute("xx")%>

以及

var c = <s:porperty value="#request.xx"/>

却是能获取数据的！

这令我很不解，于是我把获取session的语句

<s:porperty value="#session.xx"/>

放在了JSP中测试能否取得值，却惊讶的发现可以，不刷新也可取得session。




对于这样的情况，我很不理解是什么原因造成的，因为OGNL表达式里面也提到

	#session.xx  和 session.getAttribute("xx")是相同的，难道是标签的问题？


## 解决方法
通过hiden传值
前提是，在body中能显示session的值

	<s:property value="#session.filepath"/>能显示
	<input type="hidden" id="fp"  value="<s:property value="#session.filepath"/>"/>
js中调用方法

	var fp = document.getElementById("fp").value;
