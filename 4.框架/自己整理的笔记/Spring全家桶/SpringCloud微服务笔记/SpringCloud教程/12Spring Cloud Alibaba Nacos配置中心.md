## Spring Cloud Alibaba Nacos配置中心

 2020-03-13 |  Visit count 645026

上一节[Spring Cloud Alibaba Nacos注册中心](https://mrbird.cc/Spring-Cloud-Alibaba-Nacos注册中心.html)记录了Nacos作为注册中心的使用方式，这节继续记录下Nacos作为配置中心的使用方式。本节使用的Spring Cloud版本为Hoxton.SR3，Spring Cloud Alibaba版本为2.2.0.RELEASE，Spring Boot版本为2.2.3.RELEASE。



## 框架搭建

新建一个Spring Boot项目，artifactId为spring-cloud-alibaba-nacos-config，项目结构如下图所示：

![QQ20200313-161613@2x](https://mrbird.cc/img/QQ20200313-161613@2x.png)

项目的pom内容：

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
    <artifactId>spring-cloud-alibaba-nacos-config</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>spring-cloud-alibaba-nacos-config</name>
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
            <artifactId>spring-cloud-alibaba-nacos-config</artifactId>
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



因为这节记录的是Nacos作为配置中心的功能，所以引入的是`spring-cloud-alibaba-nacos-config`依赖。

## 基本使用

在项目配置文件application.yml中添加如下配置：

```
server:
  port: 8080
spring:
  application:
    name: my-project
```



上面配置指定应用端口为8080，应用名称为my-project。

接着在resources目录下新建配置文件bootstrap.yml，在里面添加如下Nacos config配置（必须在bootstrap.yml中配置，bootstrap.yml优先级比application.yml高）：

```
spring:
  cloud:
    nacos:
      config:
        server-addr: localhost:8848
        file-extension: yaml
```



- `spring.cloud.nacos.config.server-addr`配置了Nacos配置中心的地址，也可以通过`spring.cloud.nacos.server-addr`指定，它们两个是等价的；
- `spring.cloud.nacos.config.file-extension`指定待会在Nacos配置中心读取的配置的格式为yaml格式。

我们回到Nacos控制台http://localhost:8848/nacos，在配置列表中新建一个配置：

![QQ20200316-091128@2x](https://mrbird.cc/img/QQ20200316-091128@2x.png)

![QQ20200316-091434@2x](https://mrbird.cc/img/QQ20200316-091434@2x.png)

我们新建了一个my-project.yaml配置（dataId为my-project.yaml，group为DEFAULT_GROUP，它们的具体含义下面会介绍到），配置了`message: 'hello nacos config'`，创建好后，点击发布即可。

接着回到我们的项目，在cc.mrbird.nacos目录下新建controller包，然后在该包下新建`TestController`用于测试配置获取规则：

```
@RestController
@RefreshScope
public class TestController {

    @Value("${message:null}")
    private String message;

    @GetMapping("message")
    public String getMessage() {
        return this.message;
    }
}
```



上面代码我们从刚刚在Nacos控制台配置的配置文件中获取`message`配置的值，`@RefreshScope`用于刷新配置，即我们在Nacos控制台修改了相关配置点击发布后，我们的应用能够在不重启的情况下获取到最新的配置。

启动项目，在浏览器中访问：http://localhost:8080/message：

![QQ20200316-092401@2x](https://mrbird.cc/img/QQ20200316-092401@2x.png)

配置获取成功，在Nacos控制台中将`message`值修改为`hello world`后发布，再次访问http://localhost:8080/message：

![QQ20200316-092754@2x](https://mrbird.cc/img/QQ20200316-092754@2x.png)

## 获取配置规则

Nacos配置中心通过namespace、dataId和group来唯一确定一条配置。

1. namespace，即命名空间。默认的命名空间为public，我们可以在Nacos控制台中新建命名空间；

2. dataId，即配置文件名称，dataId的拼接格式如下：

   ```
   ${prefix} - ${spring.profiles.active} . ${file-extension}
   ```

   - `prefix`默认为`pring.application.name`的值，也可以通过配置项`spring.cloud.nacos.config.prefix`来配置；
   - `spring.profiles.active`即为当前环境对应的`profile`。**注意，当`spring.profiles.active`为空时，对应的连接符`-`也将不存在，dataId的拼接格式变成`${prefix}.${file-extension}`**；
   - `file-extension`为配置内容的数据格式，可以通过配置项`spring.cloud.nacos.config.file-extension`来配置。

3. group，即配置分组，默认为DEFAULT_GROUP，可以通过`spring.cloud.nacos.config.group`配置。

所以根据这些规则，上面示例中我们的应用名称`spring.application.name`为my-project，`spring.cloud.nacos.config.file-extension`的值为yaml，没有指定`spring.profiles.active`，于是dataId为my-project.yaml，分组为默认的DEFAULT_GROUP，命名空间为默认的public。这就是我们在Nacos控制台中新建配置时的根据。

## 配置划分实战

Nacos配置中心的namespace、dataId和group可以方便灵活地划分配置。比如，我们现在有一个项目需要开发，项目名称为febs，项目开发人员分为两个组：GROUP_A和GROUP_B，项目分为三个环境：开发环境dev、测试环境test和生产环境prod。

假如现在GROUP_A组的组长需要在Nacos中新建一个开发环境的febs项目配置，那么他可以这样做：

1. 在Nacos控制台中新建一个名称为febs的命名空间：

![QQ20200316-094921@2x](https://mrbird.cc/img/QQ20200316-094921@2x.png)

![QQ20200316-094952@2x](https://mrbird.cc/img/QQ20200316-094952@2x.png)

![QQ20200316-095015@2x](https://mrbird.cc/img/QQ20200316-095015@2x.png)

新建febs命名空间后，会生成一个唯一标识该命名空间的命名空间id`2ef2186e-078c-4904-8643-ff5e90555456`。

1. 在Nacos控制台中新建一个配置：

![QQ20200316-100032@2x](https://mrbird.cc/img/QQ20200316-100032@2x.png)

![QQ20200316-095417@2x](https://mrbird.cc/img/QQ20200316-095417@2x.png)

1. 最后在febs项目的bootstrap.yml配置文件中添加如下配置即可：

   ```
   spring:  profiles:    active: dev  cloud:    nacos:      config:        server-addr: localhost:8848        file-extension: yaml        prefix: febs        namespace: '2ef2186e-078c-4904-8643-ff5e90555456'        group: GROUP_A
   ```

![QQ20200316-100224@2x](https://mrbird.cc/img/QQ20200316-100224@2x.png)

## 配置回滚

Nacos中，修改配置点击发布后会创建一个对应的历史版本快照，我们可以在Nacos控制台的历史版本列表中找到这些快照：

![QQ20200316-100425@2x](https://mrbird.cc/img/QQ20200316-100425@2x.png)

点击回滚按钮即可将配置恢复到指定的版本。

## 获取多个配置

除了通过上面的方式指定一个唯一配置外，我们还可以同时获取多个配置文件的内容，比如，将项目的bootstrap.yml内容修改为：

```
spring:
  cloud:
    nacos:
      config:
        server-addr: localhost:8848
        extension-configs:
          - dataId: ext-config-one.yaml
            group: DEFAULT_GROUP
            refresh: true
          - dataId: ext-config-two.yaml
            group: DEFAULT_GROUP
            refresh: false
```



- `spring.cloud.nacos.config.extension-configs[n].dataId`，指定多个配置的dataId，必须包含文件格式，支持properties、yaml或yml；
- `spring.cloud.nacos.config.extension-configs[n].group`，指定分组；
- `spring.cloud.nacos.config.extension-configs[n].refresh`，是否支持刷新。

上面的配置中，我们分别从DEFAULT_GROUP中获取了`ext-config-one.yaml`和`ext-config-two.yaml`配置内容，并且`ext-config-one.yaml`支持刷新，`ext-config-two.yaml`不支持刷新。

没有namespace的配置，言外之意就是Nacos目前还不支持多个配置指定不同的命名空间。

我们在Nacos控制台中新建这两个配置：

![QQ20200316-105741@2x](https://mrbird.cc/img/QQ20200316-105741@2x.png)

`ext-config-one.yaml`配置内容：

```
ext1: 'hello'
```



`ext-config-two.yaml`配置内容：

```
ext2: 'world'
```



在项目的`TestController`中添加：

```
@RestController
@RefreshScope
public class TestController {

    @Value("${ext1:null}")
    private String ext1;
    @Value("${ext2:null}")
    private String ext2;

    @GetMapping("multi")
    public String multiConfig() {
        return String.format("ext1: %s ext2: %s", ext1, ext2);
    }

    ...
}
```



启动项目，浏览器访问：http://localhost:8080/multi：

![QQ20200316-112930@2x](https://mrbird.cc/img/QQ20200316-112930@2x.png)

将`ext1`的值修改为`nice`，`ext2`的值修改为`job`：

![QQ20200316-113120@2x](https://mrbird.cc/img/QQ20200316-113120@2x.png)

可以看到`ext1`的值更新了，`ext2`没有更新。

## 多配置共享

多配置共享其实和获取多个文件配置作用差不多，下面演示下多配置共享。

将bootstrap.yml配置修改为：

```
spring:
  cloud:
    nacos:
      config:
        server-addr: localhost:8848
        shared-configs: ext-config-one.yaml,ext-config-two.yaml
```



`spring.cloud.nacos.config.shared-configs`指定了共享`ext-config-one.yaml`和`ext-config-two.yaml`的配置。

重启项目，访问http://localhost:8080/multi：

![QQ20200316-134742@2x](https://mrbird.cc/img/QQ20200316-134742@2x.png)

也可以正常获取。

可以看到，无论是多配置共享还是获取多个配置，要完成的事情是一样的，不过它们都有各自的局限性。多配置共享无法指定分组、无法指定命名空间、无法配置是否刷新；获取多个配置相对较为灵活，不过也不能配置命名空间。具体相关的讨论可以参考：https://github.com/alibaba/spring-cloud-alibaba/issues/141

## 常用配置

| 配置项                   | key                                       | 默认值        | 说明                                                         |
| :----------------------- | :---------------------------------------- | :------------ | :----------------------------------------------------------- |
| 服务端地址               | spring.cloud.nacos.config.server-addr     |               |                                                              |
| DataId前缀               | spring.cloud.nacos.config.prefix          |               | spring.application.name                                      |
| Group                    | spring.cloud.nacos.config.group           | DEFAULT_GROUP |                                                              |
| dataID后缀及内容文件格式 | spring.cloud.nacos.config.file-extension  | properties    | dataId的后缀，同时也是配置内容的文件格式，目前只支持 properties |
| 配置内容的编码方式       | spring.cloud.nacos.config.encode          | UTF-8         | 配置的编码                                                   |
| 获取配置的超时时间       | spring.cloud.nacos.config.timeout         | 3000          | 单位为 ms                                                    |
| 配置的命名空间           | spring.cloud.nacos.config.namespace       |               | 常用场景之一是不同环境的配置的区分隔离，例如开发测试环境和生产环境的资源隔离等。 |
| AccessKey                | spring.cloud.nacos.config.access-key      |               |                                                              |
| SecretKey                | spring.cloud.nacos.config.secret-key      |               |                                                              |
| 相对路径                 | spring.cloud.nacos.config.context-path    |               | 服务端 API 的相对路径                                        |
| 接入点                   | spring.cloud.nacos.config.endpoint        |               | 地域的某个服务的入口域名，通过此域名可以动态地拿到服务端地址 |
| 是否开启监听和自动刷新   | spring.cloud.nacos.config.refresh.enabled | true          |                                                              |

参考链接：

https://github.com/alibaba/spring-cloud-alibaba/blob/master/spring-cloud-alibaba-examples/nacos-example/nacos-config-example/readme-zh.md

https://spring-cloud-alibaba-group.github.io/github-pages/greenwich/spring-cloud-alibaba.html#_spring_cloud_alibaba_nacos_config