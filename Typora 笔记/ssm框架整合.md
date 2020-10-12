## [maven中央仓库太慢的解决办法](https://www.cnblogs.com/fanblogs/p/12409866.html)

在.m2目录下创建settings.xml文件，文件内容如下：

<?xml version="1.0" encoding="UTF-8"?>

<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">

  <localRepository>
    /Users/dengchao/.m2/repository
  </localRepository>

  <pluginGroups></pluginGroups>

  <proxies></proxies>

  <servers></servers>

  <mirrors>
    <mirror> 
      <id>alimaven</id>
      <name>aliyun maven</name> 
      <url>http://maven.aliyun.com/nexus/content/groups/public/</url> 
      <mirrorOf>central</mirrorOf> 
      </mirror> 
  </mirrors>

  <profiles></profiles>

</settings>

# 一、基本环境搭建

## 1、新建maven项目在pom.xml中导入依赖

```xml
 <dependencies>
     	<!--单元测试-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
     	
     	<!--mybatis5个-->
     	<dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.4</version>
        </dependency>
     	<!--mybatis-spring-->
     	<dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>2.0.2</version>
        </dependency>
        <!--数据库驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.46</version>
        </dependency>
     	<!--jdbc-->
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.2.6.RELEASE</version>
        </dependency>
        <!--数据库连接池-->
        <dependency>
            <groupId>com.mchange</groupId>
            <artifactId>c3p0</artifactId>
            <version>0.9.5.2</version>
        </dependency>
     
        <!--springmvc有4个 ==> 1、spring-webmvc 2、jsp 3、jstl 4、servlet-->
     	<dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.1.9.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <version>2.5</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.2</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>
    </dependencies>

<!--静态资源导出问题-->
<build>
    <resources>
        <resource>
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>false</filtering>
        </resource>
        <resource>
            <directory>src/main/resources</directory>
            <includes>
                <include>**/*.xml</include>
                <include>**/*.properties</include>
            </includes>
            <filtering>false</filtering>
        </resource>
    </resources>
</build>
```

## 2、mybatis

在resource下面配置xml文件

mybatis-config.xml

- ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE configuration
          PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-config.dtd">
  
  <configuration>
  
      <!--别名-->
      <typeAliases>
          <package name="com.li.pojo"/>
      </typeAliases>
      <mappers>
          <mapper resource="mapper/BookMapper.xml"/>
      </mappers>
  </configuration>
  ```

  ## applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
        <import resource="classpath:spring-dao.xml"/>
        <import resource="classpath:spring-service.xml"/>
        <import resource="classpath:springmvc-servlet.xml"/>
</beans>
```

​	tip:Application context not configured for this file

​	去右边那个小齿轮那Create new application context



## 3、接下来搞定mybatis 关联数据库

- database.propertiess

```xml
#如果使用的是mysql 8.x+ mysql后面要加.cj
jdbc.driver=com.mysql.jdbc.Driver
#如果使用的是mysql 8.x+url需要加上时区
jdbc.url=jdbc:mysql//localhost:3306/ssmbuild?useSSL=false&useUnicode=true&characterEncoding=utf8
jdbc.username=root
jdbc.password=123
```

## 4、mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>
    <!--别名-->
    <typeAliases>
        <package name="com.li.pojo"/>
    </typeAliases>
    <mappers>
        <mapper resource="mapper/BookMapper.xml"/>
    </mappers>
</configuration>
```

## 5、spring-dao.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
    
    <!--1.关联数据库配置文件-->
    <context:property-placeholder location="classpath:db.properties"/>
    
    <!--2.连接池 dbcp:半自动化,不能自动连接 c3p0:自动化加载配置文件，并且可以设置到对象中druid:hikari-->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="${jdbc.driver}"/>
        <property name="jdbcUrl" value="${jdbc.url}"/>
        <property name="user" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>
    
    <!--3.sqlSessionFactory整合mybatis-->
    <bean class="org.mybatis.spring.SqlSessionFactoryBean" id="sqlSessionFactory">
        <property name="dataSource" ref="dataSource"/>
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
    </bean>
    
    <!--4.配置dao接口扫描包,动态实现mapper接口注入到spring容器中-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!--注入到sqlsession-->
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
        <!--扫描dao包-->
        <property name="basePackage" value="com.li.dao"/>
    </bean>
</beans>
```

## 6、spring-service.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
    
    <!--1.扫描service下的包-->
    <context:component-scan base-package="com.li.service"/>
    
    <!--2.将所有的业务类注入到spring-->
    <!--已通过@Resource注解配置好-->
    
    <!--3.声明式事务-->#这里我理解为 业务层调用增删改等方法时需要提交事务，所以在service.xml中配置
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <!--注入数据源-->
        <property name="dataSource" ref="dataSource"/>
    </bean>
</beans>
```

## 7、springmvc-servlet.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
    
    <!--1.扫描controller-->
    <context:component-scan base-package="com.li.controller"/>
    
    <!--2.处理器映射器，处理器适配器-->
    <!--3.支持注解驱动-->
    <mvc:annotation-driven/>
    
    <!--4.不处理静态资源-->
    <mvc:default-servlet-handler/>
    
    <!--5.视图解析器-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
</beans>
```