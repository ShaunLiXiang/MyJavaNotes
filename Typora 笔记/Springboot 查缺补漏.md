**实验的请求结构**

# 巨坑

## 1.mapper 路径

tmd，这个<u>config-location</u>后面要加    ！！！！**<u>s</u>**

```xml
mybatis:
  config-locations: classpath:mapper/*.xml
  type-aliases-package: com.lx.springcloud.entity
```

![请求结构](D:\Typera\img\image-20200804152643428.png)

**Springboot 静态资源映射**

Prioty:  resour<u>ce</u>>static>public

http://webjars.org/

```xml
<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>jquery</artifactId>
    <version>3.5.1</version>
</dependency>
```

在前后端不分离的情况下使用模板引擎

**<u>thymeleaf</u>**

<html lang="en" xmlns:th="http://www.thymeleaf.org">

引入模板引擎后，静态页面 ***需要***  放在templates目录下

国际化地区解析：  ===》   (   )    《===用于传参

```html
<a class="btn btn-sm" th:href="@{/index.html(language='zh_CN')}">中文</a>
<a class="btn btn-sm" th:href="@{/index.html(language='en_US')}">English</a>
```



**国际化语言转换**

LocaleResolver <u>区域解析器</u>

```java
public class MyLocaleResolver implements LocaleResolver {

    @Override
    public Locale resolveLocale(HttpServletRequest request) {
        String language = request.getParameter("language");
        Locale locale = Locale.getDefault();//使用默认的语言
        if(!StringUtils.isEmpty(language)){
            String[] split = language.split("_");
            //语言，国家地区
            locale = new Locale(split[0], split[1]);
        }
        return locale;
    }

    @Override
    public void setLocale(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Locale locale) {

    }
}
```

th:if="${not #strings.isEmpty(msg)}" 

判断msg是否不为空

```java
@Configuration
public class MyWebConfig implements WebMvcConfigurer {

    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/").setViewName("index");
        registry.addViewController("/index.html").setViewName("index");
        registry.addViewController("/index").setViewName("index");
        registry.addViewController("/dashboard").setViewName("dashboard");
    }
    
    @Bean
    public LocaleResolver localeResolver(){
        return new MyLocaleResolver();
    }

    //添加拦截器
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new LoginHandlerInterceptor())
            	/**
            	*添加拦截的东西
            	*/
                .addPathPatterns("/**")
                .excludePathPatterns("/index")
                .excludePathPatterns("/index.html")
                .excludePathPatterns("/")
        		//注意，要放行css js img等静态资源
        		.excludePathPatterns("/css/*")
                .excludePathPatterns("/js/*")
                .excludePathPatterns("/img/*");
    }
}
```



**登录拦截器**

```java
public class LoginHandlerInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {

        String loginUser = (String) request.getSession().getAttribute("loginUser");
        if(StringUtils.isEmpty(loginUser)){
            request.getRequestDispatcher("index.html").forward(request,response);
            return false;
        }
        else{
            request.getRequestDispatcher("dashboard.html").forward(request,response);
            return true;
        }
    }
}
```

**Thymeleaf共片段抽取和引入**

topbar ： 	=>> 	**自定义的片段名**

sidebar:		=>>	**id**

th:insert:将整个公共片段插入

th:replace：将整个标签替换为公共片段

th:include：将外层标签去掉，将内容插入

```html
<div th:fragment="topbar"></div>
<div th:insert="~{dashboard::topbar}"></div><==><div th:insert="~dashboard::topbar">
    <div th:replace="~{dashboard::topbar}"></div>
    <div th:include="~{dashboard::topbar}"></div>
    模板名::选择器如	---》》》<div th:replace="~{dashboard::sidebar}"></div>
    模板名::片段名 	 ---》》》<div th:replace="~{dashboard::topbar}"></div>
```



**整合Druid**

```yaml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/mysql?serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8
    username: root
    password: 123
    type: com.alibaba.druid.pool.DruidDataSource
```

测试时无法直接注入Datasource

需要配置类 ***未写完***

```java
@Configuration
public class DruidConfiguration {

    @Bean
    @ConfigurationProperties(prefix = "spring.datasource")
    public DataSource dataSource(){
        return new DruidDataSource();
    }
}
```

**o.s.b.d.LoggingFailureAnalysisReporter 错误解决方法**

在@SpringBootApplication注解后面加上(exclude = {DataSourceAutoConfiguration.class})