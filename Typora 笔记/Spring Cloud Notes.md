# Spring Cloud Notes

![SpringCloud概况图](F:\Typora 笔记\img\SpringCloud概况图.png)

# 一、搭建环境

镜像

```xml
<mirror>
    <id>nexus-aliyun</id>
    <name>Nexus aliyun</name>
    <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
    <mirrorOf>central</mirrorOf>
</mirror>
```



## 1.版本的选择

spring.io

## 2.MAVEN的版本

为了便于学习，选择和阳神一致的版本 3.5.2

## 3.POM文件

**父模块**

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.lx</groupId>
    <artifactId>springcloud</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>pom</packaging>
    <!-- packaging pom 总工程-->

    !--统一管理jar包版本-->
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <junit.version>4.12</junit.version>
        <lombok.version>1.18.12</lombok.version>
        <log4j.version>1.2.17</log4j.version>
        <logback.version>1.2.3</logback.version>
        <mysql.version>8.0.21</mysql.version>
        <druid.version>1.1.10</druid.version>
        <mybatis.spring.boot.version>1.3.0</mybatis.spring.boot.version>
    </properties>

    <!--子模块继承之后，提供作用：锁定版本+子module不用写groupId和version-->
    <dependencyManagement><!--定义规范，但不导入-->
        <dependencies>
            <dependency>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-project-info-reports-plugin</artifactId>
                <version>3.0.0</version>
            </dependency>
            <!--spring boot 2.2.2-->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>2.3.3.RELEASE</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <!--spring cloud Hoxton.SR1-->
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Hoxton.SR8</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <!--spring cloud 阿里巴巴-->
            <dependency>
                <groupId>com.alibaba</groupId>
                <artifactId>druid-spring-boot-starter</artifactId>
                <version>${druid.version}</version>
            </dependency>
            <!--mysql-->
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>${mysql.version}</version>
                <scope>runtime</scope>
            </dependency>
            <!-- druid-->
            <dependency>
                <groupId>com.alibaba</groupId>
                <artifactId>druid</artifactId>
                <version>${druid.version}</version>
            </dependency>
            <!--mybatis-->
            <dependency>
                <groupId>org.mybatis.spring.boot</groupId>
                <artifactId>mybatis-spring-boot-starter</artifactId>
                <version>${mybatis.spring.boot.version}</version>
            </dependency>
            <!--junit-->
            <dependency>
                <groupId>junit</groupId>
                <artifactId>junit</artifactId>
                <version>${junit.version}</version>
            </dependency>
            <!--log4j-->
            <dependency>
                <groupId>log4j</groupId>
                <artifactId>log4j</artifactId>
                <version>${log4j.version}</version>
            </dependency>
            <dependency>
                <groupId>ch.qos.logback</groupId>
                <artifactId>logback-core</artifactId>
                <version>${logback.version}</version>
            </dependency>
            <dependency>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok</artifactId>
                <version>${lombok.version}</version>
            </dependency>
        </dependencies>
    </dependencyManagement>
    <!--热启动插件-->
<!--    <build>-->
<!--        <plugins>-->
<!--            <plugin>-->
<!--                <groupId>org.springframework.boot</groupId>-->
<!--                <artifactId>spring-boot-maven-plugin</artifactId>-->
<!--                <configuration>-->
<!--                    <fork>true</fork>-->
<!--                    <addResources>true</addResources>-->
<!--                </configuration>-->
<!--            </plugin>-->
<!--        </plugins>-->
<!--    </build>-->
</project>
```

**子模块**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>springcloud</artifactId>
        <groupId>com.lx</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-provider-payment8001</artifactId>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.1.10</version>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
</project>
```

# 二、服务

## 1.cloud-provider-payment8001

多个服务注册，服务名保持一致，负载均衡会用到

非必要将数据库相关属性注释

```yaml
server:
  port: 8001
sping:
  application:
    name: cloud-payment-service
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/db2020?serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&useSSL=false
    username: root
    password: 123
    driver-class-name: com.mysql.cj.jdbc.Driver
#    type: com.alibaba.druid.pool.DruidDataSource

mybatis:
  type-aliases-package: com.lx.springcloud.entity
  mapper-locations: classpath:mapper/*.xml
```

### 1）Controller

```java
@RestController
public class PaymentController {
    @Resource
    PaymentServiceImpl paymentService;
    @GetMapping("/payment/{id}")
    public BaseResult<Payment> get(@PathVariable("id") Long id){
        return new BaseResult<Payment>(200,"成功",paymentService.findById(id));
    }
    @PostMapping("/payment-create")
    public BaseResult<Integer> get(@RequestBody Payment payment){
        return new BaseResult<Integer>(200,"成功", paymentService.create(payment));
    }
}
```

tips: **@RequestBody接收前端传过来的json数据** 这玩意很重要，不能掉

## 2.cloud-consumer-order80

```yaml
server:
  port: 80
```

### 1）配置RestTemplte

```java
@Configuration
public class ApplicationContext {

    @Bean
    public RestTemplate getRestTemplate(){
        return new RestTemplate();
    }
}
```

### 2）订单服务中OrderController 调用payment服务

```java
@RestController
public class OrderController {

    public static final String PAYMENT_SRV = "http://CLOUD-PAYMENT-SERVICE";

    @Autowired
    private RestTemplate restTemplate;

    @GetMapping("/consumer/payment-create")
    public BaseResult<Payment> create(Payment payment){
        return restTemplate.postForObject(PAYMENT_SRV + "/payment-create",payment, BaseResult.class);
    }

    @GetMapping("/consumer/payment/{id}")
    public BaseResult<Payment> get(@PathVariable("id") Long id){
        return restTemplate.getForObject(PAYMENT_SRV+"/payment/"+id,BaseResult.class);
    }
}
```

# 三、工程重构

## 1.new Module ==》 cloud-api-commons

cv通用实体类，删除冗余的实体类

## 2.新的依赖

```xml
		<dependency>
            <groupId>cn.hutool</groupId>
            <artifactId>hutool-all</artifactId>
            <version>5.1.0</version>
        </dependency>
```

## 3.检查maven环境

**<u>*refresh  ==>  clean 一定要  install*</u>**

## 4.引入自定义的api通用包

可以使用Payment支付Entity

```xml
		<dependency>
            <groupId>com.lx</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>${project.version}</version>
        </dependency>
```

## 5.add 依赖

# 四、服务注册中心

## **简介：**

