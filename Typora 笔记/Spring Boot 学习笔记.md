# Spring Boot 学习笔记

## 1、Spring Hello World

### 1.1、创建一个maven工程

### 1.2、导入springboot的相关依赖

```xml
<parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.0.RELEASE</version>
    </parent>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
```

## 2、编写一个主程序，启动spring boot应用

## 3、编写一个controller或者service

## 4、启动主程序的main方法，测试

## 5、简化部署

可以在maven project 里面找Life cycle 执行package打包

```xml
<!-- 这个插件可以将spring应用打包成一个可执行jar包 -->
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
```

## 6、Hello World探究

### 6.1、POM文件
父项目

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.3.0.RELEASE</version>
</parent>
它里面还有一个父项目
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.3.0.RELEASE</version>
  </parent>
他来管理Spring Boot所有依赖的版本
```

Spring Boot的版本仲裁中心;

以后导入依赖不需要写版本(没有在dependencies里面的自然需要写版本号咯0.0比如说Lombok插件);

### 6.2、导入的依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

spring-boot-starter-**web**

spring-boot-starter : spring-boot场景启动器；帮我们导入web模块正常运行所依赖的组件;

Spring Boot将所有的功能场景都抽取出来，做成一个个的starter(启动器), 只需要在项目里面引入 写starter相关场景的所有依赖都会导入进来。要用什么功能就导入什么场景的启动器