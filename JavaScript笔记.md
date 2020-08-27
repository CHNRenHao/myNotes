#  介绍

1. Javascript是运行在浏览器上的脚本语言。简称JS。

   Javascript程序不需要我们程序员手动编译，编写完源代码之后，浏览器直接打开解释执行。

   JavaScript的”目标程序“以普通文本形式保存，这种语言都叫做”脚本语言”。

2. JS和JSP的区别

   JSP：JavaServer Pages（隶属于Java语言的，运行在JVM中）

   JS：Javascript(运行在浏览器中)

3. javascript包括三大块

   - ECMAScript:JS的核心语法(ES规范/ECMA-262标准)

   - DOM：documen object model

     对网页当中的节点进行增删改的过程。HTML被看作一棵DOM树来看待

   - BOM：browser object model

     关闭浏览器窗口，打开一个新的浏览器窗口，后退、前进、浏览器地址栏上的地址等，都是BOM编程

     > BOM的顶级对象是window，DOM的顶级对象是document，实际上BOM是包括DOM的

     ![image-20200706145747076](E:\笔记\JavaScript笔记.assets\image-20200706145747076.png)





# html中嵌入javascript的三种方式

## 事件驱动方式

1. js是一门事件驱动型的编程语言，依靠事件去驱动，然后执行对应的程序。在JS中有很多事件，其中有一个事件叫作：鼠标单击，单词：click。并且任何事件都会对应一个事件句柄叫作：onclick。

> 事件和事件句柄的区别是：事件句柄是在事件单词前添加一个on，事件句柄是以html标签的属性存在的

```html
<input type="button" value="hello" onclick="window.alert('hello.js')">
    
<!-- 页面打开的时候，js代码并不会执行，只是把这段js代码注册到按钮的click事件上了。等这个按钮发生click事件之后，注册在onclick后面的js代码会被浏览器自动调用-->
```

2. 怎么使用js代码弹出消息框？

   在js中有一个内置对象叫作window，全部小写，可以直接拿来使用，window代表的是浏览器对象。window对象有一个函数叫作：alert，用法是：`window.alert("消息")`；这样就可以弹窗了

3. js中的字符串可以用单引号，也可以用双引号

   js中的一条语句结束之后可以使用分号”；“，也可以不使用

## 脚本块方式

## 

1. 暴露在脚本块当中的程序，在页面打开的时候执行，并且遵守自上而下的顺序一次逐行执行(这个代码的执行不需要事件)

2. ```html
   <script type="text/javascript">
   	window.alert("hello javascript");
   </script>
   ```

## 引入外部独立的js文件

```html
<script type="text/javascript" src="js/1.js"></script>
```

1. 引入外部独立js文件的时候，js文件中的代码会遵循自上而下的顺序依次逐行执行

```html
<script type="text/javascript" src="js/1.js">
	alert("hello jack");		//引入文件时，这里不会执行
</script>
```







# 标识符和关键字

1. javascript是一种弱类型的语言

   ```javascript
   var i = 1;
   	i = false;
   	i = "abc";
   	i = new Object();
   	i = 3.14;   
   ```

2. 在js中，当一个变量没有手动赋值的时候，系统默认赋值undefined





# 函数

1. js中函数不需要指定返回值类型，返回什么类型都可以

   ```javascript 
   //方式一
   //标准形式
   function 函数名(形参列表){
       函数体
   }
   
   //方式二
   //非标准形式
   函数名 = function(形参列表){
       函数体；
   }  
   ```

2. 在浏览器去加载一个<script>时，首先将当前<script>标签中所有以【标准形式】声明的函数类型对象初始化，然后才会自上而下依次执行<script>标签中命令行

   ```html
   <script type="text/javascript">
   	function fun1(){
           alert("100");	//200
       }
       window.fun1();
       
       function fun1(){
           alert("200");	//200
       }
       window.fun1();
       
       var fun1 = function(){
           alert("300");
       }
       window.fun1();	//300
   </script>
   ```

3. 参数管理方式

   使用数组作为函数参数 

   JavaScript自动为每一个函数类型对象，分配一个属性【arguments】

   【arguments】是一个数组，在这个数组中保存当前函数运行时接收所有实参

