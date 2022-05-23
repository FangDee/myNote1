## Spring Cloud Ribbon客户端负载均衡

 2018-06-03 |  Visit count 644988

为了提高服务的可用性，我们一般会将相同的服务部署多个实例，负载均衡的作用就是使获取服务的请求被均衡的分配到各个实例中。负载均衡一般分为**服务端负载均衡**和**客户端负载均衡**，服务端的负载均衡通过硬件（如F5）或者软件（如Nginx）来实现，而Ribbon实现的是客户端负载均衡。服务端负载均衡是在硬件设备或者软件模块中维护一份可用服务清单，然后客户端发送服务请求到这些负载均衡的设备上，这些设备根据一些算法均衡的将请求转发出去。而客户端负载均衡则是客户端自己从服务注册中心（如之前提到的Eureka Server）中获取服务清单缓存到本地，然后通过Ribbon内部算法均衡的去访问这些服务。

## Ribbon简介

Ribbon是由[Netflix](https://github.com/Netflix)开发的一款基于HTTP和TCP的负载均衡的开源软件。我们可以直接给Ribbon配置好服务列表清单，也可以配合Eureka主动的去获取服务清单，需要使用到这些服务的时候Ribbon通过轮询或者随机等均衡算法去获取服务。

在[Spring Cloud Eureka服务治理](https://mrbird.cc/Spring-Cloud-Eureka.html)一节中，我们已经在Server-Consumer中配置了Ribbon，并通过加了`@LoadBalanced`注解的RestTemplate对象去均衡的消费服务，所以这节主要记录的是RestTemplate的详细使用方法和一些额外的Ribbon配置。

## RestTemplate详解

从名称上来看就可以知道它是一个用来发送REST请求的摸板，所以包含了GET,POST,PUT,DELETE等HTTP Method对应的方法。

### 发送Get请求

RestTemplate中与GET请求对应的方法有`getForEntity`和`getForObject`。

**getForEntity**

`getForEntity`方法返回`ResponseEntity`对象，该对象包含了返回报文头，报文体和状态码等信息。`getForEntity`有三个重载方法：

1. `getForEntity(String url, Class responseType, Object... uriVariables)`；
2. `getForEntity(String url, Class responseType, Map uriVariables)`；
3. `getForEntity(URI url, Class responseType)`；

第一个参数为Url，第二个参数为返回值的类型，第三个参数为请求的参数（可以是数组，也可以是Map）。

举个`getForEntity(String url, Class responseType, Object... uriVariables)`的使用例子：

```java
@GetMapping("user/{id:\\d+}")
public User getUser(@PathVariable Long id) {
    return this.restTemplate.getForEntity("http://Server-Provider/user/{name}", User.class, id).getBody();
}
```



`{1}`为参数的占位符，匹配参数数组的第一个元素。因为第二个参数指定了类型为User，所以调用`getBody`方法返回类型也为User。

方法参数除了可以放在数组里外，也可以放在Map里，举个`getForEntity(String url, Class responseType, Map uriVariables)`使用例子：

```java
@GetMapping("user/{id:\\d+}")
public User getUser(@PathVariable Long id) {
    Map<String, Object> params = new HashMap<>();
    params.put("id", id);
    return this.restTemplate.getForEntity("http://Server-Provider/user/{id}", User.class, params).getBody();
}
```



只有两个参数的重载方法`getForEntity(URI url, Class responseType)`第一个参数接收`java.net.URI`类型，可以通过`org.springframework.web.util.UriComponentsBuilder`来创建，举个该方法的使用例子：

```java
@GetMapping("user/{id:\\d+}")
public User getUser(@PathVariable Long id) {
    Map<String, Object> params = new HashMap<>();
    params.put("id", id);
    URI uri = UriComponentsBuilder.fromUriString("http://Server-Provider/user/{id}")
            .build().expand(params).encode().toUri();
    return this.restTemplate.getForEntity(uri, User.class).getBody();
}
```



其中`expand`方法也可以接收数组和Map两种类型的参数。

**getForObject**

`getForObject`方法和`getForEntity`方法类似，`getForObject`方法相当于`getForEntity`方法调用了`getBody`方法，直接返回结果对象，为不是`ResponseEntity`对象。

`getForObject`方法和`getForEntity`方法一样，也有三个重载方法，参数类型和`getForEntity`方法一致，所以不再列出。

### 发送POST请求

使用RestTemplate发送POST请求主要有`postForEntity`，`postForObject`和`postForLocation`（这个目前较少使用，所以不做介绍）三个方法。

`postForEntity`和`postForObject`也分别有三个重载方法，方法参数和使用方式和上面介绍的`getForEntity`和`getForObject`一样，所以不再详细介绍。简单举个`getForObject`的使用例子：

```java
 @GetMapping("user")
public List<User> getUsers() {
    return this.restTemplate.getForObject("http://Server-Provider/user", List.class);
}
```



### 发送PUT请求

使用RestTemplate发送PUT请求，使用的是它的`put`方法，`put`方法返回值是`void`类型，该方法也有三个重载方法：

1. `put(String url, Object request, Object... uriVariables)`；
2. `put(String url, Object request, Map uriVariables)`；
3. `put(URI url, Object request)`。

举个例子：

```java
@GetMapping("user/update")
public void updateUser() throws JsonProcessingException {
    User user = new User(1L, "mrbird", "123456");
    this.restTemplate.put("http://Server-Provider/user", user);
}
```



在RESTful风格的接口中，判断成功失败不再是通过返回值的某个标识来判断的，而是通过返回报文的状态码是否为200来判断。当这个方法成功执行并返回时，返回报文状态为200，即可判断方法执行成功。

### 发送DELETE请求

使用RestTemplate发送DELETE请求，使用的是它的`delete`方法，`delete`方法返回值是`void`类型，该方法也有三个重载方法：

1. `delete(String url, Object... uriVariables)`；
2. `delete(String url, Map uriVariables)`;
3. `delete(URI url)`。

举个例子：

```java
@@GetMapping("user/delete/{id:\\d+}")
public void deleteUser(@PathVariable Long id) {
    this.restTemplate.delete("http://Server-Provider/user/{1}", id);
}
```



## RestTemplates实战

我们在[Spring Cloud Eureka服务治理](https://mrbird.cc/Spring-Cloud-Eureka.html)中的Eureka客户端（Server-Provider）中编写一套RESTful风格的测试接口：

```
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



User对象代码：

```
public class User implements Serializable {
    private static final long serialVersionUID = 1339434510787399891L;
    private Long id;
    private String username;
    private String password;

    public User() {
    }

    public User(Long id, String username, String password) {
        this.id = id;
        this.username = username;
        this.password = password;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", password='" + password + '\'' +
                '}';
    }
    // get,set略
}
```



需要注意的是，User对象必须有默认的构造方法，否则在JSON与实体对象转换的时候会抛出如下异常：

JSON parse error: Can not construct instance of model.Class: no suitable constructor found



然后在Server-Consumer中使用RestTemplates分别去获取这些服务：

```
@RestController
public class TestController {

    @Autowired
    private RestTemplate restTemplate;

    @GetMapping("user/{id:\\d+}")
    public User getUser(@PathVariable Long id) {
        Map<String, Object> params = new HashMap<>();
        params.put("id", id);
        URI uri = UriComponentsBuilder.fromUriString("http://Server-Provider/user/{id}")
                .build().expand(params).encode().toUri();
        return this.restTemplate.getForEntity(uri, User.class).getBody();
    }

    @GetMapping("user")
    public List<User> getUsers() {
        return this.restTemplate.getForObject("http://Server-Provider/user", List.class);
    }

    @GetMapping("user/add")
    public String addUser() {
        User user = new User(1L, "mrbird", "123456");
        HttpStatus status = this.restTemplate.postForEntity("http://Server-Provider/user", user, null).getStatusCode();
        if (status.is2xxSuccessful()) {
            return "新增用户成功";
        } else {
            return "新增用户失败";
        }
    }

    @GetMapping("user/update")
    public void updateUser() {
        User user = new User(1L, "mrbird", "123456");
        this.restTemplate.put("http://Server-Provider/user", user);
    }

    @GetMapping("user/delete/{id:\\d+}")
    public void deleteUser(@PathVariable Long id) {
        this.restTemplate.delete("http://Server-Provider/user/{1}", id);
    }
}
```



我们分别启动两个Eureka Server用于集群，两个Eureka Client（Server-Provider）实例，然后启动Server-Consumer。

使用Restlet Client访问http://localhost:9000/user/1（后面每个方法我们都访问两次，用于观察负载均衡），返回结果如下：

![QQ截图20180705173808.png](https://mrbird.cc/img/QQ%E6%88%AA%E5%9B%BE20180705173808.png)

剩下的方法测试结果这里不贴出来了，当我们分别访问下面的链接后：

- http://localhost:9000/user/
- http://localhost:9000/user/add
- http://localhost:9000/user/update
- http://localhost:9000/user/delete/1

查看Eureka客户端8082和8083的后台日志：

```
2018-06-03 18:17:26.231  INFO 11188 --- [           main] .s.c.n.e.s.EurekaAutoServiceRegistration : Updating port to 8083
2018-06-03 18:17:26.236  INFO 11188 --- [           main] com.example.demo.DemoApplication         : Started DemoApplication in 52.252 seconds (JVM running for 54.321)
2018-06-03 18:21:29.097  INFO 11188 --- [io-8083-exec-10] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring FrameworkServlet 'dispatcherServlet'
2018-06-03 18:21:29.098  INFO 11188 --- [io-8083-exec-10] o.s.web.servlet.DispatcherServlet        : FrameworkServlet 'dispatcherServlet': initialization started
2018-06-03 18:21:29.177  INFO 11188 --- [io-8083-exec-10] o.s.web.servlet.DispatcherServlet        : FrameworkServlet 'dispatcherServlet': initialization completed in 79 ms
2018-06-03 18:21:29.312  INFO 11188 --- [io-8083-exec-10] c.e.demo.controller.UserController       : 获取用户id为 1的信息
2018-06-03 18:21:50.798  INFO 11188 --- [nio-8083-exec-9] c.e.demo.controller.UserController       : 获取用户信息 [User{id=1, username='mrbird', password='123456'}, User{id=2, username='scott', password='123456'}]
2018-06-03 18:22:25.351  INFO 11188 --- [trap-executor-0] c.n.d.s.r.aws.ConfigClusterResolver      : Resolving eureka endpoints via configuration
2018-06-03 18:22:44.718  INFO 11188 --- [nio-8083-exec-8] c.e.demo.controller.UserController       : 新增用户成功 User{id=1, username='mrbird', password='123456'}
2018-06-03 18:24:34.313  INFO 11188 --- [nio-8083-exec-6] c.e.demo.controller.UserController       : 删除用户成功 1


2018-06-03 18:17:21.296  INFO 16188 --- [           main] .s.c.n.e.s.EurekaAutoServiceRegistration : Updating port to 8082
2018-06-03 18:17:21.303  INFO 16188 --- [           main] com.example.demo.DemoApplication         : Started DemoApplication in 57.152 seconds (JVM running for 58.239)
2018-06-03 18:21:27.517  INFO 16188 --- [io-8082-exec-10] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring FrameworkServlet 'dispatcherServlet'
2018-06-03 18:21:27.517  INFO 16188 --- [io-8082-exec-10] o.s.web.servlet.DispatcherServlet        : FrameworkServlet 'dispatcherServlet': initialization started
2018-06-03 18:21:27.567  INFO 16188 --- [io-8082-exec-10] o.s.web.servlet.DispatcherServlet        : FrameworkServlet 'dispatcherServlet': initialization completed in 50 ms
2018-06-03 18:21:27.732  INFO 16188 --- [io-8082-exec-10] c.e.demo.controller.UserController       : 获取用户id为 1的信息
2018-06-03 18:21:49.639  INFO 16188 --- [nio-8082-exec-9] c.e.demo.controller.UserController       : 获取用户信息 [User{id=1, username='mrbird', password='123456'}, User{id=2, username='scott', password='123456'}]
2018-06-03 18:22:12.313  INFO 16188 --- [trap-executor-0] c.n.d.s.r.aws.ConfigClusterResolver      : Resolving eureka endpoints via configuration
2018-06-03 18:22:46.111  INFO 16188 --- [nio-8082-exec-8] c.e.demo.controller.UserController       : 新增用户成功 User{id=1, username='mrbird', password='123456'}
2018-06-03 18:23:55.732  INFO 16188 --- [nio-8082-exec-6] c.e.demo.controller.UserController       : 更新用户成功 User{id=1, username='mrbird', password='123456'}
2018-06-03 18:23:58.297  INFO 16188 --- [nio-8082-exec-5] c.e.demo.controller.UserController       : 更新用户成功 User{id=1, username='mrbird', password='123456'}
2018-06-03 18:24:37.266  INFO 16188 --- [nio-8082-exec-3] c.e.demo.controller.UserController       : 删除用户成功 1
2018-06-03 18:27:12.314  INFO 16188 --- [trap-executor-0] c.n.d.s.r.aws.ConfigClusterResolver      : Resolving eureka endpoints via configuration
```



发现方法都成功调用，并且是均衡的。

## Spring Cloud Ribbon配置

Spring Cloud Ribbon的配置分为全局和指定服务名称。比如我要指定全局的服务请求连接超时时间为200毫秒：

```yaml
ribbon:
  ConnectTimeout: 200
```



如果只是设置获取Server Provider服务的请求连接超时时间，我们只需要在配置最前面加上服务名称就行了，如：

```yaml
Server-Provider:
  ribbon:
    ConnectTimeout: 200
```



设置获取Server-Provider服务的负载均衡算法从轮询改为随机：

```yaml
Server-Provider:
  ribbon:
    NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RandomRule
```



设置处理Server-Provider服务的超时时间：

```yaml
Server-Provider:
  ribbon:
    ReadTimeout: 1000
```



开启重试机制，即获取服务失败是否从另外一个节点重试，默认值为false：

```yaml
spring:
  cloud:
    loadbalancer:
      retry:
        enabled: true
```



对Server-Provider的所有请求在失败的时候都进行重试：

```yaml
Server-Provider:
  ribbon:
    OkToRetryOnAllOperations: true
```



切换Server-Provider实例的重试次数：

```yaml
Server-Provider:
  ribbon:
    MaxAutoRetriesNextServer: 1
```



对Server-Provider当前实例的重试次数：

```yaml
Server-Provider:
  ribbon:
    MaxAutoRetries: 1
```



根据如上配置当访问Server-Provider服务实例（比如是8082）遇到故障的时候，Ribbon会再尝试访问一次当前实例（次数由MaxAutoRetries配置），如果不行，就换到8083实例进行访问（更换次数由 MaxAutoRetriesNextServer决定），如果还是不行，那就GG思密达，返回失败。

如果不和Eureka搭配使用的话，我们就需要手动指定服务清单给Ribbon：

```yaml
Server-Provider:
  ribbon:
    listOfServers: localhost:8082,localhost:8083
```



上面配置了名称为Server-Provider的服务，有两个节点可供使用（8082和8083）。

源码链接：https://github.com/wuyouzhuguli/SpringAll/tree/master/29.Spring-Cloud-Ribbon-LoadBalance