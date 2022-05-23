## Spring Cloud Feign 声明式服务调用

 2018-06-10 |  Visit count 645003

前面我们分别通过Spring Cloud Ribbon和Spring Cloud Hystrix实现了客户端负载均衡和服务容错，而Spring Cloud Feign不但整合了这两者的功能，而且还提供了一种比Ribbon更简单的服务调用方式 ——— 声明式服务调用。在Spring Cloud Feign中编写服务调用代码非常简单，几乎可以直接将服务提供者的代码复制过来，改为接口即可，下面通过例子来演示这个特性。

## 搭建Feign Consumer

创建一个新的Spring Boot应用，版本为`1.5.13.RELEASE`，`artifactId`改为`Feign-Consumer`，并引入下面这些依赖：

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-dependencies</artifactId>
            <version>Edgware.SR3</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
<dependencies>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-eureka</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-feign</artifactId>
    </dependency>
</dependencies>
```



除了`spring-cloud-starter-feign`依赖外，我们还引入了`spring-cloud-starter-eureka`，目的是为了从Eureka服务注册中心获取服务。

在Spring Boot的入口类中加入`@EnableFeignClients`和`@EnableDiscoveryClient`注解，用于开启Spring Cloud Feign和服务注册与发现：

```java
@EnableDiscoveryClient
@EnableFeignClients
@SpringBootApplication
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```



在前面几节中，我们曾在服务提供者Eureka-Client中定义了一个UserController，代码如下所示：

```java
@RestController
@RequestMapping("user")
public class UserController {
    private Logger log = LoggerFactory.getLogger(this.getClass());

    @GetMapping("/{id:\\d+}")
    public User get(@PathVariable Long id) {
        log.info("获取用户id为 " + id + "的信息");
        return new User(id, "mrbird", "123456");
    }

    @GetMapping
    public List<User> get() {
        List<User> list = new ArrayList<>();
        list.add(new User(1L, "mrbird", "123456"));
        list.add(new User(2L, "scott", "123456"));
        log.info("获取用户信息 " + list);
        return list;
    }

    @PostMapping
    public void add(@RequestBody User user) {
        log.info("新增用户成功 " + user);
    }

    @PutMapping
    public void update(@RequestBody User user) {
        log.info("更新用户成功 " + user);
    }

    @DeleteMapping("/{id:\\d+}")
    public void delete(@PathVariable Long id) {
        log.info("删除用户成功 " + id);
    }
}
```



在Spring Cloud Ribbon中访问这些服务需要通过`RestTemplate`对象来实现，并且参数绑定的过程也比较繁琐。Spring Cloud Feign对这个步骤进行了进一步的封装，在Feign Consumer中调用这些服务只需要定义一个UserService接口，然后将UserController中的代码复制过并将方法体去掉即可，如：

```java
@FeignClient("Server-Provider")
public interface UserService {

    @GetMapping("user/{id}")
    public User get(@PathVariable("id") Long id);

    @GetMapping("user")
    public List<User> get();

    @PostMapping("user")
    public void add(@RequestBody User user);

    @PutMapping("user")
    public void update(@RequestBody User user);

    @DeleteMapping("user/{id}")
    public void delete(@PathVariable("id") Long id);
}
```



对比Feign Consumer中的UserService和Eureka-Client中UserController代码，两者是不是很相似？

在UserService中，我们通过`@FeignClient("Server-Provider")`注解来获取我们需要的服务，其中`Server-Provider`不区分大小写。需要注意的是，在定义各参数绑定时，`@RequestParam`、`@RequestHeader`等可 以指定参数名称的注解，它们的value千万不能少。在SpringMVC 程序中，这些注解会根据参数名来作为默认值，但是在Feign中绑定参数必须通过value属性来指明具体的参数名，不然会抛出`illegalStateException`异常，value 属性不能为空。

接下来我们在Feign Consumer中定义一个TestController，来调用UserService中定义的服务：

```java
@RestController
public class TestController {

    @Autowired
    private UserService userService;

    @GetMapping("user/{id}")
    public User getUser(@PathVariable Long id) {
        return userService.get(id);
    }

