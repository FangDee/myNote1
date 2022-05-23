## 一、 SpringCloud简介

### 1、 软件架构演进

单体架构

![image-20220328172416767](https://www.ydlclass.com/doc21xnv/assets/image-20220328172416767.61fb5e07.png)

垂直架构

![image-20220328172721883](https://www.ydlclass.com/doc21xnv/assets/image-20220328172721883.2d51c59e.png)

分布式架构

![image-20220328173112642](https://www.ydlclass.com/doc21xnv/assets/image-20220328173112642.b0ee1ce2.png)

SOA架构

![image-20220328174118241](https://www.ydlclass.com/doc21xnv/assets/image-20220328174118241.23f135ec.png)

微服务架构

![image-20220328174633493](https://www.ydlclass.com/doc21xnv/assets/image-20220328174633493.6d3e8f1b.png)

### 2、微服务架构

#### （1） 微服务理念

①"微服务”一词源 于 Martin Fowler的名为 Microservices的博文,可以在他的官方博客上找到http://martinfowler.com/articles/microservices.html

②微服务是系统架构上的一种设计风格,它的主旨是将一个原本独立的系统拆分成多个小型服务,这些小型服务都在各自独立的进程中运行,服务之间一般通过 HTTP 的 RESTfuL API 进行通信协作。

**restfull 风格**：数据的增删改查，使用http的不同方式。数据传输用json。

 查询 GET ip:port/user/1

 新增 POST ip:port/user json{username:itlils,age:18}

 修改 PUT ip:port/user/1 json{username:itlils,age:19}

 删除 DELETE ip:port/user/1

③由于有了轻量级的通信协作基础,所以这些微服务可以使用不同的语言来编写。大厂，各种语言混用。

cloud官网: https://spring.io/

![image-20220329154419410](https://www.ydlclass.com/doc21xnv/assets/image-20220329154419410.35fc4235.png)

![image-20220329152020956](https://www.ydlclass.com/doc21xnv/assets/image-20220329152020956.6168854b.png)

#### [#](https://www.ydlclass.com/doc21xnv/springcloud/SpringCloud.html#_2-现在大型互联网公司-都在使用微服务架构)（2） 现在大型互联网公司，都在使用微服务架构：

京东的促销节架构：618

![image-20220329152531517](https://www.ydlclass.com/doc21xnv/assets/image-20220329152531517.e9aa4638.png)

阿里的架构：

![image-20220329152552588](https://www.ydlclass.com/doc21xnv/assets/image-20220329152552588.ea1b80bc.png)

京东物流的架构：

![image-20220329152611053](https://www.ydlclass.com/doc21xnv/assets/image-20220329152611053.1bdac904.png)

### 3、 springcloud组件：

![image-20220329152758773](https://www.ydlclass.com/doc21xnv/assets/image-20220329152758773.9e65bcd2.png)

![image-20220329153147800](https://www.ydlclass.com/doc21xnv/assets/image-20220329153147800.c1b6698c.png)

## 二、走进springcloud

### 1、了解springcloud

①Spring Cloud 是**一系列框架**的有序集合。

②Spring Cloud 并没有重复制造轮子，它只是将目前各家公司开发的比较成熟、经得起实际考验的服务框架组合起来。

netflix eureka 1.1,alibaba 2.2

③通过 **Spring Boot** 风格进行再封装,屏蔽掉了复杂的配置和实现原理，最终给开发者留出了一套简单易懂、易部署和易维护的分布式系统开发工具包。

④它利用Spring Boot的开发便利性巧妙地简化了分布式系统基础设施的开发，如服务发现注册、配置中心、消息总线、负载均衡、 断路器、数据监控等，都可以用Spring Boot的开发风格做到一键启动和部署。

⑤Spring Cloud项目官方网址：https://spring.io/projects/spring-cloud

⑥Spring Cloud 版本命名方式采用了**伦敦地铁站的名称**，同时根据字母表的顺序来对应版本时间顺序，比如：最早的Release版本：Angel，第二个Release版本：Brixton，然后是Camden、Dalston、Edgware，Finchley，Greenwich，Hoxton。

目前最新的是2021.0.1版本。

![image-20220329161144299](https://www.ydlclass.com/doc21xnv/assets/image-20220329161144299.73a4c7f1.png)

### 2、 cloud与boot版本对应关系

![image-20220329164540295](https://www.ydlclass.com/doc21xnv/assets/image-20220329164540295.576a76f5.png)

### 3、 dubbo对比

![image-20220329161102849](https://www.ydlclass.com/doc21xnv/assets/image-20220329161102849.d44ade71.png)

**相同点**：Spring Cloud 与 Dubbo 都是实现微服务有效的工具。

**不同点**：

1Dubbo 只是实现了服务治理，而 Spring Cloud 子项目分别覆盖了微服务架构下的众多部件。

2Dubbo 使用 RPC 通讯协议，Spring Cloud 使用 RESTful 完成通信，Dubbo 效率略高于 Spring Cloud。

**小结**

• 微服务就是将项目的各个模块拆分为可独立运行、部署、测试的架构设计风格。

• Spring 公司将其他公司中微服务架构常用的组件整合起来，并使用 SpringBoot 简化其开发、配置。称为 Spring Cloud。

• Spring Cloud 与 Dubbo都是实现微服务有效的工具。Dubbo 性能更好，而 Spring Cloud 功能更全面。Dubbo 已经融入到spingcloudAlibaba这一套

**本课程技术特点**： 两套springcloud.

 1. 5年前的项目。springcloud netflix(eureka config hystrix) hoxton。会讲到。

 2. 新项目 springcloud alibaba(nacos sentinel)。也会讲到。

## 三、关于Cloud各种组件的停更/升级/替换

红色不维护。

绿色是alibaba一套，推荐使用。

![image-20220329171255668](https://www.ydlclass.com/doc21xnv/assets/image-20220329171255668.cddd16e1.png)

## 四、微服务架构编码构建

![image-20220404103923982](https://www.ydlclass.com/doc21xnv/assets/image-20220404103923982.85c56e15.png)

### 1、 搭建 Provider 和 Consumer 服务

#### （1）父工程 spring-cloud-parent

![image-20220404103149867](https://www.ydlclass.com/doc21xnv/assets/image-20220404103149867.f3234ca1.png)

使用utf-8编码

![image-20220404103334670](https://www.ydlclass.com/doc21xnv/assets/image-20220404103334670.9bfcbddd.png)

maven设置

![image-20220404103417399](https://www.ydlclass.com/doc21xnv/assets/image-20220404103417399.f4982f5b.png)

pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.ydlclass</groupId>
    <artifactId>spring-cloud-parent</artifactId>
    <version>1.0.0</version>

    <!--spring boot 环境 -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.11.RELEASE</version>
        <relativePath/>
    </parent>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
    </properties>
    
</project>
```

#### （2）提供者 eureka-provider

![image-20220330164728077](https://www.ydlclass.com/doc21xnv/assets/image-20220330164728077.62e821de.png)

搭建springboot工程：步骤把大象装冰箱有几步？ 工作中别问同事

```text
1 pom 导包
2 配置文件
3 主启动类
```

pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>spring-cloud-parent</artifactId>
        <groupId>com.ydlclass</groupId>
        <version>1.0.0</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>eureka-provider</artifactId>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>

</project>
```

application.yml

```text
server:
  port: 8000
```

主启动类 ProviderApp

```java
package com.ydlclass.provider;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
@SpringBootApplication
public class ProviderApplication {
    public static void main(String[] args) {
        SpringApplication.run(ProviderApplication.class,args);
    }
}
```

Goods

```java
package com.ydlclass.provider.domain;

import java.io.Serializable;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
public class Goods implements Serializable {
    private int id;//商品id
    private String title;//商品名
    private double price;//价格
    private int count;//库存

    public Goods(int id, String title, double price, int count) {
        this.id = id;
        this.title = title;
        this.price = price;
        this.count = count;
    }

    @Override
    public String toString() {
        return "Goods{" +
                "id=" + id +
                ", title='" + title + '\'' +
                ", price=" + price +
                ", count=" + count +
                '}';
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public double getPrice() {
        return price;
    }

    public void setPrice(double price) {
        this.price = price;
    }

    public int getCount() {
        return count;
    }

    public void setCount(int count) {
        this.count = count;
    }
}
```

GoodsController

```java
package com.ydlclass.provider.controller;

import com.ydlclass.provider.domain.Goods;
import com.ydlclass.provider.service.GoodsService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
@RestController
@RequestMapping("/goods")
public class GoodsController {
    @Autowired
    GoodsService goodsService;

    @GetMapping("findById/{id}")
    public Goods findById(@PathVariable("id") int id){
        Goods goods = goodsService.findById(id);
        return goods;
    }
}
```

GoodsService

```java
package com.ydlclass.provider.service;

import com.ydlclass.provider.dao.GoodsDao;
import com.ydlclass.provider.domain.Goods;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
@Service
public class GoodsService {
    @Autowired
    GoodsDao goodsDao;

    public Goods findById(int id){
        Goods goods = goodsDao.findById(id);
        return goods;
    }
}
```

GoodsDao

```java
package com.ydlclass.provider.dao;

import com.ydlclass.provider.domain.Goods;
import org.springframework.stereotype.Repository;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
@Repository
public class GoodsDao {
    public Goods findById(int id){
        //查数据库
        return new Goods(id,"手机",2000,100);
    }
}
```

**测试：**

访问：http://localhost:8000/goods/findById/1

![image-20220330170647529](https://www.ydlclass.com/doc21xnv/assets/image-20220330170647529.2bf7e1c4.png)

#### [#](https://www.ydlclass.com/doc21xnv/springcloud/SpringCloud.html#_3-消费者-eureka-consumer)（3）消费者 eureka-consumer

![image-20220404111105516](https://www.ydlclass.com/doc21xnv/assets/image-20220404111105516.f67f7a56.png)

pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>spring-cloud-parent</artifactId>
        <groupId>com.ydlclass</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>eureka-consumer</artifactId>


    <dependencies>

        <!--spring boot web-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

    </dependencies>
</project>
```

Goods

```java
package com.ydlclass.consumer.domain;

/**
 * 商品实体类
 */
public class Goods {

    private int id;
    private String title;//商品标题
    private double price;//商品价格
    private int count;//商品库存

    public Goods() {
    }

    public Goods(int id, String title, double price, int count) {
        this.id = id;
        this.title = title;
        this.price = price;
        this.count = count;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public double getPrice() {
        return price;
    }

    public void setPrice(double price) {
        this.price = price;
    }

    public int getCount() {
        return count;
    }

    public void setCount(int count) {
        this.count = count;
    }
}
```

OrderController

```java
package com.ydlclass.consumer.controller;

import com.ydlclass.consumer.domain.Goods;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
@RestController
@RequestMapping("/order")
public class OrderController {

    @GetMapping("/add/{id}")
    public Goods add(@PathVariable("id") Integer id){
        //业务逻辑
        //1查询商品
        //2减库存
        //3支付
        //4物流

        return new Goods();
    }
}
```

application.yml

```yaml
server:
  port: 9000
```

**测试：**

http://localhost:9000/order/add/2

![image-20220404112103333](https://www.ydlclass.com/doc21xnv/assets/image-20220404112103333.e5ed7fec.png)

### 2、使用 RestTemplate 完成远程调用

- Spring提供的一种简单便捷的模板类，用于在 java 代码里访问 restful 服务。
- 其功能与 HttpClient 类似，但是 RestTemplate 实现更优雅，使用更方便。

**consumer工程中**

RestTemplateConfig

```java
package com.ydlclass.consumer.config;


import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.client.RestTemplate;

@Configuration
public class RestTemplateConfig {


    @Bean
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }
}
```

OrderController

```java
package com.ydlclass.consumer.controller;


import com.ydlclass.consumer.domain.Goods;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

/**
 * 服务的调用方
 */

@RestController
@RequestMapping("/order")
public class OrderController {

    @Autowired
    private RestTemplate restTemplate;

    @GetMapping("/goods/{id}")
    public Goods findGoodsById(@PathVariable("id") int id){
        System.out.println("findGoodsById..."+id);


        /*
            //远程调用Goods服务中的findOne接口
            使用RestTemplate
            1. 定义Bean  restTemplate
            2. 注入Bean
            3. 调用方法
         */

        String url = "http://localhost:8000/goods/findOne/"+id;
        // 3. 调用方法
        Goods goods = restTemplate.getForObject(url, Goods.class);


        return goods;
    }
}
```

**测试：**

## 五、Eureka服务注册与发现

### 1、Eureka

**概念：**

• Eureka 是 Netflix 公司开源的一个服务注册与发现的组件 。

• Eureka 和其他 Netflix 公司的服务组件（例如负载均衡、熔断器、网关等） 一起，被 Spring Cloud 社区整合为Spring-Cloud-Netflix 模块。

• Eureka 包含两个组件：Eureka Server (注册中心) 和 Eureka Client (服务提供者、服务消费者)。

**操作：**

![image-20200606113829765](https://www.ydlclass.com/doc21xnv/assets/image-20200606113829765-1648628667299.b0fb85d7.png)

### 2、 搭建 Eureka Server 服务

（1）创建 eureka-server 模块

（2） 引入 SpringCloud 和 euraka-server 相关依赖

（3）完成 Eureka Server 相关配置

（4）启动该模块

**父工程 pom**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.ydlclass</groupId>
    <artifactId>spring-cloud-parent</artifactId>
    <packaging>pom</packaging>
    <version>1.0-SNAPSHOT</version>
    <modules>
        <module>eureka-provider</module>
        <module>eureka-consumer</module>
        <module>eureka-server</module>
    </modules>


    <!--spring boot 环境 -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.0.RELEASE</version>
        <relativePath/>
    </parent>


    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <java.version>1.8</java.version>
        <!--spring cloud 版本-->
        <spring-cloud.version>Greenwich.RELEASE</spring-cloud.version>
    </properties>

    <!--引入Spring Cloud 依赖-->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>${spring-cloud.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

</project>
```

**eureka-server工程**

pom

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>spring-cloud-parent</artifactId>
        <groupId>com.ydlclass</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>eureka-server</artifactId>



    <dependencies>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- eureka-server -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
        </dependency>
    </dependencies>


</project>
```

EurekaApp

```java
package com.ydlclass.eureka;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;

@SpringBootApplication
// 启用EurekaServer
@EnableEurekaServer
public class EurekaApp {

    public static void main(String[] args) {
        SpringApplication.run(EurekaApp.class,args);
    }
}
```

application.yml

```yaml
server:
  port: 8761

# eureka 配置
# eureka 一共有4部分 配置
# 1. dashboard:eureka的web控制台配置
# 2. server:eureka的服务端配置
# 3. client:eureka的客户端配置
# 4. instance:eureka的实例配置


eureka:
  instance:
    hostname: localhost # 主机名
  client:
    service-url:
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka # eureka服务端地址，将来客户端使用该地址和eureka进行通信

    register-with-eureka: false # 是否将自己的路径 注册到eureka上。eureka server 不需要的，eureka provider client 需要
    fetch-registry: false # 是否需要从eureka中抓取路径。eureka server 不需要的，eureka consumer client 需要
```

**测试： ** 访问 localhost:8761

![image-20200611013357920](https://www.ydlclass.com/doc21xnv/assets/image-20200611013357920.c3a39136.png)

### 3、 改造 Provider 和 Consumer 称为 Eureka Client

① 引 eureka-client 相关依赖

② 完成 eureka client 相关配置

③ 启动 测试

**Provider工程**

pom

```xml
        <!-- eureka-client -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
```

ProviderApp

```java
@EnableEurekaClient
```

application.yml

```yaml
server:
  port: 8001


eureka:
  instance:
    hostname: localhost # 主机名
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka # eureka服务端地址，将来客户端使用该地址和eureka进行通信
spring:
  application:
    name: eureka-provider # 设置当前应用的名称。将来会在eureka中Application显示。将来需要使用该名称来获取路径
```

**Consumer**

pom

```xml
        <!-- eureka-client -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
```

ConsumerApp

```java
package com.ydlclass.consumer;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;

@EnableDiscoveryClient // 激活DiscoveryClient
@EnableEurekaClient
@SpringBootApplication
public class ConsumerApp {


    public static void main(String[] args) {
        SpringApplication.run(ConsumerApp.class,args);
    }
}
```

application.yml

```yaml
server:
  port: 9000

eureka:
  instance:
    hostname: localhost # 主机名
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka # eureka服务端地址，将来客户端使用该地址和eureka进行通信
spring:
  application:
    name: eureka-consumer # 设置当前应用的名称。将来会在eureka中Application显示。将来需要使用该名称来获取路径
```

### 4、 Consumer 服务 通过从 Eureka Server 中抓取 Provider 地址，完成远程调用

**Consumer**

OrderController

```java
package com.ydlclass.consumer.controller;

import com.ydlclass.consumer.domain.Goods;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.cloud.client.ServiceInstance;
import org.springframework.cloud.client.discovery.DiscoveryClient;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import java.util.List;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
@RestController
@RequestMapping("/order")
public class OrderController {
    @Autowired
    RestTemplate restTemplate;

    @Autowired
    private DiscoveryClient discoveryClient;

    @GetMapping("/add/{id}")
    public Goods add(@PathVariable("id") Integer id){
        //业务逻辑
        //1查询商品
        //2减库存
        //3支付
        //4物流
        //直接调用
        //String url="http://localhost:8000/goods/findById/"+id;
        //Goods goods = restTemplate.getForObject(url, Goods.class);

        //服务发现
        List<ServiceInstance> instances = discoveryClient.getInstances("EUREKA-PROVIDER");
        if(instances==null||instances.size()<=0){
            return null;
        }
        //通过某个策略拿到一个实例
        ServiceInstance serviceInstance = instances.get(0);
        String host = serviceInstance.getHost();
        int port = serviceInstance.getPort();
        System.out.println(host);
        System.out.println(port);

        String url="http://"+host+":"+port+"/goods/findById/"+id;
        Goods goods = restTemplate.getForObject(url, Goods.class);

        return goods;
    }
}
```

### 5、 Euraka配置详解

Eureka包含四个部分的配置

1. instance：当前Eureka Instance实例信息配置
2. client：Eureka Client客户端特性配置
3. server：Eureka Server注册中心特性配置
4. dashboard：Eureka Server注册中心仪表盘配置

##### [#](https://www.ydlclass.com/doc21xnv/springcloud/SpringCloud.html#_1-eureka-instance实例信息配置)（1）Eureka Instance实例信息配置

```yaml
eureka:
    instance:
        hostname: localhost # 主机名
        prefer-ip-address: # 是否将自己的ip注册到eureka中，默认false 注册 主机名
        ip-address: # 设置当前实例ip
        instance-id: # 修改instance-id显示
        lease-renewal-interval-in-seconds: 30 # 每一次eureka client 向 eureka server发送心跳的时间间隔
        lease-expiration-duration-in-seconds: 90 # 如果90秒内eureka server没有收到eureka client的心跳包，则剔除该服务
```

Eureka Instance的配置信息全部保存在org.springframework.cloud.netflix.eureka.EurekaInstanceConfigBean配置类里，实际上它是com.netflix.appinfo.EurekaInstanceConfig的实现类，替代了netflix的com.netflix.appinfo.CloudInstanceConfig的默认实现。

Eureka Instance的配置信息全部以eureka.instance.xxx的格式配置。

**配置列表**

- appname = unknown

应用名，首先获取spring.application.name的值，如果取值为空，则取默认unknown。

- appGroupName = null

应用组名

- instanceEnabledOnit = false

实例注册到Eureka上是，是否立刻开启通讯。有时候应用在准备好服务之前需要一些预处理。

- nonSecurePort = 80

非安全的端口

- securePort = 443

安全端口

- nonSecurePortEnabled = true

是否开启非安全端口通讯

- securePortEnabled = false

是否开启安全端口通讯

- leaseRenewalIntervalInSeconds = 30

实例续约间隔时间

- leaseExpirationDurationInSeconds = 90

实例超时时间，表示最大leaseExpirationDurationInSeconds秒后没有续约，Server就认为他不可用了，随之就会将其剔除。

- virtualHostName = unknown

虚拟主机名，首先获取spring.application.name的值，如果取值为空，则取默认unknown。

- instanceId

注册到eureka上的唯一实例ID，不能与相同appname的其他实例重复。

- secureVirtualHostName = unknown

安全虚拟主机名，首先获取spring.application.name的值，如果取值为空，则取默认unknown。

- metadataMap = new HashMap();

实例元数据，可以供其他实例使用。比如spring-boot-admin在监控时，获取实例的上下文和端口。

- dataCenterInfo = new MyDataCenterInfo(DataCenterInfo.Name.MyOwn);

实例部署的数据中心。如AWS、MyOwn。

- ipAddress=null

实例的IP地址

- statusPageUrlPath = "/actuator/info"

实例状态页相对url

- statusPageUrl = null

实例状态页绝对URL

- homePageUrlPath = "/"

实例主页相对URL

- homePageUrl = null

实例主页绝对URL

- healthCheckUrlUrlPath = "/actuator/health"

实例健康检查相对URL

- healthCheckUrl = null

实例健康检查绝对URL

- secureHealthCheckUrl = null

实例安全的健康检查绝对URL

- namespace = "eureka"

配置属性的命名空间（Spring Cloud中被忽略）

- hostname = null

主机名,不配置的时候讲根据操作系统的主机名来获取

- preferIpAddress = false

是否优先使用IP地址作为主机名的标识

##### （2）Eureka Client客户端特性配置

```yaml
eureka:
    client:
        service-url:
       		 # eureka服务端地址，将来客户端使用该地址和eureka进行通信
        	defaultZone: 
        register-with-eureka: # 是否将自己的路径 注册到eureka上。
        fetch-registry: # 是否需要从eureka中抓取数据。
```

Eureka Client客户端特性配置是对作为Eureka客户端的特性配置，包括Eureka注册中心，本身也是一个Eureka Client。

Eureka Client特性配置全部在org.springframework.cloud.netflix.eureka.EurekaClientConfigBean中，实际上它是com.netflix.discovery.EurekaClientConfig的实现类，替代了netxflix的默认实现。

Eureka Client客户端特性配置全部以eureka.client.xxx的格式配置。

**配置列表**

- enabled=true

是否启用Eureka client。

- registryFetchIntervalSeconds=30

定时从Eureka Server拉取服务注册信息的间隔时间

- instanceInfoReplicationIntervalSeconds=30

定时将实例信息（如果变化了）复制到Eureka Server的间隔时间。（InstanceInfoReplicator线程）

- initialInstanceInfoReplicationIntervalSeconds=40

首次将实例信息复制到Eureka Server的延迟时间。（InstanceInfoReplicator线程）

- eurekaServiceUrlPollIntervalSeconds=300

拉取Eureka Server地址的间隔时间（Eureka Server有可能增减）

- proxyPort=null

Eureka Server的代理端口

- proxyHost=null

Eureka Server的代理主机名

- proxyUserName=null

Eureka Server的代理用户名

- proxyPassword=null

Eureka Server的代理密码

- eurekaServerReadTimeoutSeconds=8

从Eureka Server读取信息的超时时间

- eurekaServerConnectTimeoutSeconds=5

连接Eureka Server的超时时间

- backupRegistryImpl=null

Eureka Client第一次启动时获取服务注册信息的调用的回溯实现。Eureka Client启动时首次会检查有没有BackupRegistry的实现类，如果有实现类，则优先从这个实现类里获取服务注册信息。

- eurekaServerTotalConnections=200

Eureka client连接Eureka Server的链接总数

- eurekaServerTotalConnectionsPerHost=50

Eureka client连接单台Eureka Server的链接总数

- eurekaServerURLContext=null

当Eureka server的列表在DNS中时，Eureka Server的上下文路径。如http://xxxx/eureka。

- eurekaServerPort=null

当Eureka server的列表在DNS中时，Eureka Server的端口。

- eurekaServerDNSName=null

当Eureka server的列表在DNS中时，且要通过DNSName获取Eureka Server列表时，DNS名字。

- region="us-east-1"

实例所属区域。

- eurekaConnectionIdleTimeoutSeconds = 30

Eureka Client和Eureka Server之间的Http连接的空闲超时时间。

- heartbeatExecutorThreadPoolSize=2

心跳（续约）执行器线程池大小。

- heartbeatExecutorExponentialBackOffBound=10

心跳执行器在续约过程中超时后的再次执行续约的最大延迟倍数。默认最大延迟时间=10 * eureka.instance.leaseRenewalIntervalInSeconds

- cacheRefreshExecutorThreadPoolSize=2

cacheRefreshExecutord的线程池大小（获取注册信息）

- cacheRefreshExecutorExponentialBackOffBound=10

cacheRefreshExecutord的再次执行的最大延迟倍数。默认最大延迟时间=10 *eureka.client.registryFetchIntervalSeconds

- serviceUrl= new HashMap();serviceUrl.put(DEFAULT_ZONE, DEFAULT_URL);

Eureka Server的分区地址。默认添加了一个defualtZone。也就是最常用的配置eureka.client.service-url.defaultZone=xxx

- registerWithEureka=true

是否注册到Eureka Server。

- preferSameZoneEureka=true

是否使用相同Zone下的Eureka server。

- logDeltaDiff=false

是否记录Eureka Server和Eureka Client之间注册信息的差异

- disableDelta=false

是否开启增量同步注册信息。

- fetchRemoteRegionsRegistry=null

获取注册服务的远程地区，以逗号隔开。

- availabilityZones=new HashMap()

可用分区列表。用逗号隔开。

- filterOnlyUpInstances = true

是否只拉取UP状态的实例。

- fetchRegistry=true

是否拉取注册信息。

- shouldUnregisterOnShutdown = true

是否在停止服务的时候向Eureka Server发起Cancel指令。

- shouldEnforceRegistrationAtInit = false

是否在初始化过程中注册服务。

##### [#](https://www.ydlclass.com/doc21xnv/springcloud/SpringCloud.html#_3-eureka-server注册中心端配置)（3）Eureka Server注册中心端配置

```yaml
eureka:
    server: #是否开启自我保护机制，默认true
        enable-self-preservation: 
        eviction-interval-timer-in-ms: 120 2月#清理间隔（单位毫秒，默认是60*1000）
	instance:
        lease-renewal-interval-in-seconds: 30 # 每一次eureka client 向 eureka server发送心跳的时间间隔
        lease-expiration-duration-in-seconds: 90 # 如果90秒内eureka server没有收到eureka client的心跳包，则剔除该服务        
```

Eureka Server注册中心端的配置是对注册中心的特性配置。Eureka Server的配置全部在org.springframework.cloud.netflix.eureka.server.EurekaServerConfigBean里，实际上它是com.netflix.eureka.EurekaServerConfig的实现类，替代了netflix的默认实现。

Eureka Server的配置全部以eureka.server.xxx的格式进行配置。

**配置列表**

- enableSelfPreservation=true

是否开启自我保护

- renewalPercentThreshold = 0.85

自我保护续约百分比阀值因子。如果实际续约数小于续约数阀值，则开启自我保护

- renewalThresholdUpdateIntervalMs = 15 * 60 * 1000

续约数阀值更新频率。

- peerEurekaNodesUpdateIntervalMs = 10 * 60 * 1000

Eureka Server节点更新频率。

- enableReplicatedRequestCompression = false

是否启用复制请求压缩。

- waitTimeInMsWhenSyncEmpty=5 * 60 * 1000

当从其他节点同步实例信息为空时等待的时间。

- peerNodeConnectTimeoutMs=200

节点间连接的超时时间。

- peerNodeReadTimeoutMs=200

节点间读取信息的超时时间。

- peerNodeTotalConnections=1000

节点间连接总数。

- peerNodeTotalConnectionsPerHost = 500;

单个节点间连接总数。

- peerNodeConnectionIdleTimeoutSeconds = 30;

节点间连接空闲超时时间。

- retentionTimeInMSInDeltaQueue = 3 * MINUTES;

增量队列的缓存时间。

- deltaRetentionTimerIntervalInMs = 30 * 1000;

清理增量队列中过期的频率。

- evictionIntervalTimerInMs = 60 * 1000;

剔除任务频率。

- responseCacheAutoExpirationInSeconds = 180;

注册列表缓存超时时间（当注册列表没有变化时）

- responseCacheUpdateIntervalMs = 30 * 1000;

注册列表缓存更新频率。

- useReadOnlyResponseCache = true;

是否开启注册列表的二级缓存。

- disableDelta=false。

是否为client提供增量信息。

- maxThreadsForStatusReplication = 1;

状态同步的最大线程数。

- maxElementsInStatusReplicationPool = 10000;

状态同步队列的最大容量。

- syncWhenTimestampDiffers = true;

当时间差异时是否同步。

- registrySyncRetries = 0;

注册信息同步重试次数。

- registrySyncRetryWaitMs = 30 * 1000;

注册信息同步重试期间的时间间隔。

- maxElementsInPeerReplicationPool = 10000;

节点间同步事件的最大容量。

- minThreadsForPeerReplication = 5;

节点间同步的最小线程数。

- maxThreadsForPeerReplication = 20;

节点间同步的最大线程数。

- maxTimeForReplication = 30000;

节点间同步的最大时间，单位为毫秒。

- disableDeltaForRemoteRegions = false；

是否启用远程区域增量。

- remoteRegionConnectTimeoutMs = 1000;

远程区域连接超时时间。

- remoteRegionReadTimeoutMs = 1000;

远程区域读取超时时间。

- remoteRegionTotalConnections = 1000;

远程区域最大连接数

- remoteRegionTotalConnectionsPerHost = 500;

远程区域单机连接数

- remoteRegionConnectionIdleTimeoutSeconds = 30;

远程区域连接空闲超时时间。

- remoteRegionRegistryFetchInterval = 30;

远程区域注册信息拉取频率。

- remoteRegionFetchThreadPoolSize = 20;

远程区域注册信息线程数。

##### [#](https://www.ydlclass.com/doc21xnv/springcloud/SpringCloud.html#_4-eureka-server注册中心仪表盘配置)**（4）Eureka Server注册中心仪表盘配置**

```yaml
eureka:
    dashboard:
        enabled: true # 是否启用eureka web控制台
        path: / # 设置eureka web控制台默认访问路径
```

注册中心仪表盘的配置主要是控制注册中心的可视化展示。以eureka.dashboard.xxx的格式配置。

- path="/"

仪表盘访问路径

- enabled=true

是否启用仪表盘

**改造 provider**

```yaml
server:
  port: 8001


eureka:
  instance:
    hostname: localhost # 主机名
    prefer-ip-address: true # 将当前实例的ip注册到eureka server 中。默认是false 注册主机名
    ip-address: 127.0.0.1 # 设置当前实例的ip
    instance-id: ${eureka.instance.ip-address}:${spring.application.name}:${server.port} # 设置web控制台显示的 实例id
    lease-renewal-interval-in-seconds: 3 # 每隔3 秒发一次心跳包
    lease-expiration-duration-in-seconds: 9 # 如果9秒没有发心跳包，服务器呀，你把我干掉吧~
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka # eureka服务端地址，将来客户端使用该地址和eureka进行通信
spring:
  application:
    name: eureka-provider # 设置当前应用的名称。将来会在eureka中Application显示。将来需要使用该名称来获取路径
```

**consumer**

```yaml
server:
  port: 9000


eureka:
  instance:
    hostname: localhost # 主机名
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka # eureka服务端地址，将来客户端使用该地址和eureka进行通信
spring:
  application:
    name: eureka-consumer # 设置当前应用的名称。将来会在eureka中Application显示。将来需要使用该名称来获取路径
```

**server**

```yaml
server:
  port: 8761

# eureka 配置
# eureka 一共有4部分 配置
# 1. dashboard:eureka的web控制台配置
# 2. server:eureka的服务端配置
# 3. client:eureka的客户端配置
# 4. instance:eureka的实例配置


eureka:
  instance:
    hostname: localhost # 主机名
  client:
    service-url:
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka # eureka服务端地址，将来客户端使用该地址和eureka进行通信
    register-with-eureka: false # 是否将自己的路径 注册到eureka上。eureka server 不需要的，eureka provider client 需要
    fetch-registry: false # 是否需要从eureka中抓取路径。eureka server 不需要的，eureka consumer client 需要
  server:
    enable-self-preservation: false # 关闭自我保护机制
    eviction-interval-timer-in-ms: 3000 # 检查服务的时间间隔
```

### 6、高可用

![image-20220406114821846](https://www.ydlclass.com/doc21xnv/assets/image-20220406114821846.5d110f1f.png)

1. 准备两个Eureka Server
2. 分别进行配置，**相互注册**
3. Eureka Client 分别注册到这两个 Eureka Server中

**创建eureka-server1**

```yaml
server:
  port: 8761


eureka:
  instance:
    hostname: eureka-server1 # 主机名
  client:
    service-url:
      defaultZone: http://eureka-server2:8762/eureka
    register-with-eureka: true # 是否将自己的路径 注册到eureka上。eureka server 不需要的，eureka provider client 需要
    fetch-registry: true # 是否需要从eureka中抓取路径。eureka server 不需要的，eureka consumer client 需要


spring:
  application:
    name: eureka-server-ha
```

**创建eureka-server2**

```yaml
server:
  port: 8762


eureka:
  instance:
    hostname: eureka-server2 # 主机名
  client:
    service-url:
      defaultZone: http://eureka-server1:8761/eureka

    register-with-eureka: true # 是否将自己的路径 注册到eureka上。eureka server 不需要的，eureka provider client 需要
    fetch-registry: true # 是否需要从eureka中抓取路径。eureka server 不需要的，eureka consumer client 需要
spring:
  application:
    name: eureka-server-ha
```

**修改**：C:\Windows\System32\drivers\etc\hosts

127.0.0.1 eureka-server1 127.0.0.1 eureka-server2

![image-20220406144138369](https://www.ydlclass.com/doc21xnv/assets/image-20220406144138369.3a3af0a9.png)

测试：

provider

```yaml
server:
  port: 8001


eureka:
  instance:
    hostname: localhost # 主机名
    prefer-ip-address: true # 将当前实例的ip注册到eureka server 中。默认是false 注册主机名
    ip-address: 127.0.0.1 # 设置当前实例的ip
    instance-id: ${eureka.instance.ip-address}:${spring.application.name}:${server.port} # 设置web控制台显示的 实例id
    lease-renewal-interval-in-seconds: 3 # 每隔3 秒发一次心跳包
    lease-expiration-duration-in-seconds: 9 # 如果9秒没有发心跳包，服务器呀，你把我干掉吧~
  client:
    service-url:
      defaultZone: http://eureka-server1:8761/eureka,http://eureka-server2:8762/eureka # eureka服务端地址，将来客户端使用该地址和eureka进行通信
spring:
  application:
    name: eureka-provider # 设置当前应用的名称。将来会在eureka中Application显示。将来需要使用该名称来获取路径
```

consumer

```yaml
server:
  port: 9000


eureka:
  instance:
    hostname: localhost # 主机名
  client:
    service-url:
      defaultZone:  http://eureka-server1:8761/eureka,http://eureka-server2:8762/eureka  # eureka服务端地址，将来客户端使用该地址和eureka进行通信
spring:
  application:
    name: eureka-consumer # 设置当前应用的名称。将来会在eureka中Application显示。将来需要使用该名称来获取路径
```

访问：http://localhost:9000/order/add/4

![image-20220406144153992](https://www.ydlclass.com/doc21xnv/assets/image-20220406144153992.acdf6713.png)

高可用测试：停掉一个eureka,依然可以访问consumer。

eureka不更新了，所以淘汰了。

## 六、Zookeeper服务注册与发现

![image-20220406194741473](https://www.ydlclass.com/doc21xnv/assets/image-20220406194741473.cc3720e9.png)

有的老项目以前是dubbo，升级到微服务，使用zookeeper做注册中心。

zookeeper是一个分布式协调工具，可以实现注册中心功能。dubbo,大数据组件hadoop,hive,kafka。

**实质**：注册中心换成zk

1下载：https://zookeeper.apache.org/

![image-20220406152809933](https://www.ydlclass.com/doc21xnv/assets/image-20220406152809933.e8b336fb.png)

zoo.cfg

```text
# The number of milliseconds of each tick
tickTime=2000
# The number of ticks that the initial 
# synchronization phase can take
initLimit=10
# The number of ticks that can pass between 
# sending a request and getting an acknowledgement
syncLimit=5
# the directory where the snapshot is stored.
# do not use /tmp for storage, /tmp here is just 
# example sakes.
dataDir=F:/apache-zookeeper-3.5.6-bin/data
# the port at which the clients will connect
clientPort=2181
# the maximum number of client connections.
# increase this if you need to handle more clients
#maxClientCnxns=60
#
# Be sure to read the maintenance section of the 
# administrator guide before turning on autopurge.
#
# http://zookeeper.apache.org/doc/current/zookeeperAdmin.html#sc_maintenance
#
# The number of snapshots to retain in dataDir
#autopurge.snapRetainCount=3
# Purge task interval in hours
# Set to "0" to disable auto purge feature
#autopurge.purgeInterval=1
```

启动 bin目录下

```text
zkServer.cmd
```

1

![image-20220406153622431](https://www.ydlclass.com/doc21xnv/assets/image-20220406153622431.66776d12.png)

2zookeeper-provider

pom

```text
<artifactId>zookeeper-provider</artifactId>

<dependencies>
    <!--springcloud 整合 zookeeper 组件-->
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <!--zk发现-->
        <artifactId>spring-cloud-starter-zookeeper-discovery</artifactId>
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
        <exclusions>
            <exclusion>
                <groupId>org.slf4j</groupId>
                <artifactId>slf4j-log4j12</artifactId>
            </exclusion>
        </exclusions>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

</dependencies>
```

yml

```text
server:
  port: 8004

spring:
  application:
    name: zookeeper-provider
  cloud:
    zookeeper:
      connect-string: 127.0.0.1:2181 # zk地址
```

主启动类

```java
package com.ydlclass.zk;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
@SpringBootApplication
@EnableDiscoveryClient //开启发现客户端
public class ProviderApp {
    public static void main(String[] args) {
        SpringApplication.run(ProviderApp.class,args);
    }
}
```

业务逻辑代码直接复制粘贴过来

![image-20220406172259585](https://www.ydlclass.com/doc21xnv/assets/image-20220406172259585.d9bdd516.png)

3zookeeper-consumer

pom

```xml
   <artifactId>zookeeper-consumer</artifactId>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>

    <dependencies>
        <!--springcloud 整合 zookeeper 组件-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <!--zk发现-->
            <artifactId>spring-cloud-starter-zookeeper-discovery</artifactId>
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
            <version>3.5.6</version>
            <exclusions>
                <exclusion>
                    <groupId>org.slf4j</groupId>
                    <artifactId>slf4j-log4j12</artifactId>
                </exclusion>
            </exclusions>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

    </dependencies>
```

yml

```yaml
server:
  port: 8005

spring:
  application:
    name: zookeeper-consumer
  cloud:
    zookeeper:
      connect-string: 127.0.0.1:2181 # zk地址
```

启动类

```java
package com.ydlclass.zk;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
@SpringBootApplication
@EnableDiscoveryClient
public class ConsumerApp {
    public static void main(String[] args) {
        SpringApplication.run(ConsumerApp.class,args);
    }
}
```

业务逻辑代码直接复制粘贴过来

![image-20220406185656845](https://www.ydlclass.com/doc21xnv/assets/image-20220406185656845.d3703bb1.png)

controller只改一个

```text
List<ServiceInstance> instances = discoveryClient.getInstances("zookeeper-provider");
```

测试：http://localhost:8005/order/add/5

![image-20220407144912417](https://www.ydlclass.com/doc21xnv/assets/image-20220407144912417.fd30484b.png)

## [](https://www.ydlclass.com/doc21xnv/springcloud/SpringCloud.html#七、consul服务注册与发现)七、Consul服务注册与发现

![image-20220407161804433](https://www.ydlclass.com/doc21xnv/assets/image-20220407161804433.7424151c.png)

### [](https://www.ydlclass.com/doc21xnv/springcloud/SpringCloud.html#_1、-是什么)1、 是什么：

- Consul 是由 HashiCorp 基于 GoLanguage 语言开发的，支持多数据中心，分布式高可用的服务发布和注册服务软件。
- 用于实现分布式系统的服务发现与配置。
- 使用起来也较 为简单。具有天然可移植性(支持Linux、windows和Mac OS X)；安装包仅包含一个可执行文件，方便部署 。
- Consul官网：https://www.consul.io/ Consul中文文档：https://www.springcloud.cc/spring-cloud-consul.html

### [](https://www.ydlclass.com/doc21xnv/springcloud/SpringCloud.html#_2、-怎么用)2、 怎么用：

#### ](https://www.ydlclass.com/doc21xnv/springcloud/SpringCloud.html#_1-准备)（1） 准备

![image-20220407161630689](https://www.ydlclass.com/doc21xnv/assets/image-20220407161630689.ac7c0b2d.png)

```text
consul.exe agent -dev
```

![image-20220407161746354](https://www.ydlclass.com/doc21xnv/assets/image-20220407161746354.b746e58c.png)

#### [](https://www.ydlclass.com/doc21xnv/springcloud/SpringCloud.html#_2-搭建-consul-provider)（2） 搭建 consul-provider

pom

```xml
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-consul-discovery</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

    </dependencies>
```

yml

```yaml
server:
  port: 8006

spring:
  application:
    name: consul-provider
  cloud:
    consul:
      host: 127.0.0.1
      port: 8500
      discovery:
        service-name: ${spring.application.name}
```

启动类

```java
package com.ydlclass.consul;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
@SpringBootApplication
@EnableDiscoveryClient
public class ProviderApp {
    public static void main(String[] args) {
        SpringApplication.run(ProviderApp.class,args);
    }
}
```

业务逻辑复制

![image-20220407163424691](https://www.ydlclass.com/doc21xnv/assets/image-20220407163424691.9008997e.png)

启动

![image-20220407162423209](https://www.ydlclass.com/doc21xnv/assets/image-20220407162423209.689703ea.png)

#### [](https://www.ydlclass.com/doc21xnv/springcloud/SpringCloud.html#_3-搭建-consul-consumer)（3）搭建 consul-consumer

pom

```xml
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-consul-discovery</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

    </dependencies>
```

yml

```yaml
server:
  port: 8007

spring:
  application:
    name: consul-consumer
  cloud:
    consul:
      host: 127.0.0.1
      port: 8500
      discovery:
        service-name: ${spring.application.name}
```

启动类

```java
package com.ydlclass.consul;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
@SpringBootApplication
@EnableDiscoveryClient
public class ConsumerApp {
    public static void main(String[] args) {
        SpringApplication.run(ConsumerApp.class,args);
    }
}
```

业务逻辑复制

![image-20220407164001822](https://www.ydlclass.com/doc21xnv/assets/image-20220407164001822.47a4a47b.png)

controller只改一句话

```text
List<ServiceInstance> instances = discoveryClient.getInstances("consul-provider");
```

启动

![image-20220407164133404](https://www.ydlclass.com/doc21xnv/assets/image-20220407164133404.c379b922.png)

最终测试调用成功即可：http://localhost:8007/order/add/9

![image-20220407164211124](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAY4AAAC6CAMAAACORlpBAAAAflBMVEX///8jIyMxMTE6OjotLS0aGhomJiggISQRERENDQ0zMzNAQEAAAAAfHx8HBweKjYf//twaAdRGREULdQDN//7c3NyRAcyKck1siJB+SS4gOns/ISQ8b5O4urueoqZmaGkgIU3D7v+XISRBtOMgNmVERI/yyotiNiTTnSQgIawFNpvTAAALc0lEQVR42u1dC0OjOhMlXsJjvyJulY9WW1DXte7//4N3ZsIjPAttVfb2HFoIZEhrTk5mEig6AbAgOP8DFgTnB7AgODfAguC4wIIAOkDHMXieUv5VwnG9pS2ef71wvKXB90HHYqD8K6dDLWm5cjZ8Ry0KPuj4euwzD3RMpsNNktHa9PLcG869iRPHSbIb5WUDp2eOkx0nw8vjLIvz63Ltjh+p1uImTqI7R61F5XHuDmXmjsFdvieq+ywyJ0kc0kcnJ7L5WGdZTsiytUXI48t7NPSX/Lp7HcsuON7EcfxERjveqnL7FjOy1BWTbU8TeN5su0XvUlf12RQJLumRyt3OoCNqwyM2vGgMyiU++k08YiNbR9E6Y0r6LPTeSVxiPOsWa/293jrb3HDiZpOtG3QM/SGBoaM3T1OeJNQudr1N6utd/ORv0qdySx/ouVSJbPJIhzpQJrNDUa+Nqkt6iw/uY3w4nQ7WRqSjcbA+VF+G6zjUkbHFfoAO0oaST8m6BdRfy81iWmcZreLsfHVQnoTQVE/ap1qS5ktVVW6Ltn5jNn3Rdp86qCA1pg4qiQujlVaT6dCNRREbSkfHlkj46B6PudVTQu9FHR0D7qk8zveSwtIutG7OeXZT0uFleXVYmr9+oaI/fJOget7R7rsfGjpeOF3b6F9kcuCdu1eqGuo+lPeYPhkGNiltlX4muVRtnVu2ohX3Yb73Fv/O2DqO15Qrh9PyKFez8jmPTy9t6gSXREpUfrRLnyarQzcgfkOPUlHzEXUzE2ctR6WvcrpnZxXZbpsPra2vtbFc+Caucv68vBMld69Uyx/Kf0le9e7jMTn4O6p6ouNPwlvnw6tsdg8fZMKdlY64Y3pLqbM6cDeiuP5pSw7L9DEkDtOkb8iVpFTZZPMYp6ye1Hc38TZ6i7ceN3w5yhV/oKjGJXY4q7IpEtQxpj/oTeUXspuoDl2/gsSxkOgO8jguX7R081kRTGfBWguKWapNnayZbX0towhRBymlUsfby3tAtUyF/7o7/EpYIs++Fpoq30ErLTaKbchpPItyJHSLHjN2re6buHNWB7Vbd5dajmDDTFBzjqJNSo74wLXpcd1vPbajpq7kaNXoI2aotKmMTUlsSgzFp6qjSUfQpSM3RAgfedRDhx5Dsrfp2CdT6PDzTFu+g1t64FNVv0qCyPjzwL2VNnR44l8oHYoN5b1r8R1PHLxR29Xc7KlhR0YdB81bZRyBafLaM70Xtex6T3qemMlMf8hZFDexRqRbSysbv0pISUpx5Jan3ml0BAG7jqDcCQbrNdTcWXUO0+lrOYlWah+3zqfdyKajfb71tWKmQxd0bGxX/rxPXk2stL87SERFHRQdZwX8KdRR23jsPD44j7sXbtrUp8dbVkVEnZSogzsrcQSKQ680MrVJNZmKimSPOqmD+BiPQjA5y0RgRf9l2ZjE1pRE0YqnNH/qVDoCqqVAVlxdQcSBVbGjdZ1ZrcwrylkbQWeJuQMyZhn5hkbhciLRUewxMc2irdAlz36IOoiiH7UrF99hqtaoQ5moKSpU88Z0NGwiUg/RI51VGTxRZd1IRLU5uFVkRRypqGjXpuvh1s86MHuP2Zaqlccl0gNpLksZt0CGurQpjMl3FJ2f4oIOM9Rhar6qM4pQydvaVdxaSUK0EfQsFOiaBA1A/gkahRf5tWW7fDuSzDZlZ7XJPNt3iCsPSRKB8dcftH765bwXkVXThrIL5XyIR+AGzi6da42aL1e8y9uirati0MH+mIckogNyIFvuk7aKXYLeiedREoGJEZW3VZUNJVSREAltfW+GOEQdDG3eVCs8DNTlAV3mlSvTkkkbutoJamMdyDAw0DwMzFvZpSYqdbTKb/RW6yw2w8C4NQzUJAUKWz8073FUK2vioRx3sI1f2kjmM28o0JWeR0bhKtpl1OPzlnt+6aRcauzc0MWDcET7FIk6mCOKXTlY4kAgDUQdigMx0hMbkowqmzIhJXGQG5swbDIdZa3r8tYr1od1oMUIg4Mq614t++zbcpLEyS0i6lMdp52yim4Oy8tJEvvrythEy0JbIpACq4g6a8+nP+JJP0WFjS5tuLfnpIR71JmrcrYtkkkyxVcfOUyKVOHIZaZGyaUXtoqKHa/oeMQy0hI3meLlC9U2vHVNFGx2/cifR0ejXglestej98rledg6pUaoeQpxH0ftDF2QUKNTrm5OIW6ybJMf+f6RtT4GqT8zj+YLF3ae6k5CGdbqU+ujyheF9BRvTWdFJ8xPOz21HQbjbAS34UhmGIYj+fuajaybG/lfgJFp/NbVSNVbozK5ptT4dz3xuqZz24fV7ThWo3mrE8+kzC/hY8kT7L3DimhwwGHG2tVkyaDF8Pnlup4csU+9djpuw56l92C1hLdHl7B495waNspof2xw3YQ44UxUdWmqtteirvmek2/DmpYeXDcfTljXz5RXo9aP2YydXhLSfQVQx7LQHBGCDqDROor3JKtgqn0vHDrxpCUIP3Uxq0BWgxbFq3z354Y9pbSygrAccHU+pDha7TTHZ0GzjM7Xn/13Owv9kVwYtKqq36JFSCM3tGy6pbSywv7PCe3v0OWq0WLsEka/+dhvA/HzyEX9VPMfYEFwAAAAAAAAAAAAAAAAAOAvxd12/fsB1bAYNkDGkuh4BR3fgKRCi44UdHwnH1DHoumA7/hWPhx0VguiA33VkuhAoLsoPhDoLoqPLh3wHd9HB4aBSx+VQx3L4uMVU4gAAAAA8NdijyoAHQAAdQAXx1peX3ce1AE2/mviWDFmnHf3/58/f2JuZq46prBheFg19o+z8cBvEPAJaNHhrI6eIVSAj8+JrEbpWK9BxycooHAOpY+wtzYdXScCOi6vjpX15lV7W9Kxmt5ZsTcHHaf3RxekQyKrPdRxsu+wu6QL0CEAHWd56wvTAd9xljqGOqtVi44JrlyoEDbWGLufE8v2RFaOvJr7x+hwKkcOOk6MrCb7hLmzMMBJ4w6w8VeMygEA6gAAqAMAAKgDaOO+tYU6jo3TZg3Vjtnf2zV/z6jpsHabdsAns3FvV31FR62S62Rj9n1WnamS1TniAB1niqNFR3smaxYbdbLQSk3HlbIxPbIaUsWJE4v3NXrVAVyWjinXlWyhgI4Z6rDu62nsN27oOY+Oe9Bxgjr6t6ddnrKdulmgju+g474J0HFpdaxOuVx43wq4oI5LquPkCZJWZAWBTKJj1aJh9P6qma58gI4ru+9nxoxudcf0qm//3Miqnj1pHsatDV+F+146nOZUFtTxhXwMkgNxAFeuDgCAOgAA6gCAK1PHpe9dAM4B2FiUOoaqd2g2fZwOeTKMPBymekZMNwHMb+wDdEx4NEyVqB690ExAHV+GBh0DnEADn6KrNej4LHWUv042P1Ne2be7zbreYXzHg/2MmG4CmMJHccWvdVWw+O3/vEfDPNTPiOkmoI4JdFhVbx3pPBxjugMpnxFz10kA59Mx406SukcaSUAdZ6tjIhNNf96fACb6jiYdw75jMLIyVV49I6abuOKnxkxXR/fGkdX8yMoaelduu5vALMukzuoLpwKgjvG+agU2AKgDAACoAwCgDgCAOq4CuJVkSeoAG/9tcZRz6N2ZQ+Ai//vJsW7zOWZv31XVmlcHTsFqxtFhdeAGkgtFVlPpGLqIBDouIopVeUNP63JT9Y8kpvzPJ9BxGXUM/tuU+uLsrBsX7nB71WU8RpOO+taFqd6jqHqo4yzf0Xp+VYeO1Tw6ynutQMeZEhlSx7x/wNW6aR10nOE7Vr2+w5n2TJJCCRh3XCCwqod8zciqupX6GB3F720eMCq/0LgDAKAOAIA6AAAAAAD4S3G3Xf/GcHhRhIAP0AEM0PEKOpZERwo6oA4A6oA8AERWEAcA3wF1AFDHFTlyTCECAAAAAAAAAAAAwGfgX2ekYuNyalvhAAAAAElFTkSuQmCC)

### [](https://www.ydlclass.com/doc21xnv/springcloud/SpringCloud.html#_3、-三个注册中心的异同)3、 三个注册中心的异同

| 组件      | 语言 | cap  | 健康检查 | 暴露接口 | cloud集成 |
| --------- | ---- | ---- | -------- | -------- | --------- |
| eureka    | java | ap   | 支持     | http     | 已经集成  |
| zookeeper | java | cp   | 支持     | tcp      | 已经集成  |
| consul    | go   | cp   | 支持     | http     | 已经集成  |

cap

c:consustency 强一致性

a:avalibility 可用性

p:partition tolerance 分区容忍性

## 八、Ribbon负载均衡服务调用

### 1、是什么

Netflix公司推出的http和TCP的客户端负载均衡工具。

ribbon:

 1简化远程调用代码

 2内置很多负载均衡算法

#### （1）服务端负载均衡

负载均衡算法在服务端

服务端维护服务列表

![image-20220407173053676](https://www.ydlclass.com/doc21xnv/assets/image-20220407173053676.2349faf0.png)

#### （2） 客户端负载均衡

负载均衡算法在客户端

客户端维护服务列表

![image-20220407173409352](https://www.ydlclass.com/doc21xnv/assets/image-20220407173409352.2bb522c4.png)

### 2、 如何使用

1.新版的eureka依赖以及集成了Ribbon依赖，所以可以不引用

```xml
  <!--Ribbon的依赖-->
  <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-netflix-ribbon</artifactId>
 </dependency>
```

![image-20220408112040713](https://www.ydlclass.com/doc21xnv/assets/image-20220408112040713.49705355.png)

2.声明restTemplate时@LoadBalanced

3.restTemplate请求远程服务时，ip端口替换为服务名

```java
String url="http://EUREKA-PROVIDER/goods/findById/"+id;
Goods goods = restTemplate.getForObject(url, Goods.class);
```

测试：

1.启动2个provider

controller

```java
goods.setTitle(goods.getTitle()+"|端口号："+port);
```

idea设置 能启动两份 provider

![image-20220408112658153](https://www.ydlclass.com/doc21xnv/assets/image-20220408112658153.df840762.png)

![image-20220408112814966](https://www.ydlclass.com/doc21xnv/assets/image-20220408112814966.37a23e92.png)

2.多次访问consumer

![image-20220408113212684](https://www.ydlclass.com/doc21xnv/assets/image-20220408113212684.cc4cce36.png)

![image-20220408113134662](https://www.ydlclass.com/doc21xnv/assets/image-20220408113134662.3d63cf60.png)

多次刷新，发现：ribbon客户端，默认使用轮询算法，经行负载均衡调用。

### [](https://www.ydlclass.com/doc21xnv/springcloud/SpringCloud.html#_3、ribbon-负载均衡策略)3、ribbon 负载均衡策略

| **内置负载均衡规则类**    | **规则描述**                                                 |
| ------------------------- | ------------------------------------------------------------ |
| RoundRobinRule            | 简单轮询服务列表来选择服务器。它是Ribbon默认的负载均衡规则。 |
| AvailabilityFilteringRule | 对以下两种服务器进行忽略：（1）在默认情况下，这台服务器如果3次连接失败，这台服务器就会被设置为“短路”状态。短路状态将持续30秒，如果再次连接失败，短路的持续时间就会几何级地增加。注意：可以通过修改配置loadbalancer.``.connectionFailureCountThreshold来修改连接失败多少次之后被设置为短路状态。默认是3次。（2）并发数过高的服务器。如果一个服务器的并发连接数过高，配置了AvailabilityFilteringRule规则的客户端也会将其忽略。并发连接数的上线，可以由客户端的``.``.ActiveConnectionsLimit属性进行配置。 |
| WeightedResponseTimeRule  | 为每一个服务器赋予一个权重值。服务器响应时间越长，这个服务器的权重就越小。这个规则会随机选择服务器，这个权重值会影响服务器的选择。 |
| ZoneAvoidanceRule         | 以区域可用的服务器为基础进行服务器的选择。使用Zone对服务器进行分类，这个Zone可以理解为一个机房、一个机架等。 |
| BestAvailableRule         | 忽略哪些短路的服务器，并选择并发数较低的服务器。             |
| RandomRule                | 随机选择一个可用的服务器。                                   |
| Retry                     | 重试机制的选择逻辑                                           |

### 4、 设置ribbon 负载均衡策略

#### （1）代码

consumer工程

1.MyRule 返回想要的规则即可

```java
package com.ydlclass.consumer.config;

import com.netflix.loadbalancer.IRule;
import com.netflix.loadbalancer.RandomRule;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * creste by ydlclass.itcast
 */
@Configuration
public class MyRule {
    @Bean
    public IRule rule(){
        return new RandomRule();
    }
}
```

2.启动类

```java
@RibbonClient(name ="EUREKA-PROVIDER",configuration = MyRule.class)
```

总结：

1.irule的具体实现类，看到他带的几个策略的写法。

2.仿照策略的写法，自己写策略。

3.调用不同的其他微服务时，可以采用不同的策略。

#### [](https://www.ydlclass.com/doc21xnv/springcloud/SpringCloud.html#_2-配置)(2) 配置

consumer工程

application.yml

```yaml
EUREKA-PROVIDER: #远程服务名
  ribbon:
    NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RandomRule #策略
```

作用：方便运维修改，重启。随时切换策略。

## 九、OpenFeign服务接口调用

### 1、概述

- Feign 是一个声明式的 REST 客户端，它用了**基于接口的注解方式**，很方便实现客户端像调用本地接口方法一样，进行远程调用。
- Feign 最初由 Netflix 公司提供，但不支持SpringMVC注解，后由 SpringCloud 对其封装，支持了SpringMVC注解，让使用者更易于接受。
- https://cloud.spring.io/spring-cloud-static/Hoxton.SR1/reference/htmlsingle/#spring-cloud-openfeign

### 2、快速入门

a.在消费端引入 open-feign 依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

b.编写Feign调用接口。**复制粘贴被调方的conreoller方法,加上类路径。**

GoodsFeignClient

```java
package com.ydlclass.consumer.feign;

import com.ydlclass.consumer.domain.Goods;
import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
@FeignClient("EUREKA-PROVIDER")
public interface GoodsFeign {
    @GetMapping("/goods/findById/{id}")
    public Goods findById(@PathVariable("id") Integer id);
}
```

c.在启动类 添加 @EnableFeignClients 注解，开启Feign功能

d.测试调用

OrderController

```java
package com.ydlclass.consumer.controller;

import com.ydlclass.consumer.domain.Goods;
import com.ydlclass.consumer.feign.GoodsFeign;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.cloud.client.ServiceInstance;
import org.springframework.cloud.client.discovery.DiscoveryClient;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import java.util.List;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
@RestController
@RequestMapping("/order")
public class OrderController {
    @Autowired
    RestTemplate restTemplate;

    @Autowired
    private DiscoveryClient discoveryClient;

    @Autowired
    GoodsFeign goodsFeign;

    @GetMapping("/add/{id}")
    public Goods add(@PathVariable("id") Integer id){
        //业务逻辑
        //1查询商品
        //2减库存
        //3支付
        //4物流
        //直接调用
        //String url="http://localhost:8000/goods/findById/"+id;
        //Goods goods = restTemplate.getForObject(url, Goods.class);

//        //1服务发现
//        List<ServiceInstance> instances = discoveryClient.getInstances("EUREKA-PROVIDER");
//        if(instances==null||instances.size()<=0){
//            return null;
//        }
//        //2通过某个策略拿到一个实例 算法
//        ServiceInstance serviceInstance = instances.get(0);
//        String host = serviceInstance.getHost();
//        int port = serviceInstance.getPort();
//        System.out.println(host);
//        System.out.println(port);
//
//        String url="http://"+host+":"+port+"/goods/findById/"+id;
//        Goods goods = restTemplate.getForObject(url, Goods.class);

        //ribbon
//        String url="http://EUREKA-PROVIDER/goods/findById/"+id;
//        Goods goods = restTemplate.getForObject(url, Goods.class);

        //feign调用
        Goods goods = goodsFeign.findById(id);

        return goods;
    }
}
```

### 3、其他设置

#### （1）超时设置

• Feign 底层依赖于 Ribbon 实现负载均衡和远程调用

![image-20220408171431530](https://www.ydlclass.com/doc21xnv/assets/image-20220408171431530.17dcc892.png)

• Ribbon默认1秒超时。

• 超时配置： yml中

```yaml
# 设置Ribbon的超时时间
ribbon:
  ConnectTimeout: 1000 # 连接超时时间 默认1s
  ReadTimeout: 3000 # 逻辑处理的超时时间 默认1s
```

测试：

1.连接超时，provider都停掉

![image-20220408172036937](https://www.ydlclass.com/doc21xnv/assets/image-20220408172036937.989a933a.png)

2.逻辑处理的超时时间

provider

```java
    @GetMapping("/findById/{id}")
    public Goods findById(@PathVariable("id") Integer id){
        Goods goods = goodsService.findById(id);
        goods.setTitle(goods.getTitle()+"|端口号："+port);
        
        //模拟业务逻辑比较繁忙
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return goods;
    }
```

![image-20220408172302166](https://www.ydlclass.com/doc21xnv/assets/image-20220408172302166.9b28bb4d.png)

#### [](https://www.ydlclass.com/doc21xnv/springcloud/SpringCloud.html#_2-日志记录)(2) 日志记录

1.Feign 只能记录 debug 级别的日志信息。

```text
# 设置当前的日志级别 debug，feign只支持记录debug级别的日志
logging:
  level:
    com.ydlclass: debug
```

• 定义Feign日志级别Bean

FeignLogConfig

```java
package com.ydlclass.consumer.config;

import feign.Logger;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class FeignLogConfig {


    /*
        NONE,不记录
        BASIC,记录基本的请求行，响应状态码数据
        HEADERS,记录基本的请求行，响应状态码数据，记录响应头信息
        FULL;记录完成的请求 响应数据


     */

    @Bean
    public Logger.Level level(){
        return Logger.Level.FULL;
    }
}
```

1. 启用该Bean：

GoodsFeignClient

```text
@FeignClient(value = "FEIGN-PROVIDER",configuration = FeignLogConfig.class
```

<img src="https://www.ydlclass.com/doc21xnv/assets/image-20220408182551922.f2ea6561.png" alt="image-20220408182551922" style="zoom:80%;" />

设置为headers

![image-20220408182713438](https://www.ydlclass.com/doc21xnv/assets/image-20220408182713438.68ea41ae.png)

## 十、Hystrix断路器 （豪猪）-保险丝

### 1、概述

重点：能让服务的调用方，够快的知道被调方挂了！不至于说让用户在等待。

Hystix 是 Netflix 开源的一个延迟和容错库，用于隔离访问远程服务、第三方库，防止出现级联失败（雪崩）。

雪崩：一个服务失败，导致整条链路的服务都失败的情形。

![image-20220411101214886](https://www.ydlclass.com/doc21xnv/assets/image-20220411101214886.e1894fb1.png)

![image-20220411104119551](https://www.ydlclass.com/doc21xnv/assets/image-20220411104119551.5dbb216d.png)

![image-20220411101248131](https://www.ydlclass.com/doc21xnv/assets/image-20220411101248131.23fdbc68.png)

![image-20220411101310179](https://www.ydlclass.com/doc21xnv/assets/image-20220411101310179.039fb050.png)

**Hystix 主要功能**

![image-20220411101741096](https://www.ydlclass.com/doc21xnv/assets/image-20220411101741096.ad89764b.png)

**隔离**

1. 线程池隔离

   没有hystrix,a重试100次，才知道c挂了！

   ![image-20220411102317061](https://www.ydlclass.com/doc21xnv/assets/image-20220411102317061.a569d3eb.png)

   使用了hystrix,更细分线程池，只需要重试40次，让a更快的知道c挂了

   ![image-20220411102720425](https://www.ydlclass.com/doc21xnv/assets/image-20220411102720425.31875f98.png)

2. 信号量隔离

   没有hystrix,a一个带着认证信息的线程，重试100次，才知道c挂了！

   ![image-20220411103036778](https://www.ydlclass.com/doc21xnv/assets/image-20220411103036778.e2674a15.png)

   使用了hystrix,更细分线程池，一个带着认证信息的线程,只需要重试40次，让a更快的知道c挂了

   ![image-20220411103152275](https://www.ydlclass.com/doc21xnv/assets/image-20220411103152275.926f5142.png)

**降级**：

服务提供方降级(异常，超时)

![image-20220411104951728](https://www.ydlclass.com/doc21xnv/assets/image-20220411104951728.51506d7b.png)

消费方降级

![image-20220411112946547](https://www.ydlclass.com/doc21xnv/assets/image-20220411112946547.e3d81fb6.png)

**熔断**

**限流**

 是有限流，但是，项目一般不用。nginx或者网关限流。

### 2、服务降级

**服务提供方**

a、在服务提供方，引入 hystrix 依赖

```xml
		<!-- hystrix -->
         <dependency>
             <groupId>org.springframework.cloud</groupId>
             <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
         </dependency>
```

b、方法

```java
/** 定义降级方法   返回特殊对象
     *  1方法的返回值要和原方法一致
     *  2方法参数和原方法一样
     */
    public Goods findById_fallback(Integer id){
        Goods goods=new Goods();
        goods.setGoodId(-1);
        goods.setTitle("provider提供方降级！");
        goods.setPrice(-9.9);
        goods.setStock(-10);

        return goods;
    }
```

c、使用 @HystrixCommand 注解配置降级方法

```java
@GetMapping("/findById/{id}")
    @HystrixCommand(fallbackMethod = "findById_fallback",commandProperties = {
            //设置Hystrix的超时时间，默认1s
            @HystrixProperty(name="execution.isolation.thread.timeoutInMilliseconds",value = "3000")
    })
    public Goods findById(@PathVariable("id") Integer id){
        Goods goods = goodsService.findById(id);
        goods.setTitle(goods.getTitle()+"|端口号："+port);

        //模拟出异常
//        int a=1/0;

        //模拟业务逻辑比较繁忙
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return goods;
    }
```

d、在启动类上开启Hystrix功能：@EnableCircuitBreaker

测试：http://localhost:9000/order/add/10

1.出错，服务方降级了

![image-20220411110206538](https://www.ydlclass.com/doc21xnv/assets/image-20220411110206538.c3351b31.png)

2.3000超时，服务方降级了

![image-20220411111020806](https://www.ydlclass.com/doc21xnv/assets/image-20220411111020806.407514d5.png)

**服务消费方**

a、feign 组件已经集成了 hystrix 组件。

b、

![image-20220411113035040](https://www.ydlclass.com/doc21xnv/assets/image-20220411113035040.f3c891d5.png)

c、定义feign 调用接口实现类，复写方法，即 降级方法

GoodsFeignClientFallback

```java
package com.ydlclass.consumer.feign;

import com.ydlclass.consumer.domain.Goods;
import org.springframework.stereotype.Component;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
@Component
public class GoodsFeignCallback implements GoodsFeign{
    @Override
    public Goods findById(Integer id) {
        Goods goods=new Goods();
        goods.setGoodId(-2);
        goods.setTitle("调用方降级了！");
        goods.setPrice(-5.5);
        goods.setStock(-5);
        return goods;
    }
}
```

d、在 @FeignClient 注解中使用 fallback 属性设置降级处理类。

GoodsFeignClient

```java
package com.ydlclass.consumer.feign;

import com.ydlclass.consumer.config.FeignLogConfig;
import com.ydlclass.consumer.domain.Goods;
import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
@FeignClient(value = "EUREKA-PROVIDER",configuration = FeignLogConfig.class,fallback = GoodsFeignCallback.class)
public interface GoodsFeign {
    @GetMapping("/goods/findById/{id}")
    public Goods findById(@PathVariable("id") Integer id);
}
```

e、配置开启

```text
# 开启feign对hystrix的支持
feign:
  hystrix:
    enabled: true
```

测试：停掉provider

http://localhost:9000/order/add/10

![image-20220411113635280](https://www.ydlclass.com/doc21xnv/assets/image-20220411113635280.1081b217.png)

### 3、熔断

![image-20220411114750082](https://www.ydlclass.com/doc21xnv/assets/image-20220411114750082.1a48c986.png)

测试：

provider

```java
package com.ydlclass.proviver.controller;

import com.netflix.hystrix.contrib.javanica.annotation.HystrixCommand;
import com.netflix.hystrix.contrib.javanica.annotation.HystrixProperty;
import com.ydlclass.proviver.domain.Goods;
import com.ydlclass.proviver.service.GoodsService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
@RestController
@RequestMapping("/goods")
public class GoodsController {
    @Autowired
    GoodsService goodsService;
    @Value("${server.port}")
    int port;

    @GetMapping("/findById/{id}")
    @HystrixCommand(fallbackMethod = "findById_fallback",commandProperties = {
            //设置Hystrix的超时时间，默认1s
            @HystrixProperty(name="execution.isolation.thread.timeoutInMilliseconds",value = "3000")
    })
    public Goods findById(@PathVariable("id") Integer id){
        Goods goods = goodsService.findById(id);
        goods.setTitle(goods.getTitle()+"|端口号："+port);

        if(id==1){
            //模拟出异常
            int a=1/0;
        }

        //模拟出异常
//        int a=1/0;

        //模拟业务逻辑比较繁忙
//        try {
//            Thread.sleep(5000);
//        } catch (InterruptedException e) {
//            e.printStackTrace();
//        }
        return goods;
    }

    /** 定义降级方法   返回特殊对象
     *  1方法的返回值要和原方法一致
     *  2方法参数和原方法一样
     */
    public Goods findById_fallback(Integer id){
        Goods goods=new Goods();
        goods.setGoodId(-1);
        goods.setTitle("provider提供方降级！");
        goods.setPrice(-9.9);
        goods.setStock(-10);

        return goods;
    }
}
```



访问两个接口

1. http://localhost:9000/order/add/10

![image-20220411115310796](https://www.ydlclass.com/doc21xnv/assets/image-20220411115310796.faba6ed0.png)

1. 多次访问 http://localhost:9000/order/add/1

![image-20220411115332277](https://www.ydlclass.com/doc21xnv/assets/image-20220411115332277.ce2b6c94.png)

3.导致10也不能访问了

![image-20220411115128870](https://www.ydlclass.com/doc21xnv/assets/image-20220411115128870.15162f17.png)

4.再过一会儿，半开状态

http://localhost:9000/order/add/10

![image-20220411115423173](https://www.ydlclass.com/doc21xnv/assets/image-20220411115423173.573f7494.png)

Hyst rix 熔断机制，用于监控微服务调用情况，当失败的情况达到预定的阈值（5秒失败20次），会打开断路器，拒绝所有请求，直到服务恢复正常为止。

```text
circuitBreaker.sleepWindowInMilliseconds：监控时间
circuitBreaker.requestVolumeThreshold：失败次数
circuitBreaker.errorThresholdPercentage：失败率
```

提供者controller中

```java
    @HystrixCommand(fallbackMethod = "findOne_fallback",commandProperties = {
            //设置Hystrix的超时时间，默认1s
            @HystrixProperty(name="execution.isolation.thread.timeoutInMilliseconds",value = "3000"),
            //监控时间 默认5000 毫秒
            @HystrixProperty(name="circuitBreaker.sleepWindowInMilliseconds",value = "5000"),
            //失败次数。默认20次
            @HystrixProperty(name="circuitBreaker.requestVolumeThreshold",value = "20"),
            //失败率 默认50%
            @HystrixProperty(name="circuitBreaker.errorThresholdPercentage",value = "50")

    })
```

### **4、熔断监控**-运维

![image-20200612072519498](https://www.ydlclass.com/doc21xnv/assets/image-20200612072519498.077b0382.png)

Hystrix 提供了 Hystrix-dashboard 功能，用于实时监控微服务运行状态。

但是Hystrix-dashboard只能监控一个微服务。

Netflix 还提供了 Turbine ，进行聚合监控。

![image-20200612072621556](https://www.ydlclass.com/doc21xnv/assets/image-20200612072621556.5e1b0aa0.png)

**Turbine聚合监控**

#### （1）搭建监控模块

**a、 创建监控模块**

创建hystrix-monitor模块，使用Turbine聚合监控多个Hystrix dashboard功能,

**b、引入Turbine聚合监控起步依赖**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>spring-cloud-parent</artifactId>
        <groupId>com.ydlclass</groupId>
        <version>1.0.0</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>hystrix-monitor</artifactId>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix-dashboard</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-turbine</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>


        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
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

**d、修改application.yml**

```yaml
spring:
  application:
    name: hystrix-monitor
server:
  port: 8769
turbine:
  combine-host-port: true
  # 配置需要监控的服务名称列表
  app-config: EUREKA-PROVIDER,EUREKA-CONSUMER
  cluster-name-expression: "'default'"
  aggregator:
    cluster-config: default
  #instanceUrlSuffix: /actuator/hystrix.stream
eureka:
  client:
    serviceUrl:
      defaultZone: http://localhost:8761/eureka/
hystrix:
  dashboard:
    proxy-stream-allow-list: "*"
```

**e、创建启动类**

```java
package com.ydlclass.monitor;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;
import org.springframework.cloud.netflix.hystrix.dashboard.EnableHystrixDashboard;
import org.springframework.cloud.netflix.turbine.EnableTurbine;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
@SpringBootApplication
@EnableEurekaClient
@EnableTurbine //开启Turbine 很聚合监控功能
@EnableHystrixDashboard //开启Hystrix仪表盘监控功能
public class HystrixMonitorApp {
    public static void main(String[] args) {
        SpringApplication.run(HystrixMonitorApp.class,args);
    }
}
```

#### [](https://www.ydlclass.com/doc21xnv/springcloud/SpringCloud.html#_2-修改被监控模块)（2）修改被监控模块

需要分别修改 hystrix-provider 和 hystrix-consumer 模块：

**a、导入依赖：**

```xml
		<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix-dashboard</artifactId>
        </dependency>
```

**b、配置Bean**

此处为了方便，将其配置在启动类中。

```java
	@Bean
    public ServletRegistrationBean getServlet() {
        HystrixMetricsStreamServlet streamServlet = new HystrixMetricsStreamServlet();
        ServletRegistrationBean registrationBean = new ServletRegistrationBean(streamServlet);
        registrationBean.setLoadOnStartup(1);
        registrationBean.addUrlMappings("/actuator/hystrix.stream");
        registrationBean.setName("HystrixMetricsStreamServlet");
        return registrationBean;
    }
```

**c、启动类上添加注解@EnableHystrixDashboard**

```java
@EnableDiscoveryClient
@EnableEurekaClient
@SpringBootApplication
@EnableFeignClients 
@EnableHystrixDashboard // 开启Hystrix仪表盘监控功能
public class ConsumerApp {


    public static void main(String[] args) {
        SpringApplication.run(ConsumerApp.class,args);
    }
    @Bean
    public ServletRegistrationBean getServlet() {
        HystrixMetricsStreamServlet streamServlet = new HystrixMetricsStreamServlet();
        ServletRegistrationBean registrationBean = new ServletRegistrationBean(streamServlet);
        registrationBean.setLoadOnStartup(1);
        registrationBean.addUrlMappings("/actuator/hystrix.stream");
        registrationBean.setName("HystrixMetricsStreamServlet");
        return registrationBean;
    }
}
```

#### [](https://www.ydlclass.com/doc21xnv/springcloud/SpringCloud.html#_3-启动测试)（3）启动测试

**a、启动服务：**

- eureka-server
- hystrix-provider
- hystrix-consumer
- hystrix-monitor

**b、访问：**

在浏览器访问http://localhost:8769/hystrix/ 进入Hystrix Dashboard界面

![1585421193757](https://www.ydlclass.com/doc21xnv/assets/1585421193757.41f28cbc.png)

界面中输入监控的Url地址 http://localhost:8769/turbine.stream，监控时间间隔2000毫秒和title，如下图

![image-20220411154048113](https://www.ydlclass.com/doc21xnv/assets/image-20220411154048113.1c256874.png)

- 实心圆：它有颜色和大小之分，分别代表实例的监控程度和流量大小。如上图所示，它的健康度从绿色、黄色、橙色、红色递减。通过该实心圆的展示，我们就可以在大量的实例中快速的发现故障实例和高压力实例。
- 曲线：用来记录 2 分钟内流量的相对变化，我们可以通过它来观察到流量的上升和下降趋势。

## 十一、zuul路由网关

zuul核心人员走了两个，zuul2的研发过久，spring公司等不及，自己研发的Gateway网关。

https://github.com/Netflix/zuul/wiki

## 十二、Gateway新一代网关

## 功能：路由+过滤

### 1、 **概述**

https://cloud.spring.io/spring-cloud-static/spring-cloud-gateway/2.2.1.RELEASE/reference/html/

不使用

存在的问题：

```text
解决缺点：1客户端需要记录不同微服务地址，增加客户端的复杂性
2每个后台微服务都需要认证
3http 发请求，涉及到跨域
4后台新增微服务，不能动态知道地址
```

![image-20220411172117753](https://www.ydlclass.com/doc21xnv/assets/image-20220411172117753.e3893d54.png)

使用了网关的话：

• 网关旨在为微服务架构提供一种简单而有效的统一的API路由管理方式。

• 在微服务架构中，不同的微服务可以有不同的网络地址，各个微服务之间通过互相调用完成用户请求，客户端可能通过调用N个微服务的接口完成一个用户请求。

• 网关就是系统的入口，封装了应用程序的内部结构，为客户端提供统一服务，一些与业务本身功能无关的公共逻辑可以在这里实现，诸如认证、鉴权、监控、缓存、负载均衡、流量管控、路由转发等

• 在目前的网关解决方案里，有Nginx+ Lua、Netflix Zuul/zuul2 、Spring Cloud Gateway等等

![image-20220411172632761](https://www.ydlclass.com/doc21xnv/assets/image-20220411172632761.c415bd79.png)

### 2、 **快速入门**

（1）搭建网关模块 api-gateway-server

（2）引入依赖：starter-gateway

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>gateway-parent</artifactId>
        <groupId>com.ydlclass</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>api-gateway-server</artifactId>


    <dependencies>
        <!--引入gateway 网关-->

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-gateway</artifactId>
        </dependency>

        <!-- eureka-client -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
    </dependencies>

</project>
```

（3）编写启动类

ApiGatewayApp

```java
package com.ydlclass.gateway;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;

@SpringBootApplication
@EnableEurekaClient
public class ApiGatewayApp {

    public static void main(String[] args) {
        SpringApplication.run(ApiGatewayApp.class,args);
    }

}
```

（4）编写配置文件

```yaml
server:
  port: 80

spring:
  application:
    name: api-gateway-server

  cloud:
    # 网关配置
    gateway:
      # 路由配置：转发规则
      routes: #集合。
      # id: 唯一标识。默认是一个UUID
      # uri: 转发路径
      # predicates: 条件,用于请求网关路径的匹配规则
      # filters：配置局部过滤器的

      - id: eureka-provider
        # 静态路由
        # uri: http://localhost:8001/
        # 动态路由
        uri: lb://GATEWAY-PROVIDER
        predicates:
        - Path=/goods/**
        filters:
        - AddRequestParameter=username,zhangsan

      - id: eureka-consumer
        # uri: http://localhost:9000
        uri: lb://GATEWAY-CONSUMER
        predicates:
        - Path=/order/**
        # 微服务名称配置
      discovery:
        locator:
          enabled: true # 设置为true 请求路径前可以添加微服务名称
          lower-case-service-id: true # 允许为小写


eureka:
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka
```

（5）启动测试

http://localhost/goods/findById/2

![image-20220411174704253](https://www.ydlclass.com/doc21xnv/assets/image-20220411174704253.4e93d626.png)

![image-20220411174649892](https://www.ydlclass.com/doc21xnv/assets/image-20220411174649892.b2b39013.png)

### 3、 **静态路由**

```text
uri: http://localhost:8000/
```

### 4、**动态路由**

![image-20220411180512597](https://www.ydlclass.com/doc21xnv/assets/image-20220411180512597.da56cec1.png)

#### （1）引入eureka-client配置

pom

```xml
		<dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
```

yml

```yaml
eureka:
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka
```

#### （2）修改uri属性：uri: lb://服务名称

```text
uri: lb://eureka-provider
```

测试：

http://localhost/goods/findById/2

![image-20220411180644761](https://www.ydlclass.com/doc21xnv/assets/image-20220411180644761.e575ff25.png)

### 5、 **微服务名称配置**

```yaml
spring:
  cloud:
    # 网关配置
    gateway:   
    # 微服务名称配置
      discovery:
        locator:
          enabled: true # 设置为true 请求路径前可以添加微服务名称
          lower-case-service-id: true # 允许为小写
```

测试：http://localhost/eureka-provider/goods/findById/2

![image-20220411180853933](https://www.ydlclass.com/doc21xnv/assets/image-20220411180853933.7e0bf46f.png)

### 6、过滤器

（1）两个维度：

内置过滤器 自定义过滤器

局部过滤器 全局过滤器

（2）过滤器种类：

内置局部过滤器

内置全局过滤器

自定义局部过滤器

自定义全局过滤器

![image-20200612075159655](https://www.ydlclass.com/doc21xnv/assets/image-20200612075159655.4dd53e5a.png)

- Gateway 支持过滤器功能，对请求或响应进行拦截，完成一些通用操作。

- Gateway 提供两种过滤器方式：“pre”和“post”

  ```text
  pre 过滤器，在转发之前执行，可以做参数校验、权限校验、流量监控、日志输出、协议转换等。
  post 过滤器，在响应之前执行，可以做响应内容、响应头的修改，日志的输出，流量监控等。
  ```

- Gateway 还提供了两种类型过滤器

  ```text
  GatewayFilter：局部过滤器，针对单个路由
  GlobalFilter ：全局过滤器，针对所有路由
  ```

内置过滤器 局部过滤器：

```text
        - id: gateway-provider
          #uri: http://localhost:8001/
          uri: lb://GATEWAY-PROVIDER
          predicates:
            - Path=/goods/**
          filters:
            - AddResponseHeader=foo, bar
```

![image-20220411182635931](https://www.ydlclass.com/doc21xnv/assets/image-20220411182635931.65d3b487.png)

内置过滤器 全局过滤器： route同级

```text
      default-filters:
        - AddResponseHeader=yld,itlils
```

![image-20220411182831552](https://www.ydlclass.com/doc21xnv/assets/image-20220411182831552.1af37136.png)

![image-20220411182910608](https://www.ydlclass.com/doc21xnv/assets/image-20220411182910608.586ba685.png)

**拓展：**

内置的过滤器工厂

这里简单将Spring Cloud Gateway内置的所有过滤器工厂整理成了一张表格。如下：

| 过滤器工厂                  | 作用                                                         | 参数                                                         |
| :-------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| AddRequestHeader            | 为原始请求添加Header                                         | Header的名称及值                                             |
| AddRequestParameter         | 为原始请求添加请求参数                                       | 参数名称及值                                                 |
| AddResponseHeader           | 为原始响应添加Header                                         | Header的名称及值                                             |
| DedupeResponseHeader        | 剔除响应头中重复的值                                         | 需要去重的Header名称及去重策略                               |
| Hystrix                     | 为路由引入Hystrix的断路器保护                                | `HystrixCommand`的名称                                       |
| FallbackHeaders             | 为fallbackUri的请求头中添加具体的异常信息                    | Header的名称                                                 |
| PrefixPath                  | 为原始请求路径添加前缀                                       | 前缀路径                                                     |
| PreserveHostHeader          | 为请求添加一个preserveHostHeader=true的属性，路由过滤器会检查该属性以决定是否要发送原始的Host | 无                                                           |
| RequestRateLimiter          | 用于对请求限流，限流算法为令牌桶                             | keyResolver、rateLimiter、statusCode、denyEmptyKey、emptyKeyStatus |
| RedirectTo                  | 将原始请求重定向到指定的URL                                  | http状态码及重定向的url                                      |
| RemoveHopByHopHeadersFilter | 为原始请求删除IETF组织规定的一系列Header                     | 默认就会启用，可以通过配置指定仅删除哪些Header               |
| RemoveRequestHeader         | 为原始请求删除某个Header                                     | Header名称                                                   |
| RemoveResponseHeader        | 为原始响应删除某个Header                                     | Header名称                                                   |
| RewritePath                 | 重写原始的请求路径                                           | 原始路径正则表达式以及重写后路径的正则表达式                 |
| RewriteResponseHeader       | 重写原始响应中的某个Header                                   | Header名称，值的正则表达式，重写后的值                       |
| SaveSession                 | 在转发请求之前，强制执行`WebSession::save`操作               | 无                                                           |
| secureHeaders               | 为原始响应添加一系列起安全作用的响应头                       | 无，支持修改这些安全响应头的值                               |
| SetPath                     | 修改原始的请求路径                                           | 修改后的路径                                                 |
| SetResponseHeader           | 修改原始响应中某个Header的值                                 | Header名称，修改后的值                                       |
| SetStatus                   | 修改原始响应的状态码                                         | HTTP 状态码，可以是数字，也可以是字符串                      |
| StripPrefix                 | 用于截断原始请求的路径                                       | 使用数字表示要截断的路径的数量                               |
| Retry                       | 针对不同的响应进行重试                                       | retries、statuses、methods、series                           |
| RequestSize                 | 设置允许接收最大请求包的大小。如果请求包大小超过设置的值，则返回 `413 Payload Too Large` | 请求包大小，单位为字节，默认值为5M                           |
| ModifyRequestBody           | 在转发请求之前修改原始请求体内容                             | 修改后的请求体内容                                           |
| ModifyResponseBody          | 修改原始响应体的内容                                         | 修改后的响应体内容                                           |
| Default                     | 为所有路由添加过滤器                                         | 过滤器工厂名称及值                                           |

**Tips：**每个过滤器工厂都对应一个实现类，并且这些类的名称必须以`GatewayFilterFactory`结尾，这是Spring Cloud Gateway的一个约定，例如`AddRequestHeader`对应的实现类为`AddRequestHeaderGatewayFilterFactory`。

------

1、AddRequestHeader GatewayFilter Factory

为原始请求添加Header，配置示例：

```yaml
spring:
  cloud:
    gateway:
      routes:
      - id: add_request_header_route
        uri: https://example.org
        filters:
        - AddRequestHeader=X-Request-Foo, Bar
```

为原始请求添加名为 `X-Request-Foo` ，值为 `Bar` 的请求头

2、AddRequestParameter GatewayFilter Factory

为原始请求添加请求参数及值，配置示例：

```yaml
spring:
  cloud:
    gateway:
      routes:
      - id: add_request_parameter_route
        uri: https://example.org
        filters:
        - AddRequestParameter=foo, bar
```

为原始请求添加名为foo，值为bar的参数，即：`foo=bar`

3、AddResponseHeader GatewayFilter Factory

为原始响应添加Header，配置示例：

```yaml
spring:
  cloud:
    gateway:
      routes:
      - id: add_response_header_route
        uri: https://example.org
        filters:
        - AddResponseHeader=X-Response-Foo, Bar
```

为原始响应添加名为 `X-Request-Foo` ，值为 `Bar` 的响应头

4、DedupeResponseHeader GatewayFilter Factory

DedupeResponseHeader可以根据配置的Header名称及去重策略剔除响应头中重复的值，这是Spring Cloud Greenwich SR2提供的新特性，低于这个版本无法使用。

我们在Gateway以及微服务上都设置了CORS（解决跨域）Header的话，如果不做任何配置，那么请求 -> 网关 -> 微服务，获得的CORS Header的值，就将会是这样的：

```bash
Access-Control-Allow-Credentials: true, true
Access-Control-Allow-Origin: https://musk.mars, https://musk.mars
```

可以看到这两个Header的值都重复了，若想把这两个Header的值去重的话，就需要使用到DedupeResponseHeader，配置示例：

```yaml
spring:
  cloud:
    gateway:
      routes:
      - id: dedupe_response_header_route
        uri: https://example.org
        filters:
        # 若需要去重的Header有多个，使用空格分隔
        - DedupeResponseHeader=Access-Control-Allow-Credentials Access-Control-Allow-Origin
```

去重策略：

- RETAIN_FIRST：默认值，保留第一个值
- RETAIN_LAST：保留最后一个值
- RETAIN_UNIQUE：保留所有唯一值，以它们第一次出现的顺序保留

若想对该过滤器工厂有个比较全面的了解的话，建议阅读该过滤器工厂的源码，因为源码里有详细的注释及示例，比官方文档写得还好：`org.springframework.cloud.gateway.filter.factory.DedupeResponseHeaderGatewayFilterFactory`

5、Hystrix GatewayFilter Factory

为路由引入Hystrix的断路器保护，配置示例：

```yaml
spring:
  cloud:
    gateway:
      routes:
      - id: hystrix_route
        uri: https://example.org
        filters:
        - Hystrix=myCommandName
```

Hystrix是Spring Cloud第一代容错组件，不过已经进入维护模式，未来Hystrix会被Spring Cloud移除掉，取而代之的是Alibaba Sentinel/Resilience4J。所以本文不做详细介绍了，感兴趣的话可以参考官方文档：

- [Hystrix GatewayFilter Factoryopen in new window](https://cloud.spring.io/spring-cloud-static/Greenwich.SR2/single/spring-cloud.html#hystrix)

6、FallbackHeaders GatewayFilter Factory

同样是对Hystrix的支持，上一小节所介绍的过滤器工厂支持一个配置参数：`fallbackUri`，该配置用于当发生异常时将请求转发到一个特定的uri上。而`FallbackHeaders`这个过滤工厂可以在转发请求到该uri时添加一个Header，这个Header的值为具体的异常信息。配置示例：

```yaml
spring:
  cloud:
    gateway:
      routes:
      - id: ingredients
        uri: lb://ingredients
        predicates:
        - Path=//ingredients/**
        filters:
        - name: Hystrix
          args:
            name: fetchIngredients
            fallbackUri: forward:/fallback
      - id: ingredients-fallback
        uri: http://localhost:9994
        predicates:
        - Path=/fallback
        filters:
        - name: FallbackHeaders
          args:
            executionExceptionTypeHeaderName: Test-Header
```

这里也不做详细介绍了，感兴趣可以参考官方文档：

- [FallbackHeaders GatewayFilter Factoryopen in new window](https://cloud.spring.io/spring-cloud-static/Greenwich.SR2/single/spring-cloud.html#fallback-headers)

7、PrefixPath GatewayFilter Factory

为原始的请求路径添加一个前缀路径，配置示例：

```yaml
spring:
  cloud:
    gateway:
      routes:
      - id: prefixpath_route
        uri: https://example.org
        filters:
        - PrefixPath=/mypath
```

该配置使访问`${GATEWAY_URL}/hello` 会转发到`https://example.org/mypath/hello`

8、PreserveHostHeader GatewayFilter Factory

为请求添加一个preserveHostHeader=true的属性，路由过滤器会检查该属性以决定是否要发送原始的Host Header。配置示例：

```yaml
spring:
  cloud:
    gateway:
      routes:
      - id: preserve_host_route
        uri: https://example.org
        filters:
        - PreserveHostHeader
```

如果不设置，那么名为 `Host` 的Header将由Http Client控制

9、RequestRateLimiter GatewayFilter Factory

用于对请求进行限流，限流算法为令牌桶。配置示例：

```yaml
spring:
  cloud:
    gateway:
      routes:
      - id: requestratelimiter_route
        uri: https://example.org
        filters:
        - name: RequestRateLimiter
          args:
            redis-rate-limiter.replenishRate: 10
            redis-rate-limiter.burstCapacity: 20
```

由于另一篇文章中已经介绍过如何使用该过滤器工厂实现网关限流，所以这里就不再赘述了：

- [Spring Cloud Gateway - 扩展open in new window](https://blog.51cto.com/zero01/2430532)

或者参考官方文档：

- [RequestRateLimiter GatewayFilter Factoryopen in new window](https://cloud.spring.io/spring-cloud-static/Greenwich.SR2/single/spring-cloud.html#_requestratelimiter_gatewayfilter_factory)

10、RedirectTo GatewayFilter Factory

将原始请求重定向到指定的Url，配置示例：

```yaml
spring:
  cloud:
    gateway:
      routes:
      - id: redirect_route
        uri: https://example.org
        filters:
        - RedirectTo=302, https://acme.org
```

该配置使访问 `${GATEWAY_URL}/hello` 会被重定向到 `https://acme.org/hello` ，并且携带一个 `Location:http://acme.org` 的Header，而返回客户端的HTTP状态码为302

注意事项：

- HTTP状态码应为3xx，例如301
- URL必须是合法的URL，该URL会作为`Location` Header的值

11、RemoveHopByHopHeadersFilter GatewayFilter Factory

为原始请求删除[IETFopen in new window](https://tools.ietf.org/html/draft-ietf-httpbis-p1-messaging-14#section-7.1.3)组织规定的一系列Header，默认删除的Header如下：

- Connection
- Keep-Alive
- Proxy-Authenticate
- Proxy-Authorization
- TE
- Trailer
- Transfer-Encoding
- Upgrade

可以通过配置去指定仅删除哪些Header，配置示例：

```yaml
spring:
  cloud:
    gateway:
      filter:
        remove-hop-by-hop:
          # 多个Header使用逗号（,）分隔
          headers: Connection,Keep-Alive
```

12、RemoveRequestHeader GatewayFilter Factory

为原始请求删除某个Header，配置示例：

```yaml
spring:
  cloud:
    gateway:
      routes:
      - id: removerequestheader_route
        uri: https://example.org
        filters:
        - RemoveRequestHeader=X-Request-Foo
```

删除原始请求中名为 `X-Request-Foo` 的请求头

13、RemoveResponseHeader GatewayFilter Factory

为原始响应删除某个Header，配置示例：

```yaml
spring:
  cloud:
    gateway:
      routes:
      - id: removeresponseheader_route
        uri: https://example.org
        filters:
        - RemoveResponseHeader=X-Response-Foo
```

删除原始响应中名为 `X-Request-Foo` 的响应头

14、RewritePath GatewayFilter Factory

通过正则表达式重写原始的请求路径，配置示例：

```yaml
spring:
  cloud:
    gateway:
      routes:
      - id: rewritepath_route
        uri: https://example.org
        predicates:
        - Path=/foo/**
        filters:
        # 参数1为原始路径的正则表达式，参数2为重写后路径的正则表达式
        - RewritePath=/foo/(?<segment>.*), /$\{segment}
```

该配置使得访问 `/foo/bar` 时，会将路径重写为`/bar` 再进行转发，也就是会转发到 `https://example.org/bar`。需要注意的是：由于YAML语法，需用`$\` 替换 `$`

15、RewriteResponseHeader GatewayFilter Factory

重写原始响应中的某个Header，配置示例：

```yaml
spring:
  cloud:
    gateway:
      routes:
      - id: rewriteresponseheader_route
        uri: https://example.org
        filters:
        # 参数1为Header名称，参数2为值的正则表达式，参数3为重写后的值
        - RewriteResponseHeader=X-Response-Foo, password=[^&]+, password=***
```

该配置的意义在于：如果响应头中 `X-Response-Foo` 的值为`/42?user=ford&password=omg!what&flag=true`，那么就会被按照配置的值重写成`/42?user=ford&password=***&flag=true`，也就是把其中的`password=omg!what`重写成了`password=***`

16、SaveSession GatewayFilter Factory

在转发请求之前，强制执行`WebSession::save`操作，配置示例：

```yaml
spring:
  cloud:
    gateway:
      routes:
      - id: save_session
        uri: https://example.org
        predicates:
        - Path=/foo/**
        filters:
        - SaveSession
```

主要用在那种像 Spring Session 延迟数据存储（数据不是立刻持久化）的，并希望在请求转发前确保session状态保存情况。如果你将Spring Secutiry于Spring Session集成使用，并想确保安全信息都传到下游机器，你就需要配置这个filter。

17、secureHeaders GatewayFilter Factory

secureHeaders过滤器工厂主要是参考了[这篇博客open in new window](https://blog.appcanary.com/2017/http-security-headers.html)中的建议，为原始响应添加了一系列起安全作用的响应头。默认会添加如下Headers（包括值）：

- `X-Xss-Protection:1; mode=block`
- `Strict-Transport-Security:max-age=631138519`
- `X-Frame-Options:DENY`
- `X-Content-Type-Options:nosniff`
- `Referrer-Policy:no-referrer`
- `Content-Security-Policy:default-src 'self' https:; font-src 'self' https: data:; img-src 'self' https: data:; object-src 'none'; script-src https:; style-src 'self' https: 'unsafe-inline'`
- `X-Download-Options:noopen`
- `X-Permitted-Cross-Domain-Policies:none`

如果你想修改这些Header的值，那么就需要使用这些Headers对应的后缀，如下：

- `xss-protection-header`
- `strict-transport-security`
- `frame-options`
- `content-type-options`
- `referrer-policy`
- `content-security-policy`
- `download-options`
- `permitted-cross-domain-policies`

配置示例：

```yaml
spring:
  cloud:
    gateway:
      filter:
        secure-headers:
          # 修改 X-Xss-Protection 的值为 2; mode=unblock
          xss-protection-header: 2; mode=unblock
```

如果想禁用某些Header，可使用如下配置：

```yaml
spring:
  cloud:
    gateway:
      filter:
        secure-headers:
          # 多个使用逗号（,）分隔
          disable: frame-options,download-options
```

18、SetPath GatewayFilter Factory

修改原始的请求路径，配置示例：

```yaml
spring:
  cloud:
    gateway:
      routes:
      - id: setpath_route
        uri: https://example.org
        predicates:
        - Path=/foo/{segment}
        filters:
        - SetPath=/{segment}
```

该配置使访问 `${GATEWAY_URL}/foo/bar` 时会转发到 `https://example.org/bar` ，也就是原本的`/foo/bar`被修改为了`/bar`

19、SetResponseHeader GatewayFilter Factory

修改原始响应中某个Header的值，配置示例：

```yaml
spring:
  cloud:
    gateway:
      routes:
      - id: setresponseheader_route
        uri: https://example.org
        filters:
        - SetResponseHeader=X-Response-Foo, Bar
```

将原始响应中 `X-Response-Foo` 的值修改为 `Bar`

20、SetStatus GatewayFilter Factory

修改原始响应的状态码，配置示例：

```yaml
spring:
  cloud:
    gateway:
      routes:
      - id: setstatusstring_route
        uri: https://example.org
        filters:
        # 字符串形式
        - SetStatus=BAD_REQUEST
      - id: setstatusint_route
        uri: https://example.org
        filters:
        # 数字形式
        - SetStatus=401
```

SetStatusd的值可以是数字，也可以是字符串。但一定要是Spring `HttpStatus` 枚举类中的值。上面这两种配置都可以返回401这个HTTP状态码。

21、StripPrefix GatewayFilter Factory

用于截断原始请求的路径，配置示例：

```yaml
spring:
  cloud:
    gateway:
      routes:
      - id: nameRoot
        uri: http://nameservice
        predicates:
        - Path=/name/**
        filters:
        # 数字表示要截断的路径的数量
        - StripPrefix=2
```

如上配置，如果请求的路径为 `/name/bar/foo` ，那么则会截断成`/foo`后进行转发 ，也就是会截断2个路径。

22、Retry GatewayFilter Factory

针对不同的响应进行重试，例如可以针对HTTP状态码进行重试，配置示例：

```yaml
spring:
  cloud:
    gateway:
      routes:
      - id: retry_test
        uri: http://localhost:8080/flakey
        predicates:
        - Host=*.retry.com
        filters:
        - name: Retry
          args:
            retries: 3
            statuses: BAD_GATEWAY
```

可配置如下参数：

- `retries`：重试次数
- `statuses`：需要重试的状态码，取值在 `org.springframework.http.HttpStatus` 中
- `methods`：需要重试的请求方法，取值在 `org.springframework.http.HttpMethod` 中
- `series`：HTTP状态码序列，取值在 `org.springframework.http.HttpStatus.Series` 中

23、RequestSize GatewayFilter Factory

设置允许接收最大请求包的大小，配置示例：

```yaml
spring:
  cloud:
    gateway:
      routes:
      - id: request_size_route
      uri: http://localhost:8080/upload
      predicates:
      - Path=/upload
      filters:
      - name: RequestSize
        args:
          # 单位为字节
          maxSize: 5000000
```

如果请求包大小超过设置的值，则会返回 `413 Payload Too Large`以及一个`errorMessage`

24、Modify Request Body GatewayFilter Factory

在转发请求之前修改原始请求体内容，该过滤器工厂只能通过代码配置，不支持在配置文件中配置。代码示例：

```java
@Bean
public RouteLocator routes(RouteLocatorBuilder builder) {
    return builder.routes()
        .route("rewrite_request_obj", r -> r.host("*.rewriterequestobj.org")
            .filters(f -> f.prefixPath("/httpbin")
                .modifyRequestBody(String.class, Hello.class, MediaType.APPLICATION_JSON_VALUE,
                    (exchange, s) -> return Mono.just(new Hello(s.toUpperCase())))).uri(uri))
        .build();
}
 
static class Hello {
    String message;
 
    public Hello() { }
 
    public Hello(String message) {
        this.message = message;
    }
 
    public String getMessage() {
        return message;
    }
 
    public void setMessage(String message) {
        this.message = message;
    }
}
```

> Tips：该过滤器工厂处于 **BETA** 状态，未来API可能会变化，生产环境请慎用

25、Modify Response Body GatewayFilter Factory

可用于修改原始响应体的内容，该过滤器工厂同样只能通过代码配置，不支持在配置文件中配置。代码示例：

```java
@Bean
public RouteLocator routes(RouteLocatorBuilder builder) {
    return builder.routes()
        .route("rewrite_response_upper", r -> r.host("*.rewriteresponseupper.org")
            .filters(f -> f.prefixPath("/httpbin")
                .modifyResponseBody(String.class, String.class,
                    (exchange, s) -> Mono.just(s.toUpperCase()))).uri(uri)
        .build();
}
```

> Tips：该过滤器工厂处于 **BETA** 状态，未来API可能会变化，生产环境请慎用

26、Default Filters

Default Filters用于为所有路由添加过滤器工厂，也就是说通过Default Filter所配置的过滤器工厂会作用到所有的路由上。配置示例：

```yaml
spring:
  cloud:
    gateway:
      default-filters:
      - AddResponseHeader=X-Response-Default-Foo, Default-Bar
      - PrefixPath=/httpbin
```

------

官方文档：https://cloud.spring.io/spring-cloud-static/spring-cloud-gateway/2.1.0.RELEASE/single/spring-cloud-gateway.html#_gatewayfilter_factories

#### [#](https://www.ydlclass.com/doc21xnv/springcloud/SpringCloud.html#自定义过滤器)自定义过滤器

1、自定义 局部过滤器

麻烦，很少。

```java
//
// Source code recreated from a .class file by IntelliJ IDEA
// (powered by FernFlower decompiler)
//

package org.springframework.cloud.gateway.filter.factory;

import java.util.Arrays;
import java.util.List;
import org.springframework.cloud.gateway.filter.GatewayFilter;
import org.springframework.cloud.gateway.filter.GatewayFilterChain;
import org.springframework.cloud.gateway.support.GatewayToStringStyler;
import org.springframework.cloud.gateway.support.ServerWebExchangeUtils;
import org.springframework.http.server.reactive.ServerHttpRequest;
import org.springframework.util.StringUtils;
import org.springframework.web.server.ServerWebExchange;
import reactor.core.publisher.Mono;

public class StripPrefixGatewayFilterFactory extends AbstractGatewayFilterFactory<StripPrefixGatewayFilterFactory.Config> {
    public static final String PARTS_KEY = "parts";

    public StripPrefixGatewayFilterFactory() {
        super(StripPrefixGatewayFilterFactory.Config.class);
    }

    public List<String> shortcutFieldOrder() {
        return Arrays.asList("parts");
    }

    public GatewayFilter apply(StripPrefixGatewayFilterFactory.Config config) {
        return new GatewayFilter() {
            public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
                ServerHttpRequest request = exchange.getRequest();
                ServerWebExchangeUtils.addOriginalRequestUrl(exchange, request.getURI());
                String path = request.getURI().getRawPath();
                String[] originalParts = StringUtils.tokenizeToStringArray(path, "/");
                StringBuilder newPath = new StringBuilder("/");

                for(int i = 0; i < originalParts.length; ++i) {
                    if (i >= config.getParts()) {
                        if (newPath.length() > 1) {
                            newPath.append('/');
                        }

                        newPath.append(originalParts[i]);
                    }
                }

                if (newPath.length() > 1 && path.endsWith("/")) {
                    newPath.append('/');
                }

                ServerHttpRequest newRequest = request.mutate().path(newPath.toString()).build();
                exchange.getAttributes().put(ServerWebExchangeUtils.GATEWAY_REQUEST_URL_ATTR, newRequest.getURI());
                return chain.filter(exchange.mutate().request(newRequest).build());
            }

            public String toString() {
                return GatewayToStringStyler.filterToStringCreator(StripPrefixGatewayFilterFactory.this).append("parts", config.getParts()).toString();
            }
        };
    }

    public static class Config {
        private int parts;

        public Config() {
        }

        public int getParts() {
            return this.parts;
        }

        public void setParts(int parts) {
            this.parts = parts;
        }
    }
}
```

2、自定义 全局过滤器

需求：1黑客ip，直接给你拒接掉。

 2某些请求路径 如“goods/findById”,危险操作，记录日志。

自定义全局过滤器步骤：

```text
1. 定义类实现 GlobalFilter 和 Ordered接口
2. 复写方法
3. 完成逻辑处理
```

IpFilter

```java
package com.ydlclass.gateway.filter;

import org.springframework.cloud.gateway.filter.GatewayFilterChain;
import org.springframework.cloud.gateway.filter.GlobalFilter;
import org.springframework.core.Ordered;
import org.springframework.http.HttpStatus;
import org.springframework.http.server.reactive.ServerHttpRequest;
import org.springframework.http.server.reactive.ServerHttpResponse;
import org.springframework.stereotype.Component;
import org.springframework.web.server.ServerWebExchange;
import reactor.core.publisher.Mono;

import java.net.InetAddress;
import java.net.InetSocketAddress;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
@Component
public class IpFilter implements GlobalFilter, Ordered {

    //写业务逻辑
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        //1黑客ip，直接给你拒接掉。

        //拿到请求和响应，为所欲为
        ServerHttpRequest request = exchange.getRequest();
        ServerHttpResponse response = exchange.getResponse();

        InetSocketAddress remoteAddress = request.getRemoteAddress();
        String hostName = remoteAddress.getHostName();
        System.out.println(hostName);
        InetAddress address = remoteAddress.getAddress();
        String hostAddress = address.getHostAddress();
        System.out.println(hostAddress);
        String hostName1 = address.getHostName();
        System.out.println(hostName1);

        if (hostAddress.equals("192.168.31.11")) {
            //拒绝
            response.setStatusCode(HttpStatus.UNAUTHORIZED);
            return response.setComplete();
        }

        //走完了，该到下一个过滤器了
        return chain.filter(exchange);
    }

    //返回数值，越小越先执行
    @Override
    public int getOrder() {
        return 0;
    }
}
```

UrlFilter

```java
package com.ydlclass.gateway.filter;

import org.springframework.cloud.gateway.filter.GatewayFilterChain;
import org.springframework.cloud.gateway.filter.GlobalFilter;
import org.springframework.core.Ordered;
import org.springframework.http.server.reactive.ServerHttpRequest;
import org.springframework.http.server.reactive.ServerHttpResponse;
import org.springframework.stereotype.Component;
import org.springframework.web.server.ServerWebExchange;
import reactor.core.publisher.Mono;

import java.net.URI;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
@Component
public class UrlFilter implements GlobalFilter, Ordered {
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        //某些请求路径 如“goods/findById”,危险操作，记录日志。

        //拿到请求和响应，为所欲为
        ServerHttpRequest request = exchange.getRequest();
        ServerHttpResponse response = exchange.getResponse();

        URI uri = request.getURI();
        String path = uri.getPath();
        System.out.println(path);

        if(path.contains("goods/findById")){
            //打日志
            System.out.println("path很危险！");
        }

        return chain.filter(exchange);
    }

    @Override
    public int getOrder() {
        return 1;
    }
}
```

测试：

1、ipfilter

![image-20220411191527069](https://www.ydlclass.com/doc21xnv/assets/image-20220411191527069.40d06e6f.png)

把自己的ip设置为拒绝访问的话

![image-20220411191641610](https://www.ydlclass.com/doc21xnv/assets/image-20220411191641610.d8bde2dd.png)

2、

![image-20220411192132436](https://www.ydlclass.com/doc21xnv/assets/image-20220411192132436.09d8d5e2.png)

## 十三、SpringCloud Config分布式配置中心

13-16组件，超大型公司才会用到

国内BATJ,或者国外FLAG:facebook linkedin amazon google

处理大规模微服务的技术。

### 1、Config 概述

![image-20220418081959807](https://www.ydlclass.com/doc21xnv/assets/image-20220418081959807.439382b4.png)

Spring Cloud Config 解决了在分布式场景下多环境配置文件的管理和维护。

好处：

• 集中管理配置文件

• 不同环境不同配置，动态化的配置更新

• 配置信息改变时，不需要重启即可更新配置信息到服务

### 2、Config 快速入门

![image-20220418082533367](https://www.ydlclass.com/doc21xnv/assets/image-20220418082533367.271fa629.png)

**config-server：**

1. 使用gitee创建远程仓库，上传配置文件

   config-dev.yml

2. 搭建 config-server 模块

   ConfigServerApp

   ```java
   package com.ydlclass.config;
   
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.cloud.config.server.EnableConfigServer;
   
   @SpringBootApplication
   @EnableConfigServer // 启用config server功能
   public class ConfigServerApp {
   
       public static void main(String[] args) {
           SpringApplication.run(ConfigServerApp.class,args);
       }
   }
   ```

3. 导入 config-server 依赖

   pom

   ```xml
    		<!-- config-server -->
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-config-server</artifactId>
           </dependency>
   ```

4. 编写配置，设置 gitee 远程仓库地址

   application.yml

   ```yaml
   server:
     port: 9527
   
   spring:
     application:
       name: config-server
     # spring cloud config
     cloud:
       config:
         server:
           # git 的 远程仓库地址
           git:
             uri: https://gitee.com/ydlclass_cch/ydlclass-configs.git
         label: master # 分支配置
   ```

5. 测试访问远程配置文件

   http://localhost:9527/master/provider-dev.yml

![image-20220418083733698](https://www.ydlclass.com/doc21xnv/assets/image-20220418083733698.508dfd18.png)

**config-client：** provider

1. 导入 starter-config 依赖

   ```xml
   		<!--config client -->
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-config</artifactId>
           </dependency>
   ```

2. 配置config server 地址，读取配置文件名称等信息

   bootstrap.yml

   ```yaml
   # 配置config-server地址
   # 配置获得配置文件的名称等信息
   spring:
     cloud:
       config:
         # 配置config-server地址
         uri: http://localhost:9527
         # 配置获得配置文件的名称等信息
         name: provider # 文件名
         profile: dev # profile指定，  config-dev.yml
         label: master # 分支
   ```

3. 获取配置值

   GoodsController

   ```java
       @Value("${ydlclass}")
       private String ydlclass;
       
       goods.setTitle(goods.getTitle()+"|端口号："+port+"|ydlclass"+ydlclass);
   ```

4. 启动测试

   http://localhost:8000/goods/findById/9

![image-20220418084608302](https://www.ydlclass.com/doc21xnv/assets/image-20220418084608302.978afefa.png)

**Config 客户端刷新**

![image-20220418085057098](https://www.ydlclass.com/doc21xnv/assets/image-20220418085057098.ae365b9f.png)

1. 在 config 客户端引入 actuator 依赖

   ```xml
   		<dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-actuator</artifactId>
           </dependency>
   ```

2. 获取配置信息类上，添加 @RefreshScope 注解

   GoodsController

   ```java
   @RefreshScope // 开启刷新功能
   ```

3. 添加配置

   bootstrap.yml

```yaml
management:
  endpoints:
    web:
      exposure:
        include: '*'
```

1. 使用curl工具发送post请求

   curl -X POST http://localhost:8000/actuator/refresh

   ![image-20220418085907801](https://www.ydlclass.com/doc21xnv/assets/image-20220418085907801.edb89cde.png)

测试： 值已经变了

![image-20220418085930400](https://www.ydlclass.com/doc21xnv/assets/image-20220418085930400.9adcb0c7.png)

### 3、Config 集成Eureka

![image-20220418090254336](https://www.ydlclass.com/doc21xnv/assets/image-20220418090254336.8b4469ba.png)

**config-client配置：**

pom

```xml
        <!-- eureka-client -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
```

ProviderApp

```java
@EnableEurekaClient
```

bootstrap.yml

```yaml
# 配置config-server地址
# 配置获得配置文件的名称等信息
spring:
  cloud:
    config:
      # 配置config-server地址
      #uri: http://localhost:9527
      # 配置获得配置文件的名称等信息
      name: config # 文件名
      profile: dev # profile指定，  config-dev.yml
      label: master # 分支
      discovery:
        enabled: true
        service-id: CONFIG-SERVER

management:
  endpoints:
    web:
      exposure:
        include: '*'
```

**config-server配置:**

pom

```xml
    <!-- eureka-client -->
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
    </dependency>
```

ConfigServerApp

```java
@EnableEurekaClient
```

application.yml

```yaml
eureka:
   client:
    service-url:
      defaultZone: http://localhost:8761/eureka
```

测试：

![image-20220418093808497](https://www.ydlclass.com/doc21xnv/assets/image-20220418093808497.b6035ff7.png)

## 十四、SpringCloud Bus消息总线

远程的配置文件更新了，运维只需要发一个请求，所有用到这个配置文件的几百个应用更新了。

### 1、Bus 概述

![image-20220418094548555](https://www.ydlclass.com/doc21xnv/assets/image-20220418094548555.e5074738.png)

Spring Cloud Bus 是用轻量的消息中间件将分布式的节点连接起来，可以用于广播配置文件的更改或者服务的监控管理。关键的思想就是，消息总线可以为微服务做监控，也可以实现应用程序之间相通信。

Spring Cloud Bus 可选的消息中间件包括 RabbitMQ 和 Kafka 。

### 2、RabbitMQ 回顾

message queue

异步、解耦、削峰填谷

![image-20200613074044506](https://www.ydlclass.com/doc21xnv/assets/image-20200613074044506.2b1f2272.png)

![image-20200613074052830](https://www.ydlclass.com/doc21xnv/assets/image-20200613074052830.5aedea61.png)

RabbitMQ 提供了 6 种工作模式：简单模式、work queues、Publish/Subscribe 发布与订阅模式、Routing

路由模式、Topics 主题模式、RPC 远程调用模式（远程调用，不太算 MQ；暂不作介绍）。

![image-20200613074109383](https://www.ydlclass.com/doc21xnv/assets/image-20200613074109383.dc1a9615.png)

### 3、Bus 快速入门-运维

![image-20220418094548555](https://www.ydlclass.com/doc21xnv/assets/image-20220418094548555.e5074738.png)

1. 分别在 config-server 和 config-client中引入 bus依赖：bus-amqp

   ```xml
           <!-- bus -->
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-bus-amqp</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-actuator</artifactId>
           </dependency>
   ```

2. 分别在 config-server 和 config-client中配置 RabbitMQ

   server 的配置文件

   ```yaml
   server:
     port: 9527
   
   spring:
     application:
       name: config-server
     # spring cloud config
     cloud:
       config:
         server:
           # git 的 远程仓库地址
           git:
             uri: https://gitee.com/ydlclass_cch/ydlclass-configs.git
         label: master # 分支配置
     #配置rabbitmq信息
     rabbitmq:
       host: localhost
       port: 5672
       username: guest
       password: guest
       virtual-host: /
   
   
   # 将自己注册到eureka中
   eureka:
     client:
       service-url:
         defaultZone: http://localhost:8761/eureka
   
   
   
   # 暴露bus的刷新端点
   management:
     endpoints:
       web:
         exposure:
           include: 'bus-refresh'
   ```

   client的配置文件 bootstrap.yml

   ```yaml
   # 配置config-server地址
   # 配置获得配置文件的名称等信息
   spring:
     cloud:
       config:
         # 配置config-server地址
         # uri: http://localhost:9527
         # 配置获得配置文件的名称等信息
         name: config # 文件名
         profile: dev # profile指定，  config-dev.yml
         label: master # 分支
         # 从注册中心去寻找config-server地址
         discovery:
           enabled: true
           service-id: CONFIG-SERVER
     #配置rabbitmq信息
     rabbitmq:
       host: localhost
       port: 5672
       username: guest
       password: guest
       virtual-host: /
   
   management:
     endpoints:
       web:
         exposure:
           include: '*'
   ```

3. 在config-server中设置暴露监控断点：bus-refresh

   ```yaml
   # 暴露bus的刷新端点
   management:
     endpoints:
       web:
         exposure:
           include: 'bus-refresh'
   ```

bus自动在mq中建立交换机

![image-20220418095857082](https://www.ydlclass.com/doc21xnv/assets/image-20220418095857082.c146dc82.png)

只要微服务起起来，自动在mq中，建立队列，和交换机绑定

![image-20220418100013437](https://www.ydlclass.com/doc21xnv/assets/image-20220418100013437.ff032e6b.png)

启动测试

1.git配置改变

![image-20220418104835631](https://www.ydlclass.com/doc21xnv/assets/image-20220418104835631.9d49fe01.png)

2.往配置中心发请求

curl -X POST http://localhost:9527/actuator/bus-refresh

![image-20220418104622908](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAnIAAAA5CAYAAACyLsYTAAAHTElEQVR4nO3dW27kKhCAYXeURy/KS/cCsqeehyPrIBqKAgpwtf9PijTxhauNqzE9ee37/t4AAADgzs/qAgAAAKANgRwAAIBTBHIAAABOEcgBAAA4RSAHAADgFIEcAACAUwRyAAAATnUHcud5WpRjGe/lBzBXacxgTPlu9K+M9pnvte/7O9Xwx3GoEznPM3l8mO61P7VttVz5a4/pyX/b2tuj93zPRvbL6nL03j+5ATU+r5RmbnyQBuw79MkIpX6WxsKVbbI6/x7S9am5xuNjpH25fMLjvbbjDLTPGr+phreKqK/BPkw/te2bPDmoWmH19TTy02fv/ZM7NtwW7y/9fm0L80ilqfGEe2X17ISUv4f211yPtYF16p6K96Ofh+urxuz61OTHGrkKmhu8pZOP4+i6OHrP37bxg9eMgGcFi7YvpR/WzTJozT0UtW2ZK4e2fLWz/j37Vxl9ffTkP7Nc1pMDVkpB3iyex9+cbwngtu3/62Lms6am/X5SB+deDUjT0HcdSDVK5f/mGcRv4f0a9OQb7wVpfAuPaXmtijJN+69G/8qe0j53rOPv6gJsW34NTm5/br1Qbi1F7xrAktwApFlDpFn/oa2btK5Dk388ldvSvtJritxUcW//xsfUrtdM1SWV36r1nWHesweRWa+uNdeW1F+p/am+vOMgLK3hSh1Tu8axtgza+zO3PzxGc//3kspeO+vbUrbaMbjl+s0dU8p/5vgrqa2LxfO7NT6o7Z+e9FP7S8ck677v+9vi5+/vT7093Na7//o9d47mfKn8pfRqyl+bTqputeW3qr9FPVrLJ+WvSVP63Sr/UplKbRv/aK6D1vRb+0R7j7SUr+f6LNWl5tpprV9P+TTXX+53i/uv5/6wGF9q2q3Unz391LKv5dq1HL/vMv623n8W8UFp/JwRH7Smr61fqSxmM3KWn9xb15lpz6+dzdm2tWtwrD/BWrZv6ZOTFW3/9i6O7s2/J91RLO/JEbNzo9sg7jtPr4BK16/F/Xfn/qydGS/Nykvje++4kGp/q/GnNf8R+dSW3/r+aynLqPF7RPot18vyV6vxjXltC/VcoJr0JanAwJPe+mvkXkfO5ukBXSOs1+o6eguEnmDl/TdjfLFIb+Q1u3r8W53/SDOfX6OUllhZ1C/734+0fjppObeUv3WEmyujpvweH2RW/Ztyl7a4Szm+De167y853KF/Ro4vJZq8esszYibPyur8Z5Rh9PU1842AZn9L/T7++xFpWnpE5NqSZs05sz+h3O0T0ej2bTnPuo2kCz/1iUfK/479l3o1YSHXNqsfDCWl+kv9PXt861Vz/a6cDeo9R2p/Tf/l9ucmKkbILVqvGX9K+y3GrpXPRE3/Sb/PMHOGziqvj7/sUFoj1vLpR/utkFwZcue3nJs6Jj4+lX7NttINXXoYW+QV1kfaH4r7WHtuql6l41Lb4zy0/asJPKRrXNOmmnNzx7TSXn8159ceV3uP1S5biM/TlKVmjLquj5Zzw/OlOtQ8mHJ5SPvjY6R2qbmHW8a3OP+e8UW7T3tuan/uXtb2s3bG70pTM8a1XIPhubk+vuP4q7n/pPr1jK+1z6/UMdr7Z3T6pfHxte/7+yOlTh4+1Uu8lx/Af3rv5d5ADr7RvzLa5x74yw4AvtL16bXn9UXt+hZ8F/pXRvvcw5AZOQAAAIzHjBwAAIBTBHIAAABOEcgBAAA4RSAHAADgFIEcAACAUwRyAAAAThHIAQAAOEUgBwAA4BSBHAAAgFMEcgAAAE4RyAEAADhFIAcAAOAUgRwAAIBTBHIAAABOEcgBAAA4RSAHAADgFIEcAACAU92B3HmeFuVYxnv5AQDAc732fX+ngpnjONSJnOeZPD5M99qf2rZarvy1x/Tkv23t7dF7vmcj+wUAgLv7uR6E4Y+VK60wzdS2b3KeJ7N8Ex3HQXsDAB6LNXIVNAFDS4DaG0BbBOCjg6GR6RPMAQCe6icVAORek+Yelt4fpKXy8/ru/rxfgwAAtPhdXYBtS8/WhIFTvD+33i63/q53DWBJLoCQAgvNmsFw7Zumbrk6SfUP98Vr7VraN1eHVPq58tXmHx9D0A0AeAp1IDdyoX/p4Z/aH75OvGYLc8FJKXDQSgUKUvrx8al/p8oc5yflKZ1fKl98fnxcS/vGv0vpS+XT9m+MYA4A8CRma+QsX221rjPTni/N5uSs/BKDdVBi2b6lmU8r2v4liAMAPMnyV6upAMpqBk2bviQ14+RJb/01cq+AZyOIAwA8za/0aqtW62utUv69D2dt/TTl9/jqzqp/U+7SFncpBwAAM328WpXWMo2YbWlJs+ac2TNEd5uxG92+LedZtxFBHADgqT7+skPpm48tfwFBerWn+Uap5luN2nNTx8THS98o1WyTvrggHWOZV1gfaX9I+taqdG6qXqXjUtvjPLT9SxAHAHiq177vb+tEvT9cvZcfAAA8A3/ZAQAAwKkhM3IAAAAYjxk5AAAApwjkAAAAnPoHQ79Y4agfxf4AAAAASUVORK5CYII=)

3.微服务自动重启

![image-20220418104702500](https://www.ydlclass.com/doc21xnv/assets/image-20220418104702500.bbdf21ca.png)

4.访问微服务，发现配置已经改变

![image-20220418104819862](https://www.ydlclass.com/doc21xnv/assets/image-20220418104819862.b829a33e.png)

## 十五、SpringCloud Stream消息驱动

### 1、Stream 概述

![image-20220418111329857](https://www.ydlclass.com/doc21xnv/assets/image-20220418111329857.7c8547d2.png)

1.Spring Cloud Stream 是一个构建消息驱动微服务应用的框架。

2.Stream 解决了开发人员**无感知**的使用消息中间件的问题，因为Stream对消息中间件的进一步封装，可以做到代码层面对中间件的无感知，甚至于**动态的切换中间件**，使得微服务开发的高度解耦，服务可以关注更多自己的业务流程。

3.Spring Cloud Stream目前支持两种消息中间件RabbitMQ和Kafka

就像jdbc一样

![image-20220418111643163](https://www.ydlclass.com/doc21xnv/assets/image-20220418111643163.1006efdf.png)

**Stream 组件**

![image-20200613074759833](https://www.ydlclass.com/doc21xnv/assets/image-20200613074759833.b4428b4e.png)

Spring Cloud Stream 构建的应用程序与消息中间件之间是通过绑定器 Binder 相关联的。绑定器对于应用程序而言起到了隔离作用， 它使得不同消息中间件的实现细节对应用程序来说是透明的。

binding 是我们通过配置把应用和spring cloud stream 的 binder 绑定在一起

output：发送消息 Channel，内置 Source接口

input：接收消息 Channel，内置 Sink接口

### 2、Stream 消息生产者 stream-producer

pom

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>stream-parent</artifactId>
        <groupId>com.ydlclass</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>stream-producer</artifactId>


    <dependencies>

        <!--spring boot web-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>


        <!-- stream -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-stream-rabbit</artifactId>
        </dependency>

    </dependencies>

</project>
```

ProducerApp

```java
package com.ydlclass.stream;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class ProducerApp {
    public static void main(String[] args) {

        SpringApplication.run(ProducerApp.class,args);
    }
}
```

application.yml

```yaml
server:
  port: 8000

spring:
  cloud:
    stream:
      # 定义绑定器，绑定到哪个消息中间件上
      binders:
        ydlclass_binder: # 自定义的绑定器名称
          type: rabbit # 绑定器类型
          environment: # 指定mq的环境
            spring:
              rabbitmq:
                host: localhost
                port: 5672
                username: guest
                password: guest
                virtual-host: /
      bindings:
        output: # channel名称
          binder: ydlclass_binder #指定使用哪一个binder
          destination: ydlclass_exchange # 消息目的地
```

消息发送类 MessageProducer

```java
package com.ydlclass.stream.producer;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.cloud.stream.annotation.EnableBinding;
import org.springframework.cloud.stream.messaging.Source;
import org.springframework.messaging.MessageChannel;
import org.springframework.messaging.support.MessageBuilder;
import org.springframework.stereotype.Component;

import java.nio.charset.StandardCharsets;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
@Component
@EnableBinding(Source.class)//这个类是stream的发送者
public class MessageProducer {
    @Autowired
    MessageChannel output;

    public void send(String msg){
        output.send(MessageBuilder.withPayload(msg).build());
        System.out.println("MessageProducer 确实发了消息了！");
    }
}
```

调用的controller ProducerController

```java
package com.ydlclass.stream.controller;

import com.ydlclass.stream.producer.MessageProducer;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
@RestController
@RequestMapping("test")
public class ProducerController {
    @Autowired
    MessageProducer messageProducer;
    //业务逻辑
    @GetMapping("/send")
    public String send(){
        //发消息
        String msg="使用stream来发消息了！";

        messageProducer.send(msg);

        return "success!";
    }
}
```

![image-20220418114016165](https://www.ydlclass.com/doc21xnv/assets/image-20220418114016165.e7af66ff.png)

### 3、Stream 消息消费者 stream-consumer

pom

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>stream-parent</artifactId>
        <groupId>com.ydlclass</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>stream-consumer</artifactId>


    <dependencies>

        <!--spring boot web-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>


        <!-- stream -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-stream-rabbit</artifactId>
        </dependency>

    </dependencies>
</project>
```

ConsumerApp

```java
package com.ydlclass.stream;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class ConsumerApp {
    public static void main(String[] args) {

        SpringApplication.run(ConsumerApp.class,args);
    }
}
```

application.yml

```yaml
server:
  port: 9000



spring:
  cloud:
    stream:
      # 定义绑定器，绑定到哪个消息中间件上
      binders:
        ydlclass_binder: # 自定义的绑定器名称
          type: rabbit # 绑定器类型
          environment: # 指定mq的环境
            spring:
              rabbitmq:
                host: localhost
                port: 5672
                username: guest
                password: guest
                virtual-host: /
      bindings:
        input: # channel名称
          binder: ydlclass_binder #指定使用哪一个binder
          destination: ydlclass_exchange # 消息目的地
```

消息接收类 MessageListener

```java
package com.ydlclass.stream.consumer;

import org.springframework.cloud.stream.annotation.EnableBinding;
import org.springframework.cloud.stream.annotation.StreamListener;
import org.springframework.cloud.stream.messaging.Sink;
import org.springframework.messaging.Message;
import org.springframework.stereotype.Component;

/**
 * 消息接收类
 */
@EnableBinding({Sink.class})
@Component
public class MessageListener {

    @StreamListener(Sink.INPUT)
    public void receive(Message message){

        System.out.println(message);
        System.out.println(message.getPayload());
    }
}
```

测试

![image-20220418154602979](https://www.ydlclass.com/doc21xnv/assets/image-20220418154602979.4f1f002f.png)

## 十六、SpringCloud Sleuth分布式请求链路追踪

### 1、概述

![image-20220418155007573](https://www.ydlclass.com/doc21xnv/assets/image-20220418155007573.1c1b8bdd.png)

Spring Cloud Sleuth 其实是一个工具,它在整个分布式系统中能跟踪一个用户请求的过程，捕获这些跟踪数据，就能构建微服务的整个调用链的视图，这是调试和监控微服务的关键工具。

```text
耗时分析 
可视化错误 
链路优化
```

Zipkin 是 Twitter 的一个开源项目，它致力于收集服务的定时数据，以解决微服务架构中的延迟问题，包括数据的收集、存储、查找和展现

### **2、快速入门**

1. 安装启动zipkin。 java –jar zipkin.jar

   ![image-20220418185033323](https://www.ydlclass.com/doc21xnv/assets/image-20220418185033323.76dc027c.png)

2. 访问zipkin web界面。 http://localhost:9411/

   ![image-20200613080015686](https://www.ydlclass.com/doc21xnv/assets/image-20200613080015686.e297485f.png)

3. 在服务提供方和消费方分别引入 sleuth 和 zipkin 依赖

   ```xml
           <!-- sleuth-zipkin -->
           <!--<dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-sleuth</artifactId>
           </dependency>-->
   
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-zipkin</artifactId>
           </dependency>
   ```

4. 分别配置服务提供方和消费方。

   provider

   ```yaml
   server:
     port: 8001
   
   eureka:
     client:
       service-url:
         defaultZone: http://localhost:8761/eureka
   spring:
     application:
       name: feign-provider
     zipkin:
       base-url: http://localhost:9411/  # 设置zipkin的服务端路径
     sleuth:
       sampler:
         probability: 1 # 采集率 默认 0.1 百分之十。
   ```

   consumer

   ```yaml
   server:
     port: 9000
   
   eureka:
     instance:
       hostname: localhost # 主机名
     client:
       service-url:
         defaultZone: http://localhost:8761/eureka
   spring:
     application:
       name: feign-consumer # 设置当前应用的名称。将来会在eureka中Application显示。将来需要使用该名称来获取路径
     zipkin:
       base-url: http://localhost:9411/  # 设置zipkin的服务端路径
     sleuth:
       sampler:
         probability: 1 # 采集率 默认 0.1 百分之十。
   logging:
     level:
       com.ydlclass: debug
   ```

5. 启动，测试

![image-20220418160537372](https://www.ydlclass.com/doc21xnv/assets/image-20220418160537372.c7993f21.png)

调用关系图也能展现

![image-20220418160602065](https://www.ydlclass.com/doc21xnv/assets/image-20220418160602065.25c4ceff.png)

## 十七、SpringCloud Alibaba入门简介

### 1、为什么使用Alibaba

**1.因为：spring netflix进入维护模式**

https://spring.io/blog/2018/12/12/spring-cloud-greenwich-rc1-available-now#spring-cloud-netflix-projects-entering-maintenance-mode

什么是维护模式：spring cloud团队将不会再向模块添加新功能，我们将修复block级别的bug以及安全问题，我们也会考虑并审查社区的小型pull request。我们打算继续支持这些模块，知道Greenwich版本被普遍采用至少一年

SpringCloud Netflix将不再开发新的组件

以下spring cloud netflix模块和响应的starter将进入维护模式：

- spring-cloud-netflix-archaius
- spring-cloud-netflix-hystrix-contract
- spring-cloud-netflix-hystrix-dashboard
- spring-cloud-netflix-hystrix-stream
- spring-cloud-netflix-hystrix
- spring-cloud-netflix-ribbon
- spring-cloud-netflix-turbine-stream
- spring-cloud-netflix-turbine
- spring-cloud-netflix-zuul

我们都知道SpringCloud版本迭代是比较快的，因而出现了很多重大ISSUE都还来不及Flix就又推另一个RELEASE了。进入维护模式意思就是目前以及以后一段时间SpingCloud Netflix提供的报务和功能就这么多了，不在开发新的组件和功能了。以后将以雏护和Merge分支Full Request为主。

2.**所以：spring cloud alibaba来了**

https://spring.io/projects/spring-cloud-alibaba

2018.10.31，spring cloud Alibaba正式入驻了Spring Cloud官方孵化器，并在Maven中央库发布了第一个版本

主要功能：

- **服务限流降级**：默认支持 WebServlet、WebFlux, OpenFeign、RestTemplate、Spring Cloud Gateway, Zuul, Dubbo 和 RocketMQ 限流降级功能的接入，可以在运行时通过控制台实时修改限流降级规则，还支持查看限流降级 Metrics 监控。
- **服务注册与发现**：适配 Spring Cloud 服务注册与发现标准，默认集成了 Ribbon 的支持。 分布式配置管理：支持分布式系统中的外部化配置，配置更改时自动刷新。
- **消息驱动能力**：基于 Spring Cloud Stream 为微服务应用构建消息驱动能力。
- **分布式事务**：使用 @GlobalTransactional 注解， 高效并且对业务零侵入地解决分布式事务问题。。
- **阿里云对象存储**：阿里云提供的海量、安全、低成本、高可靠的云存储服务。支持在任何应用、任何时间、任何地点存储和访问任意类型的数据。
- **分布式任务调度**：提供秒级、精准、高可靠、高可用的定时（基于 Cron 表达式）任务调度服务。同时提供分布式的任务执行模型，如网格任务。网格任务支持海量子任务均匀分配到所有 Worker（schedulerx-client）上执行。
- **阿里云短信服务**：覆盖全球的短信服务，友好、高效、智能的互联化通讯能力，帮助企业迅速搭建客户触达通道。

组件：

- **Sentinel**：把流量作为切入点，从流量控制、熔断降级、系统负载保护等多个维度保护服务的稳定性。
- **Nacos**：一个更易于构建云原生应用的动态服务发现、配置管理和服务管理平台。
- **RocketMQ**：一款开源的分布式消息系统，基于高可用分布式集群技术，提供低延时的、高可靠的消息发布与订阅服务。
- **Dubbo**：Apache Dubbo™ 是一款高性能 Java RPC 框架。
- **Seata**：阿里巴巴开源产品，一个易于使用的高性能微服务分布式事务解决方案。
- **Alibaba Cloud OSS**: 阿里云对象存储服务（Object Storage Service，简称 OSS），是阿里云提供的海量、安全、低成本、高可靠的云存储服务。您可以在任何应用、任何时间、任何地点存储和访问任意类型的数据。
- **Alibaba Cloud SchedulerX**: 阿里中间件团队开发的一款分布式任务调度产品，提供秒级、精准、高可靠、高可用的定时（基于 Cron 表达式）任务调度服务。
- **Alibaba Cloud SMS**: 覆盖全球的短信服务，友好、高效、智能的互联化通讯能力，帮助企业迅速搭建客户触达通道。

### **2、如何使用？**

Spring官网：https://spring.io/projects/spring-cloud-alibaba GitHub：https://github.com/alibaba/spring-cloud-alibaba GitHub中文文档：https://github.com/alibaba/spring-cloud-alibaba/blob/master/README-zh.md Spring Cloud Alibaba参考文档：https://spring-cloud-alibaba-group.github.io/github-pages/greenwich/spring-cloud-alibaba.html

按照官网一步一步来

https://spring.io/projects/spring-cloud-alibaba#learn

新建父工程 ydl-cloud-alibaba

父工程引入

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-alibaba-dependencies</artifactId>
            <version>2021.0.1.0</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

### 3、版本对应

我们按照最新的学习

https://github.com/alibaba/spring-cloud-alibaba/wiki/%E7%89%88%E6%9C%AC%E8%AF%B4%E6%98%8E

| Spring Cloud Alibaba Version | Sentinel Version | Nacos Version | RocketMQ Version | Dubbo Version | Seata Version |
| ---------------------------- | ---------------- | ------------- | ---------------- | ------------- | ------------- |
| 2021.0.1.0*                  | 1.8.3            | 1.4.2         | 4.9.2            | 2.7.15        | 1.4.2         |

| Spring Cloud Alibaba Version | Spring Cloud Version  | Spring Boot Version |
| ---------------------------- | --------------------- | ------------------- |
| 2021.0.1.0                   | Spring Cloud 2021.0.1 | 2.6.3               |

## 十八、SpringCloud Alibaba Nacos服务注册和配置中心

### 1、是什么

官方文档：https://nacos.io/zh-cn/docs/what-is-nacos.html

1.nacos(NAming COnfiguration Service)：服务注册和配置中心

```text
Nacos = Eureka + Config + Bus
替代Eureka做服务注册中心
替代Config做服务配置中心
```

```text
github地址： https://github.com/alibaba/Nacos
Nacos地址： https://nacos.io/zh-cn/
```

2.Nacos 概念

```text
地域
物理的数据中心，资源创建成功后不能更换。

可用区
同一地域内，电力和网络互相独立的物理区域。同一可用区内，实例的网络延迟较低。

接入点
地域的某个服务的入口域名。

命名空间
用于进行租户粒度的配置隔离。不同的命名空间下，可以存在相同的 Group 或 Data ID 的配置。Namespace 的常用场景之一是不同环境的配置的区分隔离，例如开发测试环境和生产环境的资源（如配置、服务）隔离等。

配置
在系统开发过程中，开发者通常会将一些需要变更的参数、变量等从代码中分离出来独立管理，以独立的配置文件的形式存在。目的是让静态的系统工件或者交付物（如 WAR，JAR 包等）更好地和实际的物理运行环境进行适配。配置管理一般包含在系统部署的过程中，由系统管理员或者运维人员完成。配置变更是调整系统运行时的行为的有效手段。

配置管理
系统配置的编辑、存储、分发、变更管理、历史版本管理、变更审计等所有与配置相关的活动。

配置项
一个具体的可配置的参数与其值域，通常以 param-key=param-value 的形式存在。例如我们常配置系统的日志输出级别（logLevel=INFO|WARN|ERROR） 就是一个配置项。

配置集
一组相关或者不相关的配置项的集合称为配置集。在系统中，一个配置文件通常就是一个配置集，包含了系统各个方面的配置。例如，一个配置集可能包含了数据源、线程池、日志级别等配置项。

配置集 ID
Nacos 中的某个配置集的 ID。配置集 ID 是组织划分配置的维度之一。Data ID 通常用于组织划分系统的配置集。一个系统或者应用可以包含多个配置集，每个配置集都可以被一个有意义的名称标识。Data ID 通常采用类 Java 包（如 com.taobao.tc.refund.log.level）的命名规则保证全局唯一性。此命名规则非强制。

配置分组
Nacos 中的一组配置集，是组织配置的维度之一。通过一个有意义的字符串（如 Buy 或 Trade ）对配置集进行分组，从而区分 Data ID 相同的配置集。当您在 Nacos 上创建一个配置时，如果未填写配置分组的名称，则配置分组的名称默认采用 DEFAULT_GROUP 。配置分组的常见场景：不同的应用或组件使用了相同的配置类型，如 database_url 配置和 MQ_topic 配置。

配置快照
Nacos 的客户端 SDK 会在本地生成配置的快照。当客户端无法连接到 Nacos Server 时，可以使用配置快照显示系统的整体容灾能力。配置快照类似于 Git 中的本地 commit，也类似于缓存，会在适当的时机更新，但是并没有缓存过期（expiration）的概念。

服务
通过预定义接口网络访问的提供给客户端的软件功能。

服务名
服务提供的标识，通过该标识可以唯一确定其指代的服务。

服务注册中心
存储服务实例和服务负载均衡策略的数据库。

服务发现
在计算机网络上，（通常使用服务名）对服务下的实例的地址和元数据进行探测，并以预先定义的接口提供给客户端进行查询。

元信息
Nacos数据（如配置和服务）描述信息，如服务版本、权重、容灾策略、负载均衡策略、鉴权配置、各种自定义标签 (label)，从作用范围来看，分为服务级别的元信息、集群的元信息及实例的元信息。

应用
用于标识服务提供方的服务的属性。

服务分组
不同的服务可以归类到同一分组。

虚拟集群
同一个服务下的所有服务实例组成一个默认集群, 集群可以被进一步按需求划分，划分的单位可以是虚拟集群。

实例
提供一个或多个服务的具有可访问网络地址（IP:Port）的进程。

权重
实例级别的配置。权重为浮点数。权重越大，分配给该实例的流量越大。

健康检查
以指定方式检查服务下挂载的实例 (Instance) 的健康度，从而确认该实例 (Instance) 是否能提供服务。根据检查结果，实例 (Instance) 会被判断为健康或不健康。对服务发起解析请求时，不健康的实例 (Instance) 不会返回给客户端。

健康保护阈值
为了防止因过多实例 (Instance) 不健康导致流量全部流向健康实例 (Instance) ，继而造成流量压力把健康实例 (Instance) 压垮并形成雪崩效应，应将健康保护阈值定义为一个 0 到 1 之间的浮点数。当域名健康实例数 (Instance) 占总服务实例数 (Instance) 的比例小于该值时，无论实例 (Instance) 是否健康，都会将这个实例 (Instance) 返回给客户端。这样做虽然损失了一部分流量，但是保证了集群中剩余健康实例 (Instance) 能正常工作。
```

3.架构

![image-20220419101248331](https://www.ydlclass.com/doc21xnv/assets/image-20220419101248331.66dbccb5.png)

数据模型:

Nacos 数据模型 Key 由三元组唯一确定, Namespace默认是空串，公共命名空间（public），分组默认是 DEFAULT_GROUP。

![image-20220419101435094](https://www.ydlclass.com/doc21xnv/assets/image-20220419101435094.770251ff.png)

### 2、Nacos2.0

新增长连接功能

```text
Nacos2.0版本相比1.X新增了gRPC的通信方式，因此需要增加2个端口。新增端口是在配置的主端口(server.port)基础上，进行一定偏移量自动生成。
```

新增 鉴权插件

新增 配置加密

### 3、与其他注册中心对比

| 服务注册与服务框架 | CAP模型  | 控制台管理 | 社区活跃度      |
| ------------------ | -------- | ---------- | --------------- |
| Eureka             | AP高可用 | 支持       | 低(2.x版本闭源) |
| Zookeeper          | CP一致   | 支持       | 中              |
| Consul             | CP       | 支持       | 高              |
| Nacos              | AP+CP    | 支持       | 高              |

### 4、切换

nacos可以切换 AP 和 CP ,可使用如下命令切换成CP模式

```text
curl -X PUT '$NACOS_SERVER:8848/nacos/v1/ns/operator/switches?entry=serverMode&value=CP'
```

### 5、下载

1.下载地址： https://github.com/alibaba/nacos/releases

直接下载网址：https://github.com/alibaba/nacos/releases/download/2.0.4/nacos-server-2.0.4.zip

2.下载压缩包以后解压，进入bin目录，打开dos窗口，执行startup命令启动它。

```text
startup.cmd -m standalone
```

3.端口号8848

4.可访问 ： http://localhost:8848/nacos/index.html 地址，默认账号密码都是nacos

![image-20220419165738557](https://www.ydlclass.com/doc21xnv/assets/image-20220419165738557.dd0896c1.png)

### 6、注册中心功能

急速构建

#### （1）服务提供者1

新建模块 nacos-provider8000

pom

```xml
    <!--SpringCloud Alibaba nacos-->
    <dependency>
        <groupId>com.alibaba.cloud</groupId>
        <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
    </dependency>
```

yml

```text
server:
  port: 8000


spring:
  application:
    name: nacos-provider
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848
```

主启动类

```text
@EnableDiscoveryClient
```

controller 显示server.port

```java
package com.ydlclass.nacos.provider8000.controller;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
@RestController
@RequestMapping("/goods")
public class GoodsController {

    @Value("${server.port}")
    Integer port;

    @GetMapping("/findById/{id}")
    public String findById(@PathVariable("id")Integer id){
        //业务逻辑
        return "nacos provider.port:"+port+"|id:"+id;
    }
}
```

![image-20220419234430142](https://www.ydlclass.com/doc21xnv/assets/image-20220419234430142.5b0869d8.png)

#### （2）服务提供者2

新建模块 nacos-provider8001

pom

```xml
    <!--SpringCloud Alibaba nacos-->
    <dependency>
        <groupId>com.alibaba.cloud</groupId>
        <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
    </dependency>
```

yml

```text
server:
  port: 8001

spring:
  application:
    name: nacos-provider
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848
```

主启动类

```text
@EnableDiscoveryClient
```

controller 显示server.port

```java
package com.ydlclass.nacos.provider8000.controller;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
@RestController
@RequestMapping("/goods")
public class GoodsController {

    @Value("${server.port}")
    Integer port;

    @GetMapping("/findById/{id}")
    public String findById(@PathVariable("id")Integer id){
        //业务逻辑
        return "nacos provider.port:"+port+"|id:"+id;
    }
}
```

![image-20220419234839829](https://www.ydlclass.com/doc21xnv/assets/image-20220419234839829.faf2eb3e.png)

#### （3）服务消费者

新建模块 nacos-conusmer9000

pom

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>com.ydlclass</groupId>
        <artifactId>ydl-cloud-alibaba</artifactId>
        <version>1.0.0</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <artifactId>nacos-conusmer9000</artifactId>

    <name>nacos-conusmer9000</name>
    <description>nacos-conusmer9000</description>
    <properties>
        <java.version>1.8</java.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!--SpringCloud Alibaba nacos-->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
            <exclusions>
                <exclusion>
                    <groupId>org.springframework.cloud</groupId>
                    <artifactId>spring-cloud-starter-netflix-ribbon</artifactId>
                </exclusion>
            </exclusions>
        </dependency>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-loadbalancer</artifactId>
            <version>3.1.1</version>
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

yml

```yaml
server:
  port: 9000

spring:
  application:
    name: nacos-consumer
  cloud:
    loadbalancer:
      ribbon:
        enabled: false
    nacos:
      discovery:
        server-addr: localhost:8848
```

主启动类

```text
@EnableDiscoveryClient
```

注册RestTemplate

```java
package com.ydlclass.nacos.conusmer9000.config;

import org.springframework.boot.web.client.RestTemplateBuilder;
import org.springframework.cloud.client.loadbalancer.LoadBalanced;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.client.RestTemplate;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
@Configuration
public class RestTemplateConfig {

    @Bean
    @LoadBalanced //loadbalancer 客户端负载均衡
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }
}
```

controller，调用

```java
package com.ydlclass.nacos.conusmer9000.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
@RestController
@RequestMapping("/order")
public class OrderController {

    @Autowired
    private RestTemplate restTemplate;

    @GetMapping("/add/{id}")
    public String add(@PathVariable("id")Integer id){
        //业务逻辑
        String url="http://nacos-provider/goods/findById/"+id;
        String result = restTemplate.getForObject(url, String.class);

        return result;
    }
}
```

测试：http://localhost:9000/order/add/1

![image-20220420101138103](https://www.ydlclass.com/doc21xnv/assets/image-20220420101138103.a0e6fbcd.png)

![image-20220420101147833](https://www.ydlclass.com/doc21xnv/assets/image-20220420101147833.69bfae29.png)

#### （4）整合feign

1.在pom中导入

```xml
<!-- openfeign -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

2.在主启动类上加上@EnableFeignClients，激活feign。

在主启动类上加上@EnableFeignClients，激活feign。

3.新建feign接口

```java
package com.ydlclass.nacos.conusmer9000.feign;

import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
@FeignClient("nacos-provider")
public interface GoodsFeign {

    @GetMapping("/goods/findById/{id}")
    public String findById(@PathVariable("id")Integer id);
}
```



4.controller新写方法

```java
    @Autowired
    GoodsFeign goodsFeign;
    
    @GetMapping("/add2/{id}")
    public String add2(@PathVariable("id")Integer id){
        //feign
        String str = goodsFeign.findById(id);

        return str;
    }
```

测试:http://localhost:9000/order/add2/1

![image-20220420101855472](https://www.ydlclass.com/doc21xnv/assets/image-20220420101855472.f61490c5.png)

![image-20220420101839462](https://www.ydlclass.com/doc21xnv/assets/image-20220420101839462.22864b6c.png)

### 7、服务注册中心对比

1.Nacos 生态图

![image-20220420102428162](https://www.ydlclass.com/doc21xnv/assets/image-20220420102428162.3664433a.png)

2.Nacos和CAP

![image-20220420102902745](https://www.ydlclass.com/doc21xnv/assets/image-20220420102902745.21c1528d.png)

3.对比其他注册中心

![image-20220420102925313](https://www.ydlclass.com/doc21xnv/assets/image-20220420102925313.2f854356.png)

A：可用性 C：一致性 P：分区容错性

Nacos默认AP。

切换CP：

```text
curl -X PUT '$NACOS_SERVER:8848/nacos/v1/ns/operator/switches?entry=serverMode&value=CP'
```

### 8、配置中心功能

#### （1）创建工程nacos-client7777

1.pom

```xml
    <dependency>
        <groupId>com.alibaba.cloud</groupId>
        <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
    </dependency>

    <!--SpringCloud Alibaba nacos-->
    <dependency>
        <groupId>com.alibaba.cloud</groupId>
        <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
    </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-bootstrap</artifactId>
        </dependency>
```

2.application.yml

```text
spring:
  profiles:
    active: dev #表示开发环境
```

bootstrap.yml

```yaml
# nacos配置
server:
  port: 7777

spring:
  application:
    name: nacos-config-client
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848 #Nacos服务注册中心地址
      config:
        server-addr: localhost:8848 #Nacos作为配置中心地址
        file-extension: yaml #指定yaml格式的配置
        
#${spring.application.name}-${spring.profile.active}.${spring.cloud.nacos.config.file-extension}
```

3.主启动类

```text
@EnableDiscoveryClient
```

4.controller

```java
package com.ydlclass.nacos.client7777.controller;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.cloud.context.config.annotation.RefreshScope;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
@RestController
@RequestMapping("/config")
@RefreshScope // 开启刷新功能
public class ConfigClientController {
    @Value("${name}")
    String name;

    @GetMapping("/name")
    public String name() {
        return name;
    }
}
```

5.在Nacos中添加配置信息

https://nacos.io/zh-cn/docs/quick-start-spring-cloud.html

![image-20220420105534316](https://www.ydlclass.com/doc21xnv/assets/image-20220420105534316.91ce97b4.png)

6.测试：http://localhost:7777/config/name

![image-20220420111758851](https://www.ydlclass.com/doc21xnv/assets/image-20220420111758851.a9591913.png)

更改配置

![image-20220420111944907](https://www.ydlclass.com/doc21xnv/assets/image-20220420111944907.7fee8b29.png)

再次访问接口。说明bus的功能也实现了。

![image-20220420112019087](https://www.ydlclass.com/doc21xnv/assets/image-20220420112019087.ef26de3e.png)

#### （2）分类配置

![image-20220420124712175](https://www.ydlclass.com/doc21xnv/assets/image-20220420124712175.aff1a70a.png)

问题1：实际开发中，通常一个系统会准备dev/test/prod环境。如何保证环境启动时服务能正确读取nacos上相应环境的配置文件？

答案：namespace区分

问题2：一个大型分布式微服务系统有很多微服务子项目，每个微服务项目又都会有相应的开发环境、测试环境、预发环境、正式环境。那怎么对微服务配置进行管理呢？

答案：用group把不同的微服务划分到同一个分组里面去

Service就是微服务，一个service可以包含多个cluster集群，nacos默认cluster是DEFAULT，Cluster是对指定微服务的一个虚拟划分。

比方说为了容灾，将service微服务分别部署在了北京机房和上海机房，这是就可以给北京机房的service微服务起一个集群名称BJ，给上海的service微服务起一个集群名称SH，还可以尽量让同一个机房的微服务互相调用，以提升效率。

**dgn方案** 1.dataid方案（就是nacos的文件名）

指定spring.profile.active和配置文件的dataID来使不太环境下读取不同的配置 配置空间+配置分组+新建dev和test两个dataid：就是创建-后不同的两个文件名nacos-config-client-dev.yaml、nacos-config-client-test.yaml 通过IDEA里的spring.profile.active属性就能进行多环境下配置文件的读取。

2.Group方案（默认DEFAULT_GROUP）

在nacos创建配置文件时，给文件指定分组。 在IDEA中该group内容 实现的功能：当修改开发环境时，只会从同一group中进行切换。

```text
spring:
  application:
    name: nacos-config-client
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848 #Nacos服务注册中心地址
      config:
        server-addr: localhost:8848 #Nacos作为配置中心地址
        file-extension: yaml #指定yaml格式的配置
        group: BJ_GROUP
```

![image-20220420122132968](https://www.ydlclass.com/doc21xnv/assets/image-20220420122132968.ec8b2e75.png)

3.namespace方案（默认public）

这个是不允许删除的，可以创建一个新的命名空间，会自动给创建的命名空间一个流水号。 在nacos新建命名空间，自动出现e79f32ec-974a-4e90-9086-3ae5c64db7e3

![image-20220420124924806](https://www.ydlclass.com/doc21xnv/assets/image-20220420124924806.3811dd5a.png)

在IDEA的yml中指定命名空间namespace: e79f32ec-974a-4e90-9086-3ae5c64db7e3

```yaml
spring:
  application:
    name: nacos-config-client
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848 #Nacos服务注册中心地址
      config:
        server-addr: localhost:8848 #Nacos作为配置中心地址
        file-extension: yaml #指定yaml格式的配置
        group: BJ_GROUP
        namespace: e79f32ec-974a-4e90-9086-3ae5c64db7e3
```

![image-20220420122430460](https://www.ydlclass.com/doc21xnv/assets/image-20220420122430460.e1b6abe8.png)

最后，dataid、group、namespace 三者关系如下：（不同的dataid，是相互独立的，不同的group是相互隔离的，不同的namespace也是相互独立的）

![image-20220420120816153](https://www.ydlclass.com/doc21xnv/assets/image-20220419101435094.770251ff.png)

### 9、集群和持久化配置（重要）

![image-20220420151535043](https://www.ydlclass.com/doc21xnv/assets/image-20220420151535043.1ac73c85.png)

https://nacos.io/zh-cn/docs/deployment.html

#### （1）Nacos部署环境

Nacos定义为一个IDC内部应用组件，并非面向公网环境的产品，建议在内部隔离网络环境中部署，强烈不建议部署在公共网络环境。

以下文档中提及的VIP，网卡等所有网络相关概念均处于内部网络环境。

#### 2）Nacos支持三种部署模式

- 单机模式 - 用于测试和单机试用。
- 集群模式 - 用于生产环境，确保高可用。
- 多集群模式 - 用于多数据中心场景。

#### （3）单机模式下运行Nacos

Linux/Unix/Mac

```text
startup.sh -m standalone
```

Windows

```text
startup.cmd -m standalone
```

#### （4）单机模式支持mysql

在0.7版本之前，在单机模式时nacos使用嵌入式数据库实现数据的存储，不方便观察数据存储的基本情况。0.7版本增加了支持mysql数据源能力，具体的操作步骤：

- 1.安装数据库，版本要求：5.6.5+
- 2.初始化mysql数据库，数据库初始化文件：nacos-mysql.sql
- 3.修改conf/application.properties文件，增加支持mysql数据源配置（目前只支持mysql），添加mysql数据源的url、用户名和密码。

```text
spring.datasource.platform=mysql

db.num=1
db.url.0=jdbc:mysql://11.162.196.16:3306/nacos_devtest?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true
db.user=nacos_devtest
db.password=youdontknow
```

再以单机模式启动nacos，nacos所有写嵌入式数据库的数据都写到了mysql

![image-20220420153743075](https://www.ydlclass.com/doc21xnv/assets/image-20220420153743075.631b177e.png)

#### （5）集群部署说明

集群模式部署

这个快速开始手册是帮忙您快速在你的电脑上，下载安装并使用Nacos，部署生产使用的集群模式。

集群部署架构图

因此开源的时候推荐用户把所有服务列表放到一个vip下面，然后挂到一个域名下面

[http://ip1open in new window](http://ip1/):port/openAPI 直连ip模式，机器挂则需要修改ip才可以使用。

[http://SLBopen in new window](http://slb/):port/openAPI 挂载SLB模式(内网SLB，不可暴露到公网，以免带来安全风险)，直连SLB即可，下面挂server真实ip，可读性不好。

[http://nacos.comopen in new window](http://nacos.com/):port/openAPI 域名 + SLB模式(内网SLB，不可暴露到公网，以免带来安全风险)，可读性好，而且换ip方便，推荐模式

![deployDnsVipMode.jpg](https://www.ydlclass.com/doc21xnv/assets/deployDnsVipMode.1e13d905.jpg)

![image-20220420182030318](https://www.ydlclass.com/doc21xnv/assets/image-20220420182030318.aef91a1c.png)

##### [](https://www.ydlclass.com/doc21xnv/springcloud/SpringCloud.html#a-预备环境准备)a.预备环境准备

请确保是在环境中安装使用:

1. 64 bit OS Linux/Unix/Mac，推荐使用Linux系统。
2. 64 bit JDK 1.8+；[下载open in new window](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).[配置open in new window](https://docs.oracle.com/cd/E19182-01/820-7851/inst_cli_jdk_javahome_t/)。
3. Maven 3.2.x+；[下载open in new window](https://maven.apache.org/download.cgi).[配置open in new window](https://maven.apache.org/settings.html)。
4. 3个或3个以上Nacos节点才能构成集群。

##### b.下载源码或者安装包

###### 下载编译后压缩包方式

下载地址

https://github.com/alibaba/nacos/releases

```bash
  tar -zxvf nacos-server-2.0.4.tar.gz
  cd nacos/bin
```

##### c.配置集群配置文件

在nacos的解压目录nacos/的conf目录下，有配置文件cluster.conf，请每行配置成ip:port。（请配置3个或3个以上节点）

```plain
192.168.246.128:6666
192.168.246.128:7777
192.168.246.128:8888
```

##### d.确定数据源

虚拟机中，已经有mysql

root Ydlclass@666

![image-20220420165604781](https://www.ydlclass.com/doc21xnv/assets/image-20220420165604781.124e319f.png)

新建nacos_config库，导入数据

![image-20220420165815259](https://www.ydlclass.com/doc21xnv/assets/image-20220420165815259.1de5529a.png)

application配置文件

```yaml
### If use MySQL as datasource:
spring.datasource.platform=mysql

### Count of DB:
db.num=1

### Connect URL of DB:
db.url.0=jdbc:mysql://192.168.246.128:3306/nacos_config?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true&useUnicode=true&useSSL=false&serverTimezone=UTC
db.user.0=root
db.password.0=Ydlclass@666
```

nacos复制成3份，端口号改成 6666 7777 8888

##### e.启动服务器

三台nacos,修改启动参数,再启动

```text
 vim startup.sh
```

![image-20220420171828918](https://www.ydlclass.com/doc21xnv/assets/image-20220420171828918.89729a63.png)

```bash
sh startup.sh
```

##### f.结合nginx

1.上传nginx源码包

2.修改配置文件

```text
#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;
    upstream cluster{
        server 192.168.246.128:6666;
        server 192.168.246.128:7777;
        server 192.168.246.128:8888;
    }
    server {
        listen       5555;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            #root   html;
            #index  index.html index.htm;
            proxy_pass http://cluster;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }


    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}

}
```

3.安装nginx

```text
./configure
make && make install
```

4.启动nginx

```text
cd /usr/local/nginx/sbin
./nginx 
```

5.测试访问 http://192.168.246.128:5555/nacos/index.html

![image-20220420173847175](https://www.ydlclass.com/doc21xnv/assets/image-20220420173847175.69807fee.png)

##### g.项目配置获取

1.新建配置文件

![image-20220420173912640](https://www.ydlclass.com/doc21xnv/assets/image-20220420173912640.9368ec66.png)

2.项目修改获取配置文件地址

```yaml
# nacos配置
server:
  port: 7777

spring:
  application:
    name: nacos-config-client
  cloud:
    nacos:
      discovery:
        server-addr: 192.168.246.128:5555 #Nacos服务注册中心地址
      config:
        server-addr: 192.168.246.128:5555 #Nacos作为配置中心地址
        file-extension: yaml #指定yaml格式的配置
        group: SX_GROUP
        namespace: d0492f75-fbe3-4bfc-8689-c4c200233bd9

#${spring.application.name}-${spring.profiles.active}.${file-extension}
# nacos-config-client-dev.yaml
```

## 十九、SpringCloud Alibaba Sentinel实现熔断与限流

### 1、简介

**一句话**：就是hystrix的替代！

官网：https://github.com/alibaba/sentinel 中文版：[https://github.com/alibaba/Sentinel/wiki/%E4%BB%8B%E7%BB%8Dopen in new window](https://github.com/alibaba/Sentinel/wiki/介绍)

随着微服务的流行，服务和服务之间的稳定性变得越来越重要。Sentinel 以流量为切入点，从流量控制、熔断降级、系统负载保护等多个维度保护服务的稳定性。

Sentinel 具有以下特征:

- **丰富的应用场景**：Sentinel 承接了阿里巴巴近 10 年的双十一大促流量的核心场景，例如秒杀（即突发流量控制在系统容量可以承受的范围）、消息削峰填谷、集群流量控制、实时熔断下游不可用应用等。
- **完备的实时监控**：Sentinel 同时提供实时的监控功能。您可以在控制台中看到接入应用的单台机器秒级数据，甚至 500 台以下规模的集群的汇总运行情况。
- **广泛的开源生态**：Sentinel 提供开箱即用的与其它开源框架/库的整合模块，例如与 Spring Cloud、Apache Dubbo、gRPC、Quarkus 的整合。您只需要引入相应的依赖并进行简单的配置即可快速地接入 Sentinel。同时 Sentinel 提供 Java/Go/C++ 等多语言的原生实现。
- **完善的 SPI 扩展机制**：Sentinel 提供简单易用、完善的 SPI 扩展接口。您可以通过实现扩展接口来快速地定制逻辑。例如定制规则管理、适配动态数据源等。

Sentinel 的主要特性：

![image-20220424114427413](https://www.ydlclass.com/doc21xnv/assets/image-20220424114427413.95cea440.png)

Sentinel 的开源生态：

![image-20220424114442619](https://www.ydlclass.com/doc21xnv/assets/image-20220424114442619.a6e4b456.png)

**Sentinel 分为两个部分**:

- 核心库（Java 客户端）不依赖任何框架/库，能够运行于所有 Java 运行时环境，同时对 Dubbo / Spring Cloud 等框架也有较好的支持。

- 控制台（Dashboard）基于 Spring Boot 开发，打包后可以直接运行，不需要额外的 Tomcat 等应用容器。

  ![image-20220424155346668](https://www.ydlclass.com/doc21xnv/assets/image-20220424155346668.94e6cd9a.png)

### 2、下载

下载：https://github.com/alibaba/Sentinel/releases

![image-20220424155816578](https://www.ydlclass.com/doc21xnv/assets/image-20220424155816578.38f9071b.png)

运行：

```text
java -jar sentinel-dashboard-1.8.4.jar
```

![image-20220424155846911](https://www.ydlclass.com/doc21xnv/assets/image-20220424155846911.9e8b14bf.png)

http://localhost:8080

![image-20220424121030210](https://www.ydlclass.com/doc21xnv/assets/image-20220424121030210.3df6fbd1.png)

账号和密码都是sentinel

![image-20220424121050706](https://www.ydlclass.com/doc21xnv/assets/image-20220424121050706.12aada39.png)

### 3、初始化演示工程

cloudalibaba-sentinel-service8000

a、 pom

```xml
    <!-- SpringCloud ailibaba nacos-->
    <dependency>
        <groupId>com.alibaba.cloud</groupId>
        <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
    </dependency>
    <!-- SpringCloud ailibaba sentinel-datasource-nacos 持久化需要用到-->
    <dependency>
        <groupId>com.alibaba.csp</groupId>
        <artifactId>sentinel-datasource-nacos</artifactId>
    </dependency>
    <!-- SpringCloud ailibaba sentinel-->
    <dependency>
        <groupId>com.alibaba.cloud</groupId>
        <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
    </dependency>
```

b、yml

```yaml
server:
  port: 8000

spring:
  application:
    name: cloudalibaba-sentinal-service
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848
    sentinel:
      transport:
        dashboard: localhost:8080
        # 默认8719端口，假如被占用了会自动从8719端口+1进行扫描，直到找到未被占用的 端口
        port: 8719
```

c、 主启动类

@EnableDiscoveryClient

d、controller

```java
@RestController
public class FlowLimitController {

    @GetMapping("/testA")
    public String testA() {
        return "----testA";
    }

    @GetMapping("/testB")
    public String testB() {
        return "----testB";
    }

}
```



e、测试

启动8050，然后刷新sentinel后台页面（因为sentinel采用懒加载策略，所以需要调用服务后才在后台显示） 在浏览器分别输入，然后刷新sentinel后台页面： http://localhost:8000/demo/testA http://localhost:8000/demo/testB

![image-20220424163655481](https://www.ydlclass.com/doc21xnv/assets/image-20220424163655481.45b27ad3.png)

### 4、流量控制

- 资源名：唯一名称，默认请求路径

- 针对来源：sentinel可以针对调用者进行限流，填写微服务名，默认default（不区分来源）

- 阈值类型/单机值：

  - QPS（每秒钟的请求数量）：当调用该api就QPS达到阈值的时候，进行限流

    ![image-20220424174426829](https://www.ydlclass.com/doc21xnv/assets/image-20220424174426829.7aeed4f5.png)

  - 线程数．当调用该api的线程数达到阈值的时候，进行限流

    ![image-20220424174544692](https://www.ydlclass.com/doc21xnv/assets/image-20220424174544692.762c6a35.png)

- 是否集群：不需要集群

- 流控模式：

  - 直接：api达到限流条件时，直接限流。分为QPS和线程数
  - 关联：当关联的资到阈值时，就限流自己。别人惹事，自己买单
  - 链路：只记录指定链路上的流量（指定资源从入口资源进来的流量，如果达到阈值，就进行限流）【api级别的针对来源】

- 流控效果：

  - 快速失败：直接抛异常 Blocked by Sentinel (flow limiting)
  - warm up：根据codeFactor（冷加载因子，默认3）的值，从阈值codeFactor，经过预热时长，才达到设置的QPS阈值
  - 排队等待：匀速排队，让请求以均匀的速度通过，阈值类型必须设置为QPS,否则无效

| Field           | 说明                                                         | 默认值                        |
| --------------- | ------------------------------------------------------------ | ----------------------------- |
| resource        | 资源名，资源名是限流规则的作用对象                           |                               |
| count           | 限流阈值                                                     |                               |
| grade           | 限流阈值类型，QPS 或线程数模式                               | QPS 模式                      |
| limitApp        | 流控针对的调用来源                                           | `default`，代表不区分调用来源 |
| strategy        | 调用关系限流策略：直接、链路、关联                           | 根据资源本身（直接）          |
| controlBehavior | 流控效果（直接拒绝 / 排队等待 / 慢启动模式），不支持按调用关系限流 | 直接拒绝                      |

##### （1）qps 直接 快速失败

![image-20220424174126050](https://www.ydlclass.com/doc21xnv/assets/image-20220424174126050.83cb980b.png)

![image-20220424174110634](https://www.ydlclass.com/doc21xnv/assets/image-20220424174110634.2ba9c366.png)

##### （2）并发 直接

controller

```java
    @GetMapping("/testA")
    public String testA(){
        //业务逻辑比价复杂
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return "itlils testA";
    }
```

![image-20220424174723161](https://www.ydlclass.com/doc21xnv/assets/image-20220424174723161.cfdc7598.png)

浏览器开两个tab分别快速访问

![image-20220424174110634](https://www.ydlclass.com/doc21xnv/assets/image-20220424174110634.2ba9c366.png)

##### （3）qps 关联失败

设置B A的失败策略

![image-20220425162627537](https://www.ydlclass.com/doc21xnv/assets/image-20220425162627537.2580ddf1.png)

![image-20220425162638896](https://www.ydlclass.com/doc21xnv/assets/image-20220425162638896.edd61661.png)

模拟postman 500ms 访问B,让B失败

![image-20220425162746123](https://www.ydlclass.com/doc21xnv/assets/image-20220425162746123.96dcb507.png)

![image-20220425162909533](https://www.ydlclass.com/doc21xnv/assets/image-20220425162909533.906e3829.png)

![image-20220425163419417](https://www.ydlclass.com/doc21xnv/assets/image-20220425163419417.dee43d33.png)

![image-20220425163712061](https://www.ydlclass.com/doc21xnv/assets/image-20220425163712061.71ff0983.png)

##### （4）qps 链路

![image-20220425170519379](https://www.ydlclass.com/doc21xnv/assets/image-20220425170519379.6fac6da0.png)

我们可以对不同controller来的调用service方法限流，更加精细化。sentinel牛X!

a.新增GoodsService

```text
@SentinelResource("goods")
public void queryGoods(){
    System.err.println("查询商品");
}
```

b.配置文件

从1.6.3版本开始，Sentinel Web filter默认收敛所有URL的入口context，导致链路限流不生效。 从1.7.0版本开始，官方在CommonFilter引入了WEB_CONTEXT_UNIFY参数，用于控制是否收敛context，将其配置为false即可根据不同的URL进行链路限流。

```yaml
spring:
  application:
    name: cloudalibaba-sentinal-service
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848
    sentinel:
      transport:
        dashboard: localhost:8080
        # 默认8719端口，假如被占用了会自动从8719端口+1进行扫描，直到找到未被占用的端口
        port: 8719
      web-context-unify: false
```

有了资源了

![image-20220425171005883](https://www.ydlclass.com/doc21xnv/assets/image-20220425171005883.371936e1.png)

c.配置规则

![image-20220425171040497](https://www.ydlclass.com/doc21xnv/assets/image-20220425171040497.8501dfdf.png)

快速调用A

![image-20220425171143687](https://www.ydlclass.com/doc21xnv/assets/image-20220425171143687.8ac44443.png)

快速调用B

![image-20220425171157383](https://www.ydlclass.com/doc21xnv/assets/image-20220425171157383.0e068415.png)

##### （5）warm up

它从开始阈值到最大QPS阈值会有一个缓冲阶段，一开始的阈值是最大QPS阈值的1/3，然后慢慢增长，直到最大阈值，适用于将突然增大的流量转换为缓步增长的场景。

Warm Up（`RuleConstant.CONTROL_BEHAVIOR_WARM_UP`）方式，即预热/冷启动方式。当系统长期处于低水位的情况下，当流量突然增加时，直接把系统拉升到高水位可能瞬间把系统压垮。通过"冷启动"，让通过的流量缓慢增加，在一定时间内逐渐增加到阈值上限，给冷系统一个预热的时间，避免冷系统被压垮。详细文档可以参考 [流量控制 - Warm Up 文档open in new window](https://github.com/alibaba/Sentinel/wiki/限流---冷启动)，具体的例子可以参见 [WarmUpFlowDemoopen in new window](https://github.com/alibaba/Sentinel/blob/master/sentinel-demo/sentinel-demo-basic/src/main/java/com/alibaba/csp/sentinel/demo/flow/WarmUpFlowDemo.java)。

通常冷启动的过程系统允许通过的 QPS 曲线如下图所示：

![image](https://www.ydlclass.com/doc21xnv/assets/68292392-b5b0aa00-00c6-11ea-86e1-ecacff8aab51.85f95839.png)

a.设置

![image-20220425172716814](https://www.ydlclass.com/doc21xnv/assets/image-20220425172716814.bad66766.png)

##### （6）匀速排队

匀速排队（`RuleConstant.CONTROL_BEHAVIOR_RATE_LIMITER`）方式会严格控制请求通过的间隔时间，也即是让请求以均匀的速度通过，对应的是漏桶算法。详细文档可以参考 [流量控制 - 匀速器模式open in new window](https://github.com/alibaba/Sentinel/wiki/流量控制-匀速排队模式)，具体的例子可以参见 [PaceFlowDemoopen in new window](https://github.com/alibaba/Sentinel/blob/master/sentinel-demo/sentinel-demo-basic/src/main/java/com/alibaba/csp/sentinel/demo/flow/PaceFlowDemo.java)。

该方式的作用如下图所示：

![image](https://www.ydlclass.com/doc21xnv/assets/68292442-d4af3c00-00c6-11ea-8251-d0977366d9b4.bafa48ad.png)

这种方式主要用于处理间隔性突发的流量，例如消息队列。想象一下这样的场景，在某一秒有大量的请求到来，而接下来的几秒则处于空闲状态，我们希望系统能够在接下来的空闲期间逐渐处理这些请求，而不是在第一秒直接拒绝多余的请求。

> 注意：匀速排队模式暂时不支持 QPS > 1000 的场景。

a.配置

![image-20220425174036373](https://www.ydlclass.com/doc21xnv/assets/image-20220425174036373.33a9da42.png)

b.测试，高并发访问b，后台控制台显示每秒2个处理

![image-20220425174116975](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAIEAAAECCAYAAADZ87SSAAAExUlEQVR4nO3bMY4TSRgF4AfiHAQECG2wB9lboAkmmnuQE7ES5+ECe5/dYD2SZdnucldP9U/X9yWI6Wnbcj/+Lpt67/769uvfsMo/f3/d+yVs4v3eL4D9fdj7BfzOPv/x594vYRPvPn764nYwObcDhAAhIEJAhIC8QQh+/vi+22Ns8dwz2vV7gp8/vufp+WWzx3t6fnn4MVuCs+VrrGj19wRr/tVdezMvL1rrRXyr53/0+BGsngSXb8zrm3X55xq3LvD547UEZ4YLuIVNbgfX3uzWILROgXv/8q89lwC06wrB64W59Wa/Xpx7v3P+WI9etMtg3Pu7QNy2KgStF/b8d87PubxYT88vV3/e8hxbrB9m/1SxKgSXY/fSrYvccv7SY9+y9nZgQmy0JlhaXb+1njUJGy4Mt/LoY51/Grl06+e3nmdpCh01UOUmwaO3g6XbzL0gPHLRj7xuKDcJGK/MJHj9vZ5ACeM63SHY6iNc7/32qPfrEbq+J1jj2sW69y1hzyq/5f8Fls6fQff3BL3uPVbvx7yl8y0M/2e3MXYWIQRECIgQECEgQkCEgAgBEQIiBEQNjaihqaFFDU0NLWpoRA2NqKERNbSm40enhmZC1Nly3kMNrU+58oka2njlJoEa2njlJgHjlZkEamj7UUNDDW3p/BmooZ3MvDBUQ8POIoSACAERAiIERAiIEBAhIEJAhICooRE1NDW0qKGpoUUNjaihETU0oobWdPzo1NBMiDpbznuoofUpVz5RQxuv3CRQQxuv3CRgvDKTQA1tP2poqKEtnT8DNbSTmReGamjYWYQQECEgQkCEgAgBEQIiBEQIiBAQNTSihqaGFjU0NbSooRE1NKKGRtTQmo4fnRqaCVFny3kPNbQ+5conamjjlZsEamjjlZsEjFdmEqih7UcNDTW0pfNnoIZ2MvPCUA0NO4sQAiIERAiIEBAhIEJAhIAIARECooZG1NDU0KKGpoYWNTSihkbU0IgaWtPxo1NDMyHqbDnvoYbWp1z5RA1tvHKTQA1tvHKTgPHKTAI1tP2ooaGGtnT+DNTQTmZeGKqhYWcRQkCEgAgBEQIiBEQIiBAQISBCQNTQiBqaGlrU0NTQooZG1NCIGhpRQ2s6fnRqaCZEnS3nPdTQ+pQrn6ihjVduEqihjVduEjBemUmghrYfNTTU0JbOn4Ea2snMC0M1NOwsQgiIEBAhIEJAhIAIARECIgRECIgaGlFDU0OLGpoaWtTQiBoaUUMjamhNx49ODc2EqLPlvIcaWp9y5RM1tPHKTQI1tPHKTQLGKzMJ1ND2o4aGGtrS+TNQQzuZeWGohoadRQgBEQIiBEQIiBAQISBCQISACAFRQyNqaGpoUUNTQ4saGlFDI2poRA2t6fjRqaGZEHW2nPdQQ+tTrnyihjZeuUmghjZeuUnAeGUmgRraftTQUENbOn8GamgnMy8M1dCwswghIEJAhIAIARECIgRECIgQECEgamhEDU0NLWpoamhRQyNqaEQNjaihNR0/OjU0E6LOlvMeamh9ypVP1NDGKzcJ1NDGKzcJGK/MJFBD248aGmpoS+fPQA3tZOaFoRoadhYhBEQIiBAQISBCQISACAERAiIERAiIEBAhIEJAhIAIAUn+A8WJJ5yDFgFGAAAAAElFTkSuQmCC)

### 5、熔断降级

https://github.com/alibaba/Sentinel/wiki/%E7%86%94%E6%96%AD%E9%99%8D%E7%BA%A7

#### （1）概述

除了流量控制以外，对调用链路中不稳定的资源进行熔断降级也是保障高可用的重要措施之一。一个服务常常会调用别的模块，可能是另外的一个远程服务、数据库，或者第三方 API 等。例如，支付的时候，可能需要远程调用银联提供的 API；查询某个商品的价格，可能需要进行数据库查询。然而，这个被依赖服务的稳定性是不能保证的。如果依赖的服务出现了不稳定的情况，请求的响应时间变长，那么调用服务的方法的响应时间也会变长，线程会产生堆积，最终可能耗尽业务自身的线程池，服务本身也变得不可用。

![chain](https://www.ydlclass.com/doc21xnv/assets/62410811-cd871680-b61d-11e9-9df7-3ee41c618644.1f0504de.png)

现代微服务架构都是分布式的，由非常多的服务组成。不同服务之间相互调用，组成复杂的调用链路。以上的问题在链路调用中会产生放大的效果。复杂链路上的某一环不稳定，就可能会层层级联，最终导致整个链路都不可用。因此我们需要对不稳定的**弱依赖服务调用**进行熔断降级，暂时切断不稳定调用，避免局部不稳定因素导致整体的雪崩。熔断降级作为保护自身的手段，通常在客户端（调用端）进行配置。

> **注意**：本文档针对 Sentinel 1.8.0 及以上版本。1.8.0 版本对熔断降级特性进行了全新的改进升级，请使用最新版本以更好地利用熔断降级的能力。

#### （2）熔断策略

Sentinel 提供以下几种熔断策略：

- 慢调用比例 (`SLOW_REQUEST_RATIO`)：选择以慢调用比例作为阈值，需要设置允许的慢调用 RT（即最大的响应时间），请求的响应时间大于该值则统计为慢调用。当单位统计时长（`statIntervalMs`）内请求数目大于设置的最小请求数目，并且慢调用的比例大于阈值，则接下来的熔断时长内请求会自动被熔断。经过熔断时长后熔断器会进入探测恢复状态（HALF-OPEN 状态），若接下来的一个请求响应时间小于设置的慢调用 RT 则结束熔断，若大于设置的慢调用 RT 则会再次被熔断。

  ![image-20220425190913439](https://www.ydlclass.com/doc21xnv/assets/image-20220425190913439.cb66dd3a.png)

  测试

  a.代码：

  ```java
      @GetMapping("/testC/{id}")
      public String testC(@PathVariable("id")Integer id){
          if(id==10){
              //复杂的业务逻辑
              try {
                  Thread.sleep(2000);
              } catch (InterruptedException e) {
                  e.printStackTrace();
              }
          }
  
          return "itlils testC";
      }
  ```

  b.设置策略

  ![image-20220425193024049](https://www.ydlclass.com/doc21xnv/assets/image-20220425193024049.c22c5e24.png)

  c.一直访问10

  ![image-20220425192753272](https://www.ydlclass.com/doc21xnv/assets/image-20220425192753272.52de37f3.png)

  d.过一会儿，访问10

  ![image-20220425193646731](https://www.ydlclass.com/doc21xnv/assets/image-20220425193646731.52ec05e0.png)

- 异常比例 (`ERROR_RATIO`)：当单位统计时长（`statIntervalMs`）内请求数目大于设置的最小请求数目，并且异常的比例大于阈值，则接下来的熔断时长内请求会自动被熔断。经过熔断时长后熔断器会进入探测恢复状态（HALF-OPEN 状态），若接下来的一个请求成功完成（没有错误）则结束熔断，否则会再次被熔断。异常比率的阈值范围是 `[0.0, 1.0]`，代表 0% - 100%。

![image-20220426115525060](https://www.ydlclass.com/doc21xnv/assets/image-20220426115525060.abd14c2d.png)

a.代码：

```java
    @GetMapping("/testD/{id}")
    public String testD(@PathVariable("id")Integer id){
        if(id==10){
            //异常调用
           int a=1/0;
        }

        return "itlils testD";
    }
```

b.设置策略

![image-20220426115919630](https://www.ydlclass.com/doc21xnv/assets/image-20220426115919630.f07525f1.png)

c.测试

![image-20220426115942858](https://www.ydlclass.com/doc21xnv/assets/image-20220426115942858.dc5aab96.png)

d.疯狂访问

![image-20220426120122888](https://www.ydlclass.com/doc21xnv/assets/image-20220426120122888.ff7a66d7.png)

e.过2秒钟后

![image-20220426120218008](https://www.ydlclass.com/doc21xnv/assets/image-20220426120218008.69e6cf5b.png)

- 异常数 (`ERROR_COUNT`)：当单位统计时长内的异常数目超过阈值之后会自动进行熔断。经过熔断时长后熔断器会进入探测恢复状态（HALF-OPEN 状态），若接下来的一个请求成功完成（没有错误）则结束熔断，否则会再次被熔断。

![image-20220426160648474](https://www.ydlclass.com/doc21xnv/assets/image-20220426160648474.d28c75a2.png)a.代码：

```java
    @GetMapping("/testE/{id}")
    public String testE(@PathVariable("id")Integer id){
        if(id==10){
            //异常调用
            int a=1/0;
        }
        return "itlils testE";
    }
```

b.配置

![image-20220426160955002](https://www.ydlclass.com/doc21xnv/assets/image-20220426160955002.abf4d2b3.png)

c.疯狂测试http://localhost:8000/demo/testE/10

![image-20220426161032712](https://www.ydlclass.com/doc21xnv/assets/image-20220426161032712.08aa284c.png)

d.过几秒 测试http://localhost:8000/demo/testE/1

![image-20220426161101190](https://www.ydlclass.com/doc21xnv/assets/image-20220426161101190.91a227bb.png)

### 6、热点参数限流

https://github.com/alibaba/Sentinel/wiki/%E7%83%AD%E7%82%B9%E5%8F%82%E6%95%B0%E9%99%90%E6%B5%81

#### （1）是什么

何为热点？热点即经常访问的数据。很多时候我们希望统计某个热点数据中访问频次最高的 Top K 数据，并对其访问进行限制。比如：

- 商品 ID 为参数，统计一段时间内最常购买的商品 ID 并进行限制
- 用户 ID 为参数，针对一段时间内频繁访问的用户 ID 进行限制

热点参数限流会统计传入参数中的热点参数，并根据配置的限流阈值与模式，对包含热点参数的资源调用进行限流。热点参数限流可以看做是一种特殊的流量控制，仅对包含热点参数的资源调用生效。

![image-20220426121224000](https://www.ydlclass.com/doc21xnv/assets/image-20220426121224000.dd5cea85.png)

![image-20220426162234051](https://www.ydlclass.com/doc21xnv/assets/image-20220426162234051.97b222f7.png)

Sentinel 利用 LRU 策略统计最近最常访问的热点参数，结合令牌桶算法来进行参数级别的流控。热点参数限流支持集群模式。

#### （2）基本使用

a.要使用热点参数限流功能，需要引入以下依赖：

```xml
        <dependency>
            <groupId>com.alibaba.csp</groupId>
            <artifactId>sentinel-parameter-flow-control</artifactId>
            <version>1.8.4</version>
        </dependency>
```

b.controller

```java
    @GetMapping("/order")
    @SentinelResource(value = "hotKeys")
    public String order(@RequestParam("goodsId")String goodsId,@RequestParam("userId")String userId){
            //业务逻辑

        return "用户下单成功";
    }
```

c.设置规则

![image-20220426163343614](https://www.ydlclass.com/doc21xnv/assets/image-20220426163343614.97798045.png)

d.快速访问http://localhost:8000/demo/order?goodsId=1&userId=10

![image-20220426163435037](https://www.ydlclass.com/doc21xnv/assets/image-20220426163435037.9d2b6d64.png)

e.快速访问http://localhost:8000/demo/order?userId=10

![image-20220426163635673](https://www.ydlclass.com/doc21xnv/assets/image-20220426163635673.dab246f4.png)

说明设置限制源代码第一个参数，成功了。

f.为了给调用方一个正确的值，改造

```java
    @GetMapping("/order")
    @SentinelResource(value = "hotKeys",blockHandler = "block_order")
    public String order(@RequestParam(value = "goodsId",required = false)String goodsId
                        ,@RequestParam(value = "userId",required = false)String userId){
            //业务逻辑

        return "用户下单成功";
    }

    public String block_order(@RequestParam(value = "goodsId",required = false)String goodsId
            ,@RequestParam(value = "userId",required = false)String userId,
             BlockException ex){
        //记录错误日志
        //logger.error(ex.getMessage()))

        return "用户下单失败，请稍后重试";
    }
```



![image-20220426164323925](https://www.ydlclass.com/doc21xnv/assets/image-20220426164323925.7320c157.png)

#### （3）例外情况

秒杀情况下，某个参数goodsId=100,最新手机id,单独设置阈值1000.

a.配置

![image-20220426165418745](https://www.ydlclass.com/doc21xnv/assets/image-20220426165418745.305b0b1c.png)

b.疯狂访问http://localhost:8000/demo/order?goodsId=100&userId=10 不会降级

![image-20220426165532893](https://www.ydlclass.com/doc21xnv/assets/image-20220426165532893.ec99b064.png)

注意：参数例外项，仅支持基本类型和字符串类型

注意：

加一个：int a=1/0;

业务上的错，sentinel不会管

![image-20220426165926729](https://www.ydlclass.com/doc21xnv/assets/image-20220426165926729.de926f70.png)

热点key限制,sentinel才会管，疯狂访问

![image-20220426170005357](https://www.ydlclass.com/doc21xnv/assets/image-20220426170005357.77d86ed3.png)

### 7、系统自适应限流

Sentinel 系统自适应限流从整体维度对应用入口流量进行控制，结合应用的 Load、CPU 使用率、总体平均 RT、入口 QPS 和并发线程数等几个维度的监控指标，通过自适应的流控策略，让系统的入口流量和系统的负载达到一个平衡，让系统尽可能跑在最大吞吐量的同时保证系统整体的稳定性。

![image-20220426172901422](https://www.ydlclass.com/doc21xnv/assets/image-20220426172901422.ca6a7148.png)

#### （1）背景

在开始之前，我们先了解一下系统保护的目的：

- 保证系统不被拖垮
- 在系统稳定的前提下，保持系统的吞吐量

长期以来，系统保护的思路是根据硬指标，即系统的负载 (load1) 来做系统过载保护。当系统负载高于某个阈值，就禁止或者减少流量的进入；当 load 开始好转，则恢复流量的进入。这个思路给我们带来了不可避免的两个问题：

- load 是一个“结果”，如果根据 load 的情况来调节流量的通过率，那么就始终有延迟性。也就意味着通过率的任何调整，都会过一段时间才能看到效果。当前通过率是使 load 恶化的一个动作，那么也至少要过 1 秒之后才能观测到；同理，如果当前通过率调整是让 load 好转的一个动作，也需要 1 秒之后才能继续调整，这样就浪费了系统的处理能力。所以我们看到的曲线，总是会有抖动。
- 恢复慢。想象一下这样的一个场景（真实），出现了这样一个问题，下游应用不可靠，导致应用 RT 很高，从而 load 到了一个很高的点。过了一段时间之后下游应用恢复了，应用 RT 也相应减少。这个时候，其实应该大幅度增大流量的通过率；但是由于这个时候 load 仍然很高，通过率的恢复仍然不高。

[TCP BBRopen in new window](https://en.wikipedia.org/wiki/TCP_congestion_control#TCP_BBR) 的思想给了我们一个很大的启发。我们应该根据系统能够处理的请求，和允许进来的请求，来做平衡，而不是根据一个间接的指标（系统 load）来做限流。最终我们追求的目标是 **在系统不被拖垮的情况下，提高系统的吞吐率，而不是 load 一定要到低于某个阈值**。如果我们还是按照固有的思维，超过特定的 load 就禁止流量进入，系统 load 恢复就放开流量，这样做的结果是无论我们怎么调参数，调比例，都是按照果来调节因，都无法取得良好的效果。

Sentinel 在系统自适应保护的做法是，用 load1 作为启动自适应保护的因子，而允许通过的流量由处理请求的能力，即请求的响应时间以及当前系统正在处理的请求速率来决定。

#### （2）系统规则

系统保护规则是从应用级别的入口流量进行控制，从单台机器的 load、CPU 使用率、平均 RT、入口 QPS 和并发线程数等几个维度监控应用指标，让系统尽可能跑在最大吞吐量的同时保证系统整体的稳定性。

系统保护规则是应用整体维度的，而不是资源维度的，并且**仅对入口流量生效**。入口流量指的是进入应用的流量（`EntryType.IN`），比如 Web 服务或 Dubbo 服务端接收的请求，都属于入口流量。

系统规则支持以下的模式：

- **Load 自适应**（仅对 Linux/Unix-like 机器生效）：系统的 load1 作为启发指标，进行自适应系统保护。当系统 load1 超过设定的启发值，且系统当前的并发线程数超过估算的系统容量时才会触发系统保护（BBR 阶段）。系统容量由系统的 `maxQps * minRt` 估算得出。设定参考值一般是 `CPU cores * 2.5`。
- **CPU usage**（1.5.0+ 版本）：当系统 CPU 使用率超过阈值即触发系统保护（取值范围 0.0-1.0），比较灵敏。
- **平均 RT**：当单台机器上所有入口流量的平均 RT 达到阈值即触发系统保护，单位是毫秒。
- **并发线程数**：当单台机器上所有入口流量的并发线程数达到阈值即触发系统保护。
- **入口 QPS**：当单台机器上所有入口流量的 QPS 达到阈值即触发系统保护。

#### （3）原理

先用经典图来镇楼:

![TCP-BBR-pipe](https://www.ydlclass.com/doc21xnv/assets/50813887-bff10300-1352-11e9-9201-437afea60a5a.b17e76ed.png)

我们把系统处理请求的过程想象为一个水管，到来的请求是往这个水管灌水，当系统处理顺畅的时候，请求不需要排队，直接从水管中穿过，这个请求的RT是最短的；反之，当请求堆积的时候，那么处理请求的时间则会变为：排队时间 + 最短处理时间。

- 推论一: 如果我们能够保证水管里的水量，能够让水顺畅的流动，则不会增加排队的请求；也就是说，这个时候的系统负载不会进一步恶化。

我们用 T 来表示(水管内部的水量)，用RT来表示请求的处理时间，用P来表示进来的请求数，那么一个请求从进入水管道到从水管出来，这个水管会存在 `P * RT`　个请求。换一句话来说，当 `T ≈ QPS * Avg(RT)` 的时候，我们可以认为系统的处理能力和允许进入的请求个数达到了平衡，系统的负载不会进一步恶化。

接下来的问题是，水管的水位是可以达到了一个平衡点，但是这个平衡点只能保证水管的水位不再继续增高，但是还面临一个问题，就是在达到平衡点之前，这个水管里已经堆积了多少水。如果之前水管的水已经在一个量级了，那么这个时候系统允许通过的水量可能只能缓慢通过，RT会大，之前堆积在水管里的水会滞留；反之，如果之前的水管水位偏低，那么又会浪费了系统的处理能力。

- 推论二:　当保持入口的流量是水管出来的流量的最大的值的时候，可以最大利用水管的处理能力。

然而，和 TCP BBR 的不一样的地方在于，还需要用一个系统负载的值（load1）来激发这套机制启动。

> 注：这种系统自适应算法对于低 load 的请求，它的效果是一个“兜底”的角色。**对于不是应用本身造成的 load 高的情况（如其它进程导致的不稳定的情况），效果不明显。**

设置

![image-20220426173149065](https://www.ydlclass.com/doc21xnv/assets/image-20220426173149065.0a3489f0.png)

测试http://localhost:8000/demo/testA

![image-20220426173225393](https://www.ydlclass.com/doc21xnv/assets/image-20220426173225393.57ff7229.png)

### 8、@SentinelResource

https://github.com/alibaba/Sentinel/wiki/%E6%B3%A8%E8%A7%A3%E6%94%AF%E6%8C%81

`@SentinelResource` 用于定义资源，并提供可选的异常处理和 fallback 配置项。 `@SentinelResource` 注解包含以下属性：

- `value`：资源名称，必需项（不能为空）

- `entryType`：entry 类型，可选项（默认为 `EntryType.OUT`）

- `blockHandler` / `blockHandlerClass`: `blockHandler` 对应处理 `BlockException` 的函数名称，可选项。blockHandler 函数访问范围需要是 `public`，返回类型需要与原方法相匹配，参数类型需要和原方法相匹配并且最后加一个额外的参数，类型为 `BlockException`。blockHandler 函数默认需要和原方法在同一个类中。若希望使用其他类的函数，则可以指定 `blockHandlerClass` 为对应的类的 `Class` 对象，注意对应的函数必需为 static 函数，否则无法解析。

- `fallback`/`fallbackClass`：fallback 函数名称，可选项，用于在抛出异常的时候提供 fallback 处理逻辑。fallback 函数可以针对所有类型的异常（除了

  `exceptionsToIgnore`里面排除掉的异常类型）进行处理。fallback 函数签名和位置要求：

  - 返回值类型必须与原函数返回值类型一致；
  - 方法参数列表需要和原函数一致，或者可以额外多一个 `Throwable` 类型的参数用于接收对应的异常。
  - fallback 函数默认需要和原方法在同一个类中。若希望使用其他类的函数，则可以指定 `fallbackClass` 为对应的类的 `Class` 对象，注意对应的函数必需为 static 函数，否则无法解析。

- `defaultFallback`（since 1.6.0）：默认的 fallback 函数名称，可选项，通常用于通用的 fallback 逻辑（即可以用于很多服务或方法）。默认 fallback 函数可以针对所有类型的异常（除了`exceptionsToIgnore`

  里面排除掉的异常类型）进行处理。若同时配置了 fallback 和 defaultFallback，则只有 fallback 会生效。defaultFallback 函数签名要求：

  - 返回值类型必须与原函数返回值类型一致；
  - 方法参数列表需要为空，或者可以额外多一个 `Throwable` 类型的参数用于接收对应的异常。
  - defaultFallback 函数默认需要和原方法在同一个类中。若希望使用其他类的函数，则可以指定 `fallbackClass` 为对应的类的 `Class` 对象，注意对应的函数必需为 static 函数，否则无法解析。

- `exceptionsToIgnore`（since 1.6.0）：用于指定哪些异常被排除掉，不会计入异常统计中，也不会进入 fallback 逻辑中，而是会原样抛出。

1.8.0 版本开始，`defaultFallback` 支持在类级别进行配置。

> 注：1.6.0 之前的版本 fallback 函数只针对降级异常（`DegradeException`）进行处理，**不能针对业务异常进行处理**。

特别地，若 blockHandler 和 fallback 都进行了配置，则被限流降级而抛出 `BlockException` 时只会进入 `blockHandler` 处理逻辑。若未配置 `blockHandler`、`fallback` 和 `defaultFallback`，则被限流降级时会将 `BlockException` **直接抛出**（若方法本身未定义 throws BlockException 则会被 JVM 包装一层 `UndeclaredThrowableException`）。

blockHandler熔断测试

a.controller

```java
    //减库存接口
    @GetMapping("/resume")
    @SentinelResource(value = "resume",
            blockHandlerClass = CommonBlockHandler.class,
            blockHandler = "handlerBlock2")
    public String resume(@RequestParam(value = "goodsId",required = false)String goodsId){
        //业务逻辑
//        int a=1/0;
        return "减库存成功";
    }
```

b.CommonBlockHandler

```java
package com.ydlclass.cloudalibabasentinelservice8000.handler;

import com.alibaba.csp.sentinel.slots.block.BlockException;
import org.springframework.web.bind.annotation.RequestParam;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 * 公共 流速限制 类。给用户返回容易解析的对象
 * Result<code,msg,data>
 */
public class CommonBlockHandler {

    public static String handlerBlock1(BlockException ex){
        //logger.error(限流了)

        return "系统出错，请稍后再试！";
    }

    public static String handlerBlock2(@RequestParam(value = "goodsId",required = false)String goodsId,
                                       BlockException ex){
        //logger.error(限流了)

        return "系统出错，请稍后再试！handlerBlock2";
    }
}
```



c.测试 狂点http://localhost:8000/demo/resume?goodsId=100

![image-20220429165149264](https://www.ydlclass.com/doc21xnv/assets/image-20220429165149264.31742c8d.png)

### 9、降级

#### （1）解释

a.熔断：微服务自己限流，不可用了。

b.降级：调用方，提供方错了，返回给客户一个友好对象。

![image-20220429171002222](https://www.ydlclass.com/doc21xnv/assets/image-20220429171002222.886e09b5.png)

#### （2）新建3个项目

**cloudalibaba-sentinal-provider9001**

a.pom

```xml
    <dependency>
        <groupId>com.alibaba.cloud</groupId>
        <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
    </dependency>
    <!-- SpringCloud ailibaba sentinel-->
    <dependency>
        <groupId>com.alibaba.cloud</groupId>
        <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-openfeign</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
```

b.yml

```yaml
server:
  port: 9001

spring:
  application:
    name: cloudalibaba-sentinal-provider
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848
    sentinel:
      transport:
        dashboard: localhost:8080
        # 默认8719端口，假如被占用了会自动从8719端口+1进行扫描，直到找到未被占用的端口
        port: 8719
      web-context-unify: false #关闭收敛URL
```

c.启动类

@EnableDiscoveryClient

d.controller

```java
package com.ydlclass.provider.controller;

import com.ydlclass.provider.domain.Goods;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
@RestController
@RequestMapping("/goods")
public class GoodsController {
    @Value("${server.port}")
    Integer port;
    
    @GetMapping("/findById/{id}")
    public Goods findById(@PathVariable("id") Integer id){
        //servive-->dao/mapper
        Goods goods=new Goods();
        goods.setGoodId(id);
        goods.setPrice(123);
        goods.setTitle("手机.port:"+port);
        goods.setStock(10);

        return goods;
    }
}
```



e.doamin

```java
package com.ydlclass.provider.domain;

import java.io.Serializable;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
public class Goods implements Serializable {
    private Integer goodId;
    private String title;
    private double price;
    private Integer stock;

    public Goods() {
    }


    public Goods(Integer goodId, String title, double price, Integer stock) {
        this.goodId = goodId;
        this.title = title;
        this.price = price;
        this.stock = stock;
    }

    @Override
    public String toString() {
        return "Goods{" +
                "goodId=" + goodId +
                ", title='" + title + '\'' +
                ", price=" + price +
                ", stock=" + stock +
                '}';
    }

    public Integer getGoodId() {
        return goodId;
    }

    public void setGoodId(Integer goodId) {
        this.goodId = goodId;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public double getPrice() {
        return price;
    }

    public void setPrice(double price) {
        this.price = price;
    }


    public Integer getStock() {
        return stock;
    }

    public void setStock(Integer stock) {
        this.stock = stock;
    }
}
```



测试：http://localhost:9001/goods/findById/1

![image-20220429172614387](https://www.ydlclass.com/doc21xnv/assets/image-20220429172614387.d0b8972c.png)

**cloudalibaba-sentinal-provider9002**

同理。创建。

**cloudalibaba-consumer-nacos-consumer8000**

a.pom

```xml
    <dependencies>
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
            <exclusions>
                <exclusion>
                    <groupId>org.springframework.cloud</groupId>
                    <artifactId>spring-cloud-starter-netflix-ribbon</artifactId>
                </exclusion>
            </exclusions>
        </dependency>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-loadbalancer</artifactId>
            <version>3.1.1</version>
        </dependency>
        <!-- SpringCloud ailibaba sentinel-->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
```



b.yml

```yaml
server:
  port: 8000

spring:
  application:
    name: cloudalibaba-sentinal-consumer
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848  #nacos
    sentinel:
      transport:
        dashboard: localhost:8080    #sentinel
        port: 8719

#激活Sentinel对Feign的支持
feign:
  sentinel:
    enabled: true
```



c.配置类

```java
package com.ydlclass.consumer.config;

import org.springframework.cloud.client.loadbalancer.LoadBalanced;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.client.RestTemplate;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
@Configuration
public class RestConfig {

    @Bean
    @LoadBalanced
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }
}
```

d.controller

```java
package com.ydlclass.consumer.controller;

import com.ydlclass.consumer.domain.Goods;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
@RestController
@RequestMapping("/order")
public class OrderController {
    @Autowired
    RestTemplate restTemplate;

    @GetMapping("/add/{id}")
    public Goods add(@PathVariable("id")Integer id){
        String url="http://cloudalibaba-sentinal-provider/goods/findById/"+id;

        Goods goods = restTemplate.getForObject(url, Goods.class);

        return goods;

    }

}
```



e.goods 实体类 copy

```java
package com.ydlclass.consumer.domain;

import java.io.Serializable;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
public class Goods implements Serializable {
    private Integer goodId;
    private String title;
    private double price;
    private Integer stock;

    public Goods() {
    }


    public Goods(Integer goodId, String title, double price, Integer stock) {
        this.goodId = goodId;
        this.title = title;
        this.price = price;
        this.stock = stock;
    }

    @Override
    public String toString() {
        return "Goods{" +
                "goodId=" + goodId +
                ", title='" + title + '\'' +
                ", price=" + price +
                ", stock=" + stock +
                '}';
    }

    public Integer getGoodId() {
        return goodId;
    }

    public void setGoodId(Integer goodId) {
        this.goodId = goodId;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public double getPrice() {
        return price;
    }

    public void setPrice(double price) {
        this.price = price;
    }


    public Integer getStock() {
        return stock;
    }

    public void setStock(Integer stock) {
        this.stock = stock;
    }
}
```



f.测试

http://localhost:8000/order/add/1

发现负载均衡

![image-20220429175240918](https://www.ydlclass.com/doc21xnv/assets/image-20220429175240918.42b7e40f.png)

![image-20220429175250362](https://www.ydlclass.com/doc21xnv/assets/image-20220429175250362.f1ec683f.png)

#### （3）不同情况下 blockHandler与fallback情况

a.不配置

```java
        if(id<0){
            throw new IllegalArgumentException("非法参数");
        }else if(id>100){
            throw new NullPointerException("查无此商品");
        }
```

![image-20220429180414730](https://www.ydlclass.com/doc21xnv/assets/image-20220429180414730.1efa7cfb.png)

b.只配blockHandler

```java
    public Goods fail_add(@PathVariable("id")Integer id, BlockException ex){
        Goods goods =new Goods();
        goods.setGoodId(-1);
        goods.setPrice(-1);
        goods.setStock(-1);
        goods.setTitle("限流之后的特殊对象");

        return goods;
    }
```

![image-20220429180844762](https://www.ydlclass.com/doc21xnv/assets/image-20220429180844762.b34ac559.png)

![image-20220429180818990](https://www.ydlclass.com/doc21xnv/assets/image-20220429180818990.1a6dac64.png)

正常访问错id

![image-20220429180927872](https://www.ydlclass.com/doc21xnv/assets/image-20220429180927872.c247cf02.png)

疯狂访问

![image-20220429180940663](https://www.ydlclass.com/doc21xnv/assets/image-20220429180940663.3e860e99.png)

c.只配fallback

```java
@SentinelResource(value = "add",fallback = "fallback_add")

//业务上的错的话
    public Goods fallback_add(@PathVariable("id")Integer id, Throwable ex){
        Goods goods =new Goods();
        goods.setGoodId(-2);
        goods.setPrice(-2);
        goods.setStock(-2);
        goods.setTitle("业务出错之后的特殊对象");

        return goods;
    }
```

![image-20220429181347488](https://www.ydlclass.com/doc21xnv/assets/image-20220429181347488.902a9299.png)

d.都配置

```text
@SentinelResource(value = "add",blockHandler = "fail_add",fallback = "fallback_add")
```

![image-20220429181534288](https://www.ydlclass.com/doc21xnv/assets/image-20220429181534288.e2839ace.png)

-1 慢慢访问

![image-20220429181559790](https://www.ydlclass.com/doc21xnv/assets/image-20220429181559790.b49e639a.png)

-1 疯狂访问

![image-20220429181637087](https://www.ydlclass.com/doc21xnv/assets/image-20220429181637087.8ef31560.png)

e.忽略某些异常

```text
    @SentinelResource(value = "add",blockHandler = "fail_add",fallback = "fallback_add",
            exceptionsToIgnore = {IllegalArgumentException.class})
```

![image-20220429181851414](https://www.ydlclass.com/doc21xnv/assets/image-20220429181851414.ece49582.png)

![image-20220429181925759](https://www.ydlclass.com/doc21xnv/assets/image-20220429181925759.1dd7e434.png)

### 10、feign调用

a.pom

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

b.yml

```yaml
#前面也已经添加了
#激活Sentinel对Feign的支持
feign:
  sentinel:
    enabled: true
```

c.主启动类 @EnableFeignClients

d.feign接口

```java
package com.ydlclass.consumer.feign;

import com.ydlclass.consumer.domain.Goods;
import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
@FeignClient(value = "cloudalibaba-sentinal-provider",fallback = GoodsFeignImpl.class)
public interface GoodsFeign {
    @GetMapping("/goods/findById/{id}")
    public Goods findById(@PathVariable("id") Integer id);
}
```

e.实现类PaymentFallbackService

```java
package com.ydlclass.consumer.feign;

import com.ydlclass.consumer.domain.Goods;
import org.springframework.stereotype.Component;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
@Component
public class GoodsFeignImpl implements GoodsFeign{
    @Override
    public Goods findById(Integer id) {
        Goods goods =new Goods();
        goods.setGoodId(-3);
        goods.setPrice(-3);
        goods.setStock(-3);
        goods.setTitle("feign出错之后的特殊对象");

        return goods;
    }
}
```

f.controller

```java
    @Autowired
    GoodsFeign goodsFeign;

    @GetMapping("/add1/{id}")
    public Goods add1(@PathVariable("id")Integer id){
        Goods goods = goodsFeign.findById(id);
        return goods;
    }
```

g.测试出错，直接停了provider

![image-20220429183335192](https://www.ydlclass.com/doc21xnv/assets/image-20220429183335192.ae216a5f.png)

### 11、配置持久化

sentinel的流控配置是临时的，所以我们可以把配置持久化到nacos

a.pom

```xml
    <!-- SpringCloud ailibaba sentinel-datasource-nacos 持久化需要用到-->
    <dependency>
        <groupId>com.alibaba.csp</groupId>
        <artifactId>sentinel-datasource-nacos</artifactId>
    </dependency>
```

b.yml

```yaml
	     sentinel: 
	      datasource:
	        ds1:
	          nacos:
	            server-addr: localhost:8848  #nacos
        		dataId: ${spring.application.name}
	            groupId: DEFAULT_GROUP
	            data-type: json
	            rule-type: flow
```

c.nacos添加配置

```text
[
    {
        "resource": "/order/add1/{id}",
        "limitApp": "default",
        "grade": 1,
        "count": 1,
        "strategy": 0,
        "controlBehavior": 0,
        "clusterMode": false
    }
]
```

resource：资源名称； limitApp：来源应用； grade：阈值类型，0表示线程数，1表示QPS； count：单机阈值； strategy：流控模式，0表示直接，1表示关联，2表示链路； controlBehavior：流控效果，0表示快速失败，1表示Warm Up，2表示排队等待； clusterMode：是否集群

![image-20220429185256498](https://www.ydlclass.com/doc21xnv/assets/image-20220429185256498.d321681e.png)

d.测试

![image-20220429185954032](https://www.ydlclass.com/doc21xnv/assets/image-20220429185954032.bde212b1.png)