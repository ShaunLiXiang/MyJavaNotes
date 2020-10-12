# 一、SpringMVC入门

## （一）第一个程序

1. 创建maven项目

2. 删除src目录

3. 创建modual模组

4. 在WEB-INF目录下创建子目录jsp

5. 在jsp目录中新建.jsp文件测试代码

6. 接下来配置web.xml文件

7. 无法打开ServletContext资源[/WEB-INF/springmvc-servlet.xml]在web.xml里面加上<inint-param>标签

8. ```xml
   <!--1.注册dispatcherServlet-->
       <servlet>
           <!--前端控制器-->
           <servlet-name>SpringMvc</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <!--关联一个springmvc的配置文件[servlet-name]-servlet.xml-->
           <init-param>
               <param-name>contextConfigLocation</param-name>
               <!--在Resource目录下创建springmvc-servlet.xml具体流程见（9）-->
               <param-value>classpath:springmvc-servlet.xml</param-value>
           </init-param>
           <!--启动级别-->
           <load-on-startup>1</load-on-startup>
       </servlet>
       <!--2.配置servletmapping-->
       <servlet-mapping>
           <servlet-name>SpringMvc</servlet-name>
           <url-pattern>/</url-pattern>
       </servlet-mapping>
       <welcome-file-list>
           <welcome-file>index.jsp</welcome-file>
       </welcome-file-list>
   ```

9. Right Click 目录 new XML Configuration File ==》Spring Config

10. ```xml
    <!--处理器映射器-->
    <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
    <!--处理器适配器-->
    <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>
    
    <!--视图解析器：DispatcherServlet给它的ModelAndView-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="InternalResourceViewResolver">
        <!--前缀-->
        <!--/WEB-INF/jsp/ jsp后面的斜杠别掉了，这个地方很容易遗漏-->
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
    
    <!--Controller-->
    <bean id="/hello" class="com.li.controller.HelloController"/>
    ```

11. 在java目录下新建一个com.li.controller.HelloController.class的类并且实现接口Controller(org.springframework.web.servlet.mvc.Controller)

12. 重载方法

13. new 一个ModelAndView对象

14. ModelAndView mv= new ModelAndView();

15. 将目标封装为一个对象

16. mv.addObject(参数1,参数2); 	具体参数参照键值对和session.setAtribute("mesg","内容");

17. 接下来封装视图	==》mv.setViewName("hello");

18. 返回值 return mv;

19. ```java
    package com.li.controller;
    
    import org.springframework.web.servlet.ModelAndView;
    import org.springframework.web.servlet.mvc.Controller;
    
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;
    
    public class HelloController implements Controller {
        public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
            //类型和视图
            ModelAndView modelAndView = new ModelAndView();
            //封装对象，在ModelAndView中，Model
            modelAndView.addObject("mesg","嗨喽controller");
            //封装要跳转的视图
            modelAndView.setViewName("hello");  //web-info/jsp/hello.jsp
            return modelAndView;
        }
    }
    ```

20.接下来配置Tomcat

**注意事项：**

页面404可能是lib没有导进去

***方法一***：![image-20200518111752099](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200518111752099.png)

![image-20200518111943304](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200518111943304.png)

在WEB-INF目录下新建目录lib（Directory）select lib choose button(+) add All Library Files

***方法二***：

Refresh ==》Maven库

***方法三:***

restart IDEA

## 二、使用注解开发springmvc

步骤如下:

新建一个modual==》添加框架支持，配置web.xml：

配置DispatcherServlet

其中<init-param>不要忘记

以及启动时加载的级别<load-on-startup>

