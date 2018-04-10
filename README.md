##AngularJS入门

###AngularJS为何而用?
克服HTML在构建应用的不足而设计，就是**通过新的属性和表达式扩展了HTML**。

---

####Hello World，以简单的例子认识AngularJS运用
AngularJS是一个JavaScript框架。它可通过&lt;script>标签添加到HTML页面。
[*源代码1*](./example/1.html){:target="_blank"}

		<!DOCTYPE html>
		<html ng-app>
		    <head>
		    	<meta charset="UTF-8" />
		        <title>Hello World</title>
		    </head>
		    <body>
		        Hello {{'World'}}!
		        <script src="../js/angular.min.js"></script>
		    </body>
		</html>

* 问题：与普通页面有什么不同？
* 问题：AngularJS标识自己的语法？
* 问题：数据如何显示和有几种方式？

**ng-app**: 当加载该页时，标记ng-app告诉AngularJS处理整个HTML页并引导应用。就是说所标记起内所有元素可以由AngularJS控制。

具体看这个指令，**app**顾名思义是这个“应用”，而以带有前缀**ng-**，就是扩展的HTML属性的AngularJS指令。类似如HTML5，可以添加自定义属性，前缀为**data-**。
*注：指令这个词就是表达AngularJS的带有前缀ng-属性*

**{{}}**:双大括号标记的表达式，上面例子的表达式是一个简单的字符串“World”，来把数据绑定到HTML。
AngularJS表达式很像JavaScript表达式：它们可以包含文字、运算符和变量。

		{{ 5 + 5 }} 或 {{ firstName + " " + lastName }}

进化上面例子：
[*源代码2*](./example/2.html){:target="_blank"}

		<!DOCTYPE html>
		<html ng-app>
		    <head>
		    	<meta charset="UTF-8">
		        <title>Hello World</title>
		    </head>
		    <body>
				大侠名字: <input type="text" ng-model="yourname" placeholder="World">
		        <br />
		        Hello {{yourname || 'World'}}!
		        <script src="../js/angular.min.js"></script>
		    </body>
		</html>

上面例子演示了双数据绑定，**ng-model**指令把元素值（比如输入域的值）绑定到应用程序，同样，也有单向数据绑定，**ng-bind** 指令把应用程序数据绑定到HTML视图。

*更多例子*
1. 使用**ng-bind**代替**{{}}**，实战中，多用**ng-bind**来展示数据（why?）。
[*源代码3*](./example/3.html){:target="_blank"}

		<!DOCTYPE html>
		<html ng-app>
		    <head>
		        <meta charset="UTF-8">
		        <title>Hello World</title>
		    </head>
		    <body ng-init="yourname='World'">
		        大侠名字: <input type="text" ng-model="yourname" placeholder="World">
		        <br />
		        Hello <span ng-bind="yourname">World</span><span>!</span>
		        <script src="../js/angular.min.js"></script>
		    </body>
		</html>

---

####Angular应用，MVVM模式带你飞

#####MVC模式与MVVM模式区别

![img2.png](./img/img2.png)

MVC模式

![img3.png](./img/img3.png)

MVVM模式

- 问题：如何用javascript获取并操作数据？

接下来解释AngularJS如何映射到模型-视图-控制器设计模式。

**模板**

模板是用HTML和CSS编写的文件，展现应用的视图。运用AngularJS指令给HTML添加新的元素、属性标记。

**应用程序逻辑**

应用程序逻辑是用JavaScript定义angular的控制器。

**模型数据**

模型是从AngularJS作用域对象的属性引申的。模型中的数据可能是Javascript对象、数组或基本类型，他们都属于AngularJS作用域对象（*作用域？*）。

[*源代码4*](./example/4.html){:target="_blank"}

		<!DOCTYPE html>
		<html>
		    <head>
		        <meta charset="UTF-8">
		        <title>Example</title>
		    </head>
		    <body ng-app="myApp">
		       <div ng-controller="myCtrl">
		            姓: <input type="text" ng-model="lastName" ng-keyup="print();"><br>
		            名: <input type="text" ng-model="firstName" ng-keyup="print();"><br>
		            <br>
		            姓名: {{lastName + firstName}}
		        </div>
		        <script src="../js/angular.min.js"></script>
		        <script>
		            var app = angular.module('myApp', []);
		            app.controller('myCtrl', function($scope) {
		                $scope.lastName= "王";
		                $scope.firstName= "小二";

		                $scope.print = function() {
		                    console.log('-------------------------');
		                    console.log('姓：' + $scope.lastName);
		                    console.log('名：' + $scope.firstName);
		                };
		            });
		        </script>
		    </body>
		</html>

