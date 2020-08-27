# JSP规范介绍

- 来自于Java EE规范中的一种
- JSP规范制定了如何开发JSP文件代替响应对象将处理结果写入到响应体的开发流程
- JSP规范制定了Http服务器应该如何调用管理JSP文件





# 响应对象存在弊端

- 适合将数据量较少的处理结果写入到响应体

- 如果处理结果数量较多，使用响应对象增加开发难度

  > out.print()写的比较多，不方便开发





# JSP书写规范

1. 执行标记：

   只有书写在执行标记中的内容才会被当作Java命令

   ```jsp
   <%
   	int num1 = 100;
   	int num2 = 200;
   %>
   <!-- 
   	 1.可以声明Java变量
   	 2.可以运行表达式：数学运算、关系运算、逻辑运算
   	 3.可以声明控制语句(if、for···)
   -->
   ```

2. 输出标记

   通过输出标记，通知JSP将Java变量的值写入到响应体中

   ```jsp
   <%=num1%>		//100
   <%=num1+num2%>   //300
   ```

> 所有执行标记里面的内容是一个整体







# JSP内置对象

1. 内置对象：request

   类型：HttpServletRequest

   作用：在JSP文件运行时读取请求包信息，与Servlet在请求转发过程中实现数据共享

   ​			JSP文件执行时，借助于内置request对象读取请求包参数信息

   ```jsp
   
   <%
   	String userName = request.getParameter("userName");
   	String password = request.getParameter("password");
   %>
   
   来访者姓名：<%=userName%><br/>
   来访用户密码：<%=password%><br/>
   ```

2. 内置对象：session

   类型：HttpSession

   作用：JSP文件在运行时，可以session指向当前用户私人储物柜，添加共享数据，或者读取共享数据

   ```jsp
   <!-- JSP1 -->
   <%
   	session.setAttribute("key1",200);
   %>
   ```

   ```jsp
   <!-- JSP2 -->
   <%
   	Integer value = (Integer)session.getAttribute("key1");
   %>
   ```

3. 内置对象：application

   类型：ServletContext

   作用：同一个网站中的Servlet与JSP，都可以通过当前网站的全局作用域对象实现数据共享

   ```jsp
   <!-- JSP1 -->
   <% 
   	application.setAttribute("key1","helloworld");
   %>
   ```

   





# JSP与Servlet

1. 分工

   Servlet:负责处理业务并得到处理结果---------------------大厨

   JSP：不负责业务处理，主要任务将Servlet中处理结果写入到响应体---------传菜员

2. 调用关系

   Servlet工作完毕后，一般通过请求转发方式，向Tomcat申请调用JSP

3. 如何数据共享

   Servlet将处理结果i添加到【请求作用域对象】

   JSP文件在运行时从【从请求作用域对象】得到处理结果





# JSP文件运算原理

![jsp文件运算原理](E:\笔记\JSP笔记.assets\jsp文件运算原理.png)

- Tomcat根据JSP规范，将被访问的JSP文件[编辑]为一个java文件。这个Java文件是Servlet接口实现类
- Tomcat根据JSP规范，调用JVM（javac one_jsp.java）将这个java文件[编译]为class类型
- Tomcat根据JSP规范负责生成这个class文件的实例对象。这个实例对象是一个Servlet接口实例对象
- Tomcat根据JSP规范通过实例对象调用class文件中_jspservice方法
- _jspservice方法在运行时负责将JSP文件中书写内容写入到响应体中







# EL表达式

1. 介绍

   - 命令格式：${作用域对象别名.共享数据}

   - 命令作用：

     ​	1) EL表达式是EL工具包提供的一种特殊命令格式【表达式命令格式】

     ​    2) EL表示式在JSP文件中使用

     ​	3)  负责在JSP文件上从作用域对象读取指定的共享数据并输出到响应体

   ```jsp
   <!-- 原先 -->
   <%
   	Integer sid = (Integer) application.getAttribute("sid");
   	String sname = (String)session.getAttribute("sname");
   	String home = (String)request.getAttribute("name");
   %>
   学员ID：<%=sid%>
   学员姓名：<%=sname%>
   学员地址：<%=home%>
   
   <!-- EL表达式 -->
   学员ID：${applicationScope.sid}
   学员姓名：${sessionScope.sname}
   学员地址：${requestScope.home}
   ```