*Eureka采用了CS的设计架构，Eureka Server 作为服务注册功能的服务器，它是服务注册中心。*

*而系统中的其他服务，使用Eureka的客户端连接到  Eureka Server 并维持  <u>心跳连接</u>  。这样系统的维护人员就可以通过 Eureka Server 来监控系统中的各个微服务是否正常运行。*

*在服务注册与发现中，有一个注册中心。当服务启动的时候，会把当前自己服务器的信息 比如 服务器地址通讯地址等 以  **别名**  方式注册到注册中心上。另一方（消费者| 服务提供者），以该别名的方式去注册中心上获取到实际的服务通讯地址，然后再实现本地RPC远程调用框架核心设计思想：在于注册中心，因为使用注册中心管理  <u>每个服务与服务之间</u>  的一个依赖关系（**服务治理概念**）。在任何rpc远程调用框架中，都会有一个注册中心（存放服务地址相关信息（接口地址））。*

### 1、Eureka包含两个组件 ：Eureka Server 和 Eureka Client

#### 1）.**<u>Eureka Server	 提供服务注册服务</u>**

各个微服务节点通过配置启动后，会在 Eureka Server 中进行注册，这样 Eureka Server 中的服务注册表将会存储所有可用服务节点的信息，服务节点的信息可以在界面中直观查看到。

#### 2）.**<u>Eureka Client  	通过注册中心进行访问</u>**

它是一个 Java 客户端，用于简化 Eureka Server 的交互，客户端同时也具备一个内置的、使用轮询（round-robin）负载算法的负载均衡器在应用启动后，将会向Eureka Server 发送心跳 （默认周期为30秒）。如果 Eureka Server 在**多个心跳周期内**没有接收到某个节点的心跳， Eureka Server 将会从服务注册表中把这个服务节点移除（默认90秒）。

​																								*tips:——emm...也就是某个微服务向服务注册中心发送一个持续的心跳30秒。*

​																								*当Eureka Server 90秒内 一直没检测到该心跳就会将该微服务移除。*

## 2、单机

### 1）Provier发送心跳给Euraka监测是否存活（心跳默认持续30秒），当Euraka过了一段时间一直没有检测到这个心跳（默认90秒）时就会将Provider移除。

### 2）Eureka Server 给该服务注册中心设置一个默认端口号：   <u>7001</u>

### 3）引入依赖 spring-cloud-starter-netflix-eureka-server

*<u>wdnmd这个依赖千万千万不能导错，spring-cloud-netflix-eureka-server和spring-cloud-starter版本不匹配</u>*

*<u>要导入下方这个依赖！！！！！！！！！！！！花了一晚上+2节课踩的坑</u>*

```xml
		<dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
        </dependency>
```

### 4)配置

```yaml
server:
  port: 7001

eureka:
  #eureka实例
  instance:
    hostname: localhost
  client:
    #false表示不向注册中心注册自己。
    register-with-eureka: false
    #false表示自己端就是注册中心，其职责就是维护服务实例，并不需要去检索服务
    fetch-registry: false
    service-url:
      #设置与Eureka Server交互的地址查询服务和注册服务都需要依赖这个地址
      defaultZone: http://${eureka.instance.hostname}:${server.port}
```

### 5)Eureka的主启动类要加上@EnableEurekaServer

给要注册的服务引入依赖:

==》<u>spring-cloud-starter-netflix-eureka-client</u>

```xml
		<dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
```

将服务注册进eureka server中

***这里以 payment服务为例***

```yaml
server:
  port: 8001
sping:
  application:
    name: cloud-payment-service
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/db2020?serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&useSSL=false
    username: root
    password: 123
    driver-class-name: com.mysql.cj.jdbc.Driver
    type: com.alibaba.druid.pool.DruidDataSource

mybatis:
  type-aliases-package: com.lx.springcloud.entity
  mapper-locations: classpath:mapper/*.xml

eureka:
  client:
    #是否将该服务注册进ereka server中,默认true
    register-with-eureka: true
    #是否从eureka中取出已有的注册信息，默认为true,单节点无所谓，集群必须设置为true才能配合ribbon使用负载均衡
    fetch-registry: true
    service-url:
      defaultZone: http://localhost:7001/eureka
```

同时，payment服务在主启动类上添加注解**<u>@EnableEurekaClient</u>**

## 2.集群原理说明

![Eureka集群](F:\Typora 笔记\img\Eureka集群.png)

这里是远程过程调用，上面的讲义图片错了

微服务RPC远程过程调用**最核心的是**   *高可用*

### 1）在/etc/hosts下添加域名映射

```txt
##############SpringCloud###############
127.0.0.1 eureka7001.com
127.0.0.1 eureka7002.com
127.0.0.1 eureka7003.com
```

#互相注册，相互守望

它们是并列的，协同的关系

### 2）cloud-eureka-service7001配置

```yaml
server:
  port: 7001

eureka:
  instance:
    hostname: eureka7001.com
  client:
    register-with-eureka: false
    fetch-registry: false
    service-url:
      defaultZone: http://eureka7002.com:7002/eureka
#多个注册中心url用逗号隔开
```

### 3）cloud-eureka-service7002

```yaml
server:
  port: 7002

eureka:
  instance:
    hostname: eureka7002.com
  client:
    register-with-eureka: false
    fetch-registry: false
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka
#多个注册中心url用逗号隔开
```

### 4）cloud-provider-payment注册的服务集群配置

```yaml
eureka:
  client:
    #是否将该服务注册进ereka server中,默认true
    register-with-eureka: true
    #是否从eureka中取出已有的注册信息，默认为true,单节点无所谓，集群必须设置为true才能配合ribbon使用负载均衡
    fetch-registry: true
    service-url:
      defaultZone: http://localhost:7001/eureka,http://localhost:7002/eureka

```

Tips**相比单机注册而言**，集群注册的配置在默认注册中心中用**逗号**将多个服务中心隔开就好，

订单模块中的Controller的 静态字符串常量PAYMENT_SERViCE不能写死

原地址 public static final String PAYMENT_URL = "http://localhost:8001";

换成：

public static final String PAYMENT_SRV = "http://CLOUD-PAYMENT-SERVICE";

端口号采用k8s 获取

```java
@Value("${server.port}")
private String serverPort;		//返回值加上去便于区分
```

### 5）注意：需给配置类添加负载均衡注解@LoadBalanced（ribbon）

mapper.xml也要cv过去（yaml保持同步）

