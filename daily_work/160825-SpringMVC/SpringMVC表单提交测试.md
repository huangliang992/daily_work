## Spring MVC 表单提交
### 1. Model User.java
	package org.hainan.cs.bean;
	import java.util.Date;
	import java.util.List;
	
	public class User {
	    
	    private long id;
	    
	    private String name;
	
	    private Date createTime;
	    
	    private boolean girl;
	    
	    private String[] cbx;
	  
	    private int age;
	    
	
	    private String email;
	    
	    public long getId() {
	        return id;
	    }
	    public void setId(long id) {
	        this.id = id;
	    }
	    public String getName() {
	        return name;
	    }
	    public void setName(String name) {
	        this.name = name;
	    }
	    public Date getCreateTime() {
	        return createTime;
	    }
	    public void setCreateTime(Date createTime) {
	        this.createTime = createTime;
	    }
	    public boolean isGirl() {
	        return girl;
	    }
	    public void setGirl(boolean girl) {
	        this.girl = girl;
	    }
	    public String[] getCbx() {
	        return cbx;
	    }
	    public void setCbx(String[] cbx) {
	        this.cbx = cbx;
	    }
	    public int getAge() {
	        return age;
	    }
	    public void setAge(int age) {
	        this.age = age;
	    }
	    public String getEmail() {
	        return email;
	    }
	    public void setEmail(String email) {
	        this.email = email;
	    }
	
	} 
### 2. Controller TestPostController.java
	package org.hainan.cs.controller;
	
	import java.util.ArrayList;
	import java.util.Date;
	import java.util.List;
	
	import org.hainan.cs.bean.User;
	import org.springframework.stereotype.Controller;
	import org.springframework.validation.BindingResult;
	import org.springframework.validation.FieldError;
	import org.springframework.web.bind.annotation.ModelAttribute;
	import org.springframework.web.bind.annotation.RequestMapping;
	import org.springframework.web.bind.annotation.RequestMethod;
	import org.springframework.web.servlet.ModelAndView;
	
	
	@Controller
	@RequestMapping(value="/post")
	public class TestPostController {
	    
	    private static List<User> users = new ArrayList<User>();
	    {
	        //-----------------------------------------------
	        // 设置Entity
	        // -----------------------------------------------
	        users.add(new User());
	        User user = users.get(0);
	        user.setId(1);
	        user.setName("Robin");
	        user.setCreateTime(new Date());
	        user.setGirl(true);
	        user.setCbx(new String[] {"1", "2", "3"});
	        user.setAge(18);
	        user.setEmail("abcd@abc.com");
	        
	    }
	    
	    @RequestMapping
	    public ModelAndView index() {
	        ModelAndView view = new ModelAndView("view");
	        view.addObject("users", users);
	        return view;
	    }
	    
	    @RequestMapping(value="/addUser", method=RequestMethod.POST)
	    public ModelAndView addUser(@ModelAttribute User user, BindingResult result) {
	        ModelAndView view = new ModelAndView("redirect:/post");
	        
	        if(result.hasErrors()) {
	            List<FieldError> errors = result.getFieldErrors();
	            for(FieldError err : errors) {
	                System.out.println("ObjectName:" + err.getObjectName() + "\tFieldName:" + err.getField()
	                        + "\tFieldValue:" + err.getRejectedValue() + "\tMessage:" + err.getDefaultMessage());
	            }
	            view.addObject("users", users);
	            return view;
	        }
	        
	        user.setId(users.size() + 1);
	        users.add(user);
	        view.addObject("users", users);
	        return view;
	    }
	    
	}

### 3. View view.jsp
	<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
	<%@ page import="org.hainan.cs.bean.*" %>
	<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %> 
	<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt"%>
	<%@ taglib uri="http://java.sun.com/jsp/jstl/functions" prefix="fn" %>
	<%@ taglib prefix="st" uri="http://www.springframework.org/tags" %>
	<%@ taglib uri="http://www.springframework.org/tags/form" prefix="sf" %>
	<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
	<html>
	<head>
	<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
	<title>Index</title>
	</head>
	<body>
	    <fmt:setLocale value="zh_cn" />
	    <form action="<st:url value="/post/addUser"></st:url>" method="post">
	        <c:forEach items="${users}" var="user">
	            User:${user.name}<br/>
	            Create time:<fmt:formatDate value="${user.createTime}"/><br/>
	            Is girl:
	            <c:choose>
	                <c:when test="${user.girl}">Yes</c:when>
	                <c:when test="${!user.girl}">No</c:when>
	                <c:otherwise>N/A</c:otherwise>
	            </c:choose>
	            <br/>
	            Checkboxs:
	            <c:forEach items="${user.cbx}" var="item">
	                ${item},
	            </c:forEach>
	            <br/>
	            Age:${user.age}<br/>
	            E-mail:${user.email}<br/>
	            <hr/>
	            <hr/>
	        </c:forEach>
	        
	        User name:
	        <input type="text" name="name" id="name" /><br/>
	        Is girl:
	        <input type="radio" name="girl" id="isGirl" value="true" checked="checked" /><label for="isGirl">Yes</label>
	        <input type="radio" name="girl" id="noGirl" value="false" /><label for="noGirl">No</label><br/>
	        Checkboxs:
	        <input type="checkbox" name="cbx" id="cbx1" value="1" /><label for="cbx1">1</label>
	        <input type="checkbox" name="cbx" id="cbx2" value="2" /><label for="cbx2">2</label>
	        <input type="checkbox" name="cbx" id="cbx3" value="3" /><label for="cbx3">3</label>
	        <br/>
	        Age:<input type="text" name="age" id="age" /><br/>
	        E-mail:<input type="text" name="email" id="email" /><br/>
	        <input type="submit" value="add" />
	    </form>
	    <hr/>
	
	</body>
	</html>

### 4.结果
![](http://i.imgur.com/mXYXVrY.png)

![](http://i.imgur.com/ShHKOSj.png)

![](http://i.imgur.com/6UcQjhO.png)