4. 实现重载

   ```javascript
   //如果fun1接收了一个string类型实参，fun1负责打招呼
   //如果fun1接收了两个number类型实参，fun1负责加法运算
   function fun1(){
       if(arguments.length==1 && typeof arguments[0]=="string"){
           alert("Hello" + arguments[0]);
       }else if(arguments.length == 2 && typeof arguments[0]=="number"){
           alert("加法运算结果是" + (arguments[0]+arguments[1]));
       }
   }
   ```

5. 如何创建Object对象

   ```javascript
   1. var obj = new Object();
   2. var obj = {
      			    "name":"mike",
       			"age":23
   			  }
   3. function Student(param1,param2){
   	this.sid = param1;
       this.sname = param2;
   	}
      var sutObj = new Student(10,"mike")
   4. var myArray = []    
   ```

6. 用javascript实现hashmap

   ```javascript
   function HashMap(){
       var obj = new Object();
       this.put = function(key,value){
           obj[key]=value;
       };
       this.get = function(key){
           return obj[key];
       };
   }
   ```

   

# 变量 

## 全局变量

1. 在函数体之外声明的变量属于全局便令，全局变量的生命周期是：

   ​		浏览器打开时声明，浏览器关闭时销毁，尽量少用。因为全局变量会一直在浏览器内存当中，耗费内存空间。能使局部变量尽量使用局部变量

## 局部变量

1. 在函数体内声明的变量，包括一个函数的形参都属于局部变量，局部变量的声明周期是：

   ​		函数开始执行时局部变量的内存空间开辟，函数执行结束之后，局部变量的内存空间释放。局部变量声明周期较短。

> 注：当一个变量声明的时候没有使用var关键字，那么不管这个变量在哪里声明的，都是全局变量







# 数据类型

## 简介

1. 虽然js中的变量在声明的时候不需要指定数据类型，但是在赋值，每一个数据还是有类型的

2. js中的数据类型有：原始类型、引用类型

   - 原始类型：Undefined、Number、String、Boolean、Null

   - 引用类型：Object及Object子类

     > 注：ES规范(ECMAScript规范)，在ES6之后，又基于以上6种类型之外添加了一种新的类型：Symbol

3. js种有一个运算符叫作typeof，这个运算符可以在程序运行阶段获取变量的数据类型

   ```javascript
   //语法格式
   typeof 变量名
   ```

   > 注：typeof运算符的运算结果是以下6个字符串之一，注意字符串都是全部小写
   >
   > ”undefined“、”number“、”string“、”boolean“、”object“、”function“

4. 在js种比较字符串使用”==“，没有equals

5. null、NaN、undefined三者区别

   ```javascript
   typeof null			//"object"
   typeof NaN			//"number"
   typeof undefined	//"undefined"
   
   null == NaN			//false
   null == undefined	//true
   NaN == undefined	//false
   
   null === NaN			//false
   null === undefined		//false
   NaN === undefined		//false
   ```

   > 在js中，==只判断值是否相等，===既判断值是否相等，又判断数据类型是否相等

## 数据类型介绍

1. Undefined类型

   Undefined类型只有一个值，这个值就是undefined。当一个变量没有手动赋值，系统默认赋值Undefined，或者也可以给一个变量赋值Undefined

2. Number类型

   - 包括 -1、0、1、NaN……

     > 整数、小数、正数、负数、不是数字(NaN)、无穷大(Infinity)都属于Number类型

   - 关于isNaN函数？

     用法：isNaN(数据)，结果true表示不是一个数字，结果false表示是一个数字

   - parseInt()函数

     作用：可以将字符串自动转为数字，并且取整数位

   - parseFloat()函数

     作用：可以将字符串自动转为数字

   - Math.ceil()函数

     作用：向上取整

3. Boolean类型

   - js中的布尔类型永远只有两个值：true和false

   - Boolean()函数

     用法：Boolean()

     作用：将非布尔类型转换为布尔类型

     ```javascript
     Boolean(1)			//true
     Boolean(0)			//false
     Boolean("")			//false
     Boolean("abc")		//true
     Boolean(null)		//false
     Boolean(NaN)		//false
     Boolean(undefined)		//false
     Boolean(Infinity)		//true
     ```

