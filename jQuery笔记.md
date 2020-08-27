# 承接JS部分

1. 动态为object类型对象添加属性和函数

   ```javascript
   var obj = {};		//制造学生对象
   obj.name = "mike";
   var param="major";
   obj[param]="自动化";
   alert("学员姓名" + obj.name + "学员专业" + obj.major);
   obj.study = fun1;
   function fun1(){
       alert("好好学习，赚大钱");
   }
   ```

   例：用JavaScript写一个Hashmap

   ```javascript
   function HashMap(){
       var obj = new Object();
       this.put = function (key,value){
           obj[key] = value;
       };
       this.get = function (key){
           return obj[key];
       };
   }
   
   var map = new HashMap();
   map.put("key1","100");
   var data = map.get("key1");
   alert("key1=" + data);
   ```





# jQuery封装

1. $()封装功能 ：

   - 简化DOM对象定位

     $("#ckOper")：如果参数以`#`开头，表示根据ID定位DOM

     $(".two")：如果参数以`.`开头，表示根据class定位DOM

   - 简化对一组DOM对象统一赋值的操作

     ```javascript
     function $(param){
         //声明一个数组，保存本次定位的所有DOM对象
         var domArray = [];
         if(param.indexOf(".")==0){//表示根据class定位DOM
             param = param.substring(1);
         }else if(param.indexOf("#")==0){//表示根据ID定位DOM
             param = param.substring(1);
             var domObj = document.getElementById(param);
             domArray.push(domObj);
         }
         /*
         *	prop("checked",true):对定位的所有DOM对象指定属性进行统一赋值
         *	prop("checked")：读取数组第一个DOM对象指定属性内容
         */
         
         //为domArray数组追加处理函数
         domArray.prop = function(key,value){
             if(arguments.length == 2){
                 for(var i=0;i<domArray.length;i++){
                 	var domObj = domArray[i];
                 	domObj[key] = value;
             	}
             }else if(arguments.length == 1){
                 var domObj = domArray[0];
                 var value = domObj[arguments[0]];
                 return value;
             }
     
         };
     }
     ```

     





# 选择器

## query对象与DOM对象关系：

- DOM对象：是由浏览器负责创建的对象。DOM对象管理对应html标签。一个html标签对应一个DOM对象

- jquery对象：是由jquery函数`$()`负责创建的对象；jquery对象本质上就是一个数组，数组中保存了当前被定位的所有DOM对象。开发人员可以通过jquery对象操作定位DOM对象，从而就可以间接的操作html标签

  > 一般为了区分DOM对象和jquery对象，所有jquery对象名称都是以$开头

  ```html
  <script type="text/javascript">
      function fun1(){
      	var $obj=$("#one");		//jquery对象
      	var username=$obj.val();
      	$("#one").val("hello" + username);
  	}
  </script>
  
  <body>
  	<input type="text" id="one"/>
      <input type="button" value="sayhello" onclick="fun1">
  </body>
  ```

## DOM对象和Jquery对象之间转换

- DOM对象转换为jquery对象，就是将一个DOM对象添加到一个数组中 

  var $obj = $(dom对象)

  ```javascript
  function fun1(){
      var domObj = document.getElementById("one");
      //dom转换为jquery
      var $obj = $(domObj);
  }
  ```

## jquery对象转化为DOM对象

从数组中提取DOM对象过程

```javascript
function fun1(){
    var $obj = $(":text")//定位当前页面中所有type=text的DOM对象
    //jquery转换为obj
    for(var i=0;i<$obj.length;i++){
        var domObj = $obj.get(i);
        alert(domObj);
    }
}
```

## jquery工具包如何定位DOM对象

- 使用【选择器】和【过滤器】进行定位
- jquery中共有三种【选择器】
- jquery中还有六种【过滤器】

## 选择器——基本选择器【最常用】

基本选择器：主要根据【标签ID】、【标签类型】、【标签使用样式选择器】进行定位