    @GetMapping("user")
    public List<User> getUsers() {
        return userService.get();
    }

    @PostMapping("user")
    public void addUser() {
        User user = new User(1L, "mrbird", "123456");
        userService.add(user);
    }

    @PutMapping("user")
    public void updateUser() {
        User user = new User(1L, "mrbird", "123456");
        userService.update(user);
    }

    @DeleteMapping("user/{id}")
    public void deleteUser(@PathVariable Long id) {
        userService.delete(id);
    }
}
```



最后配置一下application.yml：

```yaml
server:
  port: 9000
  
spring:
  application:
    name: Server-Consumer
    
eureka:
  client:
    serviceUrl:
      defaultZone: http://mrbird:123456@peer1:8080/eureka/,http://mrbird:123456@peer2:8081/eureka/
```



上面配置指定了Eureka服务注册中心的地址，用于获取服务。

最后我们分别启动以下服务：

1. 启动Eureka-Server集群，端口号为8080和8081；
2. 启动两个Eureka-Client，端口号为8082和8083；
3. 启动Feign-Consumer，端口号为9000。

多次访问http://localhost:9000/user/1服务，观察8082和8083服务的控制台：

```
2018-06-10 14:27:38.105  INFO 10120 --- [nio-8082-exec-8] c.e.demo.controller.UserController       : 获取用户id为 1的信息
2018-06-10 14:27:39.989  INFO 10120 --- [nio-8082-exec-7] c.e.demo.controller.UserController       : 获取用户id为 1的信息
2018-06-10 14:27:41.197  INFO 10120 --- [nio-8082-exec-6] c.e.demo.controller.UserController       : 获取用户id为 1的信息
2018-06-10 14:27:41.802  INFO 10120 --- [nio-8082-exec-5] c.e.demo.controller.UserController       : 获取用户id为 1的信息
2018-06-10 14:27:42.224  INFO 10120 --- [nio-8082-exec-4] c.e.demo.controller.UserController       : 获取用户id为 1的信息
2018-06-10 14:27:42.865  INFO 10120 --- [nio-8082-exec-3] c.e.demo.controller.UserController       : 获取用户id为 1的信息
2018-06-10 14:27:43.296  INFO 10120 --- [nio-8082-exec-2] c.e.demo.controller.UserController       : 获取用户id为 1的信息

2018-06-10 14:27:38.358  INFO 9104 --- [nio-8083-exec-8] c.e.demo.controller.UserController       : 获取用户id为 1的信息
2018-06-10 14:27:40.754  INFO 9104 --- [nio-8083-exec-7] c.e.demo.controller.UserController       : 获取用户id为 1的信息
2018-06-10 14:27:41.397  INFO 9104 --- [nio-8083-exec-6] c.e.demo.controller.UserController       : 获取用户id为 1的信息
2018-06-10 14:27:42.006  INFO 9104 --- [nio-8083-exec-5] c.e.demo.controller.UserController       : 获取用户id为 1的信息
2018-06-10 14:27:42.445  INFO 9104 --- [nio-8083-exec-4] c.e.demo.controller.UserController       : 获取用户id为 1的信息
2018-06-10 14:27:43.073  INFO 9104 --- [nio-8083-exec-3] c.e.demo.controller.UserController       : 获取用户id为 1的信息

```



使用Feign实现的消费者，依然是利用Ribbon维护了针对Seriver-Provider的服务列表信息，并且通过轮询实现了客户端负载均衡。而与Ribbon不同的是，通过Feign我们只需定义服务绑定接口，以声明式的方法，优雅而简单地实现了服务调用。

## Ribbon相关配置

Spring Cloud Feign内部的客户端负载均衡是通过Ribbon来实现的，所以在Spring Cloud Feign中配置Ribbon，和之前在[Spring Cloud Ribbon客户端负载均衡](https://mrbird.cc/Spring-Cloud-Ribbon-LoadBalance.html)中介绍的Spring Cloud Ribbon配置一样，这里不再赘述。

## Hystrix相关配置

要在Spring Cloud Feign中开启Hystrix，可以在yml中添加如下配置：

```java
feign:
  hystrix:
    enabled: true
