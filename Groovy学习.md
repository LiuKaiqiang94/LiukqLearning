# Groovy学习

---

## 学习渠道

W3CSchool
## 时间线
 > 2018年7月10日：groovy环境搭建，基本语法,文件I/O，异常处理等

## 笔记

 - groovy方法，与java类似，使用def定义，可设置缺省值，这与kotlin是相似的
 ```
 def someMethod(parameter1, parameter2 = 0, parameter3 = 0) {
   // Method code goes here 
}
```
 - 文件I/O，读取，写入，获取文件大小等操作基本和java一样，复制文件有些区别：
```//将file2复制到file1中
    flie1 << file2.text
```
 - def声明各种类型，弱类型语言
 - 装箱拆箱
 - 范围，Range `1..10;  1..<10; 'a'..'z';   10..1`等
 - 映射（关联数组，字典）指的是map
 - 正则 用~表示：`def regex=~'Groovy'`
 - 异常处理 try-catch-finally
    ![Groovy异常层次结构][1]

  [1]: https://7n.w3cschool.cn/attachments/tuploads/groovy/hierarchy_exceptions.jpg