4. Null类型

   Null类型只有一个值null，但是

   ```javascript
   typeof(null)		//object
   ```

5. String类型

   - 在js当中，字符串可以使用单引号，也可以使用双引号

   - 创建字符串对象的两种方式

     ​      第一种：var a = "abc"

     ​	  ——小string，属于原始类型String

     ​      第二种：var s = new String("abc")

     ​	  ——大string，属于Object类型

     > ​	注：无论是小string还是大string，他们的属性和函数都是通用的

   - string类型常用属性和函数

     ​     常用属性

     ​			length  获取字符串长度

     ​     常用函数

     ​			indexOf		获取指定字符串在当前字符串第一次出现处的索引

     ​			lastIndexOf		获取指定字符串在当前字符串中最后一次出现处的索引

     ​			replace		替换第一个目标字符

     ​			substr(startIndex,length)		截取子字符串

     ​			substring(startIndex,endIndex)		截取子字符串

     ​			toLowerCase		转换小写

     ​			toUpperCase		转换大写

     ​			split		拆分字符串

6. Object类型

   - Object类型是所有类型的超类，自定义任何类型，默认继承Object

   - Object类包括哪些属性？

   ​		1)  prototype属性(常用的，主要是这个)：作用是给类动态的扩展属性和函数

   ```javascript
   String.prototype.suiyi = function(){
       alert("这是给string类型扩展的一个函数，叫作suiyi");
   }
   "abc".suiyi();
   ```

   ​		2)  constructor属性

   -  Object类包括哪些函数？

     toString()

   ​		valueOf()

   ​		toLocalString()

   - 在js当中定义的类默认继承Object，会继承Object类中所有的属性以及函数

     换句话说，自己定义的类也有prototype属性

   - 在JS中怎么定义类？怎么New对象？

     定义类的语法

     ```javascript
     //第一种
     function  类名(形参){
         
     }
     
     //第二种
     类名 = function(形参){
         
     }
     ```

     创建对象的语法

     ```javascript
     new 构造方法名(实参)；	//构造方法名和类名一致
     ```

     > ```javascript
     > //js中类的定义，同时又是一个构造函数的定义
     > function User(a,b,c){
     >     this.sno = a;
     >     this.sname = b;
     >     this.sage = c;
     > }
     > 
     > //创建对象
     > var u1 = new User(111,"zhangsan",30);
     > //访问对象属性
     > alert(u1.sno)
     > //或
     > alert(u1["sno"])
     > ```





# 事件

1. 常见事件

   - blur：失去焦点

     focus：获得焦点

   

   - click：鼠标单击

     dblclick：鼠标双击

   

   - keydown：键盘按下

     keyup：键盘弹起

   

   - mousedown：鼠标按下

     mouseover：鼠标经过

     mousemove：鼠标移动

     mouseout：鼠标离开

     mouseup：鼠标弹起

   

   - reset：表单重置

     submit：表单提交

   

   - change：下拉列表选中项改变，或文本框内容改变

     

   - load：页面加载完毕(整个html所有元素加载完毕发生)

     

   - select：文本被选定

> 注：任何一个事件都会对应一个事件句柄，事件句柄是在事件前添加on。
>
> onXXX这个事件句柄出现在一个标签的属性位置上。(事件句柄以属性的方式存在)



2. 注册事件

   - 方式一

     ```html
     <script>
     	function sayHello(){
         alert("hello js!");
     	}
     </script>
     <input type="button" value="hello" onclick="sayHello()"/>
     ```

     以上代码的含义是：将sayHello()函数注册到按钮上，等待click事件发生之后，该函数被浏览器调用，我们称这个函数为回调函数

   - 方式二

     ```html
     <script>
         function doSome(){
             alert("do some!");
         }
         //1.先获取按钮对象
         //document是内置对象，代表整个Html页面
     	var btnObj = document.getElementById("mybtn");
         //2.给按钮对象的onclick属性赋值
         btnObj.onclick = doSome;	//千万别加小括号
     </script>
     <input type="button" value="hello2" id="mybtn">
     ```

     更多类型转化

     ```html
     <script>
     	var btnObj = document.getElementById("mybtn");
         btnObj.onclick = function(){
             alert("这也行！")；
         };
     </script>
     <input type="button" value="hello2" id="mybtn">
     ```

     ```html
     <script>
     	var btnObj = document.getElementById("mybtn").onclick = function(){
             alert("这也行！")；
         };
     </script>
     <input type="button" value="hello2" id="mybtn">
     ```

