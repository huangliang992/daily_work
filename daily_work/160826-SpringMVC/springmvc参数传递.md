## Spring MVC 参数传递
### 页面向后台传值

1,使用HttpServletRequest获取

	@RequestMapping("/login.do")  
	public String login(HttpServletRequest request){  
	    String name = request.getParameter("name")  
	    String pass = request.getParameter("pass")  
	}  
2,Spring会自动将表单参数注入到方法参数，和表单的name属性保持一致。和Struts2一样

	@RequestMapping("/login.do")  
	public String login(String name, @RequestParam("pass")String password) // 表单属性是pass,用变量password接收  
	{  
	   syso(name);  
	   syso(password)  
	}  

3,自动注入Bean属性

	<form action="login.do">  
	用户名：<input name="name"/>  
	密码：<input name="pass"/>  
	<input type="submit" value="登陆">  
	</form>  
	  
	//封装的User类  
	public class User{  
	  private String name;  
	  private String pass;  
	} 
 
	@RequestMapping("/login.do")  
	public String login(User user)  
	{  
	   syso(user.getName());  
	   syso(user.getPass());  
	} 


### 后台向页面传值
1. 使用ModelAndView

		@Controller
		@RequestMapping(value = "/demand")
		public class DemandToClass {
		
			private String temp = "";
		
			@RequestMapping(value = "/addDemand", method = RequestMethod.POST)
			public ModelAndView addDemand(String demand) {
				System.out.println(demand);
				temp = demand;
				return new ModelAndView("redirect:/demand");
			}
		
			@RequestMapping
			public ModelAndView index() {
				return new ModelAndView("demandToModel", "message", temp);
			}
		}


	传递多个值：

	mav.addObject("message1", message1); 
	mav.addObject("message2",message2);


2. 使用session或者ModelMap


		@RequestMapping("/login.do")
		public String login(String name,String pwd, ModelMap model,HttpServletRequest request){  
		     User user = serService.login(name,pwd);
		     //使用session传值  
		     HttpSession session = request.getSession();  
		     session.setAttribute("user",user); 
			//使用ModelMap传值 
		     model.addAttribute("user",user);  
		     return "success";  
		}  

## action写法

	<form action="<st:url value="/post/addUser"></st:url>" method="post">或者使用action="/addUser.do"