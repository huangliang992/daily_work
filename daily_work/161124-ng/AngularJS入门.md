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

**7.过滤器和服务** ng中服务是函数或者对象，过滤器讲数据变成想要的形式，用|隔开，“aA | upercase（lowercase）”将aA变成大写或者小写输出。下面这个例子是通过服务定制的过滤器，讲十进制转化为十六进制，加了两个过滤器

		<!DOCTYPE html>
		<html>
		<head>
		<meta charset="utf-8">
		<script src="http://cdn.static.runoob.com/libs/angular.js/1.4.6/angular.min.js"></script>
		</head>
		<body>
		
		<div ng-app="myApp" ng-controller="myCtrl">
		<p>在获取数组 [255, 251, 200] 值时使用过滤器:</p>
		
		<ul>
		  <li ng-repeat="x in counts">{{x | myFormat | uppercase，然后变成大写}}</li>
		</ul>
		
		<p>过滤器使用服务将10进制转换为16进制。</p>
		</div>
		
		<script>
		var app = angular.module('myApp', []);
		app.service('hexafy', function() {
			this.myFunc = function (x) {
		        return x.toString(16);
		    }
		});
		app.filter('myFormat',['hexafy', function(hexafy) {
		    return function(x) {
		        return hexafy.myFunc(x);
		    };
		}]);
		app.controller('myCtrl', function($scope) {
		    $scope.counts = [255, 251, 200];
		});
		</script>
		
		</body>
		</html>

**不加 | uppercase 过滤器**

![](http://i.imgur.com/MQrfAby.jpg)

**加 | uppercase 过滤器**

![](http://i.imgur.com/3yT6vbd.jpg)

**8.ng的选择框的两种实现方式**一种是ng-repeat，另一种是ng-options，数据源可以是数组也可以是对象

		<!DOCTYPE html>
		<html>
		<head>
		<meta charset="utf-8">
		<script src="http://cdn.static.runoob.com/libs/angular.js/1.4.6/angular.min.js"></script>
		</head>
		<body>
		
		<div ng-app="myApp" ng-controller="myCtrl">
		
		<p>选择网站:</p>
		
		<select ng-model="selectedSite">
		<option ng-repeat="x in sites" value="{{x.url}}">{{x.site}}</option>
		</select>
		
		<h1>你选择的是: {{selectedSite}}</h1>
		
		</div>
		
		<script>
		var app = angular.module('myApp', []);
		app.controller('myCtrl', function($scope) {
		   $scope.sites = [
			    {site : "Google", url : "http://www.google.com"},
			    {site : "Runoob", url : "http://www.runoob.com"},
			    {site : "Taobao", url : "http://www.taobao.com"}
			];
		});
		</script>
		
		<p>该实例演示了使用 ng-repeat 指令来创建下拉列表，选中的值是一个字符串。</p>
		</body>
		</html>

这个例子选择的数据源是数组放在sites[]中，数组中存放的是对象，对象有两个属性一个是site另一个是url。采用的是ng-repeat实现

![](http://i.imgur.com/kffUNqs.jpg)


		<!DOCTYPE html>
		<html>
		<head>
		<meta charset="utf-8">
		<script src="http://cdn.static.runoob.com/libs/angular.js/1.4.6/angular.min.js"></script>
		</head>
		<body>
		
		<div ng-app="myApp" ng-controller="myCtrl">
		
		<p>选择一辆车:</p>
		
		<select ng-model="selectedCar" ng-options="y.brand for (x, y) in cars"></select>
		<p>你选择的是: {{selectedCar.brand}}</p>
		<p>型号为: {{selectedCar.model}}</p>
		<p>颜色为: {{selectedCar.color}}</p>
		
		<p>下拉列表中的选项也可以是对象的属性。</p>
		
		</div>
		
		<script>
		var app = angular.module('myApp', []);
		app.controller('myCtrl', function($scope) {
		    $scope.cars = {
		        car01 : {brand : "Ford", model : "Mustang", color : "red"},
		        car02 : {brand : "Fiat", model : "500", color : "white"},
		        car03 : {brand : "Volvo", model : "XC90", color : "black"}
		    }
		});
		</script>
		
		</body>
		</html>

![](http://i.imgur.com/apAGXM4.jpg)

这个例子选择的数据源是json对象放在cars{}中，采用的是ng-options实现

**9.ng事件显示和隐藏元素**

	<!DOCTYPE html>
	<html>
	<head>
	<meta charset="utf-8">
	<script src="http://cdn.static.runoob.com/libs/angular.js/1.4.6/angular.min.js"></script>
	</head>
	<body>
	
	<div ng-app="myApp" ng-controller="personCtrl">
	
	<button ng-click="toggle()">隐藏/显示</button>
	
	<p ng-hide="myVar">
	名: <input type=text ng-model="firstName"><br>
	姓: <input type=text ng-model="lastName"><br><br>
	姓名: {{firstName + " " + lastName}}
	</p>
	
	</div>
	
	<script>
	var app = angular.module('myApp', []);
	app.controller('personCtrl', function($scope) {
	    $scope.firstName = "John";
	    $scope.lastName = "Doe";
	    $scope.myVar = false;
	    $scope.toggle = function() {
	        $scope.myVar = !$scope.myVar;
	    }
	});
	</script>
	
	</body>
	</html>

![](http://i.imgur.com/2Nb2GgX.jpg)![](http://i.imgur.com/eZ3uUbu.jpg)


###（三）总结
jquery有的功能ng几乎都包含了，但可能个人习惯原因，我更喜欢jq的事件处理方式，虽然给我的感觉是ng的代码少了，但看起来真的有点让我不爽。个人认为单纯做页面级的应用，ng真的很强大，但要与后台交互的话，我觉得jquery以及它封装的ajax更好用（个人意见）。