### 6）actuator微服务信息完善

```yaml
 eureka:
  instance:
    #修改主机名
    instance-id: payment8002
    #显示ip 地址
    prefer-ip-address: true
```

给注册的服务配实例 instance-id

url: <u>localhost:8001/actuator/health 可查看服务状态</u>

prefer-ip-address: true #访问路径可以显示ip地址

## 4.服务发现discovery

```java
@RestController
@Slf4j
public class PaymentController {

    @Resource
    DiscoveryClient discoveryClient;

    @GetMapping("/payment/discovery")
    public Object discovery(){
        List<String> services = discoveryClient.getServices();
        for (String service : services) {
            log.info("******{service}");
        }
        List<ServiceInstance> instances = discoveryClient.getInstances("CLOUD-PAYMENT-SERVICE");
        for (ServiceInstance instance : instances) {
            log.info(instance.getInstanceId()+"\t"+instance.getHost()+"\t"+instance.getPort()+"\t"+instance.getUri());
        }
        return this.discoveryClient;
    }
}
```

要在主启动类上加注解@EnableDiscoveryClient

**Eureka Server自我保护机制**

eureka.server.enable-self-preservation = false(默认为true)

驱逐-间隔-计时器-毫秒：2秒

eviction-interval-timer-in-ms: 2000

**Eureka Client** 

#客户端向服务端发送心跳的时间间隔，单位为秒

eureka.instance.lease.renewal.interval.in.seconds = 1(默认30秒)

#Eureka服务端在收到最后一次心跳后等待时间上限，单位秒，超时剔除服务

eureka.instance.lease.expiration-duration-in-seconds = 2（默认90秒）

## 五、Zookeeper

```shell
docker pull zookeeper:3.4.9
```

1）首先，关闭防火墙（后面的d别掉了 ）

```shell
systemctl stop firewalld
```

2）虚拟机ip  ping通win10

```shell
ping 192.168.140.1（会变动）
```

导入依赖

```xml
spring-cloud-starter-zookeeper-discovery
```

*<u>注意，这个地方可能会遭遇jar包冲突，我们可以排除掉zookeeper官方自带的jar包</u>*

<u>*然后添加自己的版本依赖  3.4.9*</u>

```xml
	<dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-zookeeper-discovery</artifactId>
            <!--排除官方自带的3.5.3-beta版本-->
            <exclusions>
                <exclusion>
                    <groupId>org.apache.zookeeper</groupId>
                    <artifactId>zookeeper</artifactId>
                </exclusion>
            </exclusions>
        </dependency>

        <dependency>
            <groupId>org.apache.zookeeper</groupId>
            <artifactId>zookeeper</artifactId>
            <version>3.4.9</version>
            <!-- 这个地方sl4j和log4j-12也会冲突 -->
            <exclusions>
                <exclusion>
                    <groupId>org.slf4j</groupId>
                    <artifactId>slf4j-log4j12</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>com.lx.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>${project.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
    </dependencies>
```

Cloud-PaymentZk8004

```yaml
server:
  port: 8004
spring:
  application:
    name: cloud-payment-service
  cloud:
    zookeeper:
    #zookeeper在虚拟机中的ip地址和映射的端口号
      connect-string: 192.168.1.247:2181
```

在主启动类上添加注解@EnableDiscoveryClient

```java
@SpringBootApplication
@EnableDiscoveryClient//该注解用于向consul或者zookeeper注册服务时使用
public class PaymentMain8004 {
    public static void main(String[] args) {
        SpringApplication.run(PaymentMain8004.class, args);
    }
}
```

**思考：zookeeper是临时节点还是持久节点？**

- *它是一个临时节点，采用的是CAP中的AP，高可用，强一致性。*

Cloud-ConsumerZk-Order

```yaml
spring:
  application:
    name: cloud-consumer-order
  cloud:
    zookeeper:
      connect-string: 192.168.1.247:2181
server:
  port: 80
```

## 六、Ribbon

Spring Cloud Ribbon 是基于Netflix Ribbon实现的一套**客户端**   **负载均衡的工具**

Ribbon是Netflix发布的开源项目，主要功能是提供客户端的软件负载均衡算法和服务调用。Ribbon客户端组件提供一系列完善的配置项如连接超时，重试等。简单的说，就是在配置文件中列出Load Balance （简称LB）后面所有的机器。Ribbon会自动的帮助你基于某种规则（如简单轮询，随机连接等）去连接这些机器。我们很容易使用Ribbon实现自定义的负载均衡算法。

1. *简单的说就是将用户的请求平摊的分配到多个服务上，从而达到系统的HA(高可用)。*

2. *常见的负载均衡有软件Nginx，LVS，硬件F5等。*

3. Ribbon本地负载均衡客户端VS Nginx服务端负载均衡区别

   - **Nginx是服务器负载均衡**，客户端所有请求都会交给nginx，然后由nginx实现转发请求。即负载均衡是由服务端实现的。

   - **Ribbon本地负载均衡**，在调用微服务接口的时候，会在注册中心上获取注册信息服务列表之后缓存到JVM本地，从而在本地实现RPC远程过程调用技术。

   

4. 集中式LB

   - 即在服务的消费放和提供方之间使用独立的LB设施（可以是硬件，如F5，也可以是软件，如nginx），由该设施负责通过某种策略转发至服务的提供方；

5. 进程内LB

   - 将LB逻辑集成到消费放，消费方从服务注册中心获知有哪些地址可用，然后自己再从这些地址中选择一个合适的服务器。
   - *Ribbon就属于进程内LB*  ，它是一个类库，集成于消费放进程，消费放通过它来获取到服务提供方的地址。

6. 一句话概括 Ribbon就是负载均衡+RestTemplate调用

总结：**<u>Ribbon其实就是一个软负载均衡的客户端组件，它可以和其他所需请求的客户端结合使用，和eureka结合只是其中的一个实例。</u>**

![Ribbon实现方式](F:\Typora 笔记\img\Ribbon实现方式.png)





Ribbon在工作时分成两步

- 第一步先选择EurekaServer，它优先选择在同一个区域内负载较少的server
- 第二步再根据用户指定的策略，在从server取到的服务注册列表中选择一个地址。
- 其中Ribbon提供了多种策略：比如轮询、随机和根据响应时间加权。

Ribbon负载均衡规则替换

1. 官方文档明确指出这个自定义配置类不能放在@ComponentScan所扫描的当前包下以及子包下，否则我们自定义的这个配置类就会被所有的Ribbon客户端所共享，达不到特殊化定制的目的了。