2. 作用域对象别名

   - JSP文件可以使用的作用域对象

     ​	1) ServletContext	application:全局作用域对象

     ​	2) HttpSession	session:会话作用域对象

     ​	3) HttpServletRequest	request:请求作用域对象

     ​	4) PageContext	pageContext:当前页作用域对象，这是JSP文件独有的作用域对象。	Servlet中不存在在当前页面作用域对象存放的共享数据仅能在JSP文件中使用，不能共	享给其他Servlet或其他JSP文件

     ​	真实开发过程中，主要用于JSTL标签与JSP文件之间数据共享

     ​	JSTL------->pageContext-------->JSP

   - EL表达式提供作用域对象别名

     |     JSP     |            EL表达式            |
     | :---------: | :----------------------------: |
     | application | ${applicationScope.共享数据名} |
     |   session   |   ${sessionScope.共享数据名}   |
     |   request   |   ${requestScope.共享数据名}   |
     | pageContext |    ${pageScope.共享数据名}     |

3. EL表达式将引用对象属性写入到响应体

   - 命令格式：${作用域对象别名.共享数据名.属性名}
   - 命令作用：从作用域对象读取指定共享数据关联的引用对象的属性值，并自动将属性的结果写入到响应体
   - 属性名：一定要去引用类型属性名完全一致(大小写)

   - EL表达式没有提供遍历集合的方法，因此无法从作用域对象读取集合内容输出

4. EL表达式简化版

   - 命令格式：${共享数据名}

   - 命令作用：EL表达式允许开发人员开发时省略作用域对象别名

   - 工作原理：EL表达式简化版由于没有指定作用域对象，所以在执行时采用【猜】算法

     ​	首先到【pageContext】定位共享数据，如果存在直接读取输出并结束执行

     ​	如果在【pageContext】没有定位成功，到【request】定位共享数据，如果存在直接	读取输出并结束执行

     ​	如果在【request】没有定位成功，到【request】定位共享数据，如果存在直接读取输	出并结束执行

     ​	如果在【session】没有定位成功，到【application】定位共享数据，如果存在直接读	取输出并结束执行

     ​	如果在【application】没有定位成功，返回null

     > ​	pageContext--->request--->session--->application

   - 存在隐患：容易降低程序执行速度

     ​                    容易导致数据定位错误

   - 应用场景：设计目的就是简化从pageContext读取共享数据并输出难度

   - EL表达式简化版尽管存在很多隐患，但是在实际开发过程中，开发人员为了节省时间，尽量使用简化版，拒绝使用标准版

5. 支持运算表达式

   - 前提：在JSP文件有时需要将读取共享数据进行一番运算之后，将运算结果写入到响应体

   - 运算表达式：

     ​	1)	数学运算

     ​	2)	关系运算： >	>=	==	<	<=	!=

     ​								gt	ge	eq	lt	  le     !=

     ```jsp
     EL表达式输出关系运算：${age >= 18?"欢迎光临":"谢绝入内"}
     ```

     ​	3)	逻辑运算：&&	||	！	

6. EL表达式提供内置对象

   - 命令格式：${param.请求参数名}

   - 命令作用：从通过请求对象读取当前请求包中请求参数内容，并将请求参数内容写入到响应体

   - 代替命令：index.jsp

     发送请求:Http://localhost:8080/myWeb/index.jsp?userName=mike&password=123

     ```jsp
     <%
     	String username = request.getParameter("userName");
     	String password = request.getParameter("password");
     %>
     <%=userName%>
     <%=password%>	
     ```

     ---

   - 命令格式：${paramValues.请求参数名[下标]}

   - 命令作用：如果浏览器发送的请求参数是[一个请求参数关联多个值]，此时可以通过paramValues读取请求参数下指定位置的值并写入到响应体

   - 代替命令：

     http://localhost:8080/myWeb.index2.jsp?pageNo=1&pageNo=2&pageNo=3

     此时pageNo请求参数在请求包以数组的形式存在	pageNo:[1,2,3]

     ```jsp
     <%
     	String array[] = request.getParameterValues("pageNo");
     %>
     第一个值：<%=array[0]%>
     第二个值：<%=array[1]%>
     ```

     > 注：这两个内置对象很少使用，因为现在浏览器一般向Servlet发送请求，不会直接向JSP发送请求

   7. EL表达式常见异常

      javax.el.PropertyNotFoundException：在对象中没有找到指定属性

   8. EL表达式缺陷
      - 只能读取与对象数据，不能向域对象中写入数据更改数据
      - 不支持控制语句       if判断          while循环

   ***     如果单独使用EL表达式，无法确保JSP文件中所有的JAVA命令都被替换    ***