1. 【标签ID】：`$("#id编号")`；将当前页面中第一个满足条件的DOM对象进行定位

2. 【标签类型】：`$("标签类型名")`；将当前页面中所有指定类型的标签关联的DOM对象进行定位

3. 【标签使用样式选择器】：`$(.样式选择器名称)`；将页面中所有采用了指定样式选择器的标签关联的DOM对象进行定位

   ```javascript
   $("#one")	//标签ID
   $("div")	//标签类型
   $(".two")	//标签使用样式选择器
   ```

4. 【定位页面中所有标签】：`$("*")`

5. 【组合条件选择】：`$("条件1，条件2")`；条件1和条件2是或的关系

## 选择器——层级选择器【常用】

- 层级选择器：根据Html标签之间【兄弟关系】和【父子关系】进行定位

- 【兄弟关系】和【父子关系】

  ```html
  <!-- 父子关系 -->
  <tr>
      <td>
      	<inpout type="text"> 
      </td>
  </tr>
  text是td标签的【直接子标签】
  text是tr标签的【间接子标签】
  
  <!-- 兄弟关系 -->
  <div>
      <p>段落一</p>		老大
      <p>段落二</p>		老二
      <span>1</span>	   三弟	
  </div>
  ```

1. 定位满足条件的子标签
   - `$("定位父标签条件>子标签定位条件")`：定位当前父标签下所有满足条件的【直接子标签】
   - `$("定位父标签条件 子标签定位条件")`：定位当前父标签下所有满足条件的【直接子标签】和【间接子标签】

2. 定位满足条件的兄弟标签

   - `$("定位当前标签条件+条件")`：定位当前标签后面与之【相邻】并且满足【定位条件的】兄弟标签

   - `$("定位当前标签条件~条件")`：定位当前标签后面，所有满足条件的兄弟标签

   - `$("定位当前标签条件").siblings("条件")`：定位当前标签的【前面与后面】所有满足条件的兄弟标签

     ```javascript
     $("#one+div").css("background-color","green");
     $("#two~div").css("background-color","black");
     $("#two").siblings("div").css("background-color","red");
     ```

## 选择器——input表单域标签选择器

1. 表单域标签分类：

   - input体系

     ```html
     <input type="text">
     <input type="password">
     <input type="radio">
     <input type="checkbox">
     <input type="file">
     <input type="button">
     <input type="submit">
     <input type="reset">
     <input type="hidden">
     ```

   - 多行表单域标签

     ```html
     <textarea></textarea>
     ```

   - 下拉列表i

     ```html
     <select>
         <option value=></option>
     </select>
     ```

2. input表单域标签选择器

   根据type属性内容，定位input标签

   `$(":type属性内容")`

   例：

   ```javascript
   $(":text")：定位所有文本框
   ```







# jquery对象常见功能函数

1. val函数
   - `$obj.val(值)`：对当前jquery对象中所有DOM对象的value属性进行统一赋值
   - `$obj.val()`：读取当前jquery对象中第一个DOM对象的value属性

2. each函数

   用于遍历当前jquery对象中保存的所有DOM对象

   ```javascript
   $obj.each(
   	function (下标值，dom对象){
           对当前遍历得到的DOM对象进行具体操作
       }
   )
   
   例：
   $obj.each(
   	function(index,domObj){
           alert(index+"位置上radio标签的value值="+$(domObj).val())
           //开发时，尽量不要直接操作DOM对象
       }
   )
   ```

3. text函数

   - `$obj.text("文字显示内容")`：对当前jquery对象中所有的DOM对象的【文字显示内容】进行统一赋值

   - `$obj.text()`：读取当前第一个DOM对象的【文字显示内容】

     ********代替innerText