2. 负载规则替换![负载规则替换](F:\Typora 笔记\img\负载规则替换.png)

3. ```java
   package com.lx.myrule;//配置类所在包
   
   import com.netflix.loadbalancer.IRule;
   import com.netflix.loadbalancer.RandomRule;
   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.Configuration;
   
   /**
   *注意，该配置类不能放在与主启动类同级包或者其子包下
   */
   @Configuration
   public class MySelfRule {
       @Bean
       public IRule myRule(){
           return new RandomRule();//定义为随机
       }
   }
   
   ```

4. 在主启动类上添加注解@RibbonClient

   ```java
      package com.lx.springcloud;
   
      import com.lx.myrule.MySelfRule;
      import org.springframework.boot.SpringApplication;
      import org.springframework.boot.autoconfigure.SpringBootApplication;
      import org.springframework.cloud.netflix.eureka.EnableEurekaClient;
      import org.springframework.cloud.netflix.ribbon.RibbonClient;
   
      @SpringBootApplication
      @EnableEurekaClient
      @RibbonClient(name = "CLOUD-PAYMENT-SERVICE",configuration = MySelfRule.class)
      public class OrderMain80 {
          public static void main(String[] args) {
              SpringApplication.run(OrderMain80.class, args);
          }
      }
   ```

   注意：public static final String PAYMENT_SERVICE = "http://CLOUD-PAYMENT-SERVICE";

   ​																							<u>这里的大小写要与主启动类保持一致</u>

负载均衡算法：rest接口第几次请求数%服务器集群总数量 = 实际调用服务器位置下标，每次服务重启后rest接口计数从1开始。

## 七、服务调用之OpenFeign

在每一个微服务上面加个注解@Feign

接口+注解 微服务调用接口@FeignClient

### OpenFeign使用步骤：

1. 依赖

   ```xml
   <dependencies>
           <dependency>
               <groupId>com.lx.springcloud</groupId>
               <artifactId>cloud-api-commons</artifactId>
               <version>${project.version}</version>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-openfeign</artifactId>
           </dependency>
       </dependencies>
   </project>
   ```

   

2. 在主启动类上加注解@EnableOpenFeignClients

3. 业务类：

   - 业务逻辑接口+@FeignClient配置调用provider服务

   - 新建PaymentFeignService接口并新增注解@FeignClient

     ```java
     package com.lx.springcloud.service;
     
     import com.lx.springcloud.entity.CommonResult;
     import com.lx.springcloud.entity.Payment;
     import org.springframework.cloud.openfeign.FeignClient;
     import org.springframework.web.bind.annotation.GetMapping;
     import org.springframework.web.bind.annotation.PathVariable;
     
     /**
     *下面的接口命名为：OpenFeignTemplate 调用组件好理解点
     */
     @FeignClient(value = "CLOUD-PAYMENT-SERVICE")
     public interface PaymentFeignService {
         @GetMapping(value = "/payment/{id}")
         public CommonResult<Payment> findById(@PathVariable("id") Long id);
     }
     ```

     **注意**:*这里service接口里的GetMapping路径要与提供方的路径保持一致,否则匹配不到*

   - 控制层Controller

     ```java
     package com.lx.springcloud.controller;
     
     import com.lx.springcloud.entity.CommonResult;
     import com.lx.springcloud.entity.Payment;
     import com.lx.springcloud.service.PaymentFeignService;
     import org.springframework.web.bind.annotation.GetMapping;
     import org.springframework.web.bind.annotation.PathVariable;
     import org.springframework.web.bind.annotation.RestController;
     
     import javax.annotation.Resource;
     
     @RestController
     public class OpenFeignController {
         @Resource
         private PaymentFeignService paymentFeignService;
     
         @GetMapping(value = "/consumer/payment/{id}")
         public CommonResult<Payment> findById(@PathVariable("id") Long id){
             return paymentFeignService.findById(id);
         }
     }
     ```

     - 即：*Controller => Feign=> Controller*  

4. Feign集成了Ribbon负载均衡

5. 超时控制

   *tips: Feign 客户端默认只等待一秒钟，但是服务端处理需要超过1秒钟，导致Feign客户端不想等待了，直接返回报错。*

   *为了避免这样的情况发生，有时候我们需要设置Feign客户端的超时控制。*

   *yml中开启配置*   **Feign超时设置  大小写特别重要**

   ```yaml
   # 设置feign客户端超时时间（OpenFeign默认支持Ribbon)
   ribbon:
     #指的是建立连接后从服务器读取到可用资源所用的时间
     ReadTimeout: 5000
     #指的是建立连接所用的时间，适用于网络状况正常的情况下，两端连接所用的时间
     ConnectTimeout: 5000
   ```

   

6. OpenFeign日志打印

   NONE:默认的，不显示任何日志；

   BASIC:仅记录请求方法、URL、相应状态码及执行时间；

   HEADERS:除了BASIC中定义的信息之外，还有请求和相应的头信息；

   FULL:除了HEADERS中定义的信息之外，还有请求和相应的正文及元数据；

   **FeignConfig的日志配置类**

```java
package com.lx.springcloud.config;

import feign.Logger;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class FeignConfig {
    @Bean
    Logger.Level feignLoggerLevel(){
        return Logger.Level.FULL;
    }
}

```

## 八、Hystrix断路器

1. 分布式系统面临的问题

   复杂分布式体系结构中的应用程序有十个依赖关系，每个依赖关系在某些时候不可避免地失败。

2. 服务雪崩

   - 多个服务之间调用的时候，假设微服务A调用微服务B和微服务C，微服务B和微服务C又调用其他的微服务，这就是所谓的“扇出”。

   - 如果扇出的链路上某个微服务的调用相应时间过长或者不可用，对微服务A的调用就会占用越来越多的系统资源，进而引起系统崩溃，所谓的“雪崩效应”。

   - 对于高流量的应用来说，单一的后端依赖可能会导致所有服务器上的所有资源都在几秒钟内饱和。比失败更糟糕的是，这些应用程序还可能导致服务之间的延迟增加，备份队列，线程和其他系统资源紧张，导致整个系统发生更多的级联故障。这些都表示需要对故障和延迟进行隔离和管理，以便单个依赖关系的失败，不能取消整个应用程序或系统。
   - 所以，通常当你发现一个模块下的某个实例失败后，这时候这个模块依然还会接收流量，然后这个有问题的模块还调用了其他的模块，这样就会发生级联故障，或者叫雪崩。