# JSTL标签

1. 介绍：Jsp	Standard	Tag	Lib：JSP中标准的标签工具类库

2. 组成：

   1)	核心标签：Java在jsp上基本功能进行封装:	if	while

   2)	sql标签：JDBC在JSP上使用功能

   3)	xml标签：DOM4j在JSP使用功能

   4)	Format标签：JSP文件格式转换

3. 配置

   - 导入依赖jar：jstl.jar	standard.jar

   - 在JSP文件引入JSTL中core包依赖约束

     <% @taglib	uri="http://java.sun.com/jsp/jstl/core"	prefix="c"%>

4. 标签使用

   - ```jsp
     <c:set>
     作用：在JSP文件上设置域对象中共享数据
         使用：<c:set scope="session" var="key" value="10"></c:set>
         	  <c:set scope="application" var="sname" value="mike"/>
     代替：<%
         	session.setAttribute("key","10")
           %>
     属性：scope：指定操作的域对象别名
         		scope="application/session/request/page"
           var：声明作用域对象中关键字
           value：存入的共享数据
     ```

   - ```jsp
     <c:if>
         作用：在JSP文件上控制哪些内容可以写入到响应体
         使用：<c:if test="通过EL表达式进行判断">
         	 	 内容
         	  </c:if>
         例子：<c:set scope="session" var="age" value="23"/>
         	  <c:if test="${sessionScope.age ge 18}">
                   <font color="red">欢迎光临</font>
               </c:if>    
     ```

   - ```jsp
     <c:choose>
         作用：在JSP文件上实现多分支选择判断，决定哪一个内容能写入到响应体
         使用：<c:choose>
         		<c:when test="EL表达式进行判断">内容1</c:when>
         		<c:when test="EL表达式进行判断">内容2</c:when>
         		<c:otherwise>内容3</c:otherwise>
         	  </c:choose>
     ```

   - ```jsp
     <c:forEach>
         作用：循环遍历
         第1种使用方式：<c:forEach
                        	var="声明循环变量名称"
                         begin="初始化循环变量"
                         end="循环变量可以接受的最大值"
                         step="循环变量递增值或递减值"
                       >
         				*** step属性可以不写，默认每次递增1
         				*** 循环变量被保存在【pageContext】
         			  </c:forEach>
         第二种使用方式:<c:forEach
                       	items:"通过EL表达式获得域对象集合"
                         var="声明循环变量"
                       >
         				${循环变量.对象属性名}
         			  </c:forEach>
         第二种例：设对象作用域已存储的数据为key:stu
         		 <c:forEach items="${key}" var="stu">
         			<tr>
                         <td>${stu.sid}</td>
                         <td>${stu.sname}</td>
         			</tr>
         		 </c:forEach>
         注：当遍历map集合时，每次从map集合得到一个【键值对】
         【键值对】交给循环变量
         循环变量.key获得【键值对】中关键字名字	班级名称
         循环变量.value获得【键值对】中内容	stu对象
         <c:forEach items="${mapLey}" var="key_value">
             <tr>
                 <td>${key_value.key}</td>
                 <td>${key_value.value.sid}</td>
                 <td>${key_value.value.sname}</td>
             </tr>
         </c:forEach>
     ```

5. 与EL标签联合使用

   ```jsp
   <c:set scope="request" var="age" value="20"></c:set>
   <c:set scope="request" var="age" value="${requestScope.age+2}"/>
   用户两年后的年龄：${age}
   ```

   