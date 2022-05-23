## Spring Cloud Eureka服务治理

 2018-06-01 |  Visit count 644944

考虑当前有两个微服务实例A和B，A服务需要调用B服务的某个REST接口。假如某一天B服务迁移到了另外一台服务器，IP和端口也发生了变化，这时候我们不得不去修改A服务中调用B服务REST接口的静态配置。随着公司业务的发展，微服务的数量也越来越多，服务间的关系可能变得非常复杂，传统的微服务维护变得愈加困难，也很容易出错。所谓服务治理就是用来实现各个微服务实例的自动化注册与发现，在这种模式下，服务间的调用不再通过指定具体的实例地址来实现，而是通过向服务注册中心获取服务名并发起请求调用实现。

Eureka是由[Netflix](https://github.com/Netflix)开发的一款服务治理开源框架，Spring-cloud对其进行了集成。Eureka既包含了服务端也包含了客户端，Eureka服务端是一个**服务注册中心(Eureka Server)**，提供服务的注册和发现，即当前有哪些服务注册进来可供使用；Eureka客户端为**服务提供者(Server Provider)**，它将自己提供的服务注册到Eureka服务端，并周期性地发送心跳来更新它的服务租约，同时也能从服务端查询当前注册的服务信息并把它们缓存到本地并周期性地刷新服务状态。这样**服务消费者(Server Consumer)**便可以从服务注册中心获取服务名称，并消费服务。

三者关系如下图所示:

![687474703a2f2f6e6f74652e796f7564616f2e636f6d2f7977732f6170692f706572736f6e616c2f66696c652f33363341343841303839344634334330394143433739433042443237383335443f6d6574686f643d646f776e6c6f61642673686172654b65793d61.png](https://mrbird.cc/img/687474703a2f2f6e6f74652e796f7564616f2e636f6d2f7977732f6170692f706572736f6e616c2f66696c652f33363341343841303839344634334330394143433739433042443237383335443f6d6574686f643d646f776e6c6f61642673686172654b65793d61.png)

## 搭建Eureka-Server服务注册中心

说了那么多，我们先来搭建一个Eureka服务端来充当服务注册中心。

新建一个Spring Boot项目，`artifactId`填Eureka-Service，然后引入`Spring Cloud Edgware.SR3`和`spring-cloud-starter-eureka-server`:

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
        <artifactId>spring-cloud-starter-eureka-server</artifactId>
    </dependency>
</dependencies>
```



在启动类上添加`@EnableEurekaServer`注解，表明这是一个Eureka服务端：

```yaml
@EnableEurekaServer
@SpringBootApplication
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```



接着在application.yml中添加一些配置：

```yaml
server:
  port: 8080
  
eureka:
  instance:
    hostname: localhost
  client:
    register-with-eureka: false
    fetch-registry: false
    serviceUrl:
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
```



上面配置了服务的端口为8080，剩下几个为Eureka配置：

- `eureka.instance.hostname`指定了Eureka服务端的IP；
- `eureka.client.register-with-eureka`表示是否将服务注册到Eureka服务端，由于自身就是Eureka服务端，所以设置为false；
- `eureka.client.fetch-registry`表示是否从Eureka服务端获取服务信息，因为这里只搭建了一个Eureka服务端，并不需要从别的Eureka服务端同步服务信息，所以这里设置为false；
- `eureka.client.serviceUrl.defaultZone`指定Eureka服务端的地址，默认值为`http://localhost:8761/eureka`。

配置完毕后启动服务，访问http://localhost:8080/，可看到：

![QQ截图20180703185733.png](https://mrbird.cc/img/QQ%E6%88%AA%E5%9B%BE20180703185733.png)

由于还没有Eureka客户端将服务注册进来，所以Instances currently registered with Eureka列表是空的。

下面我们接着搭建一个Eureka客户端来提供服务。

## 搭建Eureka-Client服务提供者

新建一个Spring Boot项目，`artifactId`填Eureka-Client，然后引入以下依赖：

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
</dependencies>
```



接着编写一个TestController，对外提供一些REST服务：

```java
@RestController
public class TestController {
    private Logger log = LoggerFactory.getLogger(this.getClass());

    @Autowired
    private DiscoveryClient client;

    @GetMapping("/info")
    public String info() {
        @SuppressWarnings("deprecation")
        ServiceInstance instance = client.getLocalServiceInstance();
        String info = "host：" + instance.getHost() + "，service_id：" + instance.getServiceId();
        log.info(info);
        return info;
    }
    @GetMapping("/hello")
    public String hello() {
        return "hello world";
    }
}
```



上面代码注入了`org.springframework.cloud.client.discovery.DiscoveryClient`对象，可以获取当前服务的一些信息。

编写启动类，在启动类上加`@EnableDiscoveryClient`注解，表明这是一个Eureka客户端：

```java
@EnableDiscoveryClient
@SpringBootApplication
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```



最后配置application.yml：

```yaml
server:
  port: 8082
  
spring:
  application:
    name: Server-Provider
    
eureka:
  client:
    register-with-eureka: true
    fetch-registry: true
    serviceUrl:
      defaultZone: http://localhost:8080/eureka/
```



稍微说明下这些配置的意思：

- `server.port`指定了服务的端口为8082；
- `spring.application.name`指定服务名称为`Server-Provider`，后续服务消费者要获取上面TestController中接口的时候会用到这个服务名；
- `eureka.client.serviceUrl.defaultZone`指定Eureka服务端的地址，这里为上面定义的Eureka服务端地址；
- `eureka.client.register-with-eureka`和`eureka.client.fetch-registry`上面已经解释了其意思，虽然这两个配置的默认值就是true，但这里还是显式配置下，使Eureka客户端的功能更为直观（即向服务端注册服务并定时从服务端获取服务缓存到本地）。

配置好后，启动Eureka-Client，可以从控制台中看到注册成功的消息：

```
Registered Applications size is zero : true
Application version is -1: true
Getting all instance registry info from the eureka server
The response status is 200
Starting heartbeat executor: renew interval is: 30
InstanceInfoReplicator onDemand update allowed rate per min is 4
Discovery Client initialized at timestamp 1530667498944 with initial instances 
Registering application Server-Provider with eureka with status UP
Saw local status change event StatusChangeEvent [timestamp=1530667498949, current=UP, previous=STARTING] 
DiscoveryClient_SERVER-PROVIDER/192.168.73.109:Server-Provider:8082: registering service... 
DiscoveryClient_SERVER-PROVIDER/192.168.73.109:Server-Provider:8082 - registration status: 204
Tomcat started on port(s): 8082 (http)
Updating port to 8082
Started DemoApplication in 7.007 seconds (JVM running for 8.355)
```

第3，4行输出表示已经成功从Eureka服务端获取到了服务；第5行表示发送心跳给Eureka服务端，续约（renew）服务；第8到11行表示已经成功将服务注册到了Eureka服务端。

再次访问http://localhost:8080/，可看到服务列表里已经出现了名字为Server-provider的服务了：

![QQ截图20180704085804.png](https://mrbird.cc/img/QQ%E6%88%AA%E5%9B%BE20180704085804.png)

UP表示在线的意思（如果Eureka客户端正常关闭，那么这里的状态将变为DOWN），点击后面的链接[192.168.73.109:Server-Provider:8082](https://mrbird.cc/192.168.73.109:Server-Provider:8082)将访问该服务的`/info`接口：

![QQ截图20180704090853.png](https://mrbird.cc/img/QQ%E6%88%AA%E5%9B%BE20180704090853.png)

这时候关闭Eureka客户端，再次刷新http://localhost:8080/：

![QQ截图20180704091123.png](https://mrbird.cc/img/QQ%E6%88%AA%E5%9B%BE20180704091123.png)

可看到虽然Eureka客户端已经关闭了，但是Eureka服务端页面的服务服务列表中依然还有该服务，并且页面红色文字提示：

EMERGENCY! EUREKA MAY BE INCORRECTLY CLAIMING INSTANCES ARE UP WHEN THEY’RE NOT. RENEWALS ARE LESSER THAN THRESHOLD AND HENCE THE INSTANCES ARE NOT BEING EXPIRED JUST TO BE SAFE.

大致意思是Eureka已经进入了**保护模式**。微服务在部署之后可能由于网络问题造成Eureka客户端无法成功的发送心跳给Eureka服务端，这时候Eureka服务端认定Eureka客户端已经挂掉了，虽然实际上Eureka客户端还在正常的运行着。而保护模式就是为了解决这个问题，即当Eureka服务端在短时间内同时丢失了过多的Eureka客户端时，Eureka服务端会进入保护模式，不去剔除这些客户端。因为我们这里只部署了一个Eureka客户端服务，所以关闭客户端后满足“短时间内丢失过多Eureka客户端”的条件。

在开发中可以先将保护模式给关了，我们在Eureka服务端加上一条配置:

```
eureka:
  server:
    enable-self-preservation: false
```



## Eureka-Server集群

Eureka服务端充当了重要的角色，所有Eureka客户端都将自己提供的服务注册到Eureka服务端，然后供所有服务消费者使用。如果单节点的Eureka服务端宕机了，那么所有服务都无法正常的访问，这必将是灾难性的。为了提高Eureka服务端的可用性，我们一般会对其集群部署，即同时部署多个Eureka服务端，并且可以相互间同步服务。

在搭建Eureka服务端的时候我们曾把下面两个配置给关闭了：

```
eureka:
  client:
    register-with-eureka: false
    fetch-registry: false
```



实际上在Eureka集群模式中，开启这两个参数可以让当前Eureka服务端将自己也作为服务注册到别的Eureka服务端，并且从别的Eureka服务端获取服务进行同步。所以这里我们将这两个参数置为true（默认就是true），下面开始搭建Eureka服务端集群，为了简单起见这里只搭建两个节点的Eureka服务端集群。

在Eureka-Server项目的`src/main/resource`目录下新建application-peer1.yml，配置如下：

```yaml
server:
  port: 8080

spring:
  application:
    name: Eureka-Server

eureka:
  instance:
    hostname: peer1
  client:
    serviceUrl:
      defaultZone: http://peer2:8081/eureka/
  server:
    enable-self-preservation: false
```



`server.port=8080`指定端口为8080；`spring.application.name=Eureka-Server`指定了服务名称为Eureka-Server；`eureka.instance.hostname=peer1`指定地址为peer1；`eureka.client.serviceUrl.defaultZone=http://peer2:8081/eureka/`指定Eureka服务端的地址为另外一个Eureka服务端的地址peer2。

下面我们创建另外一个Eureka服务端peer2的yml配置application-peer2.yml：

```yaml
server:
  port: 8081

spring:
  application:
    name: Eureka-Server

eureka:
  instance:
    hostname: peer2
  client:
    serviceUrl:
      defaultZone: http://peer1:8080/eureka/
  server:
    enable-self-preservation: false
```



peer2中的`serviceUrl`我们指向Eureka服务端peer1。

为了让这种在一台机器上配置两个hostname的方式生效，我们需要修改下hosts文件（位置C:\Windows\System32\drivers\etc）：

```
127.0.0.1       peer1
127.0.0.1       peer2
```



我们将Eureka-Server项目打包成jar，然后分别运行以下两条命令来部署peer1和peer2：

```java
java -jar Eureka-Service-0.0.1-SNAPSHOT.jar --spring.profiles.active=peer1
java -jar Eureka-Service-0.0.1-SNAPSHOT.jar --spring.profiles.active=peer2
```



启动后，我们来访问peer1http://localhost:8080/：

![QQ截图20180704100749.png](https://mrbird.cc/img/QQ%E6%88%AA%E5%9B%BE20180704100749.png)

可看到DS Replicas(副本)指向了peer2，registered-replicas和available-replicas都指向了http://peer2:8081/eureka/。

访问http://localhost:8081/我们也可以看到类似的信息。

因为Eureka服务端做了集群处理，所以Eureka客户端指定的服务端地址也要进行修改：

```
eureka:
  client:
    serviceUrl:
      defaultZone: http://peer1:8080/eureka/,http://peer2:8081/eureka/
```



我们将Eureka客户端（Server-Provider）打成jar包，然后分别用端口8082和8083启动两个服务：

```
java -jar Eureka-Client-0.0.1-SNAPSHOT.jar --server.port=8082
java -jar Eureka-Client-0.0.1-SNAPSHOT.jar --server.port=8083
```



然后访问http://peer2:8080/eureka/或者http://peer2:8081/eureka/：![QQ截图20180704104219.png](https://mrbird.cc/img/QQ%E6%88%AA%E5%9B%BE20180704104219.png)

## 搭建Server-Consumer服务消费者

在实际项目中，Eureka客户端即是服务提供者，也是服务消费者，即自身的接口可能被别的服务访问，同时也可能调用别的服务接口。这里为了更好的演示，我们把服务消费者单独的分开来演示。

新建一个Spring Boot项目，`artifactId`填Server-Consumer，其主要的任务就是将自身的服务注册到Eureka服务端，并且获取Eureka服务端提供的服务并进行消费。这里服务的消费我们用[Ribbon](https://github.com/Netflix/ribbon)来完成，Ribbon是一款实现服务负载均衡的开源软件，这里不做详细介绍。

引入Eureka客户端和Ribbon依赖：

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
        <artifactId>spring-cloud-starter-ribbon</artifactId>
    </dependency>
</dependencies>
```



同样的，在入口类中加入`@EnableDiscoveryClient`注解用于发现服务和注册服务，并配置一个`RestTemplate Bean`，然后加上`@LoadBalanced`注解来开启负载均衡：

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
import org.springframework.cloud.client.loadbalancer.LoadBalanced;
import org.springframework.context.annotation.Bean;
import org.springframework.web.client.RestTemplate;

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



接着编写一个TestController，用于消费服务：

```
@RestController
public class TestController {
    @Autowired
    private RestTemplate restTemplate;

    @GetMapping("/info")
    public String getInfo() {
        return this.restTemplate.getForEntity("http://Server-Provider/info", String.class).getBody();
    }
}
```



上面代码注入了`RestTemplate`，`getInfo`中使用`RestTemplate`对象均衡的去获取服务并消费。可以看到我们使用服务名称（Server-Provider）去获取服务的，而不是使用传统的IP加端口的形式。这就体现了使用Eureka去获取服务的好处，我们只要保证这个服务名称不变即可，IP和端口不再是我们关心的点。

最后编写下配置文件application.yml：

```yaml
server:
  port: 9000
  
spring:
  application:
    name: Server-Consumer
    
eureka:
  client:
    serviceUrl:
      defaultZone: http://peer1:8080/eureka/,http://peer2:8081/eureka/
```



端口为9000，服务名称为`Server-Consumer`并指定了Eureka服务端的地址。

启动该项目，访问http://localhost:9000/info：

![QQ截图20180704112445.png](https://mrbird.cc/img/QQ%E6%88%AA%E5%9B%BE20180704112445.png)

成功获取到了信息，我们多次访问这个接口，然后观察8082和8083Eureka客户端的后台：

![QQ截图20180704112611.png](https://mrbird.cc/img/QQ%E6%88%AA%E5%9B%BE20180704112611.png)

![QQ截图20180704112650.png](https://mrbird.cc/img/QQ%E6%88%AA%E5%9B%BE20180704112650.png)

可以看到它们的后台都打印出了信息，说明我们从9000去获取服务是均衡的。

这时候我们关闭一个Eureka服务端，再次访问http://localhost:9000/info，还是可以成功获取到信息，这就是Eureka服务端集群的好处。

## Eureka-Server添加认证

出于安全的考虑，我们可能会对Eureka服务端添加用户认证的功能。我们在Eureka-Server引入Spring-Security依赖：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```



然后在application.yml中配置用户名和密码：

```yaml
security:
  basic:
    enabled: true
  user:
    name: mrbird
    password: 123456
```



Eureka服务端配置了密码之后，所有`eureka.client.serviceUrl.defaultZone`的配置也必须配置上用户名和密码，格式为：`eureka.client.serviceUrl.defaultZone=http://${userName}:${password}@${hosetname}:${port}/eureka/`，如：

```yaml
eureka:
  client:
    serviceUrl:
      defaultZone: http://mrbird:123456@peer2:8081/eureka/
```



重新打包并部署后，访问http://localhost:8080/，页面将弹出验证窗口，输入用户名和密码后即可访问。

## Eureka配置

下面我们总结一下在Eureka中常用的配置选项及代表的含义：

| 配置                                                     | 含义                                                         | 默认值                       |
| :------------------------------------------------------- | :----------------------------------------------------------- | :--------------------------- |
| eureka.client.enabled                                    | 是否启用Eureka Client                                        | true                         |
| eureka.client.register-with-eureka                       | 表示是否将自己注册到Eureka Server                            | true                         |
| eureka.client.fetch-registry                             | 表示是否从Eureka Server获取注册的服务信息                    | true                         |
| eureka.client.serviceUrl.defaultZone                     | 配置Eureka Server地址，用于注册服务和获取服务                | http://localhost:8761/eureka |
| eureka.client.registry-fetch-interval-seconds            | 默认值为30秒，即每30秒去Eureka Server上获取服务并缓存        | 30                           |
| eureka.instance.lease-renewal-interval-in-seconds        | 向Eureka Server发送心跳的间隔时间，单位为秒，用于服务续约    | 30                           |
| eureka.instance.lease-expiration-duration-in-seconds     | 定义服务失效时间，即Eureka Server检测到Eureka Client木有心跳后（客户端意外下线）多少秒将其剔除 | 90                           |
| eureka.server.enable-self-preservation                   | 用于开启Eureka Server自我保护功能                            | true                         |
| eureka.client.instance-info-replication-interval-seconds | 更新实例信息的变化到Eureka服务端的间隔时间，单位为秒         | 30                           |
| eureka.client.eureka-service-url-poll-interval-seconds   | 轮询Eureka服务端地址更改的间隔时间，单位为秒。               | 300                          |
| eureka.instance.prefer-ip-address                        | 表示使用IP进行配置为不是域名                                 | false                        |
| eureka.client.healthcheck.enabled                        | 默认Erueka Server是通过心跳来检测Eureka Client的健康状况的，通过置为true改变Eeureka Server对客户端健康检测的方式，改用Actuator的/health端点来检测。 | false                        |

Eureka还有许多别的配置，具体可以参考`EurekaClientConfigBean`，`EurekaServerConfigBean`和`EurekaInstanceConfigBean`配置类的源码。