3. Hystrix对于上面的为题给出了一个解决方式

   - Hystrix是一个用于处理分布式系统的延迟和容错的开源库，在分布式系统里，许多依赖不可避免的会调用失败，比如超时、异常等。

   - Hystrix能够保证在一个依赖出问题的情况下，不会导致整体服务失败，避免级联故障，以提高分布式系统的弹性。

   - “断路器”本身是一种开关装置，当某个服务单元发生故障之后，通过断路器的故障监控（类似熔断保险丝），向调用方返回一个符合预期的、可处理的备选相应（FallBack），而不是长时间的等待或者抛出调用方无法处理的异常，这样就保证了服务调用方的线程不会被长时间、不必要地占用，从而避免了故障在分布式系统中的蔓延，乃至雪崩。

4. What can Hystrix do?

   - 服务降级

- 服务熔断
  - 服务限流
- 接近实时的监控

5. Hystrix的重要概念

   - 服务降级(fallback)

     1. 服务器繁忙，请稍后再试，不让客户端等待并立刻返回一个友好提示，fallback

     2. 哪些情况会触发降级

        - 程序运行异常

        - 超时

        - 服务熔断触发服务降级

        - 线程池/信号量打满也会导致服务降级

     3. 在业务中指正常流程跑不通，先记录下来，然后再用程序这些数据做补救

   - 服务熔断（break）
     - 类比保险丝达到最大服务访问后，直接拒绝访问，拉闸限电，然后调用服务降级的方法并返回友好提示
     - 服务的降级 -> 进而熔断 -> 回复调用链路

   - 服务限流（flowlimit）
     - 秒杀高并发等操作，严禁一窝蜂的过来拥挤，大家排队，一秒钟N个，有序进行

6. 依赖

   ```xml
       <dependencies>
           <dependency>
               <groupId>com.lx.springcloud</groupId>
               <artifactId>cloud-api-commons</artifactId>
               <version>${project.version}</version>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-actuator</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-test</artifactId>
           </dependency>
           <dependency>
               <groupId>mysql</groupId>
               <artifactId>mysql-connector-java</artifactId>
           </dependency>
           <dependency>
               <groupId>com.alibaba</groupId>
               <artifactId>druid-spring-boot-starter</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
           </dependency>
       </dependencies>
   </project>
   ```

   

7. Jmeter并发测试下

   - Tomcat的默认工作线程数被打满了，没有多余的线程来分解压力和处理

   - 8001同一层次的接口服务被困死，因为Tomcat的线程池打满了80此时调用8001，消费者端响应也会缓慢*

8. 讲解容错解决的维度要求

   正是上述故障或表现不佳才有我们的降级/容错/限流等技术诞生

   - 超时不再等待
   - 出错要有兜底

9. 解决方案

   - 对方服务（8001）超时了，调用者（80）不能一直卡死等待，必须有服务降级

   - 对方服务（8001）down机了，调用者不能一直卡死等待，必须有服务降级

   - 对方服务（8001）OK，调用者（80）自己出故障或有自我要求（自己的等待时间小于服务提供者

10. 降级配置

    注解：@HystrixCommand

    - 8001先从自身找问题：

      - 设置自身调用超时时间的峰值，峰值内可以正常运行，超过了需要有兜底的方法处理，作服务降级fallback

      - @HystrixCommand报异常后处理方式如下：

      - 业务类启用

      - ```java
        package com.lx.springcloud.service;
        
        import com.netflix.hystrix.contrib.javanica.annotation.HystrixCommand;
        import com.netflix.hystrix.contrib.javanica.annotation.HystrixProperty;
        import org.springframework.stereotype.Service;
        
        import java.util.concurrent.TimeUnit;
        
        @Service
        public class PaymentServiceImpl {
        
            public String paymentInfo_OK(Integer id){
                return "线程池：  "+Thread.currentThread().getName()+"paymentInfo_OK，id:"+id;
            }
        
            @HystrixCommand(fallbackMethod = "paymentInfo_TimeoutHandler", commandProperties = {
                    @HystrixProperty(name= "execution.isolation.thread.timeoutInMilliseconds",value = "5000")
            })
            public String paymentInfo_Timeout(Integer id){
                int timeNumber = 3;
                try {
                    TimeUnit.SECONDS.sleep(timeNumber);
                } catch (InterruptedException e){
                    e.printStackTrace();
                };
                return "线程池：  "+Thread.currentThread().getName()+"paymentInfo_Timeout，id:"+id+"耗时:"+timeNumber+"秒";
            }
        
            public String paymentInfo_TimeoutHandler(Integer id){
                return "线程池：  "+Thread.currentThread().getName()+"paymentInfo_TimeoutHandler，id:"+id+"o(╥﹏╥)o";
            }
        }
        ```

      - 主启动类激活 添加新注解 @EnableCircuitBreaker //启用断路器

      - 不管是  **运行异常 **  还是 **服务调用超时异常** 都会触发  **服务降级**

      - 注意，服务降级方法要与原方法的签名保持一致（参数，返回类型）

      8001fallback

      ```yaml
      server:
        port: 80
      
      eureka:
        client:
          register-with-eureka: false
          service-url:
            defaultZone: http://eureka7001.com:7001/eureka/
      
      ribbon:
        ReadTimeout: 5000
        ConnectTimeout: 5000
      
      #feign:
      # hystrix:
      #	enabled: true
      ```

      80fallback

      消费端的主启动类添加注解@EnableHystrix

      ```java
      package com.lx.springcloud.controller;
      
      import com.lx.springcloud.service.PaymentHystrixService;
      import com.netflix.hystrix.contrib.javanica.annotation.HystrixCommand;
      import com.netflix.hystrix.contrib.javanica.annotation.HystrixProperty;
      import org.springframework.web.bind.annotation.GetMapping;
      import org.springframework.web.bind.annotation.PathVariable;
      import org.springframework.web.bind.annotation.RestController;
      
      import javax.annotation.Resource;
      
      @RestController
      public class PaymentHystrixController {
          @Resource
          private PaymentHystrixService service;
      
          @GetMapping("/consumer/payment/hystrix/ok/{id}")
          public String paymentInfo_OK(@PathVariable("id") Integer id){
              return service.paymentInfo_OK(id);
          }
      
          /**
           * 调用提供方的服务时注意设置 Ribbon: ReadTimeout: 5000
           * Ribbon 默认1秒得不到相应会 Timeout!
           * @param id
           * @return
           */
          @HystrixCommand(fallbackMethod = "paymentInfo_TimeoutHandler", commandProperties = {
                  @HystrixProperty(name= "execution.isolation.thread.timeoutInMilliseconds",value = "8000")
          })
          @GetMapping("/consumer/payment/hystrix/timeout/{id}")
          public String paymentInfo_Timeout(@PathVariable Integer id){
              return service.paymentInfo_Timeout(id);
          }
      
          public String paymentInfo_TimeoutHandler(Integer id){
              return "骚瑞，我是客户端80\n系统繁忙，请稍后再试o(╥﹏╥)o";
          }
      }
      ```

      