AngularJS**模块（Module）**定义了AngularJS应用。--> **ng-app**指令定义了应用。
AngularJS**控制器（Controller）**用于控制AngularJS应用。 --> **ng-controller **定义了控制器。
*注：一个应用可以有多个控制器。*

---

####作用域

*作用：*作用域(scope)是应用在 HTML(视图)和JavaScript(控制器)之间的纽带。
*属性能力：*scope是一个对象，有可用的方法和属性。
*表现形式：*scope可应用在视图和控制器上。

#####作用域范围

一个控制器只有一个作用域；多个控制器就有各自作用域。

#####根作用域

所有的应用都有一个**$rootScope**，它可以作用在**ng-app**指令包含的所有HTML元素中。
**$rootScope**可作用于整个应用中，是各个**controlle**中**scope**的桥梁。用**rootscope**定义的值，可以在各个**controller**中使用。

[*源代码5*](./example/5.html){:target="_blank"}

		<!DOCTYPE html>
		<html>
		    <head>
		        <meta charset="UTF-8">
		        <title>Example</title>
		    </head>
		    <body ng-app="myApp">
		        <div ng-controller="myCtrlOne">
		            <h1>{{lastname}}家小字辈:</h1>
		            <ul>
		                <li ng-repeat="x in names">{{lastname}}{{x}}</li>
		            </ul>
		        </div>
		        <div ng-controller="myCtrlTwo">
		            <h1>{{lastname}}家大字辈:</h1>
		            <ul>
		                <li ng-repeat="x in names">{{lastname}}{{x}}</li>
		            </ul>
		        </div>
		        <script src="../js/angular.min.js"></script>
		        <script>
		            var app = angular.module('myApp', []);

		            app.controller('myCtrlOne', function($scope, $rootScope) {
		                $scope.names = ["小明", "小花", "小王"];
		                $rootScope.lastname = "李";
		            });

		            app.controller('myCtrlTwo', function($scope, $rootScope) {
		                $scope.names = ["大明", "大花", "大王"];
		            });
		        </script>
		    </body>
		</html>

#####MVC映射小结

![img1.png](./img/img1.png)

AngularJS应用程序由**ng-app**定义。应用程序在html内标签元素运行。
**ng-controller="myCtrl"**属性是一个AngularJS 指令，用于定义一个控制器。
**myCtrl**函数是一个JavaScript函数。
AngularJS使用**$scope**对象来调用控制器。
在AngularJS中，**$scope**是一个应用对象(属于应用变量和函数)。
控制器的**$scope**（相当于作用域、控制范围）用来保存AngularJS Model(模型)的对象。
控制器在作用域中创建了两个属性 (firstName 和 lastName)。
**ng-model**指令绑定输入域到控制器的属性（firstName 和 lastName）。

---

####其他指令

#####ng-repeat指令
**ng-repeat**指令对于集合中（数组中）的每个项会克隆一次HTML元素。

	*HTML:*
	<ul>
	  <li ng-repeat="x	in names">
	    {{ x.name + ', ' + x.country }}
	  </li>
	</ul>

	*JS:*
	$scope.names = [
		{name:'Jani',country:'Norway'},
		{name:'Hege',country:'Sweden'},
		{name:'Kai',country:'Denmark'}
	];


#####ng-show、ng-hide指令
**ng-show**、**ng-hide**指令隐藏或显示一个HTML元素。
	
	<p ng-show="true">我是可见的。</p>
	<p ng-show="false">我是不可见的。</p>

	<p ng-hide="true">我是不可见的。</p>
	<p ng-hide="false">我是可见的。</p>

#####ng-disabled指令
**ng-disabled**指令直接绑定应用程序数据到HTML的disabled属性。

	<button ng-disabled="true">点我!</button>