4. attr函数
   - `$obj.attr("基本属性名"，"值")`：对当前jquery对象中所有的DOM对象的【指定基本属性】进行统一赋值
   - `$obj.attr("基本属性名")`：读取当前jquery对象中第一个DOM对象的【指定基本属性】值

5. prop函数
   - `$obj.prop("工作状态属性",boolean值)`：对当前jquery对象中所有的DOM对象的【指定工作状态】进行统一赋值
   - `$obj.prop("工作状态属性")`：读取当前jquery对象中第一个DOM对象的【指定工作状态】值









# jquery过滤器

1. 作用：针对选择器定位的DOM对象进行二次过滤筛选的，相当于【where】
2. 使用方式：过滤器不能独立使用，必须声明在选择器后方，相当于mysql中group by和having关系

## 过滤器——基本过滤器【最常用】

1. 基本过滤器

   - DOM对象在jquery对象中存储位置

     ```html
     <div>1</div>		domObj1
     <div>2</div>		domObj2
     var $obj = $("div");
     $obj[0] = domObj1
     $obj[1] = domObj2
     ```

   - 根据DOM对象在jquery中存储位置进行过滤筛选

2. 基本过滤器使用

   - `$("选择器：first")`：定位满足条件的第一个DOM

     ```javascript
     $(":button:first")
     ```

   - `$(":button:last")`：定位满足条件的最后一个DOM

     ```javascript
     $(":button:last")
     ```

   - `$(":button:eq(位置)")`：定位指定位置的DOM对象

     ```javascript
     $(":button:eq(1)")		//定位指定页面中第二个button
     ```

   - `$(":button:lt(位置)")`：定位小于指定位置的DOM对象

     ```javascript
     $(":button:lt(3)")      //定位指定页面中前3个button
     ```

   - `$(":button:gt(位置)")`：定位大于指定位置的DOM对象

     ```javascript
     $(":button:gt(0)")      //定位指定页面中除了第一个button之外的所有button
     ```

## 过滤器——表单域标签专有工作状态过滤器

1. html标签中属性分类

   - 基本属性：所有html标签都拥有属性【id,name,title】

   - 样式属性：

     ```html
     <div style="background-color:red">
     ```

   - value属性：表单域标签

   - 工作状态属性：仅存在表单域标签

     ```html
     <input type="button" disabled>
     <input type="text" value="mike" readOnly>
     <input type="radio" checked>
     <input type="checkbox" checked>
     <select>
         <option>1</option>
         <option selected>2</option>
         <option>1</option>
     </select>
     ```

   - 监听事件属性

     ```html
     <input type="button" onclick>
     <input type="text" onblur>
     ```

2. 表单域标签专有工作状态过滤器：根据当前表单域标签的工作状态进行二次过滤筛选的

   `$("选择器：disabled")`：定位的是满足条件的并且处于【不可用状态】的表单域标签

   `$("选择器：checked")`：定位的是满足条件的并且处于【被选中状态】的表单域标签【radio,checkbox】

   `$("选择器：selected")`：定位的是满足条件的并且处于【被选中状态】的标签【option】

   `$("选择器：enabled")`：定位的是满足条件的并且处于【可用状态】的表单域标签

3. 标签内容分为哪几大类

   - value属性

   - 基本属性内容

   - 文字显示内容

     ```html
     <option>文字显示内容</option>
     ```

   - 工作状态属性内容  【checked,selected,disabled】

## 过滤器——属性过滤器

1. 根据当前标签中是否【手动声明了指定的属性】

   ```html
   <div>1</div>		有name属性，但是没有手动声明
   <div name="myDiv">2</div>		有手动声明的name属性
   <div name="myTest">3</div>
   ```

- `$(选择器[属性名])`：定位的DOM对象对应的标签中必须手动声明了指定属性

  例：定位声明了name属性的所有DIV	

  ```javascript
  $("div[name]")		//<div name="myDiv">2</div>	
  ```



2. 根据当前标签中属性内容进行过滤

