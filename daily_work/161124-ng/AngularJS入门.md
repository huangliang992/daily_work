##AngularJS 入门

###（一）说明
例子都来源于[http://www.runoob.com/angularjs/angularjs-model.html](http://www.runoob.com/angularjs/angularjs-model.html "runoob.com"),根据该网站总结而来。

###（二）例子
**1.hello world的例子**，在输入框中输入姓名，会在h1标签中显示。当时正式看到这个强大的功能才选择用ng的，试想下如果不用ng的话，要实现这个功能，可能我们要加个提交按钮，通过js代码来显示，或者通过输入框焦点变化，同样写个js函数实现，总之没它那么方便。
	
		<!DOCTYPE html>
		<html>
		<head>
		<meta charset="utf-8">
		<script src="http://cdn.static.runoob.com/libs/angular.js/1.4.6/angular.min.js"></script>
		</head>
		<body>
		<div ng-app="">
		  <p>名字 : <input type="text" ng-model="name"></p>
		  <h1>Hello {{name}}</h1>
		</div>
		</body>
		</html>

![](http://i.imgur.com/wzkhXE4.jpg)

**2.计算总价例子**，ng-app 初始化一个ngjs应用程序，对这个例子而言，这个应用程序在div范围类， ng-modle将元素值绑定到应用程序，比如数量输入框的值绑定到了应用程序的quantity变量， ng-init对变量初始化

		<!DOCTYPE html>
		<html>
		<head>
		<meta charset="utf-8">
		<script src="http://cdn.static.runoob.com/libs/angular.js/1.4.6/angular.min.js"></script> 
		</head>
		<body>
		
		<div ng-app="" ng-init="quantity=1;price=5">
		
		<h2>价格计算器</h2>
		
		数量: <input type="number" ng-model="quantity">
		价格: <input type="number" ng-model="price">
		
		<p><b>总价:</b> {{quantity * price}}</p>
		
		</div>
		
		</body>
		</html>

![](http://i.imgur.com/dfSccTi.jpg)

**3.重复打印数组中的内容**

		<!DOCTYPE html>
		<html>
		<head>
		<meta charset="utf-8">
		<script src="http://cdn.static.runoob.com/libs/angular.js/1.4.6/angular.min.js"></script> 
		</head>
		<body>
		
		<div ng-app="" ng-init="names=[
		{name:'Jani',country:'Norway'},
		{name:'Hege',country:'Sweden'},
		{name:'Kai',country:'Denmark'}]">
		
		<p>循环对象:</p>
		<ul>
		  <li ng-repeat="x in names">
		  {{ x.name + ', ' + x.country }}</li>
		</ul>
		
		</div>
		
		</body>
		</html>

![](http://i.imgur.com/zD3rnzg.jpg)

**4.双向绑定** name初始化在控制器中进行，和之前用ng-init不一样，一般推荐是这种模式，符合mvc的模式，m（model）是scope范围内的数据，v（view）是展示的html元素，c（controller）控制器。数据的计算和展示在{{}}内通过表达式完成，还有$rootScope定义的值可以在所有的controller中使用。$scope 对象其实是调用了控制器

		<!DOCTYPE html>
		<html>
		<head>
		<meta charset="utf-8">
		<script src="http://cdn.static.runoob.com/libs/angular.js/1.4.6/angular.min.js"></script> 
		</head>
		<body>
		
		<div ng-app="myApp" ng-controller="myCtrl">
		名字: <input type="text" ng-model="name">
		<h1>你输入了: {{name}}</h1>
		</div>
		
		<script>
		var app = angular.module('myApp', []);
		app.controller('myCtrl', function($scope) {
		    $scope.name = "John Doe";
		});
		</script>
		
		<p>修改输入框的值，标题的名字也会相应修改。</p>
		
		</body>
		</html>

![](http://i.imgur.com/6yLecOB.jpg)

**5.邮箱验证**

		<!DOCTYPE html>
		<html>
		<head>
		<meta charset="utf-8">
		<script src="http://cdn.static.runoob.com/libs/angular.js/1.4.6/angular.min.js"></script> 
		</head>
		<body>
		
		<form ng-app="" name="myForm">
		    Email:
		    <input type="email" name="myAddress" ng-model="text">
		    <span ng-show="myForm.myAddress.$error.email">不是一个合法的邮箱地址</span>
		</form>
		
		<p>在输入框中输入你的邮箱地址，如果不是一个合法的邮箱地址，会弹出提示信息。</p>
		
		</body>
		</html>

![](http://i.imgur.com/HIvoLgA.jpg)

**6.控制器方法** 控制器里面的方法，控制器也可以写在外部的js文件中通过`<script src="xx.js"></script>`引用该文件中的控制器

		<!DOCTYPE html>
		<html>
		<head>
		<meta charset="utf-8">
		<script src="http://cdn.static.runoob.com/libs/angular.js/1.4.6/angular.min.js"></script>
		</head>
		<body>
		
		<div ng-app="myApp" ng-controller="personCtrl">
		
		名: <input type="text" ng-model="firstName"><br>
		姓: <input type="text" ng-model="lastName"><br>
		<br>
		姓名: {{fullName()}}
		
		</div>
		
		<script>
		var app = angular.module('myApp', []);
		app.controller('personCtrl', function($scope) {
		    $scope.firstName = "John";
		    $scope.lastName = "Doe";
		    $scope.fullName = function() {
		        return $scope.firstName + " " + $scope.lastName;
		    }
		});
		</script>
		
		</body>
		</html>

![](http://i.imgur.com/EClcrzI.jpg)