---
title: maven扫盲
date: 2022-03-26
tags: 
- 扫盲
- java
categories:
- "后端"
---
## pom文件扫盲
```xml
<!-- 取值jar：生成jar包 -->
<!-- 取值war，生成war包 -->
<!-- 取值pom，说明这个工程是用来管理其他工程的工程 -->
<packaging>jar</packaging>
<!-- scope 标签： 配置当前依赖的范围 -->
<scope>test</scope>
```
## maven指令扫盲
1. 清理操作 mvn clean 效果：删除target目录
2. 验证 validate 验证项目是否正确且所有必须信息是可用的
3. 编译操作 主程序编译：mvn compile 测试程序编译： mvn test-compile
4. 测试操作 mvn test 测试的报告存放目录 target/surefire-reports
5. 包装 package	打包	创建JAR/WAR包如在 pom.xml 中定义提及的包
6. 检查 verify	检查	对集成测试的结果进行检查，以保证质量达标
7. 安装 install	安装	安装打包的项目到本地仓库，以供其他项目使用
8. 部署 deploy	部署	拷贝最终的工程包到远程仓库中，以共享给其他开发人员和工程

## 测试依赖范围
```xml
<!-- scope 标签： 配置当前依赖的范围 -->
<scope>test</scope>
```
标签的可选值：compile/test/provided/system/runtime/import
### 对比
|         | main目录(空间) | test目录(空间) | 开发过程(时间) | 部署到服务器(时间) |
| ------- | -------------- | -------------- | -------------- | ------------------ |
| compile | 有效Y         | 有效Y         | 有效Y         | Y                  |
| test    | N              | Y              | Y              | 无效N                  |
| test    | Y              | Y              | Y              | N                  |
### 结论
compile：通常使用的第三方框架的 jar 包这样在项目实际运行时真正要用到的 jar 包都是以compile范围进行依赖的。比如 SSM 框架所需jar包。<br />
Test：测试过程中使用的jar包，以test范围依赖进来。比如junit。<br/>
provided：在开发过程中需要用到的"服务器上的 jar 包"通常以 provided 范围依赖进来。比如 servlet-api、jsp-api。而这个范围的 jar 包之所以不参与部署、不放进 war 包，就是避免和服务器上已有的同类 jar 包产生冲突，同时减轻服务器的负担。说白了就是:“服务器上已经有了，你就别带啦！”<br/>

## 继承依赖
只有打包方式为pom的Maven工程才能管理其他的Maven工程，打包方式为pom的Maven工程中不写业务代码，他是专门管理Maven工程的工程

主要作用是在父工程中统一管理依赖信息
```xml
<dependencyManagement>
        <dependencies>
            <!--MyBatis分页插件starter-->
            <dependency>
                <groupId>com.github.pagehelper</groupId>
                <artifactId>pagehelper-spring-boot-starter</artifactId>
                <version>${pagehelper-starter.version}</version>
        </dependency>
</dependencyManagement>
```
## build标签扫盲
生成微服务可运行的jar包，应用微服务打包插件，可以以spring boot微服务形式直接运行jar包
- 当前微服务本身的代码
- 当前微服务所依赖的jar包
- 内置Tomcat
- 与jar包可以通过java -jar方式直接启动微服务

仅靠Maven自身的构建能力是不够的，所以要通过build标签引入下面的插件 


```xml
 <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <excludes>
                        <exclude>
                            <groupId>org.projectlombok</groupId>
                            <artifactId>lombok</artifactId>
                        </exclude>
                    </excludes>
                </configuration>
            </plugin>
        </plugins>
    </build>
```