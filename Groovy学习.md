# Groovy学习

---

## 学习渠道

W3CSchool
## 时间线
 > 2018年7月10日：groovy环境搭建，基本语法,文件I/O，异常处理等
 
 > 2018年7月11日：groovy面向对象
 

## 笔记

 - groovy方法，与java类似，使用def定义，可设置缺省值，这与kotlin是相似的
```
 def someMethod(parameter1, parameter2 = 0, parameter3 = 0) {
   // Method code goes here   
}
```
 - 文件I/O，读取，写入，获取文件大小等操作基本和java一样，复制文件有些区别：
```
//将file2复制到file1中
    flie1 << file2.text
```
 - def声明各种类型，弱类型语言
 - 装箱拆箱
 - 范围，Range 
```
 1..10;  1..<10; 'a'..'z';   10..1
```
 - 映射（关联数组，字典）指的是map
 - 正则 用~表示：`def regex=~'Groovy'`
 - 异常处理 try-catch-finally
    ![Groovy异常层次结构][1]
 - 面向对象 set、get、继承、内部类、抽象、接口 和java类似
 - 泛型 同java
 - 特征 关键字 trait 相当于接口中包含方法，支持成员变量，子类实现后可以调用，可以多实现，和interface区分
```
 trait Marks { 
    int Marks1;
   void DisplayMarks() {
     this.Marks1 = 10;
      println("Display Marks"+this.Mark1);
   } 
} 

class Student implements Marks { 
   int StudentID
   int Marks1;
}
```
 - 闭包 匿名代码块 {}内的代码块，用call语句执行，可以引用变量，也可以作为参数传入方法中使用，集合使用each遍历
```
class Example {
   static void main(String[] args) {
      def str1="hello";
      def clos = {param->println "${str1} ${param}"};
      clos.call("World");
   } 
}
```
- groovy注释，同java注解，可以声明元注释，管理多个注释
- xml，BuilderSupport 构建xml，XmlParser解析xml（使用少）
- JMX，监控奇特与Java虚拟环境有关的程序，监视JVM，Tomcat等
- JSON，JsonSlurper解析，JsonOutput生成
```
http.request( GET, TEXT ) {
   headers.Accept = 'application/json'
   headers.'User-Agent' = USER_AGENT
    
   response.success = { 
      res, rd ->  
      def jsonText = rd.text 
        
      //Setting the parser type to JsonParserLax
      def parser = new JsonSlurper().setType(JsonParserType.LAX)
      def jsonResp = parser.parseText(jsonText)
   }
}
```
- DSLS,域特定语言，没怎么看懂
- 数据库，groovy-sql,支持HSQLDB、Oracle、SQL Server、MySQL、MongoDB，增删改查和SQLlite相似，运行demo需要MySQL基础（todo）
- 构建器，Swing构建器（用户图形界面，用不着）、DOM生成器（解析HTML，xml，XHTML）、JsonBuilder、NodeBuilder、FileTreeBuilder。JsonBuilder为例：
```
def builder = new groovy.json.JsonBuilder() 

def root = builder.students {
   student {
      studentname 'Joe'
      studentid '1'
        
      Marks(
         Subject1: 10,
         Subject2: 20,
         Subject3:30,
      )
   } 
} 
println(builder.toString());
```
- 命令行，groovysh（进入groovy环境）、groovyconsole(进入窗口页面)
- 单元测试，JUnit,需继承GroovyTextCase,多个测试用例一起运行代码：
```
import groovy.util.GroovyTestSuite 
import junit.framework.Test 
import junit.textui.TestRunner 

class AllTests { 
   static Test suite() { 
      def allTests = new GroovyTestSuite() 
      allTests.addTestSuite(StudentTest.class) 
      allTests.addTestSuite(EmployeeTest.class) 
      return allTests  
     } 
} 

TestRunner.run(AllTests.suite());
```
- 模板引擎，就是${it} 这样引用变量的方式,和kotlin相同，拓展到xml中，类似dataBinding
- 元对象编程，像js中为对象随时创建新的属性
  [1]: https://7n.w3cschool.cn/attachments/tuploads/groovy/hierarchy_exceptions.jpg

  <meta http-equiv="refresh" content="1">
