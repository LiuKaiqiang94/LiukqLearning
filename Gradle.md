# Gradle学习

## 时间线
- 2018年7月11日 开始学习Gradle 环境搭建
- 2018年7月17日 构建基础（任务依赖，任务操纵，方法抽取等）
- 2018年7月18日 

## 笔记
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
- 调用Ant任务（Ant：Apache Ant，有时间了解下）


 <meta http-equiv="refresh" content="1">