## AMQP(高级消息队列协议)

## RabbitMQ

5672		  端口是客户端与RabbitMQ之间通信的端口号

15672		管理界面访问web界面的端口

管理页面的用户名和密码都是guest

核心概念

**Message:** 

消息是不具名的，它由消息头和消息体组成。消息体是不透明的，而消息头则是由一系列的可选属性组成，这些属性包括routing-key(路由键)、priority(相对其他消息的优先权)、delivery-mode（指定该消息可能需要持久性存储）等。

**Publisher:**

消息的生产者，也是一个向交换器发布消息的客户端程序。

**Exchange:**

交换器，用来接收生产者发送的消息并将这些消息路由给服务器中的队列。

### Exchange（交换器 ）的4种类型：

**1.direct（默认）：**

点对点模式：

当路由键和绑定的键**完全一致**时，将消息派发给队列。 即点对点通信模型（也称单播模式）

**2.fanout：**

广播模式：速度最快（发布订阅）

不处理路由键，并将消息派发给交换器下的所有队列。

**3.topic**：

允许我们对路由键做些模糊匹配，选择性的将消息派发给队列。

它将**路由键和绑定键的字符串切分成单词**，这些**单词之间用点隔开**。

它同样也**会识别两个通配符**： # 和 *

“#”代表 0 个或 多个**单词** ，“* ”匹配一个**单词**。

**4.headers**：不采用路由键，匹配消息头，用得比较少

不同类型的Exchange转发消息的策略有所区别。



**Quene:**

消息队列，用来保存消息直到发送给消费者。它是消息的容器，也是消息的终点。一个消息可投入一个或多个队列。消息一直在队列里面，等待消费者连接到这个队列将其取走。

**Binding：**

绑定，用于消息队列和交换器之间的关联。一个绑定就是基于路由键将交换器和消息队列连接起来的路由规则，所以可以将交换器理解成一个由绑定构成的路由表。

**Connection:**

网络连接，比如一个TCP连接

**Channel:**

信道，多路复用连接中的一条独立的双向数据流通道。信道是建立在真实的TCP连接内的虚拟连接，AMQP命令都是通过信道发出去的，不管是发布消息、订阅队列还是接收消息，这些动作都是通过信道完成，因为对于操作系统来说建立和销毁TCP都是非常昂贵的开销，所以引入了信道的概念，以复用一条TCP连接。

**Consumer:**

消息的消费者，表示从一个消息队列中取得消息的客户端应用程序。

**Virtual** **Host:**

虚拟主机，表示一批交换器、消息队列和相关对象。虚拟主机是共享相同的身份认证和加密环境的独立服务器域。每个Vhost本质上就是一个mini版的RaabbitMQ服务器，拥有自己的队列、交换器、绑定和权限机制。Vhost是AMQP概念的基础，必须在连接时指定，RabbitMQ默认的Vhost是 / 。

**Broker:**

表示消息队列服务器的实体



个人理解：

<u>消息生产者（也就是消息发布者）发送一条消息给服务器，然后服务器找到交换器，接下来根据交换器绑定的路由关系（路由键）找到对应的消息队列，然后消息消费者（消息接收人）从队列中获取消息。</u>



### 自动配置

#### 1.RabbitAutoConfiguration

#### 2.自动配置了连接工厂 ConnectionFactory

#### 3.RabbitProperties 封装 RabbitMQ的配置

properties:

spring.rabbitmq.host= 虚拟主机地址

spring.rabbitmq.username=guest

spring.rabbitmq.password-=guest

#### 4.还会给容器中添加RabbitTemplate：给RabbitMQ发送和接收消息

#### 5.AmqpAdmin ： RabbitMQ的**系统管理功能组件** 可以创建交换器和队列

AmqpAdmin：创建和删除Queue ,Exchange ,Binding



```java
/**

* 1.单播（点对点）

*/

@AutoWired

RabbitTemplate rabbitTemplate;

//message需要自己构造一个；定义消息头和消息体

rabbitTemplate.send(exchange,routeKey,message);

// object 默认当成消息体只需要传入需要发送的对象，自动序列化发送给rabbitmq;

rabbitTemplate.convertAndSend(exchange,routeKey,object);

new HashMap<>();

map.put("msg","第一个消息");

map.put("data",Arrays.asList("helloworld",123,true));

rabbitTemplate.convertAndSend("exchange.direct","路由键",map);



//接收消息

rabbitTemplate.receiveAndConvert();

默认的序列化机制是JDK的

@AutoWired
AmqpAdmin amqpAdmin;

@Test
public void createExchange(){
    //交换器名字，是否持久化，是否自动删除
    //amqpAdmin.declareExchange(new DirectExchange(String name,boolean durable,boolean autoDelete));
    amqpAdmin.declareExchange(new DirectExchange("amqpAdmin.direct");
    amqpAdmin.declareQueue(new Queue("amqpAdmin.queue",true));
	//目的地，目的地类型，交换器，路由键，参数
	amqpAdmin.declareBinding(new Binding(String destination,DestinationType destinationType, String exchange,String routeKey,Map<String,Object> arguments));
	amqpAdmin.declareBinding(new Binding("amqpAdmin.queue",Binding.DestinationType.QUEUE,"amqpAdmin.direct","amqpAdmin.queue",null));
}
```



单独写个配置类MyAMQPConfig

```java
@Configuration

public class MyAMQPConfig{
    @Bean
    //导入amqp.support那个包
    public MessageConverter messageConverter(){
        return new Jackson2JsonMessageConverter();
    }
}
```

//监听消息队列来调用接收方法

**//主启动类要加上注解@EnableRabbit**

//开启基于注解的RabbitMQ

@Service

public class BookService{

​	@RabbitListener(queues = {"AMQP.direct"} )

​	//监听消息队列里面的内容

​	public void receive (Book book){

​		sout("收到消息:"+book);

​	}



​	@RabbitListener(queues = {"AMQP.direct"} )

​	//监听消息队列里面的内容

​	//amqp.core包

​	public void receive (Message message){

​		sout("消息头:"+message.getMessageProperties());

​		sout("消息体:"+message.getBody());

​	}

}