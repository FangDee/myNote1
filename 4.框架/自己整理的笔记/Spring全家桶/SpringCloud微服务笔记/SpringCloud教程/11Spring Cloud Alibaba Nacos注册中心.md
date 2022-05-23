## Spring Cloud Alibaba Nacos注册中心

 2020-03-13 |  Visit count

[Nacos](https://nacos.io/zh-cn/index.html)是一款集服务注册发现、服务配置和管理于一身的开源软件，这节主要记录Nacos的服务注册发现功能的使用。借助Spring Cloud Alibaba Nacos Discovery，我们可以轻松地使用Spring Cloud编程模型体验Nacos的服务注册发现功能。本节使用的Spring Cloud版本为Hoxton.SR3，Spring Cloud Alibaba版本为2.2.0.RELEASE，Spring Boot版本为2.2.3.RELEASE。



## Nacos安装

因为Spring Cloud Alibaba 2.2.0.RELEASE内置的Nacos client版本为1.1.4，所以我们使用这个版本的Nacos。Nacos下载地址：https://github.com/alibaba/nacos/releases，选择nacos-server-1.1.4.zip 下载并解压：

![QQ20200313-095737@2x](https://mrbird.cc/img/QQ20200313-095737@2x.png)

解压后，打开conf目录下的配置文件，在末尾添加数据源配置：

```
spring.datasource.platform=mysql
db.num=1
db.url.0=jdbc:mysql://localhost:3306/nacos?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true
db.user=root
db.password=123456spring.datasource.platform=mysqldb.num=1db.url.0=jdbc:mysql://localhost:3306/nacos?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=truedb.user=rootdb.password=123456
```



然后在MySQL数据库中新建nacos数据库，并导入Nacos解压包conf目录下的nacos-mysql.sql脚本，导入后，库表如下图所示：

![QQ20200313-100402@2x](https://mrbird.cc/img/QQ20200313-100402@2x.png)

数据层准备好后，我们就可以启动Nacos了。笔者的电脑为Mac，所以这里以Unix环境为例。终端切换到Nacos解压目录下的bin目录，然后执行`sh startup.sh -m standalone`启动单机版Nacos。

为了方便，我们也可以创建一个run.sh脚本：

```
#/bin/bashsh shutdown.shsh startup.sh -m standalonetail -10f /Users/mrbird/Desktop/nacos/logs/start.out
```



其中`/Users/mrbird/Desktop/nacos`为我的Nacos解压目录。启动后，浏览器访问：http://localhost:8848/nacos/#/login：

![QQ20200313-101410@2x](https://mrbird.cc/img/QQ20200313-101410@2x.png)

说明Nacos启动成功，账号密码都为nacos。

## 框架搭建

使用IDEA创建一个maven项目，artifactId为spring-cloud-alibaba-nacos-register，然后在其下面创建两个Module（Spring Boot项目），artifactId分别为consumer和provider，充当服务消费端和服务提供端，项目结构如下图所示：

![QQ20200313-103900@2x](https://mrbird.cc/img/QQ20200313-103900@2x.png)

spring-cloud-alibaba-nacos-register的pom内容：

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>cc.mrbird</groupId>
    <artifactId>spring-cloud-alibaba-nacos-register</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>pom</packaging>

    <modules>
        <module>provider</module>
        <module>consumer</module>
    </modules>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.3.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <properties>
        <java.version>1.8</java.version>
        <spring-cloud.version>Hoxton.SR3</spring-cloud.version>
        <com-alibaba-cloud.version>2.2.0.RELEASE</com-alibaba-cloud.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-alibaba-nacos-discovery</artifactId>
        </dependency>
    </dependencies>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>${spring-cloud.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <dependency>
                <groupId>com.alibaba.cloud</groupId>
                <artifactId>spring-cloud-alibaba-dependencies</artifactId>
                <version>${com-alibaba-cloud.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
</project>
```



因为本节演示的是Nacos的服务注册发现功能，所以引入的是`spring-cloud-alibaba-nacos-discovery`依赖。

provider的pom的内容如下所示：

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>cc.mrbird</groupId>
        <artifactId>spring-cloud-alibaba-nacos-register</artifactId>
        <version>1.0-SNAPSHOT</version>
        <relativePath>../pom.xml</relativePath>
    </parent>

    <artifactId>provider</artifactId>
    <name>provider</name>
    <description>服务提供端</description>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```



consumer的pom内容如下：

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>cc.mrbird</groupId>
        <artifactId>spring-cloud-alibaba-nacos-register</artifactId>
        <version>1.0-SNAPSHOT</version>
        <relativePath>../pom.xml</relativePath>
    </parent>

    <artifactId>consumer</artifactId>
    <name>consumer</name>
    <description>服务消费端</description>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```



## Nacos作为注册中心

在provider的cc.mrbird.provider目录下新建controller包，然后在该包下新建`HelloController`：

```
@RestController
@RequestMapping("provide")
public class HelloController {

    @GetMapping("{message}")
    public String hello(@PathVariable String message) {
        return String.format("hello %s", message);
    }
}
```



提供了一个REST接口，供consumer调用。

然后在provider的项目配置文件application.yml里添加如下配置：

```
server:
  port: 8001
spring:
  application:
    name: provider
  cloud:
    nacos:
      server-addr: localhost:8848
```



- `server.port`，provider服务端口为8001；
- `spring.application.name`，服务名称为provider
- `spring.cloud.nacos.server-addr`，指定Nacos注册中心的地址。

provider代码准备好后，接着在consumer项目的cc.mrbird.consumer目录下新建controller包，然后在该包下新建`ConsumeController`：

```
@RestController
@RequestMapping("consume")
public class ConsumeController {

    @Autowired
    private LoadBalancerClient loadBalancerClient;
    @Autowired
    private RestTemplate restTemplate;

    @GetMapping("hello/{message}")
    public String hello(@PathVariable String message) {
        ServiceInstance serviceInstance = loadBalancerClient.choose("provider");
        String path = String.format("http://%s:%s/provide/%s", serviceInstance.getHost(), serviceInstance.getPort(), message);
        String result = restTemplate.getForObject(path, String.class);
        return String.format("%s from %s %s", result, serviceInstance.getHost(), serviceInstance.getPort());
    }
}
```

因为`spring-cloud-alibaba-nacos-discovery`内置了Ribbon，所以我们可以直接注入`LoadBalancerClient`，`RestTemplate`我们稍后再配置。`hello()`方法中我们通过服务提供者的名称provider（即provider项目配置的`spring.application.name`）从Nacos注册中心中获取对应的服务实例，然后访问其提供的`/provide/{message} GET`服务。这些在[Spring Cloud Ribbon客户端负载均衡](https://mrbird.cc/Spring-Cloud-Ribbon-LoadBalance.html)一节中都介绍过了，有不懂的可以参阅下。

接着在consumer项目的cc.mrbird.consumer目录下新建configure包，然后在该包下新建`ConsumerConfigure`：

```
@Configuration
public class ConsumerConfigure {

    @Bean
    public RestTemplate restTemplate() {
        return new RestTemplate();
    }
}
```

上面代码配置了`RestTemplate` Bean。

最后在consumer项目的配置文件application.yml中添加如下配置：

```
server:
  port: 9001
spring:
  application:
    name: consumer
  cloud:
    nacos:
      server-addr: localhost:8848
```



配置和provider项目类似，不再赘述。

配置和provider项目类似，不再赘述。

分别启动provider和consumer项目：

![QQ20200313-122632@2x](https://mrbird.cc/img/QQ20200313-122632@2x.png)

观察Nacos控制台服务列表：

![QQ20200313-122748@2x](https://mrbird.cc/img/QQ20200313-122748@2x.png)

可以看到，两个服务都注册进来了。接着浏览器访问：http://localhost:9001/consume/hello/nacos：

![QQ20200313-122916@2x](https://mrbird.cc/img/QQ20200313-122916@2x.png)

调用成功，说明服务发现成功。

如下图所示，在ProviderApplication上右键选择Copy Configuration…：

![QQ20200313-123539@2x](https://mrbird.cc/img/QQ20200313-123539@2x.png)

然后按照下图所示填写相关内容：

![QQ20200313-123806@2x](https://mrbird.cc/img/QQ20200313-123806@2x.png)

点击ok保存后，启动它：

![QQ20200313-123912@2x](https://mrbird.cc/img/QQ20200313-123912@2x.png)

观察Nacos控制台，可以看到provider已经有两个实例了：

![QQ20200313-124004@2x](https://mrbird.cc/img/QQ20200313-124004@2x.png)

然后多次访问：http://localhost:9001/consume/hello/nacos，可以看到请求是均衡的（默认为轮询算法）：

![2020-03-13 12.54.05.gif](https://mrbird.cc/img/2020-03-13%2012.54.05.gif)

## Nacos注册中心配置

更多可用Nacos Descovery配置：

| 配置项              | Key                                            | 默认值                     | 说明                                                         |
| :------------------ | :--------------------------------------------- | :------------------------- | :----------------------------------------------------------- |
| 服务端地址          | spring.cloud.nacos.discovery.server-addr       | 无                         | Nacos Server 启动监听的ip地址和端口                          |
| 服务名              | spring.cloud.nacos.discovery.service           | ${spring.application.name} | 给当前的服务命名                                             |
| 服务分组            | spring.cloud.nacos.discovery.group             | DEFAULT_GROUP              | 设置服务所处的分组                                           |
| 权重                | spring.cloud.nacos.discovery.weight            | 1                          | 取值范围 1 到 100，数值越大，权重越大                        |
| 网卡名              | spring.cloud.nacos.discovery.network-interface | 无                         | 当IP未配置时，注册的IP为此网卡所对应的IP地址，如果此项也未配置，则默认取第一块网卡的地址 |
| 注册的IP地址        | spring.cloud.nacos.discovery.ip                | 无                         | 优先级最高                                                   |
| 注册的端口          | spring.cloud.nacos.discovery.port              | -1                         | 默认情况下不用配置，会自动探测                               |
| 命名空间            | spring.cloud.nacos.discovery.namespace         | 无                         | 常用场景之一是不同环境的注册的区分隔离，例如开发测试环境和生产环境的资源（如配置、服务）隔离等。 |
| AccessKey           | spring.cloud.nacos.discovery.access-key        | 无                         | 当要上阿里云时，阿里云上面的一个云账号名                     |
| SecretKey           | spring.cloud.nacos.discovery.secret-key        | 无                         | 当要上阿里云时，阿里云上面的一个云账号密码                   |
| Metadata            | spring.cloud.nacos.discovery.metadata          | 无                         | 使用Map格式配置，用户可以根据自己的需要自定义一些和服务相关的元数据信息 |
| 日志文件名          | spring.cloud.nacos.discovery.log-name          | 无                         |                                                              |
| 集群                | spring.cloud.nacos.discovery.cluster-name      | DEFAULT                    | 配置成Nacos集群名称                                          |
| 接入点              | spring.cloud.nacos.discovery.enpoint           | UTF-8                      | 地域的某个服务的入口域名，通过此域名可以动态地拿到服务端地址 |
| 是否集成Ribbon      | ribbon.nacos.enabled                           | true                       | 一般都设置成true即可                                         |
| 是否开启Nacos Watch | spring.cloud.nacos.discovery.watch.enabled     | true                       | 可以设置成false来关闭 watch                                  |

> 本节源码链接：https://github.com/wuyouzhuguli/SpringAll/tree/master/74.spring-cloud-alibaba-nacos-register。