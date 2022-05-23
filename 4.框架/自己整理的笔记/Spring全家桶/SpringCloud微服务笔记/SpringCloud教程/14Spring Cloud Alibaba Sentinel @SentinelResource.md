## Spring Cloud Alibaba Sentinel @SentinelResource

 2020-03-19 |  Visit count 645049

Sentinel提供了[@SentinelResource](https://github.com/alibaba/Sentinel/wiki/注解支持)注解用于定义资源，并提供可选的异常回退和Block回退。异常回退指的是`@SentinelResource`注解标注的方法发生Java异常时的回退处理；Block回退指的是当`@SentinelResource`资源访问不符合Sentinel控制台定义的规则时的回退（默认返回Blocked by Sentinel (flow limiting)）。这节简单记录下该注解的用法。



## 框架搭建

使用IDEA创建一个maven项目，artifactId为spring-cloud-alibaba-sentinelresource，然后在其下面创建两个Module（Spring Boot项目），artifactId分别为consumer和provider，充当服务消费端和服务提供端，项目结构如下图所示：

![QQ20200320-145134](https://mrbird.cc/img/QQ20200320-145134.png)

spring-cloud-alibaba-sentinelresource的pom内容：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>cc.mrbird</groupId>
    <artifactId>spring-cloud-alibaba-sentinelresource</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>pom</packaging>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.3.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <modules>
        <module>provider</module>
        <module>consumer</module>
    </modules>

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



引入了spring-boot-starter-web和spring-cloud-alibaba-nacos-discovery Nacos服务注册发现依赖。

provider的pom的内容如下所示：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>cc.mrbird</groupId>
        <artifactId>spring-cloud-alibaba-sentinelresource</artifactId>
        <version>1.0-SNAPSHOT</version>
        <relativePath>../pom.xml</relativePath>
    </parent>

    <artifactId>provider</artifactId>
    <name>provider</name>
    <description>服务提供者</description>

    <properties>
        <java.version>1.8</java.version>
    </properties>

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

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>cc.mrbird</groupId>
        <artifactId>spring-cloud-alibaba-sentinelresource</artifactId>
        <version>1.0-SNAPSHOT</version>
        <relativePath>../pom.xml</relativePath>
    </parent>

    <artifactId>consumer</artifactId>
    <name>consumer</name>
    <description>服务消费者</description>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
        </dependency>
    </dependencies>

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



因为要演示在消费端使用`@SentinelResource`注解，所以我们引入了spring-cloud-starter-alibaba-sentinel依赖。

provider的配置文件application.yml内容如下：

```yaml
server:
  port: 8081
spring:
  application:
    name: provider
  cloud:
    nacos:
      server-addr: localhost:8848

```



配置了端口号，服务名和nacos地址。

consumer的配置文件application.yml内容如下：

```yaml
server:
  port: 9091
spring:
  application:
    name: consumer
  cloud:
    nacos:
      server-addr: localhost:8848
    sentinel:
      transport:
        dashboard: localhost:8080
        port: 8719
```



配置了端口号，服务名，nacos地址和sentinel控制台地址等。

## 基本用法

我们在provider下添加一个REST资源。在provider的cc.mrbird.provider目录下新建controller包，然后在该包下新建`GoodsController`：

```java
@RestController
@RequestMapping("goods")
public class GoodsController {

    @GetMapping("buy/{name}/{count}")
    public String buy(@PathVariable String name, @PathVariable Integer count) {
        return String.format("购买%d份%s", count, name);
    }
}
```



接着在consumer端通过Ribbon消费这个资源。在consumer的启动类`ConsumerApplication`里注册`RestTemplate`：

```java
@SpringBootApplication
public class ConsumerApplication {

    public static void main(String[] args) {
        SpringApplication.run(ConsumerApplication.class, args);
    }

    @Bean
    @LoadBalanced
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }
}
```



在consumer的cc.mrbird.consumer下新建controller包，然后在该包下新建`BuyController`：

```java
@RestController
public class BuyController {

    @Autowired
    private RestTemplate restTemplate;

    @GetMapping("buy/{name}/{count}")
    @SentinelResource(value = "buy", fallback = "buyFallback", blockHandler = "buyBlock")
    public String buy(@PathVariable String name, @PathVariable Integer count) {
        if (count >= 20) {
            throw new IllegalArgumentException("购买数量过多");
        }
        if ("miband".equalsIgnoreCase(name)) {
            throw new NullPointerException("已售罄");
        }
        Map<String, Object> params = new HashMap<>(2);
        params.put("name", name);
        params.put("count", count);
        return this.restTemplate.getForEntity("http://provider/goods/buy/{name}/{count}", String.class, params).getBody();
    }

    // 异常回退
    public String buyFallback(@PathVariable String name, @PathVariable Integer count, Throwable throwable) {
        return String.format("【进入fallback方法】购买%d份%s失败，%s", count, name, throwable.getMessage());
    }

    // sentinel回退
    public String buyBlock(@PathVariable String name, @PathVariable Integer count, BlockException e) {
        return String.format("【进入blockHandler方法】购买%d份%s失败，当前购买人数过多，请稍后再试", count, name);
    }
}
```



在`buy`方法中，我们通过Ribbon的`RestTemplate`访问provider的`/goods/buy`接口。当count参数大于20或者name参数的值为miband的时候，方法将抛出异常。`buy`方法上使用`@SentinelResource`注解标注，标识为一个sentinel资源，资源名称为buy，并且配置了fallback方法和blockHandler方法。

如前面所说，当`buy`方法本身抛出异常时，会进入fallback指定的回退方法中；当`buy`方法调用不符合sentinel控制台规定的规则（如流控规则，降级规则等）时，会进入blockHander指定的block方法中。为了确保成功地进入回退方法（成功反射），它们必须满足以下规则：

- 函数访问范围需要是`public`；
- Fallback函数，函数签名与原函数一致或末尾加一个`Throwable`类型的参数；
- Block异常处理函数，参数最后多一个`BlockException`，其余与原函数一致。

启动provider、consumer、nacos和sentinel控制台，浏览器访问：http://localhost:9091/buy/ipad/2：

![QQ20200320-151518](https://mrbird.cc/img/QQ20200320-151518.png)

我们在sentinel控制台中添加如下流控规则：

![QQ20200320-151651@2x.png](https://mrbird.cc/img/QQ20200320-151651@2x.png)

QPS阈值为2。

然后快速访问http://localhost:9091/buy/ipad/2：

![QQ20200320-152106@2x](https://mrbird.cc/img/QQ20200320-152106@2x.png)

可以看到，当方法访问不符合sentinel控制台规则时，进入的是blockHandler指定的回退方法。

如果访问：http://localhost:9091/buy/ipad/21

![QQ20200320-152155@2x](https://mrbird.cc/img/QQ20200320-152155@2x.png)或者：http://localhost:9091/buy/miband/2

![QQ20200320-152224@2x](https://mrbird.cc/img/QQ20200320-152224@2x.png)

方法自身抛出异常引发回退，进入的是fallback指定的回退方法。

## 其他属性

在当前类中编写回退方法会使得代码变得冗余耦合度高，我们可以将回退方法抽取出来到一个指定类中。

在cc.mrbird.consumer包下新建reveal包，然后在该包下新建`BuyBlockHandler`：

```java
public class BuyBlockHandler {

    // sentinel回退
    public static String buyBlock(@PathVariable String name, @PathVariable Integer count, BlockException e) {
        return String.format("【进入blockHandler方法】购买%d份%s失败，当前购买人数过多，请稍后再试", count, name);
    }
}
```



可以看到我们只是将`buyBlock`方法挪到了`BuyBlockHandler`中，不过这里的方法必须是`static`的。

接着新建`BuyFallBack`：

```java
public class BuyFallBack {

    // 异常回退
    public static String buyFallback(@PathVariable String name, @PathVariable Integer count, Throwable throwable) {
        return String.format("【进入fallback方法】购买%d份%s失败，%s", count, name, throwable.getMessage());
    }
}
```



这样`BuyController`的代码就可以精简为：

```java
@RestController
public class BuyController {

    @Autowired
    private RestTemplate restTemplate;

    @GetMapping("buy/{name}/{count}")
    @SentinelResource(value = "buy",
            fallback = "buyFallback",
            fallbackClass = BuyFallBack.class,
            blockHandler = "buyBlock",
            blockHandlerClass = BuyBlockHandler.class
    )
    public String buy(@PathVariable String name, @PathVariable Integer count) {
        if (count >= 20) {
            throw new IllegalArgumentException("购买数量过多");
        }
        if ("miband".equalsIgnoreCase(name)) {
            throw new NullPointerException("已售罄");
        }
        Map<String, Object> params = new HashMap<>(2);
        params.put("name", name);
        params.put("count", count);
        return this.restTemplate.getForEntity("http://provider/goods/buy/{name}/{count}", String.class, params).getBody();
    }
}
```



`fallbackClass`和`blockHandlerClass`指定回退方法所在的类。

此外我们也可以当遇到某个类型的异常时，不进行回退。比如：

```java
@RestController
public class BuyController {

    @Autowired
    private RestTemplate restTemplate;

    @GetMapping("buy/{name}/{count}")
    @SentinelResource(value = "buy",
            fallback = "buyFallback",
            fallbackClass = BuyFallBack.class,
            blockHandler = "buyBlock",
            blockHandlerClass = BuyBlockHandler.class,
            exceptionsToIgnore = NullPointerException.class
    )
    public String buy(@PathVariable String name, @PathVariable Integer count) {
        if (count >= 20) {
            throw new IllegalArgumentException("购买数量过多");
        }
        if ("miband".equalsIgnoreCase(name)) {
            throw new NullPointerException("已售罄");
        }
        Map<String, Object> params = new HashMap<>(2);
        params.put("name", name);
        params.put("count", count);
        return this.restTemplate.getForEntity("http://provider/goods/buy/{name}/{count}", String.class, params).getBody();
    }
}
```



`exceptionsToIgnore`指定，当遇到空指针异常时，不回退。

重启consumer，浏览器访问：http://localhost:9091/buy/miband/2：

![QQ20200320-153530@2x](https://mrbird.cc/img/QQ20200320-153530@2x.png)

可以看到，此次并没有进行回退，而是直接返回error page。

> 本节源码链接：https://github.com/wuyouzhuguli/SpringAll/tree/master/78.spring-cloud-alibaba-sentinelresource