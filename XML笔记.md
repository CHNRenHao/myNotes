# 简介

1. XML是指可扩展标记语言(Extensible Markup Language)
2. XML是一种标记语言，HTML尤其发展而来
3. XML标签没有被预定义，需要自定义标签
4. XML设计宗旨是传输数据，而非显示数据，这一点与HTML恰恰相反
5. 所有的XML文档必须在第一行加上<?xml version="1.0" encoding="UTF-8"?>



# 读取方式

1. SAX读取方式：根据开发人员需要，一次将若干个满足条件的标签加载到内存中
   -  优点：可以节省内存
   - 缺点：如果读取大量标签信息时，运行效率相对较低

2. DOM读取方式：一次性将XML文档的所有内容加载到内存中
   - 优点：如果读取大量标签信息时，此时由于是在内存中进行定位，所以运行速度较快
   - 缺点：浪费内存

3. 实际开发过程中，一般都采用DOM方式来读取



# 约束文档

## 分类

1. XML约束文档作用：
   - 设置可以在当前XML文档中声明的【标签类型名】
   - 设置可以在标签中出现的【属性名】
   - 设置标签之间的父子关系和兄弟关系

2. XML约束文档分类：
   - DTD约束文档：简单约束文档
   - SCHEMA约束文档：高级约束文档

## DTD文档介绍

1. 使用DTD文档前，必须将以下代码导入到目标文件：

   ```xml
   <!DOCTYPE web-app SYSTEM "web-app_2_3.dtd">
   ```

2. <!ELEMENT 标签类型名>：声明可以在XML文档中出现的标签类型名
3. <!ATTLIST 标签类型名 属性名>：声明可以在当前标签内部使用的属性名称
4. <!ELEMENT 标签类型名 (子标签名？)>： 子标签可以出现在父标签内部，也可以不出现
   子标签如果出现只能出现一次
5. <!ELEMENT 标签类型名 (子标签名+ )>： 子标签必须出现在父标签内部，并可以出现多次
6. <!ELEMENT 标签类型名 (#PCDATA)>：当前标签没有子标签，一般填充文字内容
7. <!ELEMENT 标签类型名 (子标签名* )>：子标签可以出现在父标签内部，也可以不出现，子标签的出现可以多次：
8. <!ELEMENT 标签类型名 (子标签名 )>：子标签必须出现在父标签内部，且只能出现一次
9. <!ELEMENT 标签类型名 ((子标签名|子标签名2))>：这两个子标签必须有一个子标签出现在父标签中，但是不能同时出现

```xml-dtd
<!ELEMENT web-app (servlet*,servlet-mapping,welcome-file-list?)>
<!ELEMENT servlet (servlet-name,description?,(servlet-class|jsp-file))>
<!ELEMENT servlet-mapping (servlet-name,url-pattern)>
<!ELEMENT servlet-name (#PCDATA)>
<!ELEMENT servlet-class (#PCDATA)>
<!ELEMENT url-pattern (#PCDATA)>

<!ELEMENT welcome-file-list (welcome-dile+)>
<!ELEMENT welcome-file (#PCDATA)>
<!ELEMENT jsp-file (#PCDATA)>
<!ATTLIST web-app version CDATA #IMPLIED>
```

