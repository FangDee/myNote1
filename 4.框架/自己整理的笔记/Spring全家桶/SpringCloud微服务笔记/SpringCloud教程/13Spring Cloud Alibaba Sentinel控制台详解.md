## Spring Cloud Alibaba Sentinel控制台详解

 2020-03-18 |  Visit count

[Sentinel](https://github.com/alibaba/Sentinel)提供一个轻量级的开源控制台，它提供机器发现以及健康情况管理、监控（单机和集群），规则管理和推送的功能。本节将详细记录何如通过Sentinel控制台控制Sentinel客户端的各种行为。Sentinel控制台的功能主要包括：流量控制、降级控制、热点配置、系统规则和授权规则等。



## 安装控制台

Sentinel控制台下载地址：https://github.com/alibaba/Sentinel/releases，本节我们下载sentinel-dashboard-1.7.1.jar版本，下载好后使用`java -jar sentinel-dashboard-1.7.1.jar`命令启动即可，默认的端口号为8080：

![QQ20200319-092904@2x](https://mrbird.cc/img/QQ20200319-092904@2x.png)

账号密码都是sentinel。

更多可用的启动参数配置：

- `Dsentinel.dashboard.auth.username=sentinel`用于指定控制台的登录用户名为 sentinel；
- `Dsentinel.dashboard.auth.password=123456`用于指定控制台的登录密码为 123456，如果省略这两个参数，默认用户和密码均为 sentinel；
- `Dserver.servlet.session.timeout=7200`用于指定 Spring Boot 服务端 session 的过期时间，如 7200 表示 7200 秒；60m 表示 60 分钟，默认为 30 分钟；
- `-Dcsp.sentinel.dashboard.server=consoleIp:port`指定控制台地址和端口。

## 搭建客户端

新建一个Spring Boot项目，artifactId为spring-cloud-alibaba-sentinel-dashboard-guide，项目结构如下所示：

![QQ20200319-094028@2x](https://mrbird.cc/img/QQ20200319-094028@2x.png)

项目pom内容：

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.5.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>cc.mrbird</groupId>
    <artifactId>spring-cloud-alibaba-sentinel-dashboard-guide</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>spring-cloud-alibaba-sentinel-dashboard-guide</name>
    <description>Demo project for Spring Boot</description>

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
            <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
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



引入了spring-cloud-starter-alibaba-sentinel和spring-boot-starter-web依赖。

接着在cc.mrbird.sentinel目录下新建controller包，然后在该包下新建`TestController`：

```
@RestController
public class TestController {

    @GetMapping("test1")
    public String test1() {
        return "test1";
    }
}
```



最后在项目配置文件application.yml中添加如下配置：

```
server:
  port: 8081
spring:
  application:
    name: my-project
  cloud:
    sentinel:
      transport:
        dashboard: localhost:8080
        port: 8719
```



- `spring.cloud.sentinel.transport.dashboard`，指定了sentinel控制台的ip和端口地址；
- `spring.cloud.sentinel.transport.port`，sentinel客户端和控制台通信的端口，默认为8719，如果这个端口已经被占用，那么sentinel会自动从8719开始依次+1扫描，直到找到未被占用的端口。

启动项目，使用浏览器多次访问：http://localhost:8081/test1，然后登录sentinel控制台：

![QQ20200319-100041@2x](https://mrbird.cc/img/QQ20200319-100041@2x.png)

在簇点链路中可以看到刚刚那笔请求，我们可以对它进行流控、降级、授权、热点等配置（控制台是懒加载的，如果没有任何请求，那么控制台也不会有任何内容）。

## 流控规则

在簇点链路列表中，点击`/test1`后面的流控按钮：

![QQ20200319-100633@2x](https://mrbird.cc/img/QQ20200319-100633@2x.png)

- 资源名：标识资源的唯一名称，默认为请求路径，也可以在客户端中使用`SentinelResource`配置；
- 针对来源：Sentinel可以针对服务调用者进行限流，填写微服务名称即`spring.application.name`，默认为default，不区分来源；
- 阈值类型、单机阈值：
  - QPS（Queries-per-second，每秒钟的请求数量）：当调用该api的QPS达到阈值的时候，进行限流；
  - 线程数：当调用该api的线程数达到阈值的时候，进行限流。
- 是否集群：默认不集群；
- 流控模式：
  - 直接：当api调用达到限流条件的时，直接限流；
  - 关联：当关联的资源请求达到阈值的时候，限流自己；
  - 链路：只记录指定链路上的流量（指定资源从入口资源进来的流量，如果达到阈值，则进行限流）。
- 流控效果：
  - 快速失败：直接失败；
  - Warm Up：根据codeFactor（冷加载因子，默认值为3）的值，从阈值/codeFactor，经过预热时长，才达到设置的QPS阈值；
  - 排队等待：匀速排队，让请求匀速通过，阈值类型必须设置为QPS，否则无效。

### QPS直接失败

演示下QPS直接失败设置及效果。点击簇点链路列表中`/test1`请求后面的流控按钮：

![QQ20200319-103726@2x](https://mrbird.cc/img/QQ20200319-103726@2x.png)

上面设置的效果是，1秒钟内请求/test1资源的次数达到2次以上的时候，进行限流。

点击新增按钮后，列表会跳转到”流控规则”菜单下，所以我们也可以在这里设置流控规则，不过一般还是习惯直接在簇点链路列表中直接选中资源添加流控规则。新增后，我们访问http://localhost:8081/test1，看看效果：

![2020-03-19 10.47.02.gif](https://mrbird.cc/img/2020-03-19%2010.47.02.gif)

当手速快点的时候（1秒超过2次），页面返回Blocked by Sentinel (flow limiting)。

### 线程数直接失败

改造下`TestController`：

```
@RestController
public class TestController {

    @GetMapping("test1")
    public String test1() throws InterruptedException {
        TimeUnit.SECONDS.sleep(1);
        return "test1";
    }
}
```



让方法休眠1秒，更容易出现效果。重启项目，然后在sentinel控制台中添加如下流控规则：

![QQ20200319-105234@2x](https://mrbird.cc/img/QQ20200319-105234@2x.png)

多次访问http://localhost:8081/test1，效果：

![2020-03-19 10.57.46.gif](https://mrbird.cc/img/2020-03-19%2010.57.46.gif)

### 关联

改造`TestController`，添加一个新的api接口：

```
@RestController
public class TestController {

    @GetMapping("test1")
    public String test1() {
        return "test1";
    }

    @GetMapping("test2")
    public String test2() {
        return "test2";
    }
}
```



重启项目，浏览器中访问下这两个请求，然后在sentinel控制台簇点链路列表中，点击test1后的流控按钮：

![QQ20200319-110332@2x](https://mrbird.cc/img/QQ20200319-110332@2x.png)

上述流控规则表示：当1秒内访问/test2的次数大于2的时候，限流/test1。

我们使用postman来密集访问/test2，然后我们手动浏览器请求/test1，看看效果。postman设置如下规则：

![QQ20200319-111530@2x](https://mrbird.cc/img/QQ20200319-111530@2x.png)

然后点击Run按钮，我们回到浏览器，访问：http://localhost:8081/test1：

![2020-03-19 11.17.11.gif](https://mrbird.cc/img/2020-03-19%2011.17.11.gif)

可以看到/test1已经被限流了。

### 链路

在演示链路限流之前，我们先改造改造sentinel客户端代码。在项目的cc.mrbird.sentinel目录下新建service包，然后在该包下新建`HelloService`：

```
@Service
public class HelloService {
    @SentinelResource("hello")
    public String hello() {
        return "hello";
    }
}
```



`@SentinelResource("hello")`将该方法标识为一个sentinel资源，名称为hello。

接着在`TestController`中使用该资源：

```
@RestController
public class TestController {

    @Autowired
    private HelloService helloService;

    @GetMapping("test1")
    public String test1() {
        return "test1 " + helloService.hello();
    }

    @GetMapping("test2")
    public String test2() {
        return "test2 " + helloService.hello();
    }
}
```



重启项目，在浏览器中多次访问这两个api，然后查看sentinel控制台簇点链路：

![QQ20200319-151619@2x](https://mrbird.cc/img/c.png)

然后点击hello资源后面的流控按钮：

![QQ20200319-151704@2x](https://mrbird.cc/img/QQ20200319-151704@2x.png)

上述配置的意思是，当通过/test1访问hello的时候，QPS大于1则进行限流；言外之意就是/test2访问hello请求并不受影响。

但是实际测试并没有生效😡，具体可以参考issue：https://github.com/alibaba/Sentinel/issues/1213。

### 预热Warm Up

流控效果除了直接失败外，我们也可以选择预热Warm Up。

Warm Up（RuleConstant.CONTROL_BEHAVIOR_WARM_UP）方式，即预热/冷启动方式。当系统长期处于低水位的情况下，当流量突然增加时，直接把系统拉升到高水位可能瞬间把系统压垮。通过”冷启动”，让通过的流量缓慢增加，在一定时间内逐渐增加到阈值上限，给冷系统一个预热的时间，避免冷系统被压垮。

sentinel客户端的默认冷加载因子coldFactor为3，即请求QPS从 threshold / 3 开始，经预热时长逐渐升至设定的QPS阈值。

比如：

![QQ20200319-152842@2x](https://mrbird.cc/img/QQ20200319-152842@2x.png)

上面的配置意思是：对于/test1资源，一开始的QPS阈值为3（10/3），经过10秒后，QPS阈值达到10，过程类似于下图：

![QQ20200319-153013@2x](https://mrbird.cc/img/QQ20200319-153013@2x.png)

效果我就不演示了，大概效果就是使用浏览器访问http://localhost:8081/test1，以最快的手速点刷新，一开始会常看到Blocked by Sentinel (flow limiting)的提示，10秒后几乎不再出现（因为你的手速很难达到1秒10下）。

### 排队等待

排队等待方式不会拒绝请求，而是严格控制请求通过的间隔时间，也即是让请求以均匀的速度通过。

修改`TestController`的代码：

```
@RestController
public class TestController {

    private Logger log = LoggerFactory.getLogger(this.getClass());

    @GetMapping("test1")
    public String test1() {
        log.info("test1");
        return "test1";
    }

}
```



重启项目，浏览器访问[localhost:8081/test1](localhost:8081/test1)，然后点击sentinel控制台簇点链路列表的/test1流控按钮：

![QQ20200319-161608@2x](https://mrbird.cc/img/QQ20200319-161608@2x.png)

上述配置的含义是，访问/test1请求每秒钟最多只能1次，超过的请求排队等待，等待超过1000毫秒则超时。新增该规则后，多次快速访问[localhost:8081/test1](localhost:8081/test1)，sentinel客户端控制台日志打印如下：

```
2020-03-18 16:17:23.247  INFO 44688 --- [nio-8081-exec-2] c.m.sentinel.controller.TestController   : test1
2020-03-18 16:17:24.250  INFO 44688 --- [nio-8081-exec-6] c.m.sentinel.controller.TestController   : test1
2020-03-18 16:17:25.251  INFO 44688 --- [nio-8081-exec-5] c.m.sentinel.controller.TestController   : test1
2020-03-18 16:17:26.251  INFO 44688 --- [nio-8081-exec-7] c.m.sentinel.controller.TestController   : test1
2020-03-18 16:17:27.251  INFO 44688 --- [nio-8081-exec-8] c.m.sentinel.controller.TestController   : test1

```



每笔请求时间间隔1秒。

## 降级规则

降级配置页面：

![QQ20200319-164303@2x](https://mrbird.cc/img/QQ20200319-164303@2x.png)

降级策略分为3种：

- RT，平均响应时间 (DEGRADE_GRADE_RT)：当 1s 内持续进入 5 个请求，对应时刻的平均响应时间（秒级）均超过阈值（count，以 ms 为单位），那么在接下的时间窗口（DegradeRule 中的 timeWindow，以 s 为单位）之内，对这个方法的调用都会自动地熔断（抛出 DegradeException）。注意 Sentinel 默认统计的 RT 上限是 4900 ms，超出此阈值的都会算作 4900 ms，若需要变更此上限可以通过启动配置项 `-Dcsp.sentinel.statistic.max.rt=xxx` 来配置。
- 异常比例 (DEGRADE_GRADE_EXCEPTION_RATIO)：当资源的每秒请求量 >= 5，并且每秒异常总数占通过量的比值超过阈值（DegradeRule 中的 count）之后，资源进入降级状态，即在接下的时间窗口（DegradeRule 中的 timeWindow，以 s 为单位）之内，对这个方法的调用都会自动地返回。异常比率的阈值范围是 [0.0, 1.0]，代表 0% - 100%。
- 异常数 (DEGRADE_GRADE_EXCEPTION_COUNT)：当资源近 1 分钟的异常数目超过阈值之后会进行熔断。注意由于统计时间窗口是分钟级别的，若 timeWindow 小于 60s，则结束熔断状态后仍可能再进入熔断状态。

### RT

修改`TestController`的代码：

```
@RestController
public class TestController {

    @GetMapping("test1")
    public String test1() throws InterruptedException {
    	// 休眠1秒
        TimeUnit.SECONDS.sleep(1);
        return "test1";
    }

}
```



重启项目，浏览器访问[localhost:8081/test1](localhost:8081/test1)，然后点击sentinel控制台簇点链路列表的/test1降级按钮：

![QQ20200319-171421@2x](https://mrbird.cc/img/QQ20200319-171421@2x.png)

上面配置的含义是：如果/test1的QPS大于5，并且平均响应时间（以秒级统计）大于500（RT）毫秒，那么在未来的1秒钟（时间窗口）内，sentinel断路器打开，该api接口不可用。

我们使用JMeter模拟并发访问情形：

![QQ20200319-171907@2x](https://mrbird.cc/img/QQ20200319-171907@2x.png)

![QQ20200319-171941@2x](https://mrbird.cc/img/QQ20200319-171941@2x.png)

上面的QPS为10，而且我们的/test1接口响应时间在1秒左右，根据我们上面配置的降级规则，肯定会触发降级，使用JMeter测试结果如下：

![QQ20200319-172138@2x](https://mrbird.cc/img/QQ20200319-172138@2x.png)

可以看到，程序很快就触发了降级，断路器打开，后续的请求都返回了Blocked by Sentinel (flow limiting)。停掉JMeter，浏览器访问[localhost:8081/test1](localhost:8081/test1)

![QQ20200319-172646@2x](https://mrbird.cc/img/QQ20200319-172646@2x.png)

降级规则不再符合，所以断路器合上，请求正常响应。

### 异常比例

修改`TestController`的代码：

```
@RestController
public class TestController {

    @GetMapping("test1")
    public String test1() {
    	// 每次请求都抛出异常
        throw new RuntimeException("服务异常");
    }

}
```



重启项目，浏览器访问[localhost:8081/test1](localhost:8081/test1)，然后点击sentinel控制台簇点链路列表的/test1降级按钮：

![QQ20200319-173124@2x](https://mrbird.cc/img/QQ20200319-173124@2x.png)

上面的配置含义是：如果/test1的QPS大于5，并且每秒钟的请求异常比例大于0.5的话，那么在未来的1秒钟（时间窗口）内，sentinel断路器打开，该api接口不可用。

也就是说，如果一秒内有10个请求进来，超过5个以上都出错，那么会触发熔断，1秒钟内这个接口不可用。

还是使用上面的JMeter测试，开启JMeter后，使用浏览器访问：http://localhost:8081/test1

![QQ20200319-173524@2x](https://mrbird.cc/img/QQ20200319-173524@2x.png)

关掉JMeter后，再次访问http://localhost:8081/test1：

![QQ20200319-173631@2x](https://mrbird.cc/img/QQ20200319-173631@2x.png)

### 异常数

奖及策略为异常数时表示：当指定时间窗口内，请求异常数大于等于某个值时，触发降级。比如有如下规则：

![QQ20200319-182819@2x](https://mrbird.cc/img/QQ20200319-182819@2x.png)

上面的规则表示：在70秒内，访问/test1请求异常的次数大于等于5，则触发降级。测试一波：

![2020-03-19 18.28.50.gif](https://mrbird.cc/img/2020-03-19%2018.28.50.gif)

可以看到，当第5次访问的时候成功触发了降级。

## 热点规则

热点即经常访问的数据。很多时候我们希望统计某个热点数据中访问频次最高的数据，并对其访问进行限制。

比如在`TestController`中有如下方法：

```
@GetMapping("buy")
@SentinelResource(value = "buy")
public String buy(String goodName, Integer count) {
    return "买" + count + "份" + goodName;
}
```



对这个资源添加热点规则：

![QQ20200319-185115@2x](https://mrbird.cc/img/QQ20200319-185115@2x.png)

上面的配置含义是：对buy资源添加热点规则，当第0个参数的值为miband的时候QPS阈值为10，否则为1。此外，如果第0个参数不传，那么这笔请求不受该热点规则限制。效果如下：

![2020-03-19 18.58.12.gif](https://mrbird.cc/img/2020-03-19%2018.58.12.gif)

## 系统规则

系统规则则是针对整个系统设置限流规则，并不针对某个资源，设置页面如下：

![QQ20200319-190751@2x](https://mrbird.cc/img/QQ20200319-190751@2x.png)

阈值类型包含以下五种：

- Load 自适应（仅对 Linux/Unix-like 机器生效）：系统的 load1 作为启发指标，进行自适应系统保护。当系统 load1 超过设定的启发值，且系统当前的并发线程数超过估算的系统容量时才会触发系统保护（BBR 阶段）。系统容量由系统的 maxQps *minRt 估算得出。设定参考值一般是 CPU cores* 2.5。
- CPU usage（1.5.0+ 版本）：当系统 CPU 使用率超过阈值即触发系统保护（取值范围 0.0-1.0），比较灵敏。
- 平均 RT：当单台机器上所有入口流量的平均 RT 达到阈值即触发系统保护，单位是毫秒。
- 并发线程数：当单台机器上所有入口流量的并发线程数达到阈值即触发系统保护。
- 入口 QPS：当单台机器上所有入口流量的 QPS 达到阈值即触发系统保护。

比较直观就不演示了。

## 授权规则

授权规则用于配置资源的黑白名单：

![QQ20200319-191154@2x](https://mrbird.cc/img/QQ20200319-191154@2x.png)

上述配置表示：只有appA和appB才能访问hello资源。

sentinel控制台规则会在客户端重启后丢失，可以配合nacos等进行配置持久化，具体可以参考云大佬的博客：[https://www.sonake.com/2019/12/16/Sentinel-Nacos%E5%AE%9E%E7%8E%B0%E8%A7%84%E5%88%99%E6%8C%81%E4%B9%85%E5%8C%96/](https://www.sonake.com/2019/12/16/Sentinel-Nacos实现规则持久化/)。

> 参考链接 https://github.com/alibaba/Sentinel/wiki

> 本节源码链接：https://github.com/wuyouzhuguli/SpringAll/tree/master/77.spring-cloud-alibaba-sentinel-dashboard-guide。