```



剩下的Hystrix配置和之前在[Spring Cloud Hystrix服务容错](https://mrbird.cc/Spring-Cloud-Hystrix-Circuit-Breaker.html)中介绍的Hystrix属性配置一样。

在Spring Cloud Feign中配置服务降级和在Spring Cloud Hystrix中配置服务降级区别很大，下面具体来看下怎么在Feign-Consumer中配置服务降级。

定义一个用于处理服务降级方法的类UserServiceFallback，并且实现上面定义的UserService接口：

```java
@Component
public class UserServiceFallback implements UserService {
    private Logger log = LoggerFactory.getLogger(this.getClass());

    @Override
    public User get(Long id) {
        return new User(-1L, "default", "123456");
    }

    @Override
    public List<User> get() {
        return null;
    }

    @Override
    public void add(User user) {
        log.info("test fallback");
    }

    @Override
    public void update(User user) {
        log.info("test fallback");
    }

    @Override
    public void delete(Long id) {
        log.info("test fallback");
    }
}
```



在UserService的中通过`@FeignClient`注解的`fallback`属性来指定对应的服务降级实现类:

```java
@FeignClient(value = "Server-Provider", fallback = UserServiceFallback.class)
public interface UserService {
    ...
}
```



重启Feign-Consumer，并关闭Eureka Client服务，访问http://localhost:9000/user/1，由于Eureka-Client服务提供者都关闭了，所以这里会直接触发服务降级，响应结果如下：

![QQ截图20180720164914.png](https://mrbird.cc/img/QQ%E6%88%AA%E5%9B%BE20180720164914.png)

可看到响应信息为服务降级方法中的返回结果。

## 其余Feign配置

除了Ribbon和Hystrix配置之外，Feign也有一些自个儿的配置。

### 请求压缩

Spring Cloud Feign支持对请求与响应进行GZIP压缩，以减少通信过程中的性能损耗：

```yaml
feign:
  compression:
    request:
      enabled: true
    response:
      enabled: true
```



同时，我们还能对请求压缩做一些更细致的设置，比如下面的配置内容指定了压缩的请求数据类型，并设置了请求压缩的大小下限，只有超过这个大小的请求才会对其进行压缩:

```yaml
feign:
  compression:
    request:
      enabled: true
      mime-types: text/xml,application/xml,application/json
      min-request-size: 2048
```



### 日志配置

Feign提供了日志打印的功能，Feign的日志级别分为四种：

- `NONE`: 不记录任何信息。
- `BASIC`: 仅记录请求方法、`URL`以及响应状态码和执行时间。
- `HEADERS`: 除了记录`BASIC`级别的信息之外，还会记录请求和响应的头信息。
- `FULL`: 记录所有请求与响应的明细，包括头信息、请求体、元数据等。

日志级别默认为`NONE`，要改变级别可以在入口类中定义一个日志配置Bean：

```java
@EnableDiscoveryClient
@EnableFeignClients
@SpringBootApplication
public class DemoApplication {
    @Bean
    Logger.Level feignLoggerLevel() {
        return Logger.Level.FULL;
    }
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```



然后在yml中配置Feign客户端的日志级别为debug，Feign日志记录仅响应debug级别：

```yaml
logging:
  level:
    com:
      example:
        demo:
          service:
            UserService: debug
```



重启项目访问，可以看到控制台打印日志如下：

```
[UserService#get] <--- HTTP/1.1 200 (506ms)
[UserService#get] content-type: application/json;charset=UTF-8
[UserService#get] date: Stu, 10 Jun 2018 01:44:45 GMT
[UserService#get] transfer-encoding: chunked
[UserService#get] 
[UserService#get] {"id":1,"username":"mrbird","password":"123456"}
[UserService#get] <--- END HTTP (48-byte body)
Flipping property: Server-Provider.ribbon.ActiveConnectionsLimit to use NEXT property: niws.loadbalancer.availabilityFilteringRule.activeConnectionsLimit = 2147483647

```