Spring Boot 缓存

# 使用缓存抽象时注释掉redis的依赖

# 缓存对象需要实现 Serializable 接口

## 一、JSR 107

**CachingProvider** 定义了创建、配置、获取、管理和控制多个CacheManager。一个应用可以在运行期间访问多个CachingProvider

**CacheManager**定义了创建、配置、获取、管理和控制多个唯一命名的Cache,这些Cache存在于CacheManager的上下文中。一个CacheManager仅被一个CachingProvider所有用。

**Cache**是一个类似Map的数据结构并临时存储以Key为索引的值。一个Cache仅被一个CacheManager所拥有。

**Entry**是一个存储在Cache中的key-value对。

**Expiry**每一个存储在Cache中的条目有一个定义的有效期。一旦超过这个时间，条目为过期的状态。一旦过期，条目讲不可访问、更新和删除。缓存有效期可以通过ExpiryPolicy设置。

**tips:**

CachingProvider管理  <u>多个</u>  CacheManager

CacheManager仅被  <u>一个</u>  CachingProvider管理

CacheManager管理  <u>多个</u>  唯一命名的Cache

Cache仅被  <u>一个</u>  CacheManager管理

## 二、几个重要概念&缓存注解

| Cache          | 缓存接口，定义缓存操作。实现有：RedisCache、EhCache、ConcurrentMapCache等 |
| :------------- | ------------------------------------------------------------ |
| CacheManager   | 缓存管理器，管理各种缓存（Cache）的组件                      |
| @Cacheable     | 主要针对方法配置，能够根据方法的请求参数对其结果进行缓存     |
| @CacheEvict    | 清空缓存                                                     |
| @CachePut      | 保证方法被调用，又希望结果被缓存                             |
| @EnableCaching | 开启基于注解的缓存                                           |
| keyGenerator   | 缓存数据时key生成策略                                        |
| serialize      | 缓存数据时value序列化策略                                    |

## 三、缓存用例

1.**开启缓存**

```java
/**
*第一次查询后才缓存
*/
@Cacheable
public User getUser(Integer id);
```

2.清空缓存

```java
@CacheEvict
public void delUser(Integer id);
```

3.更新缓存

```java
/**
*每次调用都会更新缓存
*/
@CachePut
public void updateUser(User user);
```

4.key生成策略



5.缓存数据时value序列化策略

**基本环境搭建**

1.导入数据库文件

2.创建javaBean封装数据

3.整合mybatis操作数据库

配置数据源

使用注解版的Mybatis

@MapperScan制定需要扫描的mapper接口所在的包

二、体验缓存

**1.开启基于注解的缓存@EnableCaching**

```java
@MapperScan("com.li.cache.mapper")
@SpringBootApplication
@EnableCaching
```

2.标注缓存注解

@Cacheable

@CacheEvict

@CachePut

```java
/**
*将方法的运行结果进行缓存，以后再要相同的数据，直接从缓存中获取，不用再调用方法;
CaheManager管理多个Cache组件，对缓存的真正CRUD操作在Cache组件中，每一个缓存组件有自己唯一一个名字
几个属性：
		cacheNames/value:指定缓存组件的名字
		key:缓存数据使用的key:可以用它来制定。默认是使用方法参数的值 1-方法的返回值
		编写SpEL
		keyGenerator:key的生成器；可以自己指定key的生成器的组件id
		key/keyGenerator：二选一
		cacheManager:指定缓存管理器 或者 cacheResolver指定缓存解析器 二选一
		condition：指定符合条件的情况下缓存
		unless: 为true 则不缓存
		sync:异步
*/
@Cacheable(cacheNames = "emp" ,key = "#id",condition = "#id > 0", unless = "#result == null")
public User getEmp(Integer id){
    Employee emp = employeeMapper.getEmpById(id);
    return emp;
}
#id ===》形参中的Id
```

**SpEL表达式**

| 名字          | 位置               | 描述                                                         | 实例                 |
| ------------- | ------------------ | ------------------------------------------------------------ | -------------------- |
| methodName    | rootobject         | 当前被调用的方法名                                           | #root.methodName     |
| method        | root object        | 当前被调用的方法                                             | #root.method.name    |
| target        | root object        | 当前被调用的目标对象                                         | #root.target         |
| targetClass   | root object        | 当前被调用的目标对象类                                       | #root.targetClass    |
| args          | root object        | 当前被调用的方法的参数列表                                   | #root.args[0]        |
| caches        | root object        | 当前方法调用使用的缓存列表（如@Cacheable(value={"cache1","cache1"})）,则有两个cache | #root.caches[0].name |
| argument name | evaluation context | 方法参数的名字，可以直接 #参数名 ， 也可以使用 #p0或#a0的形式 ,0代表参数的索引； | #iban 、#a0 、#p0    |
| result        | evaluation context | 方法执行后的返回值（仅当方法执行之后的判断有效，如 ’unless‘, 'cache put' 的表达式 'cache evict'的表达式beforeInvocation = false） | #result              |