## spring MVC 工作原理

![](http://i.imgur.com/EDSWNcD.jpg)

1. 用户提交请求 url连接或者http请求
2. DispatcherServlet《web.xml中》找 XXX-servlet.xml即handlerMapping
3. 在XXX-servlet.xml中找到相应的包，在包中找controller
4. 找到相应的controller类后
5. 依据requestmapping，返回ModelAndView（这是逻辑视图）例如new ModelAndView（"hello","message",message）hello对应hello页面，"message"表示该页面中用来接收本类中message变量的变量
6. viewResolver 将逻辑视图转化为正式视图（加前缀和后缀）
7. 返回视图给用户