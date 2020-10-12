代码生成

# Mybatis-plus 依赖版本3.3.1后面加上.tmp

[分页](https://www.cnblogs.com/seekknowledge/p/11734955.html)

[结果集映射](https://blog.csdn.net/qq_42780864/article/details/81429114)

## 引入依赖

```xml
<dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-cache</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <!-- mybatis plus 场景启动器 -->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.3.1.tmp</version>
        </dependency>
        <!-- 代码生成的依赖 -->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-generator</artifactId>
            <version>3.3.1.tmp</version>
        </dependency>
        <dependency>
            <groupId>org.apache.velocity</groupId>
            <artifactId>velocity-engine-core</artifactId>
            <version>2.0</version>
        </dependency>
    </dependencies>
```

# Mybatis-Plus 配置

```properties
#禁用模板缓存
spring.thymeleaf.cache=false
#数据库配置
server.port=9000
spring.datasource.username=root
spring.datasource.password=123
spring.datasource.url=jdbc:mysql://localhost:3306/boot_crm?useSSL=false&useUnicode=true&characterEncoding=utf8&serverTimezone=GMT%2B8
spring.datasource.driver-class=com.mysql.cj.jdbc.Driver
spring.datasource.type=com.alibaba.druid.pool.DruidDataSource
#配置日志
mybatis-plus.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
#配置逻辑删除
mybatis-plus.global-config.db-config.logic-delete-value=1
mybatis-plus.global-config.db-config.logic-not-delete-value=0

```

![image-20200810101948298](D:\Java笔记\Typora\img\GlobalConfig包.png)

```java
package com.lx.utils;

import com.baomidou.mybatisplus.annotation.DbType;
import com.baomidou.mybatisplus.generator.AutoGenerator;
import com.baomidou.mybatisplus.generator.config.DataSourceConfig;
import com.baomidou.mybatisplus.generator.config.GlobalConfig;
import com.baomidou.mybatisplus.generator.config.PackageConfig;
import com.baomidou.mybatisplus.generator.config.StrategyConfig;
import com.baomidou.mybatisplus.generator.config.rules.NamingStrategy;

public class CodeGenerator {
    public static void main(String[] args) {
        AutoGenerator mpg = new AutoGenerator();

        //全局配置w
        GlobalConfig gc = new GlobalConfig();
        gc.setAuthor("李某帅");
        gc.setOpen(false);
        gc.setOutputDir(System.getProperty("user.dir")+"/cloud-api-commons/src/main/java/");
        gc.setFileOverride(true);
        gc.setMapperName("%sMapper");
        gc.setXmlName("../../../../../resources/mapper/%sMapper");
        mpg.setGlobalConfig(gc);

        //数据源配置
        DataSourceConfig dsc = new DataSourceConfig();
        dsc.setDbType(DbType.MYSQL);
        dsc.setDriverName("com.mysql.cj.jdbc.Driver");
        dsc.setUrl("jdbc:mysql:///db2020?useSSL=false&useUnicode=true&serverTimezone=GMT%2B8&characterEncoding=utf8");
        dsc.setUsername("root");
        dsc.setPassword("123");
        mpg.setDataSource(dsc);

        //策略配置
        StrategyConfig sc = new StrategyConfig();
        sc.setColumnNaming(NamingStrategy.underline_to_camel);
        sc.setColumnNaming(NamingStrategy.underline_to_camel);
        sc.setInclude(new String[]{"user"});
        sc.setEntityLombokModel(true);
        mpg.setStrategy(sc);

        //包配置
        PackageConfig pc = new PackageConfig();
        pc.setParent("com.lx");
        pc.setEntity("entity");
        pc.setMapper("mapper");
        pc.setService("service");
        pc.setServiceImpl("serviceImpl");
        pc.setController("controller");
        mpg.setPackageInfo(pc);

        mpg.execute();
    }
}

```

```properties
#mappe.xml 配置
#如果引用mybatis-plus-boot-starter 依赖，需要配置
mybatis-plus.mapper-locations=classpath:mapper/*.xml
#如果引用mybatis-plus 依赖，需要配置
#mybatis.mapper-locations=classpath:mapper/*.xml

#禁用模板缓存
spring.thymeleaf.cache=false

#数据库配置
server.port=9000
spring.datasource.username=root
spring.datasource.password=123
spring.datasource.url=jdbc:mysql://localhost:3306/boot_crm?useSSL=false&useUnicode=true&characterEncoding=utf8&serverTimezone=GMT%2B8
spring.datasource.driver-class=com.mysql.cj.jdbc.Driver
spring.datasource.type=com.alibaba.druid.pool.DruidDataSource

#配置日志
#mybatis-plus.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
#配置逻辑删除
#mybatis-plus.global-config.db-config.logic-delete-value=1
#mybatis-plus.global-config.db-config.logic-not-delete-value=0

#数据库脚本
#spring.datasource.initialization-mode=always
# schema.sql中一般存放的是建表语句DDL
#spring.datasource.data =  classpath:/db/data.sql
```

**分页**



```java
package com.lx.boot.config;

import com.baomidou.mybatisplus.extension.plugins.PaginationInterceptor;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
//配置拦截器
@Configuration
public class MybatisPlusConfig {
    @Bean
    public PaginationInterceptor paginationInterceptor(){
        return new PaginationInterceptor();
    }
}

//分页实现
package com.lx.boot.serviceImpl;

import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
import com.baomidou.mybatisplus.core.metadata.IPage;
import com.baomidou.mybatisplus.extension.plugins.pagination.Page;
import com.lx.boot.entity.Customer;
import com.lx.boot.mapper.CustomerMapper;
import com.lx.boot.service.ICustomerService;
import com.baomidou.mybatisplus.extension.service.impl.ServiceImpl;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import javax.annotation.Resource;

/**
 * <p>
 *  服务实现类
 * </p>
 *
 * @author 李享
 * @since 2020-08-11
 */
@Service
public class CustomerServiceImpl extends ServiceImpl<CustomerMapper, Customer> implements ICustomerService {

    @Autowired
    private CustomerMapper customerMapper;

    @Override
    public IPage<Customer> listAll(Integer current) {
        Page<Customer> page = new Page<Customer>(current, 2);
        QueryWrapper<Customer> queryWrapper = new QueryWrapper<Customer>().orderByDesc("cust_createtime");
        IPage<Customer> result = customerMapper.selectPage(page,queryWrapper);
        // 遍历当前页的数据
        result.getRecords().forEach(System.out::println);
        return result;
    }
}

```

