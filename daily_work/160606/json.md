## JSON数据显示

<h3 class="text-primary">The JSON of all the classes in the class diagram is</h3>
<s:property value="classes" /><br><br>
<h3 class="text-primary">The JSON of all the Association in the class diagram is</h3>
<s:property value="ass" /><br>
<h3 class="text-primary">The JSON of all the Aggregation in the class diagram is</h3>
<s:property value="agg" /><br>
<h3 class="text-primary">The JSON of all the Dependency in the class diagram is</h3>
<s:property value="dp" /><br>
<h3 class="text-primary">The JSON of all the Generalization in the class diagram is</h3>
<s:property value="gl" /><br>
<h3 class="text-primary">The JSON of all the Composition in the class diagram is</h3>
<s:property value="cp" /><br>

## struts 2 action 类中属性的生命周期？

个人猜测，只能支持一次action跳转，就是说jsp页面提交了内容到action类处理后，action类中的属性可以共其他的jsp页面调用。但再一次提交后，原来action类中的属性值就没了。

笨方法：每个action类里面都加上各个页面需要用到的公共属性



