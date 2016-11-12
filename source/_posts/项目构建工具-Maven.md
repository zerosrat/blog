title: 项目构建工具 Maven
date: 2016-05-13 16:20:26
tags: Maven
categories: 项目构建
description: 用 Maven 从零开始构建一个项目。
---

## 准备工作

环境准备: **JDK** & [Maven](https://maven.apache.org/)

准备完成后,命令行输入 `$mvn -v` 以检查环境是否可以了

## 快速开始

### 使用来创建一个简单的 Java 程序

> 第一次不借助IDE，而是使用命令行来进行简单操作吧

1. `$mvn archetype:generate -DgroupId=com.mycompany.helloworld -DartifactId=helloworld -Dpackage=com.mycompany.helloworld -Dversion=1.0-SNAPSHOT` , 第一次运行时间略长, 可以趁这个空隙看看下面的章节 **什么是Maven**

2. 等远程原型(archetype)加载完毕后, 我们选择 `$maven-archetype-quickstart`, 然后选择版本, 我选择的是选项6(即版本1.1), 之后会让你确认

3. 之后会显示 `BUILD SUCCESS`, 然后我们会得到如下的项目结构
![](http://7xoxnz.com1.z0.glb.clouddn.com/maven01.png)

4. `$cd helloworld` 
`$mvn package` (第一次运行的话会有点慢)
构建成功的话会显示 `BUILD SUCCESS`

5. 验证程序是否可以运行
`$java -cp target/helloworld-1.0-SNAPSHOT.jar com.mycompany.helloworld.App`
<!-- more -->
***

## 什么是 Maven

### 一句话描述

Maven 是基于项目构建模型的，管理项目构建的工具

### Maven 项目的目录结构

<table><tr><th>目录</th><th>目的</th></tr><tr><td>${basedir}</td><td>存放 pom.xml和所有的子目录</td></tr><tr><td>${basedir}/src/main/java</td><td>项目的java源代码</td></tr><tr><td>{basedir}/src/main/resources</td><td>项目的资源，比如说property文件</td></tr><tr><td>${basedir}/src/test/java</td><td>项目的测试类，比如说 JUnit代码</td></tr><tr><td>${basedir}/src/test/resources</td><td>测试使用的资源</td></tr></table>

### pom.xml

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.mycompany.helloworld</groupId>
  <artifactId>helloworld</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>jar</packaging>

  <name>helloworld</name>
  <url>http://maven.apache.org</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>
```

上面内容是第一节项目生成的pom文件的代码

再回到第一节第一步的 `$mvn archetype:generate -DgroupId=com.mycompany.helloworld -DartifactId=helloworld -Dpackage=com.mycompany.helloworld -Dversion=1.0-SNAPSHOT`, `archetype:generate` 是指生成项目使用 Maven 提供的原型，而在之后我们选用了快速开始；再看 `groupId` 、 `artifactId` 和 `version` 是不是很眼熟，没错他们都在上面的 pom 文件出现了，下面我们来看看 pom 文件的字段：

- `modelVersion` 当前项目构建模型的版本
- `groupId` 一般是指创建项目的组织或是公司
- `artifactId` 可理解为项目的id
- `packaging` jar 或是 war
- `version` 项目版本，发布时会去掉 `SNAPSHOT`
- `name` 项目名，通常用于 Maven 生成的文档
- `url` 项目地址，通常用于 Maven 生成的文档
- `description` 项目描述，通常用于 Maven 生成的文档
- `dependency` 项目依赖

### Maven 插件

在第一节里，我们用了 `mvn archetype:generate` 命令来生成一个项目。那么这里的 `archetype:generate` 是什么意思呢？archetype 是一个插件的名字，generate 是目标(goal)的名字。这个命令的意思是告诉 maven 执行 archetype 插件的 generate 目标。插件目标通常会写成 pluginId:goalId

一个目标是一个工作单元，而插件则是一个或者多个目标的集合。比如说Jar插件，Compiler插件，Surefire插件等。从看名字就能知道，Jar 插件包含建立Jar文件的目标， Compiler 插件包含编译源代码和单元测试代码的目标。Surefire 插件的话，则是运行单元测试。

看到这里，估计你能明白了，mvn本身不会做太多的事情，它不知道怎么样编译或者怎么样打包。它把构建的任务交给插件去做。插件定义了常用的构建逻辑，能够被重复利用。这样做的好处是，一旦插件有了更新，那么所有的 maven 用户都能得到更新。

下面是一个 compiler 插件的例子：

```
<plugin>
	<artifactId>maven-compiler-plugin</artifactId>
	<version>3.3</version>
	<configuration>
		<source>1.7</source>
		<target>1.7</target>
	</configuration>
</plugin>
```

### 其他一些常用命令

- `mvn compile` 执行编译，编译好的类会被放到 `${basedir}/target/classes`
- `mvn install` 执行后，可在本地的其他项目引用当前项目了
- `mvn clean` 清空 `target` 目录下的数据
- `mvn eclipse:eclipse` 生成 eclipse 项目

***

## eclipse 与 Maven

以后有空再搞啦~

***

## 参考

> [Apache Maven 入门篇](http://www.oracle.com/technetwork/cn/community/java/apache-maven-getting-started-1-406235-zhs.html)
[Maven Getting Started Guide](https://maven.apache.org/guides/getting-started/index.html)