3. 代码执行顺序

   ```html
   <body>
       <script type="text/javascript">
           document.getElementById("btn").onclick = function(){
               alert("hello js");
           }
       </script>
       <input type="button" value="hello" id="btn">
   </body>
   ```

   - 当button在script下面是代码无法执行，此时先执行script，但id为btn的元素还没加载出来，因此js找不到id为”btn“的元素，为了解决这一问题，可采用load事件

     

   ```html
   <body>
       <script type="text/javascript">
       	window.onload = function(){
               document.getElementById("btn").onclick = function(){
                   alert("hello js");
               }
           }
       </script>
       <input type="button" value="hello" id="btn">
   </body>
   ```

   - 页面加载过程中，将a函数注册给了load事件

     页面加载完毕后，load事件发生了，此时执行回调函数a

     回调函数a执行过程中，把b函数注册给了id="btn"的click事件

     当id="btn"的节点发生click事件之后，b函数被调用并执行

4. 获取鼠标键值

   ```html
   <body>
   	<script>
   		usernameElt.onkeydown = function(event){
           	if(event.keyCode === 13){
   				alert("正在进行验证……")
           	}
       	}
   	</script>
       <input type="text" id="username">
   </body>
   ```

   - 对于keydown句柄来说，触发时浏览器会自动将”键盘事件对象“赋给event，可以根据其属性keyCode来获取键值
   - 回车键键值是13，ESC健键值是27





# 运算符

## void

- 语法：void(表达式)

- 作用：执行表达式，但不返回任何结果

  ```html
  //既保留超链接样式，同时用户点击该超链接的时候执行一段js代码，但页面不能跳转
  <a href="javascript:void(0)" onclick="window.alert('test code')">
  ```

  其中javascript:告诉浏览器后面是一段js代码，程序中的javascript:是不能省略的





# 控制语句

1. for

   ```javascript
   //普通for
   for(var i=0;i<arr.length;i++){
       alert(arr[i]);
   }
   
   //for……in
   for(var i in arr){
       alert(arr[i]);
   }
   
   for(var propertyName in stu){
       alert(stu[propertyName]);
   }
   ```

   - for……in语句中，用在数组方面，i是下标；用在对象上面，i是属性名。而java中是某个元素

2. with

   ```javascript
   with(u){
       alert(username + "," + password);
       //相当于u.username  u.password
   }
   ```

   

3. 未提及的其他控制语句和java一致







# DOM编程

1. DOM对象
   - DOM对象由浏览器负责创建
   - 浏览器在加载html标签时，会为每一个html标签创建DOM对象

2. getElementById

   ```html
   <script>		  
   	window.onload = function(){
   		var btnElt = document.getElementById("btn");
   		btnElt.onclick = function(){
                //获取username节点
               /*
                    var usernameElt = document.getElementById("username");
                    var username = usernameElt.value;
                    alert(username);
               */
               
               // alert(document.getElementById("username").value);
   				 
   		    // 可以修改它的value
   				 document.getElementById("username").value = "zhangsan";
   			  }
   		  }
   </script>
   <input type="text" id="username" />
   <input type="button" value="获取文本框的value" id="btn"/>
   ```

