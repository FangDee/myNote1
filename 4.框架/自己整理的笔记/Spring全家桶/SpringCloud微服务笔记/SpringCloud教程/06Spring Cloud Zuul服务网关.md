## Spring Cloud Zuul服务网关

 2018-06-14 |  Visit count 645008

在微服务的架构中，服务网关就是一个介于客户端与服务端之间的中间层。在这种情况下，客户端只需要跟服务网关交互，无需调用具体的微服务接口。这样的好处在于，客户端可以降低复杂性；对于需要认证的服务，只需要在服务网关配置即可；同样也方便后期微服务的变更和重构，即微服务接口变更只需在服务网关调整配置即可，无需更改客户端代码。

[Zuul](https://github.com/Netflix/zuul)是一款由Netflix开发的微服务网关开源软件，可以和其自家开发的Eureka，Ribbon和Hystrix配合使用，Spring Cloud对其进行了封装。



## Spring Cloud Zuul入门

在构建微服务网关之前，我们先将相关微服务搭建好。这里我们直接使用前面的例子，分别启动如下服务：

1. 启动Eureka-Server集群，端口号为8080和8081；
2. 启动一个Eureka-Client，端口号为8082；
3. 启动一个Feign-Consumer，端口号为9000；

查看http://localhost:8080/：

![QQ截图20180725103826.png](https://mrbird.cc/img/QQ%E6%88%AA%E5%9B%BE20180725103826.png)

其中Feign-Consumer虽然之前我们将它定义为服务消费者，但其也可以充当服务提供者的角色。

启动好这些微服务后，我们开始构建一个微服务网关Zuul-Gateway。新建一个Spring Boot项目，`artifactId`为Zuul-Gateway，然后引入如下依赖：

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
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-zuul</artifactId>
    </dependency>
</dependencies>
```



查看依赖关系可看到spring-cloud-starter-zuul包含以下依赖：

![QQ截图20180725095018.png](https://mrbird.cc/img/QQ%E6%88%AA%E5%9B%BE20180725095018.png)

因为spring-cloud-starter-zuul依赖已经包含了Hystrix和Ribbon，所以zuul支持前面介绍的Hystrix和Ribbon相关配置。

接着在Spring Boot的入口类上标注`@EnableZuulProxy`注解，开启Zuul服务网关的功能：

```java
@EnableZuulProxy
@SpringBootApplication
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```



最后简单配置下application.yml：

```yaml
pring:
  application:
    name: Zuul-Gateway
server:
  port: 12580
```



至此一个简单的Zuul微服务网关就搭建好了，下面开始对路由规则进行配置。

## 路由配置

### 传统配置

传统配置就是手动指定服务的转发地址，如在yml中配置：

```yaml
zuul:
  routes:
    api-a:
      path: /api-a/**
      url: http://localhost:8082
```



通过上面的配置，所有符合`/api-a/**`规则的访问都将被路由转发到`http://localhost:8082/`地址上，即当我们向服务网关访问http://localhost:12580/api-a/hello请求的时候，请求将被转发到http://localhost:8082/hello服务上：

![QQ截图20180725113200.png](https://mrbird.cc/img/QQ%E6%88%AA%E5%9B%BE20180725113200.png)

### 基于服务名称配置

传统的配置方式不便之处在于需要知道服务的具体地址和端口号等信息，我们可以借助Eureka来实现通过服务名称配置路由。在Zuul-Gateway项目中引入Eureka依赖：

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-eureka</artifactId>
</dependency>
```



然后在入口类中加入`@EnableDiscoveryClient`注解，使其具有获取服务的能力。

接着在yml中配置基于服务名称的路由：

```yaml
eureka:
  client:
    serviceUrl:
      defaultZone: http://mrbird:123456@peer1:8080/eureka/,http://mrbird:123456@peer2:8081/eureka/
zuul:
  routes:
    api-b:
      path: /api-b/**
      serviceId: server-provider
    api-c:
      path: /api-c/**
      serviceId: server-consumer
```



上面我们首先指明了Eureka服务注册中心的地址，然后配置了`api-b`和`api-c`的路由，它们都是通过serviceId来指定服务名称的。

启动Zuul-Gateway，访问：http://localhost:12580/api-b/hello：

![QQ截图20180725140112.png](https://mrbird.cc/img/QQ%E6%88%AA%E5%9B%BE20180725140112.png)

访问：http://localhost:12580/api-c/user/1：

![QQ截图20180725140208.png](https://mrbird.cc/img/QQ%E6%88%AA%E5%9B%BE20180725140208.png)

基于服务名称的路由配置还可以进行简化，格式为`zuul.routes.=`，所以我们可以将`api-b`和`api-c`的路由配置修改为：

```yaml
zuul:
  routes:
    server-provider:
      path: /api-b/**
    server-consumer:
      path: /api-c/**
```



### 默认路由配置规则

我们尝试访问http://localhost:12580/server-consumer/user/1:

![QQ截图20180725142514.png](https://mrbird.cc/img/QQ%E6%88%AA%E5%9B%BE20180725142514.png)

我们压根没配置这个路由啊，为什么可以正常响应？？其实Zuul配合Eureka后将会成一套默认的配置规则。当我们使用服务名称作为请求的前缀路径时，实际上就会匹配上类似下面的默认路由配置：

```yaml
zuul:
  routes:
    server-consumer:
      path: /server-consumer/**
      serviceId: server-consumer
```



如果不想启动这个默认配置，我们可以使用`zuul.ignored-services`配置来关闭，如在yml中加入如下配置之后，再次访问http://localhost:12580/server-consumer/user/1将返回404：

```yaml
zuul:
  ignored-services: server-consumer
```



设置为`zuul.ignored-services=*`的时候将关闭所有默认路由配置规则。

### 优先级

假如某个请求路径可以和多个路由配置规则相匹配的话，Zuul根据匹配的先后顺序来决定最终使用哪个路由配置。比如：

```yaml
zuul:
  routes:
    api-c:
      path: /api-c/**
      serviceId: server-consumer
    api-d:
      path: /api-c/user/1
      serviceId: lol
```



当我们访问http://localhost:12580/api-c/user/1的时候，`api-c`和`api-d`的路由配置都可以匹配上，但由于`api-c`先于`api-d`配置，所以最终生效的是`api-c`的配置。

假如将`api-c`和`api-d`的配置顺序调换，再次访问http://localhost:12580/api-c/user/1时将抛出异常，原因是不存在服务名为lol的服务：

Caused by: com.netflix.client.ClientException: Load balancer does not have available server for client: lol

### 前缀配置

`zuul.prefix`可以为网关的请求路径加个前缀，比如：

```yaml
zuul:
  prefix: /gateway
```



这样配置后，我们通过Zuul网关获取服务的时候，路径也得加上这个前缀，如`http://localhost:12580/gateway/api-c/user/1`。

### 本地跳转

Zuul网关除了支持将服务转发到各个微服务上之外，还支持将服务跳转到网关本身的服务上，比如现在yml中有如下一段配置:

```yaml
zuul:
  routes:
    api-e:
      path: /api-e/**
      url: forward:/test
```



当访问http://localhost:12580/gateway/api-e/hello时，Zuul会从本地[/test/hello](https://mrbird.cc/test/hello)获取服务。

我们在Zuul-Gateway入口类中加上该REST服务：

```java
@EnableZuulProxy
@EnableDiscoveryClient
@SpringBootApplication
@RestController
public class DemoApplication {
    @GetMapping("/test/hello")
    public String hello() {
        return "hello zuul";
    }
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```



效果如下所示：

![QQ截图20180725161331.png](https://mrbird.cc/img/QQ%E6%88%AA%E5%9B%BE20180725161331.png)

## 头部过滤 & 重定向

在使用Zuul网关的时候你可能会遇到Cookie丢失的情况，这是因为默认情况下Zuul会过滤掉HTTP请求头中的一些敏感信息，这些敏感信息通过下面的配置设定：

```yaml
zuul:
  sensitive-headers: Cookie,Set-Cookie,Authorization
```



如果想关闭这个默认配置，通过设置全局参数为空来覆盖默认值：

```yaml
zuul:
  sensitive-headers:
```



如果只想关闭某个路由的HTTP请求头过滤，可以这样：

```yaml
zuul:
 routes:
   api-a:
     sensitive-headers:
```



使用Zuul另一个常见问题是重定向的问题，可以通过下面的设置解决：

```yaml
zuul:
  add-host-header: true
```



## 过滤器

Zuul另一个核心的功能就是请求过滤。Zuul中默认定义了4种不同生命周期的过滤器类型，在如下图所示

![img](https://mrbird.cc/img/zuul.png)

图片来自于 Zuul GitHub

这4种过滤器处于不同的生命周期，所以其职责也各不相同：

- **PRE**：PRE过滤器用于将请求路径与配置的路由规则进行匹配，以找到需要转发的目标地址，并做一些前置加工，比如请求的校验等；
- **ROUTING**：ROUTING过滤器用于将外部请求转发到具体服务实例上去；
- **POST**：POST过滤器用于将微服务的响应信息返回到客户端，这个过程种可以对返回数据进行加工处理；
- **ERROR**：上述的过程发生异常后将调用ERROR过滤器。ERROR过滤器捕获到异常后需要将异常信息返回给客户端，所以最终还是会调用POST过滤器。

### 核心过滤器

Spring Cloud Zuul为各个生命周期阶段实现了一批过滤器，如下所示：

![QQ截图20180921151818.png](https://mrbird.cc/img/QQ%E6%88%AA%E5%9B%BE20180921151818.png)

这些过滤器的优先级和作用如下表所示：

| 生命周期 | 优先级 | 过滤器                  | 功能描述                   |
| :------- | :----- | :---------------------- | :------------------------- |
| pre      | -3     | ServletDetectionFilter  | 标记处理Servlet的类型      |
| pre      | -2     | Servlet30WrapperFilter  | 包装HttpServletRequest请求 |
| pre      | -1     | FormBodyWrapperFilter   | 包装请求体                 |
| route    | 1      | DebugFilter             | 标记调试标志               |
| route    | 5      | PreDecorationFilter     | 处理请求上下文供后续使用   |
| route    | 10     | RibbonRoutingFilter     | serviceId请求转发          |
| route    | 100    | SimpleHostRoutingFilter | url请求转发                |
| route    | 500    | SendForwardFilter       | forward请求转发            |
| post     | 0      | SendErrorFilter         | 处理有错误的请求响应       |
| post     | 1000   | SendResponseFilter      | 处理正常的请求响应         |

其中优先级数字越小，优先级越高。

要关闭这些过滤器可以在application.yml中按照格式`zuul...disable=true`配置即可。比如关闭`SendResponseFilter`过滤器：

```yaml
zuul:
  SendResponseFilter:
    post:
      disable:
        true
```



### 自定义Zuul过滤器

我们自定义一个`PreSendForwardFilter`用于获取请求转发前的一些信息：

```java
import com.netflix.zuul.ZuulFilter;

public class PreSendForwardFilter extends ZuulFilter {
    @Override
    public String filterType() {
        return null;
    }

    @Override
    public int filterOrder() {
        return 0;
    }

    @Override
    public boolean shouldFilter() {
        return false;
    }

    @Override
    public Object run() {
        return null;
    }
}
```



可以看到自定义Zuul过滤器只需要继承`ZuulFilter`，然后实现以下四个抽象方法即可：

1. **filterType**：对应Zuul生命周期的四个阶段：pre、post、route和error；
2. **filterOrder**：过滤器的优先级，数字越小，优先级越高；
3. **shouldFilter**：方法返回boolean类型，true时表示是否执行该过滤器的`run`方法，false则表示不执行；
4. **run**：过滤器的过滤逻辑。

继续完善`PreSendForwardFilter`：

```java
@Component
public class PreSendForwardFilter extends ZuulFilter {
    private Logger log = LoggerFactory.getLogger(this.getClass());

    @Override
    public String filterType() {
        return "pre";
    }

    @Override
    public int filterOrder() {
        return 1;
    }

    @Override
    public boolean shouldFilter() {
        return true;
    }

    @Override
    public Object run() {
        RequestContext requestContext = RequestContext.getCurrentContext();
        HttpServletRequest request = requestContext.getRequest();
        String host = request.getRemoteHost();
        String method = request.getMethod();
        String uri = request.getRequestURI();
        log.info("请求URI：{}，HTTP Method：{}，请求IP：{}", uri, method, host);
        return null;
    }
}
```



这时候访问http://localhost:12580/api-a/hello，控制台将打印出：

```
c.e.demo.filter.PreSendForwardFilter     : 请求URI：/api-a/hello，HTTP Method：GET，请求IP：0:0:0:0:0:0:0:1
```