11. 目前问题：每个业务都需要一个兜底的方法，代码冗余

    解决：global fallback（通用和自定义的分开）

    - feign接口系列
      1. 类上加注解：@DefaultProperties(defaultFallback = "payment_Global_FallbackMethod")
      2. 方法上加注解：@HystrixCommand
      3. 每个方法配置一个服务降级方法，技术上可以，实际不推荐
      4. 出了个别重要的核心业务有专属兜底方法外，其他普通的可以通过@DefaultProperties(defaultFallback = "")跳转到统一结果处理页面
      5. 通用和独享各自分开，减少代码冗余
      6. 全局兜底方法不能有形参

12. 解耦，把兜底方法和业务类分开

    ```java
    @FeignClient(value = "CLOUD-PROVIDER-HYSTRIX-SERVICE",fallback = PaymentFallbackService.class)
    @Service
    public interface PaymentHystrixService {
    
        @GetMapping("/payment/hystrix/ok/{id}")
        public String paymentInfo_OK(@PathVariable("id") Integer id);
    
        @GetMapping("/payment/hystrix/timeout/{id}")
        public String paymentInfo_Timeout(@PathVariable("id") Integer id);
    }
    ```

    ```java
    //解耦的业务类
    public class PaymentFallbackService implements PaymentHystrixService{
        @Override
        public String paymentInfo_OK(Integer id) {
            return "PaymentFallbackService - fallback paymentInfo_OK o(╥﹏╥)o";
        }
    
        @Override
        public String paymentInfo_Timeout(Integer id) {
            return "PaymentFallbackService - fallback paymentInfo_Timeout o(╥﹏╥)o";
        }
    }
    ```

13. 熔断机制概述

    1. 熔断机制是应对雪崩效应的一种微服务链路的保护机制。当扇出链路的某个微服务出错不可用或者响应时间太长时，会进行服务的降级，进而熔断该节点微服务的调用，快速返回错误的响应信息。
    2. **当检测到该节点微服务调用响应正常后，恢复调用链路。**
    3. 在Spring Cloud 框架里，熔断机制通过Hystrix实现。Hystrix会监控微服务间调用的状况，当失败的调用到一定阈值，缺省是5秒内20次调用失败，就会触发熔断机制。熔断机制的注解是@HystrixCommand
    4. 名词解释：
       - 缺省：系统的默认状态
       - 扇出：调用下一个微服务的链路

14. 服务熔断（Open- > Close -> half-Open(睡眠窗口期，这里我称它为空窗期) ->Open）

    ```java
    @Service
    public class PaymentServiceImpl {
    
        public String paymentInfo_OK(Integer id){
            return "线程池：  "+Thread.currentThread().getName()+"paymentInfo_OK，id:"+id;
        }
    
        @HystrixCommand(fallbackMethod = "paymentInfo_TimeoutHandler", commandProperties = {
                @HystrixProperty(name= "execution.isolation.thread.timeoutInMilliseconds",value = "5000")
        })
        public String paymentInfo_Timeout(Integer id){
            int timeNumber = 3;
            try {
                TimeUnit.SECONDS.sleep(timeNumber);
            } catch (InterruptedException e){
                e.printStackTrace();
            };
            return "线程池：  "+Thread.currentThread().getName()+"paymentInfo_Timeout，id:"+id+"耗时:"+timeNumber+"秒";
        }
    
        public String paymentInfo_TimeoutHandler(Integer id){
            return "线程池：  "+Thread.currentThread().getName()+"系统繁忙或运行错误:"+id+"o(╥﹏╥)o";
        }
    
        /**
         * 服务熔断
         * @param id 路径变量
         * @return 订单流水号
         */
        @HystrixCommand(fallbackMethod = "paymentCircuitBreaker_fallback", commandProperties = {
                @HystrixProperty(name = "circuitBreaker.enabled", value = "true"),//开启断路器（熔断器）
                @HystrixProperty(name = "circuitBreaker.requestVolumeThreshold", value = "10"),//收到的请求阈值
                @HystrixProperty(name = "circuitBreaker.sleepWindowInMilliseconds", value = "10000"),//断路的空窗期10秒
                @HystrixProperty(name = "circuitBreaker.errorThresholdPercentage", value = "60")//错误率阈值60% 熔断
        })
        public String paymentCircuitBreaker(@PathVariable("id") Integer id){
            if(id < 0){
                throw new RuntimeException("id 不能为负数");
            }
            Snowflake snowflake = null;
            synchronized (PaymentServiceImpl.class){
                if(snowflake == null){
                    snowflake = IdUtil.createSnowflake(1,1);
                }
            }
            Long serialNumber = snowflake.nextId();
            return Thread.currentThread().getName()+"\t"+"调用成功: 订单流水号："+serialNumber.toString();
        }
    
        public String paymentCircuitBreaker_fallback(@PathVariable("id") Integer id){
            return "id 不能为负数，请稍后再试o(╥﹏╥)o"+id;
        }
    }
    
    ```

    ```java
    @HystrixProperty(name = "circuitBreaker.enabled", value = "true"),//开启断路器（熔断器）
    @HystrixProperty(name = "circuitBreaker.requestVolumeThreshold",value = "10"),//收到的请求阈值
    @HystrixProperty(name = "circuitBreaker.sleepWindowInMilliseconds",value = "10000"),//断路器的睡眠窗口期10秒
    @HystrixProperty(name = "circuitBreaker.errorThresholdPercentage",value = "60")//错误率阈值60% 熔断
    ```

    请求总数阈值：默认20次

    空窗期后（默认5秒）这个时候断路器是半开状态，尝试请求，响应正常会恢复正常的链路（Open），否则继续进入熔断状态(Close)

    错误率阈值：默认50%

    8001：Controller

    ```java
        @GetMapping("/payment/circuit/{id}")
        public String paymentCircuitBreaker(@PathVariable("id") Integer id){
            String result = paymentService.paymentCircuitBreaker(id);
            log.info("{}",result);
            return result;
        }
    ```