- `$("选择器[属性名='值']")`：定位的DOM对象对应的标签中的【指定属性内容】必须等于【指定的内容】

  ```javascript
  $("div[name='']")		//<div>1</div>，没有声明name属性就是空
  ```

- `$("选择器[属性名!='值']")`：定位的DOM对象对应的标签中的【指定属性内容】必须不等于【指定的内容】

  ```javascript
  $("div[name！='myTest']")//<div>1</div>，<div name="myTest">3</div>
  ```

- `$("选择器[属性名^='值']")`：定位的DOM对象对应的标签中的【指定属性内容】必须以【指定的内容】为开头的

  ```javascript
  $("div[name^='my']")		//<div name="myTest">3</div>
  ```

- `$("选择器[属性名$='值']")`：定位的DOM对象对应的标签中的【指定属性内容】必须以【指定的内容】为结尾的

  ```javascript
  $("div[name$='st']")		//<div name="myTest">3</div>
  ```

- `$("选择器[属性名*='值']")`：定位的DOM对象对应的标签中的【指定属性内容】必须以【指定的内容】为结尾的

  ```javascript
  $("div[name*='my']")        //<div name="myDiv">2</div> <div name="myTest">3</div>javascript
  ```

3. 根据多个属性进行判断

- `$("选择器[属性1][属性2 ='值'][属性3 $='值']")`：此时标签只有满足了所有条件才会被定位



 



# 监听事件绑定与解绑

1. `$obj.监听函数(处理函数)`

   ```html
   <input type="button" id="btn">
   <script>
   	function fun1(){//单击事件处理函数
   }
   </script>
   
   <!-- javascript源码 -->
   var dom = document.getElementById("btn");
   dom.onclick = fun1
   
   <!-- jquery源码 -->
   var $obj = $("#btn");
   $obj.click(fun1)
   ```

2. 监听函数命名

   | 监听事件 | jquery监听函数名 |
   | :------: | :--------------: |
   | onclick  |     click()      |
   | onchange |     change()     |
   |  onblur  |      blur()      |

3. `$obj.bind("jquery监听函数名"，处理函数名)`

   ```html
   <input type="button"/>
   <input type="button"/>
   
   <script>
   	function fun2(){
           
       }
       $(":button").bind("click",fun2)
   </script>
   <!-- 此时页面中所有的button上都绑定【onclick】监听事件，当某一个Onclick事件时，调用处理函数fun2 -->
   ```

4. 移除绑定的监听事件

   `$obj.unbind("jquery监听函数名")`：指定监听事件将会被移除

   `$obj.unbind()`：将当前标签上所有的监听事件进行移除







# $()功能介绍

1. `$("选择器/过滤器")`：DOM对象定位器，将所有满足条件的DOM对象封装到一个数组并返回

2. `$(domObj)`：类型转换器，将这个DOM对象添加到一个数组中并返回

3. `$(函数类型队形)`：监听器，在页面被浏览器加载完毕后，自动调用当前函数

4. `$("<开始标签></结束标签>")`：首先要求浏览器在内存中创建一个对应的HTML标签及其对应 的DOM对象，然后将DOM对象添加到数组中，最后将这个数组进行返回

   





# 补充功能

1. 为父标签添加子标签

   ```javascript
   //js代码
   父标签DOM.innerHTML="子标签"
   
   //jquery对象
   $父标签对象.append($子标签对象)
   //例
   var $option = $("<option></option>")
   $("#city").append($option)
   ```

2. 删除当前标签的子标签

   ```javascript
   $obj.remove()	//将当前标签及其所有的子标签进行删除
   $obj.empty()	//将当前标签所有的子标签进行删除，不会删除当前标签
   ```

3. 显示/隐藏标签

   ```javascript
   $obj.show()		//可以将定位的DOM对象关联标签进行显示处理
   $obj.hide()		//可以将定位DOM对象关联标签进行隐藏处理
   ```

   