3. innerHTML与innerText

   ```html
   <!DOCTYPE html>
   <html>
   	<head>
   		<meta charset="utf-8">
   		<title>DOM编程-innerHTML和innerText操作div和span</title>
   		<style type="text/css">
   			#div1{
   				background-color: aquamarine;
   				width: 300px;
   				height: 300px;
   				border: 1px black solid;
   				position: absolute;
   				top: 100px;
   				left: 100px;
   			}
   		</style>
   	</head>
   	<body>
   		
   		<!--
   			innerText和innerHTML属性有什么区别？
   				相同点：都是设置元素内部的内容。
   				不同点：
   					innerHTML会把后面的“字符串”当做一段HTML代码解释并执行。
   					innerText，即使后面是一段HTML代码，也只是将其当做普通的字符串来看待。
   		-->
   		<script type="text/javascript">
   			window.onload = function(){
   				var btn = document.getElementById("btn");
   				btn.onclick = function(){
   					// 设置div的内容
   					// 第一步:获取div对象
   					var divElt = document.getElementById("div1");
   					// 第二步:使用innerHTML属性来设置元素内部的内容
   					// divElt.innerHTML = "fjdkslajfkdlsajkfldsjaklfds";
   					// divElt.innerHTML = "<font color='red'>用户名不能为空！</font>";
   					divElt.innerText = "<font color='red'>用户名不能为空！</font>";
   				}
   			}
   		</script>
   		
   		<input type="button" value="设置div中的内容" id="btn"/>
   		
   		<div id="div1"></div>
   		
   	</body>
   </html>
   ```

4. 正则表达式

   - 什么是正则表达式，有什么用？
     		正则表达式：Regular Expression
       		正则表达式主要用在字符串格式匹配方面。

   - 正则表达式实际上是一门独立的学科，在Java语言中支持，C语言中也支持，javascript中也支持。
     大部分编程语言都支持正则表达式。正则表达式最初使用在医学方面，用来表示神经符号等。目前使用最多的是计算机编程领域，用作字符串格式匹配。包括搜索方面等。

   - 正则表达式，对于我们javascript编程来说，掌握哪些内容呢？
     		第一：常见的正则表达式符号要认识。
       		第二：简单的正则表达式要会写。
       		第三: 他人编写的正则表达式要能看懂。
       		第四：在javascript当中，怎么创建正则表达式对象！（new对象）
       		第五：在javascript当中，正则表达式对象有哪些方法！（调方法）
       		第六：要能够快速的从网络上找到自己需要的正则表达式。并且测试其有效性。

   - 常见的正则表达式符号？

     |   符号   |                    含义                    |
     | :------: | :----------------------------------------: |
     |    .     |         匹配除换行符以外的任意字符         |
     |    \w    |        匹配字母或数字或下划线或汉字        |
     |    \s    |              匹配任意的空白符              |
     |    \d    |                  匹配数字                  |
     |    \b    |            匹配单词的开始或结束            |
     |    ^     |              匹配字符串的开始              |
     |    $     |              匹配字符串的结束              |
     |          |                                            |
     |    *     |              重复零次或更多次              |
     |    +     |              重复一次或更多次              |
     |    ？    |               重复零次或一次               |
     |   {n}    |                  重复n次                   |
     |   {n,}   |              重复n次或更多次               |
     |  {n,m}   |                 重复n到m次                 |
     |          |                                            |
     |    \W    | 匹配任意不是字母，数字，下划线，汉字的字符 |
     |    \S    |          匹配任意不是空白符的字符          |
     |    \D    |            匹配任意非数字的字符            |
     |    \B    |        匹配不是单词开头或结束的位置        |
     |          |                                            |
     |   [^x]   |          匹配除了x以外的任意字符           |
     | [^aeiou] |   匹配除了aeiou这几个字母以外的任意字符    |

     正则表达式当中的小括号()优先级较高。
     [1-9] 表示1到9的任意1个数字（次数是1次。）
     [A-Za-z0-9] 表示A-Za-z0-9中的任意1个字符
     [A-Za-z0-9-] 表示A-Z、a-z、0-9、- ，以上所有字符中的任意1个字符。

     | 表示或者	

   -  简单的正则表达式要会写
              QQ号的正则表达式：^\[1-9][0-9]{4,}$

   - 他人编写的正则表达式要能看懂？
     		 E-mail正则：^\w+([-+.]\w+)**@\w+([-.]\w+)*\.\w+([-.]\w+)*$

   - 怎么创建正则表达式对象，怎么调用正则表达式对象的方法？

     ​		第一种创建方式：
     ​					var regExp = /正则表达式/flags;
     ​		第二种创建方式:使用内置支持类RegExp
     ​					var regExp = new RegExp("正则表达式","flags");	

     ​		

     ​		关于flags：

     ​					g：全局匹配

     ​					i：忽略大小写

     ​					m：多行搜索（ES规范制定之后才支持m。）当前面是正则表达式的时候，m不能   

     ​                            用。只有前面是普通字符串的时候，m才可以使用。	

     

     ​	     正则表达式对象的test()方法？

     ​					true / false = 正则表达式对象.test(用户填写的字符串);
     ​					true : 字符串格式匹配成功
     ​					false: 字符串格式匹配失败

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>DOM编程-关于正则表达式</title>
	</head>
	<body>
		<script type="text/javascript">
		   window.onload = function(){
			   // 给按钮绑定click
			   document.getElementById("btn").onclick = function(){
				   var email = document.getElementById("email").value;
				   var emailRegExp = /^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$/;
				   var ok = emailRegExp.test(email);
				   if(ok){
						//合法
						document.getElementById("emailError").innerText = "邮箱地址合法";
				   }else{
					   // 不合法
					   document.getElementById("emailError").innerText = "邮箱地址不合法";
				   }
			   }
			   // 给文本框绑定focus
			   document.getElementById("email").onfocus = function(){
				   document.getElementById("emailError").innerText = "";
			   }
		   }
		   
		</script>
		
		<input type="text" id="email" />
		<span id="emailError" style="color: red; font-size: 12px;"></span>
		<br>
		<input type="button" value="验证邮箱" id="btn" />
	</body>