[更多指令 Angular API](http://docs.angularjs.cn/api/ng){:target="_blank"}

---

####自定义指令

* 问题：自定义指令作用与使用场景？

使用**.directive**函数来添加自定义的指令。
要调用自定义指令，HTMl元素上需要添加自定义指令名。在JS代码中使用驼峰法来命名一个指令，如**myDirective**，但在HTML上对应写成以**-**分割， 如my-directive:

[*源代码6*](./example/6.html){:target="_blank"}
	*HTML:*

	<body ng-app="myApp">
        <my-directive></my-directive>
    </body>

*JS:*

    angular.module('myApp', []).directive('myDirective', function() {
	    return {
	        restrict: 'E',
	        template: '<div>Hello World</div>',
	        replace: true
	    };
	});

从上面JS代码看到指令的配置，replace:true和template意思是用这个模板代替HTML中my-directive标签，restrict代替什么元素，其配置如下面标签形式：

| 字母 |   声明风格    | 示例  |
| ---- |---------------| ------|
| E    | 元素          | &lt;my-directive></my-directive> |
| A    | 属性          | &lt;div my-directive="title"></div> |
| C    | 样式类        | &lt;div class="my-directive"></div> |
| M    | 注释          | &lt;!-- directive : my-directive --> |

除了上面的配置之外，可以用templateUrl代替template，指向指令模板的URL地址，更好把页面标签元素写在独立HTML文件中。
配置项transclude:true，作用是取出自定义指令中的内容(就是写在指令里面的子元素)，以正确的作用域解析它,然后再放回指令模板中标记的位置(通常是ng-transclude标记的地方)。

[*源代码7*](./example/7.html){:target="_blank"}
	*HTML:*

	<body ng-app="myApp">
        <my-directive>
        	<br />
        	<span>保留我</span>
        </my-directive>
    </body>

*JS:*

    angular.module('myApp', []).directive('myDirective', function() {
	    return {
	        restrict: 'E',
	        template: '<div>Hello World<span ng-transclude></span></div>',
	        transclude: true
	    };
	});

接下来，操作指令标签。配置项有**compile**定义编译函数，这个阶段进行标签解析和变换，还有**link**链接函数，这个阶段进行数据绑定等操作。

[*源代码8*](./example/8.html){:target="_blank"}
*CSS:*
	
	.redFont {
        color: red;
        font-size: 16px;
        font-weight: bolder;
    }

*HTML:*

	<body ng-app="myApp">
        <my-directive></my-directive>
    </body>

*JS:*

    angular.module('myApp', []).directive('myDirective',function() {
        return {
            restrict: 'E',
            template: '<div ng-click="change();">点我！</div>',
            replace: true,
            compile: function(element, attributes) {
                return {       
                    post : function( scope, element, attributes ) {
                        scope.toggle = false;
                        scope.change = function(){
                            scope.toggle = !scope.toggle;
                            if( scope.toggle ){
                                element.html('已点击！');
                                element.addClass('redFont');
                            }else {
                                element.html('点我！');
                                element.removeClass('redFont');
                            }
                        };
                    }
                };  
            }
        };
    });

#####指令小结
从上面例子看来，没有使用jQuery，同样可以用AngularJS指令操作标签。*Think in AngularJS*就是用AngularJS方式思考问题，尽量不用jQuery和AngularJS混合使用，AngularJS内嵌了jQLite（它内部实现的一个jQuery子集），包含了常用的 jQuery DOM 操作方法，事件绑定等等，如果网页有载入jQuery，AngularJS会优先采用jQuery，否则会用jQLite。

####坑：修改了作用域的变量，界面没反应？
首先检查有没有错误，或者是调用$scope的变量是否正确。如果以上没有问题，大多数是$apply导致（正常AngularJS使用功能，系统调用$apply刷新数据），而直接调用了setTimeout函数、jquery的ajax操作和具有延时操作等等，那么系统就可能没有调用$apply，界面也就没有刷新了。如果之后有其他操作调用了$apply，把之前没有显示出来的数据，会正常执行并显示出来。

延时刷新的BUG:
[*源代码9*](./example/9.html){:target="_blank"}

	setTimeout(function(){
        $scope.lastName = "王";
        $scope.firstName = "小二";
    },1000);

解决问题，调用$apply方法：

	setTimeout(function(){
		$scope.$apply(function(){
			$scope.lastName = "王";
        	$scope.firstName = "小二";
		});
    },1000);

更好的方法，使用AngularJS内置$timeout，控制器调用依赖模块$timeout:

	setTimeout(function(){
		$timeout(function(){
			$scope.lastName = "王";
        	$scope.firstName = "小二";
		});
    },1000);

---

###网络资源与参考
1. [Angular教程](http://www.runoob.com/angularjs/angularjs-tutorial.html){:target="_blank"}
2. [Angular中文网](http://www.apjs.net/){:target="_blank"}
3. [Angular新手容易碰到的坑](http://www.ngnice.com/posts/2c8208220edb94){:target="_blank"}
4. [Angular API](http://docs.angularjs.cn/api/ng){:target="_blank"}
5. [AngularJS directive详解](http://my.oschina.net/u/1992917/blog/406421?fromerr=lfsuVNgy){:target="_blank"}
6. [Angular性能优化心得](https://segmentfault.com/a/1190000000502981){:target="_blank"}