15. 熔断类型

    1. 熔断打开：请求不再进行调用当前服务，内部设置时钟一班为MTTR（平均故障处理时间），当打开时长达到所设时钟则进入半熔断状态

    2. 熔断关闭：熔断关闭不会对服务进行熔断

    3. 熔断搬开：部分请求根据规则调用当前服务，如果请求成功切符合规则，则认为当前服务恢复正常，关闭熔断。

       

16. HystrixCommand属性

    ![HystrixCommand](F:\Typora 笔记\img\HystrixCommand.png)

17. Hystrix工作流程图（暂无）

18. Hystrix图形化监控

       - 除了隔离依赖服务的调用外，Hystrix还提供了准实时的调用监控（Hystrix Dashboard）,Hystrix会持续地记录所有通过Hystrix发起的请求的执行信息，并以统计报表和图形的形式展示给用户，包括每秒执行多少请求多少成功，多少失败等。Netflix通过hystrix-metrics-event-stream项目实现了对以上指标的监控。Spring Cloud也提供了Hystrix Dashboard的整合，对监控内容转化成可视化界面。

       - 主启动类添加注解@EnableHystrixDashboard

       - 监控服务需要将所有的微服务的提供者都需要引入依赖：spring-boot-starter-actuator

       - ```java
         @SpringBootApplication(exclude = DruidDataSourceAutoConfigure.class)
         @EnableEurekaClient
         @EnableHystrix
         public class PaymentHystrixMain8001 {
             public static void main(String[] args) {
                 SpringApplication.run(PaymentHystrixMain8001.class, args);
             }
         
             /**
              * 此配置是为了服务监控而配置，与服务容错本身无关，springcloud升级后的坑
              * ServletRegistrationBean因为springboot的默认路径不是"/hystrix.stream"
              * 只要在自己的项目里配置上下面的servlet就可以了
              */
             @Bean
             public ServletRegistrationBean getServlet(){
                 HystrixMetricsStreamServlet streamServlet = new HystrixMetricsStreamServlet();
                 ServletRegistrationBean registrationBean = new ServletRegistrationBean(streamServlet);
                 registrationBean.setLoadOnStartup(1);
                 registrationBean.addUrlMappings("/actuator/hystrix.stream");
                 registrationBean.setName("HystrixMetricsStreamServlet");
                 return registrationBean;
             }
         }
         ```

       - ```yaml
         server:
           port: 9001
         
         hystrix:
           dashboard:
             proxy-stream-allow-list: "localhost"
         ```

         ## 九、Gate way新一代网关

       - 1. 简介

            1. 概述：Spring Cloud Gateway 使用的Webflux中的reactor-netty响应式编程组件，底层使用了Netty通讯框架。

            2. SpringCloud Gateway 特性：

               1. Gateway是基于异步非阻塞模型上进行开发的，性能方面不需要担心
               2. 基于Spring Framework 5，Project Reactor 和 Spring Boot 2.x进行构建的
               3. 动态路由：能够匹配任何请求属性
               4. 可以对路由指定Predicate（断言）和Filter（过滤器）
               5. 集成Hystrix的断路器功能
               6. 集成Spring Cloud 的服务发现功能
               7. 易于编写的Predicate（断言）和Filter（过滤器）
               8. 请求限流功能
               9. 支持路径重写

            3. 源码架构：

            4. 非阻塞-响应式的高性能框架  WebFlux和Netty

            5. 能干嘛

               1. 反向代理

               2. 鉴权

               3. 流量控制

               4. 熔断

               5. 日志监控

                  ![微服务架构中网关在哪里](F:\Typora 笔记\img\微服务架构中网关.png)

         2. 三大核心概念

            1. *Route（路由）*
               - 路由是构建网关的基本模块，它由ID，目标URI（统一资源标识符），一系列的断言和过滤器组成，如果断言为true则匹配该路由
            2. *Predicate（断言）*
               - 参考Java 8 的java.util.function.Predicate
               - 开发人员可以匹配HTTP请求中的所有内容（例如请求头或请求参数），如果请求与断言相匹配则进行路由
            3. *Filter（过滤）*
               - 指的是Spring框架中GatewayFilter的实例，使用过滤器，可以在请求被路由前或者之后对请求进行修改。
            4. *总体*
               - web请求，通过一些匹配条件，定位到真正的服务节点。并在这个转发过程的前后，进行一些精细化控制。
               - predicate就是我们的匹配条件；
               - filter：拦截器，懂得都懂：
                 1. Filter在“pre”类型的过滤器可以做参数校验、权限校验、流量监控、日志输出、协议转换等。
                 2. 在"post"类型的过滤器中可以做响应内容、响应头的修改，日志的输出，流量监控等有着非常重要的作用
               - 断言+过滤器+目标uri（资源标识符） 可以实现一个具体的路由

         3. GateWay工作流程

            - 核心逻辑：路由转发+执行过滤链

         4. 入门配置

         5. 我们目前不想暴露8001端口，希望在外面套一层9527

            - 配置方式一：application.yml

              ```yaml
              server:
                port: 9527
              
              eureka:
                instance:
                  hostname: CLOUD-GATEWAY-SERVICE
                client:
                  fetch-registry: true
                  register-with-eureka: true
                  service-url:
                    defaultZone: http://eureka7001.com:7001/eureka
              
              spring:
                application:
                  name: CLOUD-GATEWAY
                cloud:
                  gateway:
                    discovery:
                      locator:
                        enabled: true #开启从注册中心动态创建的路由功能,利用微服务名进行路由
                    routes:
                      - id: payment_routh   #payment_route    #路由的id，没有固定的规则，但要求唯一，建议配合服务名
              #          uri: http://localhost:8001           #匹配后提供服务的路由地址
                        uri: lb://cloud-payment-service       #匹配后提供服务的路由地址，lb负载均衡协议
                        predicates:
                          - Path=/payment/**               #断言，路径相匹配的进行路由
                          - After: 2020-10-11T11:36:25.148+08:00[Asia/Shanghai] #在这个时间点后生效
              #        - id: payment_routh2  #payment_routh2   #路由的Id,没有固定的规则，但要求唯一
              #          uri: http://localhost:8001
              #          predicates:
              #            - path=/payment/lb/**
              ```

            - 配置方式二：代码中注入RouteLocator的Bean

              ```java
              package com.lx.springcloud.config;
               
               import org.springframework.cloud.gateway.route.RouteLocator;
               import org.springframework.cloud.gateway.route.builder.RouteLocatorBuilder;
               import org.springframework.context.annotation.Bean;
               import org.springframework.context.annotation.Configuration;
               
               @Configuration
               public class GateWayConfig {
                   @Bean
                   public RouteLocator customRouteLocator(RouteLocatorBuilder routeLocatorBuilder){
                       RouteLocatorBuilder.Builder routes = routeLocatorBuilder.routes();
                       routes.route("news_guoji",//id
                               r -> r.path("/guoji")//这个是断言 predicates
                               .uri("http://news.baidu.com/guoji"));
                       routes.route("news_guonei",
                               r -> r.path("/guonei")
                               .uri("http://news.baidu.com/guonei"));
                       return routes.build();
                   }
               }
              ```

              

         6. 通过微服务名实现动态路由(负载均衡)

            uri: lb://cloud-payment-service  **gateway的动态路由的协议不再是http了，而是lb**

            ```yaml
            server:
              port: 9527
            
            eureka:
              instance:
                hostname: CLOUD-GATEWAY-SERVICE
              client:
                fetch-registry: true
                register-with-eureka: true
                service-url:
                  defaultZone: http://eureka7001.com:7001/eureka
            
            spring:
              application:
                name: CLOUD-GATEWAY
              cloud:
                gateway:
                  discovery:
                    locator:
                      enabled: true #开启从注册中心动态创建的路由功能,利用微服务名进行路由
                  routes:
                    - id: payment_routh   #payment_route    #路由的id，没有固定的规则，但要求唯一，建议配合服务名
                      #          uri: http://localhost:8001           #匹配后提供服务的路由地址
                      uri: lb://cloud-payment-service       #匹配后提供服务的路由地址，lb负载均衡协议
                      predicates:
                        - Path=/payment/**               #断言，路径相匹配的进行路由
            #           - After: 2020-10-11T11:36:25.148+08:00[Asia/Shanghai] #在这个时间点后生效
            #           - Cookie=Chocolate,ch.p
                        - Header=X-Request-Id, \d+ #请求头要有X-Request-Id属性并且值是整数的正则表达式
                        - Method=Get
            
            ```

            **predicates:**

            ```tex
            Loaded RoutePredicateFactory [After]
            Loaded RoutePredicateFactory [Before]
            Loaded RoutePredicateFactory [Between]
            Loaded RoutePredicateFactory [Cookie]
            Loaded RoutePredicateFactory [Header]
            Loaded RoutePredicateFactory [Host]
            Loaded RoutePredicateFactory [Method]
            Loaded RoutePredicateFactory [Path]
            Loaded RoutePredicateFactory [Query]
            Loaded RoutePredicateFactory [ReadBodyPredicateFactory]
            Loaded RoutePredicateFactory [RemoteAddr]
            Loaded RoutePredicateFactory [Weight]
            Loaded RoutePredicateFactory [CloudFoundryRouteService]
            ```

            

         7. Filter的使用

            1. 生命周期

               - pre

               - post

            2. 种类

               - GateWayFilter

               - GlobalFilter

            3. 自定义全局GlobalFilter

            4. 两个主要接口介绍

               1. implements GlobalFilter,Ordered

               2. @Ordered(0)和implements Ordered实现接口一样

                  ```java
                  @Slf4j
                  @Component
                  public class MyLogGlobalFilter implements GlobalFilter, Ordered {
                      //@Order(0)也阔以，数字越小过滤器优先级越高
                      @Override
                      public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
                          log.info("we r comming MyLogGlobalFilter 》》》》", LocalDateTime.now());
                          String userName = exchange.getRequest().getQueryParams().getFirst("userName");
                          if(userName == null){
                              log.info("用户名非法!");
                              exchange.getResponse().setStatusCode(HttpStatus.NOT_ACCEPTABLE);
                              return exchange.getResponse().setComplete();
                          }
                          return chain.filter(exchange);
                      }
                  
                      @Override
                      public int getOrder() {
                          return 0;
                      }
                  }
                  ```

            5. 请求测试辅助工具

               ```
               1. jmeter
               2. postman
               3. cmd命令 curl
               ```

               ```shell
                C:\Users\Administrator>
                curl http://localhost:9527/payment/1 --cookie "Chocolate=ch.p"
                curl http://localhost:9527/payment/1 -H "X-Request-Id:123"
               ```

