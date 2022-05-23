# 2.Maven的依赖冲突

### 安装maven3.6.10

上篇文章中安装的是Maven3.6.2版本，这个版本在运行过程中会有一些问题，请大家按照上一篇文章的介绍重新安装3.6.1版本。

### idea中配置maven

> 先说几句，如果你使用的是eclipse，建议你去尝试使用一下idea，非常优秀的一款开发工具，后面我们一直采用idea作为开发工具来讲解案例，建议大家也使用这个。

打开idea，点击File->setting

![图片](https://mmbiz.qpic.cn/mmbiz_png/xicEJhWlK06BmBu5jcGtXXESwqDCMf1S2dDakWY6wMjDIVwPpojLE0XgibsQYnXac6pib9oKe9K0qhU1vqoibEffcg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

按照如下配置maven的信息，点击“ok”，idea中maven配置完成

> 注意"Maven home directory"选择我们上面安装的3.6.1
>
> User settings file 和 Local repository 我们使用用户级别的。

![图片](https://mmbiz.qpic.cn/mmbiz_png/xicEJhWlK06BmBu5jcGtXXESwqDCMf1S29mkgCRPJ5HPibz3UzHJJa53zam065cbWhbId29GfM8T4KkC882iaA1qA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

### 使用mven创建一个springboot项目

我们来创建一个web项目，然后输出一句话，我们采用maven的方式来创建看看有多简单。

打开idea，点击File->New->Project，选择Spring Initializr，如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/xicEJhWlK06BmBu5jcGtXXESwqDCMf1S2dRG4gAXicQksYu6SeiaHjtYbkLJIdVtKpqUcxL488zGibSFP888ibNYEmA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击"Next"，如下图，按照图中的信息输入，点击"Next"：

> 咱们先不用关注需要输入的信息具体是什么意思，后面会讲解。

![图片](https://mmbiz.qpic.cn/mmbiz_png/xicEJhWlK06BmBu5jcGtXXESwqDCMf1S2HSVIwugJNkCugmn8U3V9iaKErBlZAKZ98E3ibyiadMrzg7cDe9VsLmH9Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

选择springboot版本，勾选"Lombook"、"Spring Web"，如下图：

![图片](https://mmbiz.qpic.cn/mmbiz_png/xicEJhWlK06BmBu5jcGtXXESwqDCMf1S2v6c94o6Ed7NbViaGZa1DQey4Ffz6F2VAqFkWbhBzKUXrSNlx6iaAccjw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/xicEJhWlK06BmBu5jcGtXXESwqDCMf1S2MB9fdkLlITn0Dm1hLfLDD7Snb0KDQLDs0kzzf8AVw4Dp606JFnSPLQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击"Next"->"Finish"完成springboot项目的创建，如下图：

![图片](https://mmbiz.qpic.cn/mmbiz_png/xicEJhWlK06BmBu5jcGtXXESwqDCMf1S2T6TgXpbSckOCXAjBPgShBYVRicWmsEvSDUJSiavUSeSBVmwEsTx4yGtg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

删掉下图中无关的文件

> 按住Shift健，多选，然后点击Del健删除。

![图片](https://mmbiz.qpic.cn/mmbiz_png/xicEJhWlK06BmBu5jcGtXXESwqDCMf1S27ZwvyXOhM1ic0IbTbmQlj0drAPb5L2t8h74EGnnoluGBvQdLAUnYkZQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/xicEJhWlK06BmBu5jcGtXXESwqDCMf1S2KoXBFOa5ibh1CbOcV2oFVaIyLa1q69J1FMcgGicc9ic0rjo34wCGlNnGA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

新建一个Controller类：

```
package com.javacode2018.controller;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class IndexController {
    @RequestMapping("")
    public String index() {
        return "你好，欢迎你和【路人甲Java】一起学些Maven相关技术!";
    }
}
```

springboot-chat01目录中打开cmd窗口，执行下面命令

```
mvn spring-boot:run
```

cmd中输出如下：

```
D:\code\IdeaProjects\springboot-chat01>mvn spring-boot:run
[INFO] Scanning for projects...
[INFO]
[INFO] -----------------< com.javacode2018:springboot-chat01 >-----------------
[INFO] Building springboot-chat01 0.0.1-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] >>> spring-boot-maven-plugin:2.2.1.RELEASE:run (default-cli) > test-compile @ springboot-chat01 >>>
[INFO]
[INFO] --- maven-resources-plugin:3.1.0:resources (default-resources) @ springboot-chat01 ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 1 resource
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.8.1:compile (default-compile) @ springboot-chat01 ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 2 source files to D:\code\IdeaProjects\springboot-chat01\target\classes
[INFO]
[INFO] --- maven-resources-plugin:3.1.0:testResources (default-testResources) @ springboot-chat01 ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory D:\code\IdeaProjects\springboot-chat01\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.8.1:testCompile (default-testCompile) @ springboot-chat01 ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 1 source file to D:\code\IdeaProjects\springboot-chat01\target\test-classes
[INFO]
[INFO] <<< spring-boot-maven-plugin:2.2.1.RELEASE:run (default-cli) < test-compile @ springboot-chat01 <<<
[INFO]
[INFO]
[INFO] --- spring-boot-maven-plugin:2.2.1.RELEASE:run (default-cli) @ springboot-chat01 ---
[INFO] Attaching agents: []

  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v2.2.1.RELEASE)

2019-11-11 14:43:58.758  INFO 19004 --- [           main] c.j.SpringbootChat01Application          : Starting SpringbootChat01Application on DESKTOP-3OB6NA3 with PID 19004 (D:\code\IdeaProjects\springboot-chat01\target\classes started by Think in D:\code\IdeaProjects\springboot-chat01)
2019-11-11 14:43:58.761  INFO 19004 --- [           main] c.j.SpringbootChat01Application          : No active profile set, falling back to default profiles: default
2019-11-11 14:44:01.116  INFO 19004 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port(s): 8080 (http)
2019-11-11 14:44:01.125  INFO 19004 --- [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2019-11-11 14:44:01.125  INFO 19004 --- [           main] org.apache.catalina.core.StandardEngine  : Starting Servlet engine: [Apache Tomcat/9.0.27]
2019-11-11 14:44:01.213  INFO 19004 --- [           main] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2019-11-11 14:44:01.213  INFO 19004 --- [           main] o.s.web.context.ContextLoader            : Root WebApplicationContext: initialization completed in 2417 ms
2019-11-11 14:44:01.339  INFO 19004 --- [           main] o.s.s.concurrent.ThreadPoolTaskExecutor  : Initializing ExecutorService 'applicationTaskExecutor'
2019-11-11 14:44:01.457  INFO 19004 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8080 (http) with context path ''
2019-11-11 14:44:01.460  INFO 19004 --- [           main] c.j.SpringbootChat01Application          : Started SpringbootChat01Application in 2.972 seconds (JVM running for 3.348)
2019-11-11 14:44:25.892  INFO 19004 --- [nio-8080-exec-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring DispatcherServlet 'dispatcherServlet'
2019-11-11 14:44:25.892  INFO 19004 --- [nio-8080-exec-1] o.s.web.servlet.DispatcherServlet        : Initializing Servlet 'dispatcherServlet'
2019-11-11 14:44:25.896  INFO 19004 --- [nio-8080-exec-1] o.s.web.servlet.DispatcherServlet        : Completed initialization in 3 ms
```

浏览器中访问

```
http://localhost:8080/
```

效果如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/xicEJhWlK06BmBu5jcGtXXESwqDCMf1S2xia1FpjTBGtd3YdIvcbhIvb5scMklONfPs9d6PkrztyygfFdoC8j7oA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

上面整个过程中，我们基本上没写什么代码，没有去添加springmvc相关的jar，然后很快就写了一个springmvc的例子来。

以往我们做一个springmvc项目，我们需要添加很多springmvc相关的jar，然后将其复制到项目的lib下面，然后添加到classpath中，期间还有可能出现jar版本冲突、jar包不全的问题，导致我们花费很多时间在解决jar包的问题上。

而上面我们使用了maven，通过maven这些问题都解决了，上面我们创建项目之后，有一个非常重要的文件pom.xml，大家可以打开看一下，如下：

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.1.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.javacode2018</groupId>
    <artifactId>springboot-chat01</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>springboot-chat01</name>
    <description>Demo project for Spring Boot</description>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
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

上面的pom.xml是自动生成的，作用是将springmvc所有需要的包都自动导入了，并且还为我们提供了支持`mvn spring-boot:run`启动的功能，是不是很神奇，这些功能都是通过maven来实现的，通过pom来进行配置，maven会读取pom中的配置信息，来加载我们项目中需要依赖的jar包，还有需要如何构件项目等等信息，都可以在pom文件中进行配置。

关于`mvn spring-boot:run`是如何启动项目的，这个后面学到maven声明周期和插件的时候，一切您都会明白的，敬请期待。

用过ant的都知道，ant中有个build.xml文件需要配置，而pom.xml文件类似于build.xml的功能，不过不是给ant执行的，而是给maven去执行的，maven说你们如果需要用我来帮你们解决版本依赖问题、帮你们解决jar冲突的问题、帮你们打包、部署，那你们都必须要给我提供一个pom.xml配置文件，并且项目结构也必须按照我指定的结构来，我只认pom.xml文件。而之前我们使用ant的时候，项目源码、资源文件、输出目录、打包目录所在的位置都是自己随意指定的，所以需要我们自己去写很多配置，相当麻烦。而maven中这些位置都是约定好的，这就是约定配置，如下。

### 约定配置

Maven 提倡使用一个共同的标准目录结构，Maven 使用约定优于配置的原则，大家尽可能的遵守这样的目录结构，如下所示：

![图片](https://mmbiz.qpic.cn/mmbiz_png/xicEJhWlK06BmBu5jcGtXXESwqDCMf1S2J6ibRiaiaia9HPQicIV4cbJf57Nns7lrdDkhsDkoNIHbqxOFfeL5qgcTxuw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

大家结合上面表格中的信息，再去看看springboot-chat01项目的结构，这是maven项目标准的结构，大家都按照这个约定来，然后maven中打包、运行、部署时候就非常方便了，maven他自己就知道你项目的源码、资源、测试代码、打包输出的位置，这些都是maven规定好的，就不是你随意搞的一个结构，所以不需要我们再去配置了，所以使用maven去打包、部署、运行都是非常方便的。

这块现实中也有很多案例，比如USB接口，电压，这些都是规定好的，如果USB接口所有厂商制造的大小都不一致，那我们使用电子设备的时候是相当难受的。

### pom文件

当我们在项目中需要用到maven帮我们解决jar包依赖问题，帮我们解决项目中的编译、测试、打包、部署时，项目中必须要有pom.xml文件，这些都是依靠pom的配置来完成的。

POM( Project Object Model，项目对象模型 ) 是 Maven 工程的基本工作单元，是一个XML文件，包含了项目的基本信息，用于描述项目如何构件，声明项目依赖，等等。

执行任务或目标时，Maven 会在当前目录中查找 POM。它读取 POM，获取所需的配置信息，然后执行目标。

POM 中可以指定以下配置：

- 项目依赖
- 插件
- 执行目标
- 项目构件 profile
- 项目版本
- 项目开发者列表
- 相关邮件列表信息

在创建 POM 之前，我们首先需要描述项目组 (groupId)，项目的唯一ID。

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <!-- 模型版本 -->
    <modelVersion>4.0.0</modelVersion>

    <!-- 定义当前构件所属的组，通常与域名反向一一对应 -->
    <groupId>com.javacode2018</groupId>
    <!--项目的唯一ID，一个groupId下面可能多个项目，就是靠artifactId来区分的-->
    <artifactId>maven-chat02</artifactId>

    <!-- 版本号 -->
    <version>1.0-SNAPSHOT</version>

</project>
```

pom的内容比较多，我们慢慢讲，大家慢慢吸收，利于消化。

### maven坐标

maven中引入了坐标的概念，每个构件都有唯一的坐标，我们使用maven创建一个项目需要标注其坐标信息，而项目中用到其他的一些构件，也需要知道这些构件的坐标信息。

maven中构件坐标是通过一些元素定义的，他们是groupId、artifactId、version、packaging、classifier，如我们刚刚上面创建的springboot项目，它的pom中坐标信息如下：

```
<groupId>com.javacode2018</groupId>
<artifactId>springboot-chat01</artifactId>
<version>0.0.1-SNAPSHOT</version>
<packaging>jar</packaging>
```

> goupId：定义当前构件所属的组，通常与域名反向一一对应。
>
> artifactId：项目组中构件的编号。
>
> version：当前构件的版本号，每个构件可能会发布多个版本，通过版本号来区分不同版本的构件。
>
> package：定义该构件的打包方式，比如我们需要把项目打成jar包，采用`java -jar`去运行这个jar包，那这个值为jar；若当前是一个web项目，需要打成war包部署到tomcat中，那这个值就是war，可选（jar、war、ear、pom、maven-plugin），比较常用的是jar、war、pom，这些后面会详解。

上面接元素中，groupId、artifactId、version是必须要定义的，packeage可以省略，默认为jar。

我们可以将上面创建的springboot项目发布出去，然后只需要告诉别人springboot-chat01这个项目的坐标信息，其他人就可以直接使用了，而且其他人用的时候，不用关心springboot-chat01运行需要依赖什么，springboot-chat01依赖的这些都会自动导入，非常方便。

### maven导入依赖的构件

maven可以帮我们引入需要依赖的构件(jar等)，而maven是如何定位到某个构件的呢？

项目中如果需要使用第三方的jar，我们需要知道其坐标信息，然后将这些信息放入pom.xml文件中的`dependencies`元素中：

```
<project>
    <dependencies>
        <!-- 在这里添加你的依赖 -->
        <dependency>
            <groupId></groupId>
            <artifactId></artifactId>
            <version></version>
            <type></type>
            <scope></scope>
            <optional></optional>
            <exclusions>
                <exclusion></exclusion>
                <exclusion></exclusion>
            </exclusions>
        </dependency>
    </dependencies>
</project>
```

- dependencies元素中可以包含多个`dependency`，每个`dependency`就表示当前项目需要依赖的一个构件的信息
- dependency中groupId、artifactId、version是定位一个构件必须要提供的信息，所以这几个是必须的
- type：依赖的类型，表示所要依赖的构件的类型，对应于被依赖的构件的packaging。大部分情况下，该元素不被声明，默认值为jar，表示被依赖的构件是一个jar包。
- scope：依赖的范围，后面详解
- option：标记依赖是否可选，后面详解
- exclusions：用来排除传递性的依赖

通常情况下我们依赖的都是一些jar包，所以大多数情况下，只需要提供`groupId、artifactId、version`信息就可以了。

### 创建一个maven项目（maven-chat02）

打开idea，点击`File->New->Project`，选择`Maven`，如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/xicEJhWlK06BmBu5jcGtXXESwqDCMf1S2C3GTGBGgralvR5icuSVbSG8wkw0Rqq6nZG0aFU4nUib2Y63eTfMPMF2g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击`Next`，输入项目的maven坐标信息，如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/xicEJhWlK06BmBu5jcGtXXESwqDCMf1S23pewwwtZaGIRA5LfulzXJ8yYFkanwpO9hXAZ6pq4P6v598WnMnlhIg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击`Next`->`Finish`，完成项目的创建，项目结构如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/xicEJhWlK06BmBu5jcGtXXESwqDCMf1S2QlrNwKazuqGTXIGh71oXpX5gsEwltQT3BvmFRz2QPOn3icvChkwuYvA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

打开pom.xml，引入springmvc依赖，如下：

```
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-web</artifactId>
    <version>5.2.1.RELEASE</version>
</dependency>
```

pom.xml文件最终如下：

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.javacode2018</groupId>
    <artifactId>maven-chat02</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
            <version>5.2.1.RELEASE</version>
        </dependency>
    </dependencies>

</project>
```

> 可以看到我们只引入了一个spring-web的jar包，springmvc还需要spring-core等jar包的支持，我们来看一下，maven是否帮我们也导入了。

pom.xml文件中，右键选中`Show Dependencies`，可以显示当前项目依赖的jar包，如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/xicEJhWlK06BmBu5jcGtXXESwqDCMf1S20CoaPfbicka4rtH3NkvxiaChjibCT3Q7ZlzfK5ibjoKOTN8VMMaGFicHesw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/xicEJhWlK06BmBu5jcGtXXESwqDCMf1S2De4f7DfMbA15QOoYQWh1Tgficj3r5Wbe76kJUBl3PgeZciatGwBEuNNg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

上图中显示了项目中的依赖信息，箭头有多级，从左向右，第一个箭头表示第一级，是直接依赖，后面的都是间接依赖，属于传递性依赖。

我们在`maven-chat02`中添加了`spring-web`的依赖，并没有引入`spring-beans、spring-core、spring-jcl`的依赖，但是maven都自动帮我们导入了，这是因为`spring-web`的pom.xml中定义了它自己的依赖，当我们使用`spring-web`的时候，spring-web需要依赖的jar也会自动被依赖进来，maven是不是很强大。

如果没有maven，我们找jar是相当痛苦的，经常会出现少添加了一些jar，或者依赖的jar版本对不上等问题，而maven直接帮我们解决了。

### maven依赖范围（scope）

我们的需求：

1. 我们在开发项目的过程中，可能需要用junit来写一些测试用例，此时需要引入junit的jar包，但是当我们项目部署在线上运行了，测试代码不会再执行了，此时junit.jar是不需要了，所以junit.jar只是在编译测试代码，运行测试用例的时候用到，而上线之后用不到了，所以部署环境中是不需要的
2. 我们开发了一个web项目，在项目中用到了servlet相关的jar包，但是部署的时候，我们将其部署在tomcat中，而tomcat中自带了servlet的jar包，那么我们的需求是开发、编译、单元测试的过程中需要这些jar，上线之后，servlet相关的jar由web容器提供，也就是说打包的时候，不需要将servlet相关的jar打入war包了
3. 像jdbc的驱动，只有在运行的时候才需要，编译的时候是不需要的

这些需求怎么实现呢？

**我们都知道，java中编译代码、运行代码都需要用到classpath变量，classpath用来列出当前项目需要依赖的jar包，这块不清楚的可以去看一下classpath和jar。**

```
classpath和jar关系详解：http://www.itsoku.com/article/234
```

**maven用到classpath的地方有：编译源码、编译测试代码、运行测试代码、运行项目，这几个步骤都需要用到classpath。**

如上面的需求，编译、测试、运行需要的classpath对应的值可能是不一样的，这个maven中的scope为我们提供了支持，可以帮我们解决这方面的问题，scope是用来控制被依赖的构件与classpath的关系（编译、打包、运行所用到的classpath），scope有以下几种值：

#### compile

编译依赖范围，如果没有指定，默认使用该依赖范围，**对于编译源码、编译测试代码、测试、运行4种classpath都有效**，比如上面的spring-web。

#### test

测试依赖范围，使用此依赖范围的maven依赖，**只对编译测试、运行测试的classpath有效，在编译主代码、运行项目时无法使用此类依赖**。比如junit，它只有在编译测试代码及运行测试的时候才需要。

#### provide

已提供依赖范围。表示项目的运行环境中已经提供了所需要的构件，对于此依赖范围的maven依赖，**对于编译源码、编译测试、运行测试中classpath有效，但在运行时无效**。比如上面说到的servlet-api，这个在编译和测试的时候需要用到，但是在运行的时候，web容器已经提供了，就不需要maven帮忙引入了。

#### runtime

运行时依赖范围，使用此依赖范围的maven依赖，**对于编译测试、运行测试和运行项目的classpath有效，但在编译主代码时无效**，比如jdbc驱动实现，运行的时候才需要具体的jdbc驱动实现。

#### system

系统依赖范围，该依赖与3中classpath的关系，和provided依赖范围完全一致。但是，使用system范围的依赖时必须通过systemPath元素显示第指定依赖文件的路径。这种依赖直接依赖于本地路径中的构件，可能每个开发者机器中构件的路径不一致，所以如果使用这种写法，你的机器中可能没有问题，别人的机器中就会有问题，所以建议谨慎使用。

如下：

```
<dependency>
    <groupId>com.javacode2018</groupId>
    <artifactId>rt</artifactId>
    <version>1.8</version>
    <scope>system</scope>
    <systemPath>${java.home}/lib/rt.jar</systemPath>
</dependency>
```

#### import

这个比较特殊，后面的文章中单独讲，springboot和springcloud中用到的比较多。

**依赖范围与classpath的关系如下：**

![图片](https://mmbiz.qpic.cn/mmbiz_png/xicEJhWlK06BmBu5jcGtXXESwqDCMf1S2UL9JT09L2kC2CjvVtlFLjLpofGqYxefhutCALMBhGP6cjlo1hcTic6g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

> scope如果对于运行范围有效，意思是指依赖的jar包会被打包到项目的运行包中，最后运行的时候会被添加到classpath中运行。如果scope对于运行项目无效，那么项目打包的时候，这些依赖不会被打包到运行包中。

### 依赖的传递

上面我们创建的`maven-chat02`中依赖了spring-web，而我们只引入了`spring-web`依赖，而spring-web又依赖了`spring-beans、spring-core、spring-jcl`，这3个依赖也被自动加进来了，这种叫做依赖的传递。

不过上面我们说的scope元素的值会对这种传递依赖会有影响。

假设A依赖于B，B依赖于C，我们说A对于B是第一直接依赖，B对于C是第二直接依赖，而A对于C是传递性依赖，而第一直接依赖的scope和第二直接依赖的scope决定了传递依赖的范围，即决定了A对于C的scope的值。

下面我们用表格来列一下这种依赖的效果，表格最左边一列表示第一直接依赖（即A->B的scope的值）,而表格中的第一行表示第二直接依赖（即B->C的scope的值），行列交叉的值显示的是A对于C最后产生的依赖效果。

![图片](https://mmbiz.qpic.cn/mmbiz_png/xicEJhWlK06BmBu5jcGtXXESwqDCMf1S2sfFZnIOz520Zf0xZF1y3L0m2tZuLwzJCQuM1EgIKic6icssicbbq05aaQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

> 解释一下：
>
> 1. 比如A->B的scope是`compile`，而B->C的scope是`test`，那么按照上面表格中，对应第2行第3列的值`-`，那么A对于C是没有依赖的，A对C的依赖没有从B->C传递过来，所以A中是无法使用C的
> 2. 比如A->B的scope是`compile`，而B->C的scope是`runtime`，那么按照上面表格中，对应第2行第5列的值为`runtime`，那么A对于C是的依赖范围是`runtime`，表示A只有在运行的时候C才会被添加到A的classpath中，即对A进行运行打包的时候，C会被打包到A的包中
> 3. 大家仔细看一下，上面的表格是有规律的，当B->C依赖是compile的时候（表中第2列），那么A->C的依赖范围和A->B的sope是一样的；当B->C的依赖是test时（表中第3列），那么B->C的依赖无法传递给A；当B->C的依赖是provided（表第4列），只传递A->C的scope为provided的情况，其他情况B->C的依赖无法传递给A；当B->C的依赖是runtime（表第5列），那么C按照B->C的scope传递给A
> 4. 上面表格大家多看几遍，理解理解

### maven依赖调解功能

现实中可能存在这样的情况，A->B->C->Y(1.0)，A->D->Y(2.0)，此时Y出现了2个版本，1.0和2.0，此时maven会选择Y的哪个版本？

解决这种问题，maven有2个原则：

#### 路径最近原则

上面`A->B->C->Y(1.0)，A->D->Y(2.0)`，Y的2.0版本距离A更近一些，所以maven会选择2.0。

但是如果出现了路径是一样的，如：`A->B->Y(1.0)，A->D->Y(2.0)`，此时maven又如何选择呢？

#### 最先声明原则

如果出现了路径一样的，此时会看A的pom.xml中所依赖的B、D在`dependencies`中的位置，谁的声明在最前面，就以谁的为主，比如`A->B`在前面，那么最后Y会选择1.0版本。

**这两个原则希望大家记住：路径最近原则、最先声明原则。**

### 可选依赖（optional元素）

有这么一种情况：

```
A->B中scope:compile
B->C中scope:compile
```

按照上面介绍的依赖传递性，C会传递给A，被A依赖。

假如B不想让C被A自动依赖，可以怎么做呢？

`dependency元素下面有个optional，是一个boolean值，表示是一个可选依赖`，B->C时将这个值置为true，那么C不会被A自动引入。

### 排除依赖

A项目的pom.xml中

```
<dependency>
    <groupId>com.javacode2018</groupId>
    <artifactId>B</artifactId>
    <version>1.0</version>
</dependency>
```

B项目1.0版本的pom.xml中

```
<dependency>
    <groupId>com.javacode2018</groupId>
    <artifactId>C</artifactId>
    <version>1.0</version>
</dependency>
```

上面A->B的1.0版本，B->C的1.0版本，而scope都是默认的compile，根据前面讲的依赖传递性，C会传递给A，会被A自动依赖，但是C此时有个更新的版本2.0，A想使用2.0的版本，此时A的pom.xml中可以这么写：

```
<dependency>
    <groupId>com.javacode2018</groupId>
    <artifactId>B</artifactId>
    <version>1.0</version>
    <exclusions>
        <exclusion>
            <groupId>com.javacode2018</groupId>
            <artifactId>C</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

上面使用使用exclusions元素排除了B->C依赖的传递，也就是B->C不会被传递到A中。

exclusions中可以有多个`exclusion`元素，可以排除一个或者多个依赖的传递，声明exclusion时只需要写上groupId、artifactId就可以了，version可以省略。

### 总结

本章节内容还是比较多的，也是相当重要的，大家多看几遍，加深理解，欢迎大家加我微信（**itsoku**）或者留言一起交流！