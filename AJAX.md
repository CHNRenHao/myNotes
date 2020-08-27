# 简介

1. AJAX   (Asynchronous JavaScript and XML )：异步的JS和XML

2. ajax是一种局部刷新的新方法，不是一种语言。局部刷新使用的核心对象是 异步对象(XMLHttpRequest)，这个异步对象存在浏览器内存中，使用js语法创建和使用XMLHttpRequest

3. javascript：负责创建异步对象，发送请求，更新页面的DOM对象。ajax请求需要服务器端的数据。

   xml：网络中的传输数据格式，使用json替换了xml



# 使用方法

1. 创建异步对象

   ```js
   var xmlHttp = new XMLHttpRequest()
   ```

2. 给异步对象绑定事件：

   - 当异步对象发起请求，获取了数据都会触发这个事件。这个事件需要指定一个函数，在函数中处理状态变化
   - 异步对象的属性readyState表示异步对象请求的状态变化

   ```js
   xmlHttp.onreadystatechange = function(){
       处理请求的状态变化
       if(xmlHttp.readyState == 4 && xmlHttp.status==200){
           //可以处理服务器端的数据，更新当时页面
       }
   }
   ```

   | readyState |                             状态                             |
   | :--------: | :----------------------------------------------------------: |
   |     0      |              var xmlHttp = new XMLHttpRequest()              |
   |     1      | 初始化异步请求对象， xmlHttp.open(请求方式，请求地址，true)  |
   |     2      |              异步对象发送请求， xmlHttp.send()               |
   |     3      | 异步对象接收应答数据从服务端返回数据。XMLHttpRequest内部处理。 |
   |     4      |    异步请求对象已经将数据解析完毕。 此时才可以读取数据。     |

   - 异步对象的status属性，表示网络请求的状况的，200，404，500，需要当status==200表示网络请求是成功的

3. 初始异步请求对象

   异步的方法open()

   ```js
   xmlHttp.open(请求方式get/post,"服务器端的访问地址"，同步/异步请求(默认true,异步请求))
   例：xmlHttp.open("get","http:192.168.1.20:8080/myweb/query",true)
   ```

4. 使用异步对象发送请求

   ```js
   xmlHttp.send()
   ```

   获取服务器端返回的数据，使用异步对象的属性 responseText

   使用例子：

   ```js
   xmlHttp.responseText
   ```

   > 回调：当请求的状态变化时，异步对象会自动调用onreadystatechange事件对应的函数







# json

1. 分类

   json对象：JSONObject	

   ```json
   {name:"河北",simpleName:"冀",shenghui:"石家庄"}
   ```

   json数组：JSONArray(多个json对象)

   ```json
   [{name:"河北",simpleName:"冀",shenghui:"石家庄"},{name:"陕西",simpleName:"陕",shenghui:"西安"}]
   ```

2. 为什么要使用Json
   - json格式好理解
   - json格式数据在多种语言中，比较容易处理。使用java  javascript读写json格式的数据比较容易
   - json格式数据占用的空间小，在网络中传输快，用户的体验好

3. 处理json工具库
   - Gson(google)        功能最全
   - FastJson(Alibaba)        速度最快，但不是最符合json处理规范
   - Jackson        性能好，规范好
   - json-lib        性能差，依赖多

4. 示例

   ```java
   //将java对象转为json格式字符串
   import com.bjpowernode.entity.Province;
   import com.fasterxml.jackson.core.JsonProcessingException;
   import com.fasterxml.jackson.databind.ObjectMapper;
   
   public class TestJson {
   
       public static void main(String[] args) throws JsonProcessingException {
           // 使用jackson 把java对象转为json格式的字符串
   
           Province p = new Province();
           p.setId(1);
           p.setName("河北");
           p.setJiancheng("冀");
           p.setShenghui("石家庄");
   
           //使用jackson 把 p 转为 json
           ObjectMapper om  = new ObjectMapper();
           // writeValueAsString：把参数的java对象转为json格式的字符串
           String json  = om.writeValueAsString(p);
           System.out.println("转换的json=="+json);
       }
   }
   ```

   

   