## 十、服务配置

配置问题：

微服务意味着要将单体应用中的业务拆分成一个个子服务，每个服务的粒度相对较小，因此系统中会出现大量的服务。由于每个服务都需要必要的配置信息才能运行，所以一套集中式的、动态的配置管理设施是必不可少的。

SpringCloud提供了**ConfigServer**来解决这个问题，我们每一个微服务自己带着一个application.yml，上百个配置文件的管理……

Spring Cloud Config是什么：

Spring Cloud Config为微服务架构中的微服务提供集中化的外部配置支持，配置服务器为各个不同微服务应用的所有环境提供了一个中心化的外部配置。

Spring Cloud Config怎么玩：

Spring Cloud Config分为服务端和客户端两部分。

服务端也称为分布式配置中心，它是一个独立的微服务应用，用来连接配置服务器并为客户提供获取配置信息，加密、解密信息等访问接口

客户端则是通过制定的配置中心来管理应用资源，以及与业务相关的配置内容，并在启动的时候从配置中心获取和加载配置信息配置服务器默认采用git来存储配置信息，这样就有助于对环境配置进行版本管理，并且可以通过git客户端工具来方便的管理和访问配置内容。

Spring Cloud Config 能干啥：

集中管理配置文件

不同环境不同配置，动态化的配置更新，分环境部署比如dev/test/prod/beta/release

运行期间动态调整配置，不再需要在每个服务部署到机器上编写配置文件，服务会向配置中心统一拉去自己的信息

当配置发生变动时，服务不需要重启即可感知到配置的变化并应用新的配置

将配置信息以REST接口的形式暴露：

​	post、curl访问刷新均可

与GitHub整合配置：

由于Spring Cloud Config 默认使用Git来存储配置文件（也有其它方式，比如SVN和本地文件），但最推荐的还是Git，而且使用的是http/https访问的形式





