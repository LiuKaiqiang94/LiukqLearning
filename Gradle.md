# Gradle学习

## 时间线
- 2018年7月11日 开始学习Gradle 环境搭建
- 2018年7月17日 构建基础（任务依赖，任务操纵，方法抽取等）
- 2018年7月18日 调用Ant任务，默认任务,java项目多项目构建，工程依赖等
- 2018年7月19日 依赖管理，命令行

## 笔记
### 构建基础
- 基于约定，约定由于配置的构建框架
- 基于ApacheIvy的依赖管理
- build脚本由groovy dsl编写
- hello world: gradle -q hello
```
task hello {
    doLast {
        println 'Hello world!'
    }
}
```
- 任务依赖，gradle -q intro 
```
task hello << {
    println 'Hello world!'
}
task intro(dependsOn: hello) << {
    println "I'm Gradle"
}
```
- 动态任务 gradle -q task1(task2..4) 动态创建task，很强大
```
4.times { counter ->
    task "task$counter" << {
        println "I'm task number $counter"
    }
}
```

- 任务操纵 ，为已存在的任务增加行为 
```
4.times { counter ->
    task "task$counter" << {
        println "I'm task number $counter"
    }
}
task0.dependsOn task2, task3
```
- <<是doLast的简写，还有doFirst方法
- 可以以属性的方式访问任务
- 调用Ant任务（Ant：Apache Ant，有时间了解下），方法抽取，默认任务

### Java项目构建
- java构建 项目结构：
```
//标准java项目目录结构
    project  
        +build  
        +src/main/java  
        +src/main/resources  
        +src/test/java  
        +src/test/resources
```
Gradle 默认会从 src/main/java 搜寻打包源码，在 src/test/java 下搜寻测试源码。并且 src/main/resources 下的所有文件按都会被打包，所有 src/test/resources 下的文件 都会被添加到类路径用以执行测试。所有文件都输出到 build 下，打包的文件输出到 build/libs 下。
- gradle build 命令构建项目，clean 删除build目录，assemble 编译打包jar，check 编译测试代码
- 添加maven仓库
```
repositories{//添加仓库
    mavenCentral()
}

dependencies{//添加依赖
    compile group:'commons-collections',name:'commons-collections',version:'3.2'
    testCompile group:'junit',name:'junit',version:'4.+'
}
```
- 自定义java项目，自定义MANIFEST.MF,
```
uploadArchives {//发布jar包
    repositories {
        flatDir{
            dirs 'repos'
        }
    }
}
```
- 多项目构建，在settings.gradle中include多个项目，和Android项目一样`include "shared", "api", "services:webservice", "services:shared"`
- 公共配置，在根目录build.gradle中配置所有项目属性
- 工程依赖，参考android module 的build.gradle `compile project(':shared')`
### 依赖管理
- 名词：依赖解决，依赖声明，依赖配置。依赖配置的命令：compile(编译期),runtime(运行期),testCompile,testRuntime。
```
//快速定义外部依赖，group:name:version
dependencies{
    compile 'org.hibernate:hibernate-core:3.6.7.Final'
}
//几个仓库
repositories{
    mavenCentral()//Maven中央仓库
    maven{  //maven远程仓库
        url "http://repo.mycompany.com/maven2"
    }

    ivy{    //ivy远程仓库
        url "http://repo.mycompany.com/repo"
    }
}
```
- Groovy Web `apply plugin:'war'`  然后gralde build 发出war包
### Gradle命令行
-  `gralde dist test` 依次执行dist test
-  `gradle dist -x test` -x排除掉test任务
-  简化任务名，只要能唯一区分就可以 比如`gradle di`表示dist,驼峰`gradle cT`表示compileTest
```
//测试代码
task compile << {
    println 'compiling source'
}
task compileTest(dependsOn: compile) << {
    println 'compiling unit tests'
}
task test(dependsOn: [compile, compileTest]) << {
    println 'running unit tests'
}
task dist(dependsOn: [compile, test]) << {
    println 'building the distribution'
}
```
- -b 指定构建位置，-p 指定构建目录  `description='ssss'`添加描述信息
- `gradle -q tasks` 任务列表 追加--all 查看更多
- `gradle dependencies` 依赖列表

 <meta http-equiv="refresh" content="1">