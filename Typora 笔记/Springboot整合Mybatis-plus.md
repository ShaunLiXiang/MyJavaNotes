## 1.pom依赖

maven的引用很简单，官方已经给出starter，不需要我们考虑它的依赖关系了，此处使用的是2.3版本。

```xml
<dependency>
   <groupId>com.baomidou</groupId>
   <artifactId>mybatis-plus-boot-starter</artifactId>
   <version>2.3</version>
</dependency>
```

## 2.配置

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```xml
server.port=8080
 
#mysql
spring.datasource.url=jdbc:mysql://localhost:3306/ease-run?useUnicode=true&characterEncoding=utf8
spring.datasource.username=root
spring.datasource.password=root
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
#mybatis-plus
mybatis-plus.mapper-locations=classpath:com/mht/springbootmybatisplus/mapper/xml/*.xml
mybatis-plus.type-aliases-package=com.mht.springbootmybatisplus.entity
mybatis-plus.configuration.map-underscore-to-camel-case: true
 
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

官方已经提供了基于springboot的配置，将其拷贝过来放在application.yml中即可使用，此处只是将官方部分的配置删减过一些。其中column-underline: true特别好用，会自动将下划线格式的表字段，转换为以驼峰格式命名的属性。

**官方提供的yml配置：**

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```YAML
mybatis-plus:
  global-config:
    db-config:
      id-type: auto
      field-strategy: not_empty
      #驼峰下划线转换
      column-underline: true
      #逻辑删除配置
      logic-delete-value: 0
      logic-not-delete-value: 1
      db-type: mysql
    refresh: false
  configuration:
    map-underscore-to-camel-case: true
    cache-enabled: false
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

**注意事项：**
需要更改的地方有：文件输出路径（根据项目需要定制），数据源（此类是单独的数据库反向生成代码执行文件，因此springboot的数据源不起作用），包配置，以及一些基本的生成策略...总之还是参考一下我的另一篇文章吧，谢谢！

执行，刷新，获得自动生成的业务代码，不再赘述。

注意！！！生成后一定记得在spring boot项目中添加mybatis的包扫描路径，或@Mapper注解：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```JAVA
@SpringBootApplication
@MapperScan("com.mht.springbootmybatisplus.mapper")
public class SpringBootMybatisPlusApplication {
    private static final Logger logger = LoggerFactory.getLogger(SpringBootMybatisPlusApplication.class);
 
    public static void main(String[] args) {
        SpringApplication.run(SpringBootMybatisPlusApplication.class, args);
        logger.info("========================启动完毕========================");
    }
}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

或：

```
@Mapper
public interface UserMapper extends BaseMapper<User> {
}
```

否则会报：Error creating bean with name 'xxxServiceImpl': Unsatisfied dependency expressed through field 'baseMapper';

至此，我们的底层增删改查操作全部完毕！下面来编写简单的controller来测试效果。



**controller：**

 

## 5.分页

 

1.添加配置文件，此处配置文件表示开启mybatis-plus分页功能

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```JAVA
@EnableTransactionManagement
@Configuration
public class MybatisPlusConfig {
 
    @Bean
    public PaginationInterceptor paginationInterceptor() {
        return new PaginationInterceptor();
    }
}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

**或者：**

 

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```java
package com.paic.ocss.gateway.dao.config;

import com.baomidou.mybatisplus.entity.GlobalConfiguration;
import com.github.pagehelper.PageHelper;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;

import java.util.Properties;

@Configuration
@MapperScan("com.paic.ocss.gateway.dao.mapper*")
@Import(value = { com.paic.ocss.monitor.cat.mybatis.SpringCloudCatMybatisConfig.class })
public class MybatisConfig {

    @Bean
    public GlobalConfiguration globalConfiguration() {
        GlobalConfiguration global = new GlobalConfiguration();
        global.setDbType("mysql");
        return global;
    }

    /**
     * 配置mybatis的分页插件pageHelper
     * @return
     */
    @Bean
    public PageHelper pageHelper(){
        PageHelper pageHelper = new PageHelper();
        Properties properties = new Properties();
        properties.setProperty("offsetAsPageNum","true");
        properties.setProperty("rowBoundsWithCount","true");
        properties.setProperty("reasonable","true");
        //配置mysql数据库的方言
        properties.setProperty("dialect","mysql");
        pageHelper.setProperties(properties);
        return pageHelper;
    }

}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 

 

 

**Mapper:**

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```java
/**
 * User 表数据库控制层接口
 */
public interface UserMapper extends BaseMapper<User> {
    @Select("selectUserList")
    List<User> selectUserList(Pagination page,String state);
}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

## 新建UserMapper配置文件:

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.baomidou.springmvc.mapper.system.UserMapper">

    <!-- 通用查询结果列-->
    <sql id="Base_Column_List">
        id, name, age
    </sql>

    <select id="selectUserList" resultType="User">
        SELECT * FROM sys_user WHERE state=#{state}
    </select>
</mapper>
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

## 4.新建service层类UserService:

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```java
/**
 *
 * User 表数据服务层接口实现类
 *
 */
@Service
public class UserService extends ServiceImpl<UserMapper, User>{
    public Page<User> selectUserPage(Page<User> page, String state) {
        page.setRecords(baseMapper.selectUserList(page,state));
        return page;
    }
}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

UserService继承了ServiceImpl类,mybatis-plus通过这种方式为我们注入了UserMapper,这样可以使用service层默认为我们提供的很多方法,也可以调用我们自己在dao层编写的操作数据库的方法.Page类是mybatis-plus提供分页功能的一个model,继承了Pagination,这样我们也不需要自己再编写一个Page类,直接使用即可.

## 　5,新建controller层UserController:

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```java
@Controller
public class UserController extends BaseController {

    @Autowired
    private IUserService userService;

    @ResponseBody
    @RequestMapping("/page")
    public Object selectPage(Model model){

        Page page=new Page(1,10);          //1表示当前页，而10表示每页的显示显示的条目数
        page = userService.selectUserPage(page, "NORMAL");
        return page;
    }
```