## Spring Cloud Hystrix服务容错

 2018-06-06 |  Visit count 644989

在微服务的架构中，服务间通常会形成相互依赖的关系，比如现在有三个微服务节点：A，B和C，B为A的消费者，C为B的消费者。假如由于网络波动或者A服务自身故障，导致B调用A服务的线程被挂起进入长时间的等待。在高并发的情况下可能导致B的资源被耗竭随之崩溃，从而导致C服务也不可用。这种连环式的雪崩效应在微服务中较为常见，为了解决这个问题，服务熔断技术应运而出。熔断一词来自电路学，指的是电路在出现短路状况时，“断路器”能够及时地切断故障电路，避免电路过载发热引发火灾。

类似的，微服务架构中的断路器能够及时地发现故障服务，并向服务调用方返回错误响应，而不是长时间的等待。Spring Cloud Hystrix在Hystrix（又是一款由Netflix开发的开源软件，Github地址https://github.com/Netflix/Hystrix）的基础上进行了封装，提供了服务熔断，服务降级，线程隔离等功能，通过这些功能可以提供服务的容错率。

## 使用Hystrix

这里将在上一节[Spring Cloud Ribbon客户端负载均衡](https://mrbird.cc/Spring-Cloud-Ribbon-LoadBalance.html)源码的基础上配置Hystrix。

我们先看下在没有配置Hystrix之前，关闭Eureka-Client是什么效果。

分别使用peer1和peer2配置启动Eureka-Server集群，然后启动两个Eureka-Client实例，端口分别为8082和8083，最后启动Ribbon-Consumer。准备完毕后，我们关闭端口为8082的Eureka-Client，然后发送GET请求http://localhost:9000/user/1，返回结果如下：

![QQ截图20180710151846.png](https://mrbird.cc/img/QQ%E6%88%AA%E5%9B%BE20180710151846.png)

下面开始使用使用Spring Cloud Hystrix，在项目Ribbon-Consumer中引入Spring Cloud Hystrix依赖：

```
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-hystrix</artifactId>
</dependency>
```



在入口类上加入`@EnableHystrix`或者`@EnableCircuitBreaker`注解。这两个注解是等价的，查看`@EnableHystrix`注解源码就可以证实这一点：

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@EnableCircuitBreaker
public @interface EnableHystrix {
}
```



在引入`@EnableHystrix`或者`@EnableCircuitBreaker`注解后，我们的入口类代码如下：

```java
@EnableCircuitBreaker
@EnableDiscoveryClient
@SpringBootApplication
public class DemoApplication {
    @Bean
    @LoadBalanced
    RestTemplate restTemplate() {
        return new RestTemplate();
    }
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```



入口类上总共包含了三个注解`@EnableCircuitBreaker`、`@EnableDiscoveryClient`和`@SpringBootApplication`，这三个注解的组合可以使用`@SpringCloudApplication`来代替，`@SpringCloudApplication`源码如下：

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootApplication
@EnableDiscoveryClient
@EnableCircuitBreaker
public @interface SpringCloudApplication {
}
```



接着将UserController中的方法提取出来，创建一个UserService（为了简单起见，不再创建Service接口）：

```java
@Service("userService")
public class UserService {
    private Logger log = LoggerFactory.getLogger(this.getClass());

    @Autowired
    private RestTemplate restTemplate;

    public User getUser(@PathVariable Long id) {
        return restTemplate.getForObject("http://Server-Provider/user/{id}", User.class, id);
    }

    public List<User> getUsers() {
        return this.restTemplate.getForObject("http://Server-Provider/user", List.class);
    }

    public String addUser() {
        User user = new User(1L, "mrbird", "123456");
        HttpStatus status = this.restTemplate.postForEntity("http://Server-Provider/user", user, null).getStatusCode();
        if (status.is2xxSuccessful()) {
            return "新增用户成功";
        } else {
            return "新增用户失败";
        }
    }

    public void updateUser() {
        User user = new User(1L, "mrbird", "123456");
        this.restTemplate.put("http://Server-Provider/user", user);
    }

    public void deleteUser(@PathVariable Long id) {
        this.restTemplate.delete("http://Server-Provider/user/{1}", id);
    }
}
```



接着改造UserService的`getUser`方法：

```java
@HystrixCommand(fallbackMethod = "getUserDefault")
public User getUser(@PathVariable Long id) {
   return restTemplate.getForObject("http://Server-Provider/user/{id}", User.class, id);
}
public User getUserDefault(Long id) {
    User user = new User();
    user.setId(-1L);
    user.setUsername("defaultUser");
    user.setPassword("123456");
    return user;
}
```



我们在`getUser`方法上加入了`@HystrixCommand`注解，注解的`fallbackMethod`属性指定了被调用的方法不可用时的回调方法（服务熔断时的回调处理逻辑，即服务降级），这里为`getUserDefault`方法（必须与`getUser`方法的参数及返回值类型一致）。

在UserController中调用UserService的`getUser`方法：

```java
@RestController
public class TestController {
    private Logger log = LoggerFactory.getLogger(this.getClass());

    @Autowired
    private UserService userService;

    @GetMapping("user/{id}")
    public User getUser(@PathVariable Long id) {
        return userService.getUser(id);
    }
}
```



修改完后启动Ribbon-Consumer并重重新启动8082端口的Eureka-Client，发送数次GET请求http://localhost:9000/user/1后，再次关闭8082端口的Eureka-Client。

断开后，继续发送GET请求http://localhost:9000/user/1，当轮询到8082端口时返回数据如下图所示：

![QQ截图20180710154714.png](https://mrbird.cc/img/QQ%E6%88%AA%E5%9B%BE20180710154714.png)

可以看到，当轮询到服务不可用时，触发了熔断机制，接口回调了`fallbackMethod`指定的方法。

我们也可以模拟服务超时的情况，可以在Eureka-Client提供的接口方法中设置线程等待，等待时间大于2000（Hystrix默认超时时间为2000 毫秒）即可触发调用方Ribbon-Consumer的服务熔断。

## @HystrixCommand详解

`@HystrixCommand`注解还包含许多别的属性功能，下面介绍一些常用的属性配置。

### 服务降级

上面TestController中的`getUser`中我们用`@HystrixCommand`注解指定了服务降级方法`getUserDefault`。如果`getUserDefault`方法也抛出异常，那么我们可以再次使用`@HystrixCommand`注解指定`getUserDefault`方法降级的方法，比如定义一个`getUserDefault2`方法：

```java
@HystrixCommand(fallbackMethod = "getUserDefault2")
public User getUserDefault(Long id) {
    String a = null;
    // 测试服务降级
    a.toString();
    User user = new User();
    user.setId(-1L);
    user.setUsername("defaultUser");
    user.setPassword("123456");
    return user;
}

public User getUserDefault2(Long id) {
    User user = new User();
    user.setId(-2L);
    user.setUsername("defaultUser2");
    user.setPassword("123456");
    return user;
}
```



重启Ribbon-Consumer，并关闭8082端口的Eureka Client服务，访问http://localhost:9000/user/1：

![QQ截图20180712112335.png](https://mrbird.cc/img/QQ%E6%88%AA%E5%9B%BE20180712112335.png)

### 异常处理

在使用`@HystrixCommand`注解标注的方法中，除了`HystrixBadRequestException`异常外，别的异常都会触发服务降级。假如我们想指定某个异常不触发服务降级，可以使用`@HystrixCommand`注解的`ignoreExceptions`属性进行忽略。如：

```java
@HystrixCommand(fallbackMethod = "getUserDefault2", ignoreExceptions = {NullPointerException.class})
public User getUserDefault(Long id) {
    String a = null;
    // 测试服务降级
    a.toString();
    User user = new User();
    user.setId(-1L);
    user.setUsername("defaultUser");
    user.setPassword("123456");
    throw new HystrixBadRequestException()
    return user;
}
```



此外，对于方法抛出的异常信息，我们可以在服务降级的方法中使用`Throwable`对象获取，如：

```java
@HystrixCommand(fallbackMethod = "getUserDefault2")
public User getUserDefault(Long id, Throwable e) {
    System.out.println(e.getMessage());
    User user = new User();
    user.setId(-2L);
    user.setUsername("defaultUser2");
    user.setPassword("123456");
    return user;
}
```



### 命名与分组

通过指定`@HystrixCommand`注解的`commandKey`、`groupKey`以及`threadPoolKey`属性可以设置命令名称、分组以及线程池划分，如：

```java
@HystrixCommand(fallbackMethod = "getUserDefault", commandKey = "getUserById", groupKey = "userGroup",
        threadPoolKey = "getUserThread")
public User getUser(@PathVariable Long id) {
	log.info("获取用户信息");
    return restTemplate.getForObject("http://Server-Provider/user/{id}", User.class, id);
}
```



上面的配置指定了命令的名称为`getUserById`，组名为`userGroup`，线程池名称为`getUserThread`。

通过设置命令组，Hystrix会根据组来组织和统计命令的告警、仪表盘等信息。默认情况下，Hystrix命令通过组名来划分线程池，即组名相同的命令放到同一个线程池里，如果通过`threadPoolKey`设置了线程池名称，则按照线程池名称划分。

当`getUser`方法被调用时，日志打印如下：

```
2018-06-06 15:32:55.945  INFO 16192 --- [getUserThread-1] com.example.demo.Service.UserService  : 获取用户信息
```



可看到线程名称为getUserThread-1。

### Hystrix缓存

我们在Controller中调用三次`getUser`方法，参数都为1L：

```java
@RestController
public class TestController {

    @Autowired
    private UserService userService;

    @GetMapping("testCache")
    public void testCache(){
        userService.getUser(1L);
        userService.getUser(1L);
        userService.getUser(1L);
    }
}
```



当访问http://localhost:9000/testCache时，控制台输出如下：![QQ截图20180712173742.png](https://mrbird.cc/img/QQ%E6%88%AA%E5%9B%BE20180712173742.png)

开启缓存可以让`getUser`方法只被调用一次，剩下两次直接从缓存里获取。

**开启缓存**

要在Hystrix中开启缓存很简单，只需使用`@CacheResult`注解即可，修改UserService的`getUser`方法：

```java
@CacheResult
public User getUser(@PathVariable Long id) {
    return restTemplate.getForObject("http://Server-Provider/user/{id}", User.class, id);
}
```



通过上面的设定，Hystrix会将返回的User对象进行缓存，缓存的key默认为方法的所有参数，这里只有一个id参数，所以缓存的key为用户id。

这里在测试的时候遇到一个异常：

java.lang.IllegalStateException: Request caching is not available. Maybe you need to initialize the HystrixRequestContext? at com.netflix.hystrix.HystrixRequestCache.get(HystrixRequestCache.java:104) ~[hystrix-core-1.5.12.jar:1.5.12] at com.netflix.hystrix.AbstractCommand$7.call(AbstractCommand.java:478) ~[hystrix-core-1.5.12.jar:1.5.12] at com.netflix.hystrix.AbstractCommand$7.call(AbstractCommand.java:454) ~[hystrix-core-1.5.12.jar:1.5.12] …



在Hystrix的issue中找到了类似的提问：https://github.com/Netflix/Hystrix/issues/1314。

大致意思是在使用Hytrix缓存之前，需要通过`HystrixRequestContext.initializeContext`初始化Hystrix请求上下文，请求结束之后需要调用`shutdown`方法关闭请求。

所以我们可以定义一个过滤器来实现这个过程：

```java
@Component
@WebFilter(filterName = "hystrixRequestContextServletFilter", urlPatterns = "/*", asyncSupported = true)
public class HystrixRequestContextServletFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) {
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        HystrixRequestContext context = HystrixRequestContext.initializeContext();
        filterChain.doFilter(servletRequest, servletResponse);
        context.close();
    }

    @Override
    public void destroy() {
    }
}
```



到这里，我才意识到，其实Hystrix的缓存还是蛮鸡肋的，请求缓存不是只写入一次结果就不再变化的，而是每次请求到达Controller的时候，我们都需要为HystrixRequestContext进行初始化，之前的缓存也就是不存在了，我们是在同一个请求中保证结果相同，同一次请求中的第一次访问后对结果进行缓存，缓存的生命周期只有一次请求！

改造完毕后，重启项目再次访问http://localhost:9000/testCache，控制台输出如下：

![QQ截图20180712181439.png](https://mrbird.cc/img/QQ%E6%88%AA%E5%9B%BE20180712181439.png)

**设定key值**

我们也可以明确的指定缓存的key值是什么。指定key的值有两种方式：

1. 通过`@CacheKey`注解指定，如：

   ```java
   @CacheResult
   @HystrixCommand(fallbackMethod = "getUserDefault", commandKey = "getUserById", groupKey = "userGroup",
           threadPoolKey = "getUserThread")
   public User getUser(@CacheKey("id") @PathVariable Long id) {
       log.info("获取用户信息");
       return restTemplate.getForObject("http://Server-Provider/user/{id}", User.class, id);
   }
   ```

   也可以指定参数对象内部属性为key值：

   ```java
   @CacheResult
   @HystrixCommand(fallbackMethod = "getUserDefault", commandKey = "getUserById", groupKey = "userGroup",
           threadPoolKey = "getUserThread")
   public User getUser(@CacheKey("id") User user) {
       log.info("获取用户信息");
       return restTemplate.getForObject("http://Server-Provider/user/{id}", User.class, user.getId());
   }
   ```

2. 通过方法来指定，方法的返回值必须是String类型：

   ```java
   public String getCacheKey(Long id) {
       return String.valueOf(id);
   }
   
   @CacheResult(cacheKeyMethod = "getCacheKey")
   @HystrixCommand(fallbackMethod = "getUserDefault", commandKey = "getUserById", groupKey = "userGroup",
           threadPoolKey = "getUserThread")
   public User getUser(Long id) {
       log.info("获取用户信息");
       return restTemplate.getForObject("http://Server-Provider/user/{id}", User.class, id);
   }
   ```

值得注意的是，方法2的优先级比方法1高。

**缓存清除**

在涉及到更新User信息的方法上，我们要及时的清除相应的缓存，否则将会导致缓存数据和实际数据不一致的问题。我们在UserService的`updateUser`方法上做缓存清除操作：

```java
@CacheRemove(commandKey = "getUserById")
@HystrixCommand
public void updateUser(@CacheKey("id") User user) {
    this.restTemplate.put("http://Server-Provider/user", user);
}
```



`@CacheRemove`的`commandKey`属性和`getUser`里定义的一致。

### 请求合并

请求合并就是将多个单个请求合并成一个请求，去调用服务提供者，从而降低服务提供者负载的，一种应对高并发的解决办法。

Hystrix中提供了一个`@HystrixCollapser`注解，该注解可以将处于一个很短的时间段（默认10 毫秒）内对同一依赖服务的多个请求进行整合并以批量方式发起请求。为了演示`@HystrixCollapser`注解的使用方法，我们改造下Eureka-Client（服务提供者）的UserController接口，提供一个批量处理的方法：

```java
@RestController
@RequestMapping("user")
public class UserController {
    private Logger log = LoggerFactory.getLogger(this.getClass());

    @GetMapping("users")
    public List<User> get(String ids) {
        log.info("批量获取用户信息");
        List<User> list = new ArrayList<>();
        for (String id : ids.split(",")) {
            list.add(new User(Long.valueOf(id), "user" + id, "123456"));
        }
        return list;
    }
    ...
}
```



然后在Ribbon-Consumer的UserService里添加两个方法：

```java
@HystrixCollapser(batchMethod = "findUserBatch", collapserProperties = {
        @HystrixProperty(name = "timerDelayInMilliseconds", value = "100")
})
public Future<User> findUser(Long id) {
    log.info("获取单个用户信息");
    return new AsyncResult<User>() {
        @Override
        public User invoke() {
            return restTemplate.getForObject("http://Server-Provider/user/{id}", User.class, id);
        }
    };
}

@HystrixCommand
public List<User> findUserBatch(List<Long> ids) {
    log.info("批量获取用户信息,ids: " + ids);
    User[] users = restTemplate.getForObject("http://Server-Provider/user/users?ids={1}", User[].class, StringUtils.join(ids, ","));
    return Arrays.asList(users);
}
```



`@HystrixCollapser`注解的`batchMethod`属性指定了批量处理的方法为下面定义的`findUserBatch`方法，`timerDelayInMilliseconds`的值为100（毫秒），意思是在100毫秒这个时间范围内的所有对`findUser`的调用，都将被合并为一个批量处理操作，进行批量处理操作的方法就是`findUserBatch`。

我们在TestController中添加一个测试方法：

```java
@GetMapping("testRequestMerge")
public void testRequerstMerge() throws InterruptedException, ExecutionException {
    Future<User> f1 = userService.findUser(1L);
    Future<User> f2 = userService.findUser(2L);
    Future<User> f3 = userService.findUser(3L);
    f1.get();
    f2.get();
    f3.get();
    Thread.sleep(200);
    Future<User> f4 = userService.findUser(4L);
    f4.get();
}
```



上面的测试方法中对`findUser`方法进行了4次的调用，最后一次调用（f4）之前先让线程等待200毫秒（大于`timerDelayInMilliseconds`中定义的100毫秒），所以我们的预期是前三次调用会被合并，而最后一次调用不会被合并进去。

启动Ribbon-Consumer，访问http://localhost:9000/testRequestMerge,控制台输出如下：

![QQ截图20180712185806.png](https://mrbird.cc/img/QQ%E6%88%AA%E5%9B%BE20180712185806.png)

可以看到，控制台的输出符合我们的预期，f1、f2和f3被合并成了一个请求。

而且可以看到，控制台并没有打印出`findUser`方法中的`获取单个用户信息`的日志，实际上`findUser`方法并不会被调用，所以上面的代码可以简化为：

```java
@HystrixCollapser(batchMethod = "findUserBatch", collapserProperties = {
        @HystrixProperty(name = "timerDelayInMilliseconds", value = "100")
})
public Future<User> findUser(Long id) {
    log.info("获取单个用户信息");
    return null;
}

@HystrixCommand
public List<User> findUserBatch(List<Long> ids) {
    log.info("批量获取用户信息,ids: " + ids);
    User[] users = restTemplate.getForObject("http://Server-Provider/user/users?ids={1}", User[].class, StringUtils.join(ids, ","));
    return Arrays.asList(users);
}
```



虽然通过请求的合并可以减轻带宽和服务的压力，但合并请求的过程也会带来额外的开销。就拿上面的`testCache`来说，比如我们对单个`findUser`的方法调用耗时5ms，那么调用4次耗时可以粗略的估算为20ms。当我们使用Hystrix的请求合并功能后，前3次请求（f1、f2和f3）进行了合并，第4次请求（f4）没有进行合并，那么耗时可以粗略的估算为`3*5+100+5=120ms`（100为上面`timerDelayInMilliseconds`中指定的时间范围，在该时间段过后，才会调用第4次请求），结果明显比单独调用4次来得高。所以实际中是否该使用Hystrix的请求合并功能，需结合实际需求进行抉择。

## Hystrix属性

除了上面涉及到的Hystrix属性配置外，其还包含了大量的别的可用配置。配置可以分为四个级别，优先级从低到高分别为：全局默认配置、全局配置、实例默认值、实例配置。

### Commond

#### execution

`execution.isolation.strategy`： 该属性用来设置执行的隔离策略，它有如下两个选项。

1. `THREAD`: 通过线程池隔离的策略。它在独立的线程上执行， 并且它的并发限制受线程池中线程数量的限制。
2. `SEMAPHORE`: 通过信号量隔离的策略。它在调用线程上执行， 并且它的并发限制受信号量计数的限制。

| 属性级别     | 默认值、配置方式、配置属性                                   |
| :----------- | :----------------------------------------------------------- |
| 全局默认配置 | THREAD                                                       |
| 全局配置     | hystrix.command.default.execution.isolation.strategy         |
| 实例默认值   | @HystrixProperty(name=”execution.isolation.strategy”, value=”THREAD”) |
| 实例配置     | hystrix.command.HystrixCommandKey.execution.isolation.strategy |

实例配置中的HystrixCommandKey对应@HystrixCommand注解中commandKey 属性指定的值。

`execution.isolation.thread.timeoutinMilliseconds`： 该属性用来配置HystrixCommand执行的超时时间，单位为毫秒。当HystrixCommand执行时间超过该配置值之后， Hystrix会将该执行命令标记为TIMEOUT并进入服务降级处理逻辑。

| 属性级别     | 默认值、配置方式、配置属性                                   |
| :----------- | :----------------------------------------------------------- |
| 全局默认配置 | 1000亳秒                                                     |
| 全局配置     | hystrix.command.default.execution.isolation.thread. timeoutinMilliseconds |
| 实例默认值   | @HystrixProperty(name=”execution.isolation.thread.timeoutinMilliseconds”,value=”2000”) |
| 实例配置     | hystrix.command.HystrixCommandKey.execution.isolation.thread.timeoutinMilliseconds |

`execution.timeout.enabled`: 该属性用来配置HystrixCommand的执行是否启用超时时间。默认为true, 如果设置为false, 那么`execution.isolation.thread.timeoutinMilliseconds`属性的配置将不再起作用。

| 属性级别     | 默认值、配置方式、配置属性                                   |
| :----------- | :----------------------------------------------------------- |
| 全局默认配置 | true                                                         |
| 全局配置     | hystrix.command.default.execution.timeout.enabled            |
| 实例默认值   | @HystrixProperty(name=”execution.timeout.enabled”, value=”false”) |
| 实例配置     | hystrix.command.HystrixCommandKey.execution.timeout.enabled  |

`execution.isolation.thread.interruptOnTimeout`: 该属性用来配置当HystrixCommand执行超时的时候是否要将它中断。

| 属性级别     | 默认值、配置方式、配置属性                                   |
| :----------- | :----------------------------------------------------------- |
| 全局默认配置 | true                                                         |
| 全局配置     | hystrix.command.default.execution.isolation.thread.interruptOnTimeout |
| 实例默认值   | @HystrixProperty(name=”execution.isolation.thread.interruptOnTimeout”,value=”false”) |
| 实例配置     | hystrix.command.HystrixCommandKey.execution.isolation.thread.interruptOnTimeout |

`execution.isolation.thread.interruptOnCancel`: 该属性用来配置当HystrixCommand执行被取消的时候是否要将它中断。

| 属性级别     | 默认值、配置方式、配置属性                                   |
| :----------- | :----------------------------------------------------------- |
| 全局默认配置 | true                                                         |
| 全局配置     | hystrix.command.default.execution.isolation.thread.interruptOnCancel |
| 实例默认值   | @HystrixProperty(name=”execution.isolation.thread.interruptOnCancel”,value= “false”) |
| 实例配置     | hystrix.command.HystrixCommandKey.execution.isolation.thread.interruptOnCancel |

`execution.isolation.semaphore.maxConcurrentRequests`: 当HystrixCommand的隔离策略使用信号量的时候，该属性用来配置信号量的大小（并发请求数）。当最大并发请求数达到该设置值时，后续的请求将会被拒绝。

| 属性级别     | 默认值、配置方式、配置属性                                   |
| :----------- | :----------------------------------------------------------- |
| 全局默认配置 | 10                                                           |
| 全局配置     | hystrix.command.default.execution.isolation.semaphore.maxConcurrentRequests |
| 实例默认值   | @HystrixProperty(name=”execution.isolation.semaphore.maxConcurrentRequests”, value=”2”) |
| 实例配置     | hystrix.command.HystrixCommandKey.execution.isolation.semaphore.maxConcurrentRequests |

#### fallback

`fallback.enabled`: 该属性用来设置服务降级策略是否启用，如果设置为false,那么当请求失败或者拒绝发生时，将不会调用降级服务。

| 属性级别     | 默认值、配置方式、配置属性                                |
| :----------- | :-------------------------------------------------------- |
| 全局默认配置 | true                                                      |
| 全局配置     | hystrix.command.default.fallback.enabled                  |
| 实例默认值   | @HystrixProperty(name= “fallback.enabled”, value=”false”) |
| 实例配置     | hystrix.command.HystrixCommandKey.fallback.enabled        |

#### circuitBreaker断路器

`circuitBreaker.enabled`: 该属性用来确定当服务请求命令失败时， 是否使用断路器来跟踪其健康指标和熔断请求。

| 属性级别     | 默认值、配置方式、配置属性                                   |
| :----------- | :----------------------------------------------------------- |
| 全局默认配置 | true                                                         |
| 全局配置     | hystrix.command.default.circuitBreaker.enabled               |
| 实例默认值   | @HystrixProperty(name=”circutBreaker.enabled”,value=”false”) |
| 实例配置     | hystrix.command.HystrixCommandKey.circuitBreaker.enabled     |

`circuitBreaker.requestVolumeThreshold`: 该属性用来设置在滚动时间窗中，断路器熔断的最小请求数。例如，默认该值为20 的时候，如果滚动时间窗（默认10秒）内仅收到了19个请求， 即使这19个请求都失败了，断路器也不会打开。

| 属性级别     | 默认值、配置方式、配置属性                                   |
| :----------- | :----------------------------------------------------------- |
| 全局默认配置 | 20                                                           |
| 全局配置     | hystrix.command.default.circuitBreaker.requestVolumeThreshold |
| 实例默认值   | @HystrixProperty(name=”circuitBreaker.requestVolumeThreshold”, value=”30”) |
| 实例配置     | hystrix.comrnand.HystrixComrnandKey.circuitBreaker.requestVolumeThreshold |

`circuitBreaker.sleepWindowinMilliseconds`: 该属性用来设置当断路器打开之后的休眠时间窗。休眠时间窗结束之后，会将断路器置为“半开” 状态， 尝试熔断的请求命令，如果依然失败就将断路器继续设置为“打开” 状态，如果成功就设置为“关闭” 状态。

| 属性级别     | 默认值、配置方式、配置属性                                   |
| :----------- | :----------------------------------------------------------- |
| 全局默认配置 | 5000                                                         |
| 全局配置     | hystrix.command.default.circuitBreaker.sleepWindowinMilliseconds |
| 实例默认值   | @HystrixProperty(name=”circuitBreaker.sleepWindowinMilliseconds”,value=”3000”) |
| 实例配置     | hystrix.command.HystrixCommandKey.circuitBreaker.sleepWindowinMilliseconds |

`circuitBreaker.errorThresholdPercentage`: 该属性用来设置断路器打开的错误百分比条件。例如，默认值为5000 的情况下，表示在滚动时间窗中，在请求数量超过`circuitBreaker.requestVolumeThreshold`阅值的前提下，如果错误请求数的百分比超过50, 就把断路器设置为“打开” 状态， 否则就设置为“关闭” 状态。

| 属性级别     | 默认值、配置方式、配置属性                                   |
| :----------- | :----------------------------------------------------------- |
| 全局默认配置 | 50                                                           |
| 全局配置     | hystrix.command.default.circuitBreaker.errorThresholdPercentage |
| 实例默认值   | @HystrixProperty(name=”circuitBreaker.errorThresholdPercentage”, value=”40”) |
| 实例配置     | hystrix.command.HystrixCommandKey.circuitBreaker.errorThresholdPercentage |

`circuitBreaker.forceOpen`: 如果将该属性设置为true, 断路器将强制进入“打开” 状态，它会拒绝所有请求。该属性优先于`circuitBreaker.forceClosed`属性。

| 属性级别     | 默认值、配置方式、配置属性                                   |
| :----------- | :----------------------------------------------------------- |
| 全局默认配置 | false                                                        |
| 全局配置     | hystrix.command.default.circuitBreaker.forceOpen             |
| 实例默认值   | @HystrixProperty (name=”circuitBreaker.forceOpen”, value=”true”) |
| 实例配置     | hystrix.command.HystrixCommandKey.circuitBreaker.forceOpen   |

`circuitBreaker.forceClosed`: 如果将该属性设置为true, 断路器将强制进入“关闭” 状态， 它会接收所有请求。如果`circuitBreaker.forceOpen`属性为true, 该属性不会生效。

| 属性级别     | 默认值、配置方式、配置属性                                   |
| :----------- | :----------------------------------------------------------- |
| 全局默认配置 | false                                                        |
| 全局配置     | hystrix.command.default.circuitBreaker.forceClosed           |
| 实例默认值   | @HystrixProperty(name=”circui七Breaker.forceClosed”, value=”true”) |
| 实例配置     | hystrix.comrnand.HystrixComrnandKey.circuitBreaker.forceClosed |

#### metrics配置

该配置与HystrixCommand执行中捕获的指标信息有关。

`metrics.rollingStats.timeinMilliseconds`: 该属性用来设置滚动时间窗的长度， 单位为毫秒。该时间用于断路器判断健康度时需要收集信息的持续时间。断路器在收集指标信息的时候会根据设置的时间窗长度拆分成多个“桶” 来累计各度量值，每个“桶” 记录了一段时间内的采集指标。例如，当采用默认值10000毫秒时， 断路器默认将其拆分成10个桶（桶的数量也可通过`metrics.rollingStats.numBuckets`参数设置），每个桶记录1000毫秒内的指标信息。

| 属性级别     | 默认值、配置方式、配置属性                                   |
| :----------- | :----------------------------------------------------------- |
| 全局默认配置 | 10000                                                        |
| 全局配置     | hystrix.command.default.metrics.rollingStats.timeinMilliseconds |
| 实例默认值   | @HystrixProperty(name=”metrics.rollingStats.timeinMilliseconds”,value=”20000”) |
| 实例配置     | hystrix.command.HystrixCommandKey.metrics.rollingStats.timeinMilliseconds |

`metrics.rollingstats.numBuckets`: 该属性用来设置滚动时间窗统计指标信息时划分“桶” 的数量。

| 属性级别     | 默认值、配置方式、配置属性                                   |
| :----------- | :----------------------------------------------------------- |
| 全局默认配置 | 10                                                           |
| 全局配置     | hystrix.command.default.metrics.rollingStats.numBuckets      |
| 实例默认值   | @HystrixProperty(name=”metrics.rollingStats.numBuckets”,value=”20”) |
| 实例配置     | hystrix.comrnand.HystrixComrnandKey.metrics.rollingStats.numBuckets |

`metrics.rollingPercentile.enabled`: 该属性用来设置对命令执行的延迟是否使用百分位数来跟踪和计算。如果设置为false，那么所有的概要统计都将返回-1。

| 属性级别     | 默认值、配置方式、配置属性                                   |
| :----------- | :----------------------------------------------------------- |
| 全局默认配置 | true                                                         |
| 全局配置     | hystrix.command.default.metrics.rollingPercentile.enabled    |
| 实例默认值   | @HystrixProperty(name=”metrics.rollingPercentile.enabled”, value=”false”) |
| 实例配置     | hystrix.command.HystrixCommandKey.metrics.rollingPercentile.enabled |

`metrics.rollingPercentile.timeinMilliseconds`: 该属性用来设置百分位统计的滚动窗口的持续时间，单位为毫秒。

| 属性级别     | 默认值、配置方式、配置属性                                   |
| :----------- | :----------------------------------------------------------- |
| 全局默认配置 | 60000                                                        |
| 全局配置     | hystrix.command.default.metrics.rollingPercentile.timeinMilliseconds |
| 实例默认值   | @HystrixProperty(name=”metrics.rollingPercentile.timeinMilliseconds”, value=”50000”) |
| 实例配置     | hystrix.command.HystrixCommandKey.metrics.rollingPercentile.timeinMilliseconds |

`metrics.rollingPercentile.numBuckets`: 该属性用来设置百分位统计滚动窗口中使用“桶”的数量。

| 属性级别     | 默认值、配置方式、配置属性                                   |
| :----------- | :----------------------------------------------------------- |
| 全局默认配置 | 6                                                            |
| 全局配置     | hystrix.command.default.metrics.rollingPercentile.numBuckets |
| 实例默认值   | @HystrixProperty(name=”metrics.rollingPercentilee.numBuckets”,value=”5”) |
| 实例配置     | hystrix.command.HystrixCommandKey.metrics.rollingPercentile.numBuckets |

`metrics.rollingPercentile.bucketSize`: 该属性用来设置在执行过程中每个“桶” 中保留的最大执行次数。如果在滚动时间窗内发生超过该设定值的执行次数，就从最初的位置开始重写。例如，将该值设置为100, 滚动窗口为10秒，若在10秒内一个“桶”中发生了500次执行，那么该“桶”中只保留最后的100次执行的统计。另外，增加该值的大小将会增加内存量的消耗，并增加排序百分位数所需的计算时间。

| 属性级别     | 默认值、配置方式、配置属性                                   |
| :----------- | :----------------------------------------------------------- |
| 全局默认配置 | 100                                                          |
| 全局配置     | hystrix.command.default.metrics.rollingPercentile.bucketSize |
| 实例默认值   | @HystrixProperty(name=”metrics.rollingPercentile.bucketSize”,value= “120”) |
| 实例配置     | hystrix.command.HystrixCommandKey.metrics.rollingPercentile.bucketSize |

`metrics.healthSnapshot.intervalinMilliseconds`: 该属性用来设置采集影响断路器状态的健康快照（请求的成功、错误百分比）的间隔等待时间。

| 属性级别     | 默认值、配置方式、配置属性                                   |
| :----------- | :----------------------------------------------------------- |
| 全局默认配置 | 500                                                          |
| 全局配置     | hystrix.comrnand.default.metrics.healthSnapshot.intervalinMilliseconds |
| 实例默认值   | @HystrixProperty(name=”metrics.healthSnapshot.intervalinMilliseconds”,value=”600”) |
| 实例配置     | hystrix.command.HystrixCommandKey.metrics.healthSnapshot.intervalinMilliseconds |

#### requestContext

`requestCache.enabled`: 此属性用来配置是否开启请求缓存。

| 属性级别     | 默认值、配置方式、配置属性                                   |
| :----------- | :----------------------------------------------------------- |
| 全局默认配置 | true                                                         |
| 全局配置     | hystrix.command.default.requestCache.enabled                 |
| 实例默认值   | @HystrixProperty(name=”requestCache.enabled”, value=”false”) |
| 实例配置     | hystrix.command.HystrixCommandKey.requestCache.enabled       |

### collapser

`maxRequestsinBatch`: 该参数用来设置一次请求合并批处理中允许的最大请求数。

| 属性级别     | 默认值、配置方式、配置属性                                |
| :----------- | :-------------------------------------------------------- |
| 全局默认配置 | Integer.MAX_VALUE                                         |
| 全局配置     | hystrix.collapser.default.maxRequestsinBatch              |
| 实例默认值   | @HystrixProperty(name=”maxRequestsinBatch”,value=”false”) |
| 实例配置     | hystrix.collapser.HystrixCollapserKey.maxRequestsinBatch  |

`timerDelayinMilliseconds`: 该参数用来设置批处理过程中每个命令延迟的时间，单位为毫秒。

| 属性级别     | 默认值、配置方式、配置属性                                   |
| :----------- | :----------------------------------------------------------- |
| 全局默认配置 | 10                                                           |
| 全局配置     | hystrix.collapser.default.timerDelayinMilliseconds           |
| 实例默认值   | @HystrixProperty(name=”timerDelayinMilliseconds”,value=”20”) |
| 实例配置     | hystrix.collapser.HystrixCollapserKey.timerDelayinMilliseconds |

`request Cache.enabled`: 该参数用来设置批处理过程中是否开启请求缓存。

| 属性级别     | 默认值、配置方式、配置属性                                   |
| :----------- | :----------------------------------------------------------- |
| 全局默认配置 | true                                                         |
| 全局配置     | hystrix.collapser.default.requestCache.enabled               |
| 实例默认值   | @HystrixProperty(name=”requestCache.enabled”, value=”false”) |
| 实例配置     | hystrix.collapser.HystrixCollapserKey.requestCache.enabled   |

### threadPool

`coreSize`: 该参数用来设置执行命令线程池的核心线程数，该值也就是命令执行的最大并发量。

| 属性级别     | 默认值、配置方式、配置属性                       |
| :----------- | :----------------------------------------------- |
| 全局默认配置 | 10                                               |
| 全局配置     | hystrix.threadpool.default.coreSize              |
| 实例默认值   | @HystrixProperty(name=”coreSize”, value=”false”) |
| 实例配置     | hystrix.threadpool.HystrixThreadPoolKey.coreSize |

`maxQueueSize`: 该参数用来设置线程池的最大队列大小。当设置为-1时，线程池将使用`SynchronousQueue`实现的队列，否则将使用`LinkedBlockingQueue`实现的队列。

| 属性级别     | 默认值、配置方式、配置属性                           |
| :----------- | :--------------------------------------------------- |
| 全局默认配置 | -1                                                   |
| 全局配置     | hystrix.threadpool.default.maxQueueSize              |
| 实例默认值   | @HystrixProperty(name=”maxQueueSize”,value=”lO”)     |
| 实例配置     | hystrix.threadpool.HystrixThreadPoolKey.maxQueueSize |

`queueSizeRejectionThreshold`: 该参数用来为队列设置拒绝阈值。通过该参数，即使队列没有达到最大值也能拒绝请求。该参数主要是对`LinkedBlockingQueue`队列的补充， 因为`LinkedBlockingQueue`队列不能动态修改它的对象大小，而通过该属性就可以调整拒绝请求的队列大小了。

| 属性级别     | 默认值、配置方式、配置属性                                   |
| :----------- | :----------------------------------------------------------- |
| 全局默认配置 | 5                                                            |
| 全局配置     | hystrix.threadpool.default.queueSizeRejectionThreshold       |
| 实例默认值   | @HystrixProperty(name=”queueSizeRejectionThreshold”, value=”lO” |
| 实例配置     | hystrix.threadpool.HystrixThreadPoolKey.queueSizeRejectionThreshold |

不得不说，配置是真的多……

![QQ图片20180713111516.gif](https://mrbird.cc/img/QQ%E5%9B%BE%E7%89%8720180713111516.gif)

源码链接：https://github.com/wuyouzhuguli/Spring-Boot-Demos/tree/master/30.Spring-Cloud-Hystrix-Circuit-Breaker