</html>
```

4. 内置对象

   - Date

     ```javascript
     var nowTime = new Date();
     		   // 输出
     		   // document.write(nowTime);
     		   // 转换成具有本地语言环境的日期格式.
     		   nowTime = nowTime.toLocaleString();
     		   document.write(nowTime);
     		   document.write("<br>");
     		   document.write("<br>");
     		   // 当以上格式不是自己想要的,可以通过日期获取年月日等信息,自定制日期格式.
     		   var t = new Date();
     		   var year = t.getFullYear(); // 返回年信息,以全格式返回.
     		   var month = t.getMonth(); // 月份是:0-11
     		   // var dayOfWeek = t.getDay(); // 获取的一周的第几天(0-6)
     		   var day = t.getDate(); // 获取日信息.
     		   document.write(year + "年" + (month+1) + "月" + day + "日");
     		   
     		   document.write("<br>");
     		   document.write("<br>");
     		   
     		   // 重点:怎么获取毫秒数？(从1970年1月1日 00:00:00 000到当前系统时间的总毫秒数)
     		   //var times = t.getTime();
     		   //document.write(times); // 一般会使用毫秒数当做时间戳. (timestamp)
     ```

     **setInterval与clearInterval**

     ```javascript
     		<script type="text/javascript">
     			function displayTime(){
     				var time = new Date();
     				var strTime = time.toLocaleString();
     				document.getElementById("timeDiv").innerHTML = strTime;
     			}
     			
     			// 每隔1秒调用displayTime()函数
     			function start(){
     				// 从这行代码执行结束开始,则会不间断的,每隔1000毫秒调用一次displayTime()函数.
     				v = window.setInterval("displayTime()", 1000);	
     			}
     			
     			function stop(){
     				window.clearInterval(v);
     			}
     		</script>
     		<br><br>
     		<input type="button" value="显示系统时间" onclick="start();"/>
     		<input type="button" value="系统时间停止" onclick="stop();" />
     		<div id="timeDiv"></div>
     ```

     

   - Array

     ```html
     <!DOCTYPE html>
     <html>
     	<head>
     		<meta charset="utf-8">
     		<title>内置支持类Array</title>
     	</head>
     	<body>
     		<script type="text/javascript">
     			/*
     			// 创建长度为0的数组
     			var arr = [];
     			alert(arr.length);
     			
     			// 数据类型随意
     			var arr2 = [1,2,3,false,"abc",3.14];
     			alert(arr2.length);
     			
     			// 下标会越界吗
     			arr2[7] = "test"; // 自动扩容.
     			
     			document.write("<br>");
     			
     			// 遍历
     			for(var i = 0; i < arr2.length; i++){
     				document.write(arr2[i] + "<br>");
     			}
     			
     			// 另一种创建数组的对象的方式
     			var a = new Array();
     			alert(a.length); // 0
     			
     			var a2 = new Array(3); // 3表示长度.
     			alert(a2.length);
     			
     			var a3 = new Array(3,2);
     			alert(a3.length); // 2
     			*/
     		   
     		   var a = [1,2,3,9];
     		   var str = a.join("-");
     		   alert(str); // "1-2-3-9"
     		   
     		   // 在数组的末尾添加一个元素(数组长度+1)
     		   a.push(10);
     		   alert(a.join("-"));
     		   
     		   // 将数组末尾的元素弹出(数组长度-1)
     		   var endElt = a.pop();
     		   alert(endElt);
     		   alert(a.join("-"));
     		   
     		   // 注意:JS中的数组可以自动模拟栈数据结构:后进先出,先进后出原则.
     		   // push压栈
     		   // pop弹栈
     		   
     		   // 反转数组.
     		   a.reverse();
     		   alert(a.join("="));
     		</script>
     	</body>
     </html>
     ```







# BOM编程

1. open与close

   ```html
   <!DOCTYPE html>
   <html>
   	<head>
   		<meta charset="utf-8">
   		<title>BOM编程-open和close</title>
   	</head>
   	<body>
   		<script type="text/javascript">
   			/*
   				1、BOM编程中，window对象是顶级对象，代表浏览器窗口。
   				2、window有open和close方法，可以开启窗口和关闭窗口。
   			*/
   		   
   		</script>
   		
   		<input type="button" value="开启百度(新窗口)" onclick="window.open('http://www.baidu.com');" />
   		<input type="button" value="开启百度(当前窗口)" onclick="window.open('http://www.baidu.com', '_self');" />
   		<input type="button" value="开启百度(新窗口)" onclick="window.open('http://www.baidu.com', '_blank');" />
   		<input type="button" value="开启百度(父窗口)" onclick="window.open('http://www.baidu.com', '_parent');" />
   		<input type="button" value="开启百度(顶级窗口)" onclick="window.open('http://www.baidu.com', '_top');" />
   		
   		<input type="button" value="打开表单验证"  onclick="window.open('002-open.html')"/>
   	</body>
   </html>
   ```

2. alert与confirm

   ```html
   <!DOCTYPE html>
   <html>
   	<head>
   		<meta charset="utf-8">
   		<title>弹出消息框和确认框</title>
   	</head>
   	<body>
   		<script type="text/javascript">
   			function del(){
   				/*
   				var ok = window.confirm("亲，确认删除数据吗？");
   				//alert(ok);
   				if(ok){
   					alert("delete data ....");
   				}
   				*/
   			    if(window.confirm("亲，确认删除数据吗？")){
   			    	alert("delete data ....");
   			    }
   			}
   		</script>
   		<input type="button" value="弹出消息框" onclick="window.alert('消息框!')" />
   		
   		<!--删除操作的时候都要提前先得到用户的确认。-->
   		<input type="button" value="弹出确认框(删除)" onclick="del();" />
   	</body>
   </html>
   ```

3. 前进与后退

   ```html
   <html>
   	<head>
   		<meta charset="utf-8">
   		<title>history对象</title>
   	</head>
   	<body>
   		<a href="007.html">007页面</a>
   		<input type="button" value="前进" onclick="window.history.go(1)"/> 
           <input type="button" value="后退" onclick="window.history.back()" />
   		<input type="button" value="后退" onclick="window.history.go(-1)" />
   	</body>
   </html>
   ```

4. location

   ```html
   <html>
   	<head>
   		<meta charset="utf-8">
   		<title>设置浏览器地址栏上的URL</title>
   	</head>
   	<body>
   		
   		<script type="text/javascript">
   			function goBaidu(){
   				// window.location.href = "http://www.jd.com";
   				// window.location = "http://www.126.com";
   				
   				//document.location.href = "http://www.sina.com.cn";
   				document.location = "http://www.tmall.com";
   			}
   		</script>
   		
   		<input type="button" value="新浪" onclick="goBaidu();"/>
   		
   		<input type="button" value="baidu" onclick="window.open('http://www.baidu.com');" />
   		
   	</body>
   </html>
   ```

   > 总结，有哪些方法可以通过浏览器往服务器发请求？
   > 		1、表单form的提交。
   > 		2、超链接。
   > 		3、document.location
   > 		4、window.location
   > 		5、window.open("url")
   > 		6、直接在浏览器地址栏上输入URL，然后回车。（这个也可以手动输入，提交数据也可以成为动态的。）
   > 		
   >
   > 以上所有的请求方式均可以携带数据给服务器，只有通过表单提交的数据才是动态的。

5. 设置顶级窗口

   ```javascript
   if(window.top!=window.self){
       window.top.location = window.self.location;
   }
   ```







# JSON

1. 什么是json，有什么用？

   JavaScript Object Notation (JavaScript对象标记)，简称JSON。(数据交换格式)

   JSON主要作用是：一种标准的数据交换格式。(目前非常流行，90%以上的系统，系统A与系统B交换数据的话，都是采用JSON。)

2. JSON是一种标准的轻量级的数据交换格式。

   特点是：体积小，易解析。

3. 在实际的开发中有两种数据交换格式，使用最多，其一是JSON，另一个是XML。
   XML体积较大，解析麻烦，但是有其优点是：语法严谨。（通常银行相关的系统之间进行数据交换的话会使用XML。）

4. JSON的语法格式：
   					var jsonObj = {
      						"属性名" : 属性值,
      						"属性名" : 属性值,
      						"属性名" : 属性值,
      						"属性名" : 属性值,
      						....
      					};

   ```javascript
    // 创建JSON对象(JSON也可以称为无类型对象。轻量级，轻巧。体积小。易解析。)
   var studentObj = {
   	"sno" : "110",
   	"sname" : "张三",
   	"sex" : "男"
   	};
   			
   // 访问JSON对象的属性
   alert(studentObj.sno + "," + studentObj.sname + "," + studentObj.sex);
   ```

5. JSON数组

   ```javascript
   // JSON数组
   var students = [
       {"sno":"110","sname":"张三","sex":"男"},
       {"sno":"120","sname":"李四","sex":"男"},
       {"sno":"130","sname":"王五","sex":"男"}
   ];
   
   // 遍历
   for(var i = 0; i < students.length; i++){
       var stuObj = students[i];
       alert(stuObj.sno + "," + stuObj.sname + "," + stuObj.sex);
   }
   ```

6. eval函数

   作用：将字符串当做一段JS代码解释并执行

   ```javascript
   window.eval("var i = 100;");
   alert("i = " + i); // i = 100
   ```

   常见应用场景： 

   java连接数据库,查询数据之后,将数据在java程序中拼接成JSON格式的“字符串”,将json格式的字符串响应到浏览器
   也就是说java响应到浏览器上的仅仅是一个"JSON格式的字符串",还不是一个json对象.
   可以使用eval函数,将json格式的字符串转换成json对象

   ```javascript
   var fromJava = "{\"name\":\"zhangsan\",\"password\":\"123\"}"; //这是java程序给发过来的json格式的"字符串"
   //将以上的json格式的字符串转换成json对象
   window.eval("var jsonObj = " + fromJava);
   // 访问json对象
   alert(jsonObj.name + "," + jsonObj.password); // 在前端取数据.
   ```

7. 在JS当中：[]和{}有什么区别？
   					[] 是数组。
      					{} 是JSON。

   ```javascript
   java中的数组：int[] arr = {1,2,3,4,5};
   JS中的数组：var arr = [1,2,3,4,5];
   JSON：var jsonObj = {"email" : "zhangsan@123.com","age":25};
   ```

8. 访问json对象属性

   ```javascript
   // JS中访问json对象的属性
   alert(json.username);
   
   // JS中访问json对象的属性
   alert(json["username"]);
   ```

   