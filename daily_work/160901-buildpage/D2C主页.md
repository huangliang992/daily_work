## 从需求到类图页面布局

###1. 截图
主页

![](http://i.imgur.com/QnhgA8j.jpg)

功能页

![](http://i.imgur.com/j5n4rsC.jpg)

###2. 主页代码

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    	pageEncoding="ISO-8859-1"%>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <link href="layoutit/src/css/bootstrap.min.css" rel="stylesheet">
    <link href="layoutit/src/css/style.css" rel="stylesheet">
    <title>D2C-HomePage</title>
    </head>
    <body>
    	<div class="container-fluid">
    		<div class="row">
    			<div class="col-md-12">
    				<nav class="navbar navbar-default navbar-inverse navbar-fixed-top"
    					role="navigation">
    				<div class="navbar-header">
    
    					<button type="button" class="navbar-toggle" data-toggle="collapse"
    						data-target="#bs-example-navbar-collapse-1">
    						<span class="sr-only">Toggle navigation</span><span
    							class="icon-bar"></span><span class="icon-bar"></span><span
    							class="icon-bar"></span>
    					</button>
    					<a class="navbar-brand" href="index.jsp"><strong
    						style="font-size: 24px; color: #FFFFFF">Home</strong></a>
    				</div>
    
    				<div class="collapse navbar-collapse"
    					id="bs-example-navbar-collapse-1">
    					<ul class="nav navbar-nav">
    						<li><a href="demand">D2C</strong></a></li>
    						<li><a href="#">About us</a></li>
    						<li><a href="#">Tutorials</a></li>
    						<li><a href="#">News</a></li>
    					</ul>
    				</div>
    
    				</nav>
    				<div class="row" style="background: #338FCC; height: 450px">
    					<div class="col-md-2"></div>
    					<div class="col-md-6">
    						<div class="row">
    							<div class="col-md-6" style="margin-top: 100px" align="right">
    								<img alt="Bootstrap Image Preview"
    									src="${pageContext.request.contextPath}/img/logo.png" />
    							</div>
    							<div class="col-md-6" style="margin-top: 100px">
    								<h1
    									style="color: #FFFFFF; font-size: 56px; font-family: 微软雅黑; font-weight: bold">
    									D2C</h1>
    								<grammarly>
    								<div
    									class="_9b5ef6-textarea_btn _9b5ef6-offline _9b5ef6-anonymous _9b5ef6-not_focused">
    									<div class="_9b5ef6-transform_wrap">
    										<div class="_9b5ef6-status"></div>
    									</div>
    								</div>
    								</grammarly>
    								<p class="lead" style="color: #FFFFFF ;text-align:justify">
    									<strong style="color: #fbb450; font-size: 24px">Demand
    										to class diagram</strong> generates class diagrams from the demands
    									described by natural language. <br>
    									<br>
    								</p>
    								<div class="row">
    									<div class="col-md-6">
    
    										<button type="button" class="btn btn-danger btn-block">
    											Start now</button>
    									</div>
    									<div class="col-md-6">
    
    										<button type="button" class="btn btn-danger btn-block">
    											User guider</button>
    									</div>
    								</div>
    							</div>
    						</div>
    					</div>
    					<div class="col-md-4"></div>
    				</div>
    
    
    				<div class="row" style="background: #f5f5f5; height: 300px">
    					<div class="col-md-12">
    						<div class="row">
    							<div class="col-md-2"></div>
    							<div class="col-md-8" style="margin-top: 50px">
    								<h1 style="font-family: 微软雅黑; font-weight: bold">Know More
    									About D2C</h1>
    								<p style="font-size: 24px; font-family: Times ;text-align:justify">The demand of
    									a system is described in English. For example, "Teacher is
    									user. Teacher has name. Teacher buy book" is part demand of
    									book shopping system. We can generate the class diagram through
    									it.</p>
    								<button class="btn btn-primary">Know more</button>
    								<grammarly>
    								<div
    									class="_9b5ef6-textarea_btn _9b5ef6-offline _9b5ef6-anonymous _9b5ef6-not_focused">
    									<div class="_9b5ef6-transform_wrap">
    										<div class="_9b5ef6-status"></div>
    									</div>
    								</div>
    								</grammarly>
    							</div>
    							<div class="col-md-2"></div>
    						</div>
    					</div>
    				</div>
    
    				<div class="row">
    					<div class="col-md-12">
    						<div class="row">
    							<div class="col-md-2"></div>
    							<div class="col-md-8">
    								<br>
    								<h3
    									style="font-family: 微软雅黑; font-weight: bold; text-align: center">Getting
    									Start With D2C</h3>
    								<br>
    								<div class="row">
    									<div class="col-md-4">
    										<div class="row">
    											<div class="col-md-4">
    												<img alt="Bootstrap Image Preview"
    													src="${pageContext.request.contextPath}/img/notice.png"
    													width="80px" height="80px" />
    											</div>
    											<div class="col-md-8">
    												<h4>Notice</h4>
    												<p style="text-align:justify">Please use simple sentences to describe you demand.
    													Please do not use complex sentences and clauses.</p>
    											</div>
    										</div>
    									</div>
    									<div class="col-md-4">
    										<div class="row">
    											<div class="col-md-4">
    												<img alt="Bootstrap Image Preview"
    													src="${pageContext.request.contextPath}/img/system.jpg"
    													width="80px" height="80px" />
    											</div>
    											<div class="col-md-8">
    												<h4>System demand</h4>
    												<p style="text-align:justify">Now, you need to think about the demand of you
    													system, then, write it down by English and go to the next
    													step.</p>
    											</div>
    										</div>
    									</div>
    									<div class="col-md-4">
    										<div class="row">
    											<div class="col-md-4">
    												<img alt="Bootstrap Image Preview"
    													src="${pageContext.request.contextPath}/img/start.jpg"
    													width="80px" height="80px" />
    											</div>
    											<div class="col-md-8">
    												<h4>Start</h4>
    												<p style="text-align:justify">
    													Now, you can copy the system demand you wrote before and
    													input it in the text area in this page <a href="">here</a>.
    												</p>
    											</div>
    										</div>
    									</div>
    								</div>
    							</div>
    							<div class="col-md-2"></div>
    						</div>
    					</div>
    				</div>
    				<br> <br> <br>
    				<div class="col-md-12">
    					<p class="text-center">
    						copyright ©D2C 2015 ICP 15003200 <a
    							href="http://www.hainu.edu.cn/">Hai Nan University</a> | design
    						by Huang Liang
    					</p>
    				</div>
    			</div>
    		</div>
    	</div>
    </body>
    </html>