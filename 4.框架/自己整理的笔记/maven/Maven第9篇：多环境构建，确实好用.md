# Maven第9篇：多环境构建，确实好用

路人甲Java [程序员路人](javascript:void(0);) *2022-03-21 20:20*

**大家好，我是路人~~~**

**点击下方卡片，关注我，java干货及时送达**

![img](http://mmbiz.qpic.cn/mmbiz_png/kvBL17F8Dr1MFuX9tnHDHzic6yBpLSiaTibZLKcia9tHwm7aFueNjgoAIibdqXibicAUEia87xvDN77VhfDKbo59BJCwlw/0?wx_fmt=png)

**程序员路人**

专注Java相关技术：SSM、Spring全家桶、微服务、MySQL、MyCat、集群、分布式、中间件、Linux、网络、多线程，偶尔讲点运维Jenkins、Nexus、Docker、ELK，偶尔分享些技术干货，致力于Java全栈开发！

1篇原创内容



公众号

***\*这是 Maven 第 9 篇，点击上方卡片，即可翻阅前面章节。\****

**整个maven系列的内容前后是有依赖的，如果之前没有接触过maven，建议从第1篇看起。**

如果你作为公司核心开发，打算使用maven来搭建项目骨架，这篇文章的内容是你必须要掌握的。

平时我们在开发系统的时候，会有开发环境、测试环境、线上环境，每个环境中配置文件可能都是不一样的，比如：数据库的配置，静态资源的配置等等，所以我们希望构建工具能够适应不同环境的构建工作，能够灵活处理，并且操作足够简单。Maven作为一款优秀的构建工具，这方面做的足够好了，能够很好的适应不同环境的构建工作，本文主要讲解maven如何灵活的处理各种不同环境的构建工作，废话不多说，上干货。

### 重点提示

**本文中的所有案例均在上一篇的`b2b`项目上进行操作，上一篇还没有看的可以点击开头的卡片，看一下上篇文章。**

**所有mvn命令均在`b2b/pom.xml`所在目录执行。**

### Maven属性

#### 自定义属性

maven属性前面我们有用到过，可以自定义一些属性进行重用，如下：

```
<properties>
    <spring.verion>5.2.1.RELEASE</spring.verion>
</properties>

<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
        <version>${spring.verion}</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-beans</artifactId>
        <version>${spring.verion}</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-aop</artifactId>
        <version>${spring.verion}</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-web</artifactId>
        <version>${spring.verion}</version>
    </dependency>
</dependencies>
```

可以看到上面依赖了4个spring相关的构建，他们的版本都是一样的，在`properties`元素中自定义了一个`spring.version`属性，值为spring的版本号，其他几个地方使用`${}`直接进行引用，这种方式好处还是比较明显的，升级spring版本的时候非常方便，只需要修改一个地方，方便维护。

上面这个是maven自定义属性，需要先在`properties`中定义，然后才可以在其他地方使用`${属性元素名称}`进行引用。

maven的属性主要分为2大类，第一类就是上面说的自定义属性，另外一类是不需要自定义的，可以直接拿来使用的。

2类属性在pom.xml中都是采用`${属性名称}`进行引用，maven运行的时候会将`${}`替换为属性实际的值。

下面我们来看一下maven中不需要自定义的5类属性。

#### 内置属性

```
${basedir}：表示项目根目录，即包含pom.xml文件的目录
${version}：表示项目的版本号
```

#### POM属性

用户可以使用该属性引用pom.xml文件中对应元素的值，例如${project.artifactId}就可以取到`project->artifactId`元素的值，常用的有：

```
${pom.build.sourceDirectory}：项目的主源码目录，默认为src/main/java/
${project.build.testSourceDirectory}：项目的测试源码目录，默认为src/test/java/
${project.build.directory}：项目构建输出目录，默认为target/
${project.build.outputDirectory}：项目主代码编译输出目录，默认为target/classes
${project.build.testOutputDirectory}：项目测试代码编译输出目录，默认为target/test-classes
${project.groupId}：项目的groupId
${project.artifactId}：项目的artifactId
${project.version}：项目的version，与${version}等价
${project.build.finalName}：项目打包输出文件的名称，默认为${project.artifactId}-${project.version}
```

#### Settings属性

这种属性以settings.开头来引用`~/.m2/settings.xml`中的内容，如:

```
${settings.localRepository}
```

指向用户本地仓库的地址。

#### java系统属性

所有java系统属性都可以使用maven属性来进行引用，例如`${user.home}`指向了当前用户目录。

java系统属性可以通过`mvn help:system`命令看到。

#### 环境变量属性

所有的环境变量都可以使用env.开头的方式来进行引用，如：

```
${env.JAVA_HOME}
```

可以获取环境变量`JAVA_HOME`的值。

用户可以使用`mvn help:system`命令查看所有环境变量的值。

上面的maven属性，我们在`pom.xml`中通过`${属性名称}`可以灵活的引用，对我们写pom.xml文件帮助还是比较大的。

#### 实操案例

将下面配置放在`b2b-account-service/pom.xml`中：

```
<properties>
    <!-- 项目的主源码目录，默认为src/main/java/ -->
    <a>${pom.build.sourceDirectory}</a>
    <!-- 项目的测试源码目录，默认为src/test/java/ -->
    <b>${project.build.testSourceDirectory}</b>
    <!-- 项目构建输出目录，默认为target/ -->
    <c>${project.build.directory}</c>
    <!-- 项目主代码编译输出目录，默认为target/classes -->
    <d>${project.build.outputDirectory}</d>
    <!-- 项目测试代码编译输出目录，默认为target/test-classes -->
    <e>${project.build.testOutputDirectory}</e>
    <!-- 项目的groupId -->
    <f>${project.groupId}</f>
    <!-- 项目的artifactId -->
    <g>${project.artifactId}</g>
    <!-- 项目的version，与${version}等价-->
    <h>${project.version}</h>
    <!-- 项目打包输出文件的名称，默认为${project.artifactId}-${project.version} -->
    <i>${project.build.finalName}</i>

    <!-- setting属性 -->
    <!-- 获取本地仓库地址-->
    <a1>${settings.localRepository}</a1>

    <!-- 系统属性 -->
    <!-- 用户目录 -->
    <a2>${user.home}</a2>

    <!-- 环境变量属性，获取环境变量JAVA_HOME的值 -->
    <a3>${env.JAVA_HOME}</a3>
</properties>
```

然后在`b2b/pom.xml`所在目录执行下面命令：

```
D:\code\IdeaProjects\b2b>mvn help:effective-pom -pl :b2b-account-service > 1.xml
```

上面这个命令会将`mvn ...`执行的结果输出到`b2b/1.xml`目录，`mvn`这段命令有看不懂的朋友，可以去看看前面的文章。

b2b目录中产生了一个1.xml，如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bb1IzXs5M1sFnGmhYXreCCTPNgUn4sCwAicJpIQxhE1oHTL7iaERCfzWarTtR9CE8zMtbQQ3jJrZ2g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

我们打开1.xml看一下，部分内容如下：

```
<properties>
  <a>D:\code\IdeaProjects\b2b\b2b-account\b2b-account-service\src\main\java</a>
  <a1>${settings.localRepository}</a1>
  <a2>C:\Users\Think</a2>
  <a3>D:\installsoft\Java\jdk1.8.0_121</a3>
  <b>D:\code\IdeaProjects\b2b\b2b-account\b2b-account-service\src\test\java</b>
  <b2>D:\code\IdeaProjects\b2b</b2>
  <c>D:\code\IdeaProjects\b2b\b2b-account\b2b-account-service\target</c>
  <d>D:\code\IdeaProjects\b2b\b2b-account\b2b-account-service\target\classes</d>
  <e>D:\code\IdeaProjects\b2b\b2b-account\b2b-account-service\target\test-classes</e>
  <f>com.javacode2018</f>
  <g>b2b-account-service</g>
  <h>1.0-SNAPSHOT</h>
  <i>b2b-account-service-1.0-SNAPSHOT</i>
</properties>
```

大家去和上面的pom.xml中的对比一下，感受一下！

### 多套环境构建问题

`b2b-account-service`会操作数据库，所以我们需要一个配置文件来放数据库的配置信息，配置文件一般都放在`src/main/resources`中，在这个目录中新建一个`jdbc.properties`文件，内容如下：

```
jdbc.url=jdbc:mysql://localhost:3306/javacode2018?characterEncoding=UTF-8
jdbc.username=root
jdbc.password=root
```

现在系统需要打包，我们运行下面命令

```
mvn clean package -pl :b2b-account-service
```

问题来了：

上面jdbc的配置的是开发库的db信息，打包之后生成的jar中也是上面开发环境的配置，那么上测试环境是不是我们需要修改上面的配置，最终上线的时候，上面的配置是不是又得重新修改一次，相当痛苦的。

我们能不能写3套环境的jdbc配置，打包的时候去指定具体使用那套配置？

还是你们聪明，maven支持这么做，pom.xml的`project`元素下面提供了一个`profiles`元素可以用来对多套环境进行配置。

在讲profiles的使用之前，需要先了解资源文件打包的过程。

### 理解资源文件打包过程

我们将`src/main/resouces/jdbc.properties`复制一份到`src/test/resources`目录，2个文件如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bb1IzXs5M1sFnGmhYXreCC2LwtYvekkMvMfc6aldLcaZjJfqx0h7DicPicnBQ8hicWX3iaNYjedq186A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

我们先运行一下下面命令：

```
mvn clean package -pl :b2b-account-service
```

输出如下：

```
D:\code\IdeaProjects\b2b>mvn clean package -pl :b2b-account-service
[INFO] Scanning for projects...
[INFO]
[INFO] ----------------< com.javacode2018:b2b-account-service >----------------
[INFO] Building b2b-account-service 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ b2b-account-service ---
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ b2b-account-service ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 1 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ b2b-account-service ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ b2b-account-service ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 1 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ b2b-account-service ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ b2b-account-service ---
[INFO]
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ b2b-account-service ---
[INFO] Building jar: D:\code\IdeaProjects\b2b\b2b-account\b2b-account-service\target\b2b-account-service-1.0-SNAPSHOT.jar
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  1.572 s
[INFO] Finished at: 2019-11-21T17:13:20+08:00
[INFO] ------------------------------------------------------------------------
```

再去看一下项目的target目录，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bb1IzXs5M1sFnGmhYXreCCIJ90tQj2wFILFibibh3aXKHpM7qmY7guttQibcXs6Gg6dBZp3HRQEHe1Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

下面我们来对这个过程做详细分析：

从输出中可以看到有下面几行：

```
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ b2b-account-service ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 1 resource
```

从上面输出中可以看出，使用了`插件maven-resources-plugin的resources目标`，将`src/main/resouces`目录中的资源文件复制到了`target/classess`目录，复制了一个文件，复制过程中使用是`GBK`编码。

还有几行输出如下：

```
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ b2b-account-service ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 1 resource
```

从上面输出中可以看出，使用了`插件maven-resources-plugin的testResources目标`，将`src/main/resouces`目录中的资源文件复制到了`target/classess`目录，复制了一个文件，复制过程中使用是`GBK`编码。

resources目录中的文件一般放的都是配置文件，配置文件一般最好我们都不会写死，所以此处有几个问题：

1. 这个插件复制资源文件如何设置编码？
2. 复制的过程中是否能够对资源文件进行替换，比如在资源文件中使用一些占位符，然后复制过程中对这些占位符进行替换。

`maven-resources-plugin`这个插件还真好，他也想到了这个功能，帮我们提供了这样的功能，下面我们来看看。

#### 设置资源文件复制过程采用的编码

这个之前有提到过，有好几种方式，具体可以去前面的文章看一下。这里只说一种：

```
<properties>
    <encoding>UTF-8</encoding>
</properties>
```

#### 设置资源文件内容动态替换

资源文件中可以通过`${maven属性}`来引用maven属性中的值，打包的过程中这些会被替换掉，替换的过程默认是不开启的，需要手动开启配置。

修改`src/main/resource/jdbc.properties`内容如下：

```
jdbc.url=${jdbc.url}
jdbc.username=${jdbc.username}
jdbc.password=${jdbc.password}
```

修改`src/test/resource/jdbc.properties`内容如下：

```
jdbc.url=${jdbc.url}
jdbc.username=${jdbc.username}
jdbc.password=${jdbc.password}
```

`b2b-account-service/pom.xml`中加入下面内容：

```
<properties>
    <!-- 指定资源文件复制过程中采用的编码方式 -->
    <encoding>UTF-8</encoding>
    <jdbc.url>jdbc:mysql://localhost:3306/javacode2018?characterEncoding=UTF-8</jdbc.url>
    <jdbc.username>root</jdbc.username>
    <jdbc.password>root</jdbc.password>
</properties>
```

开启动态替换配置，需要在pom.xml中加入下面配置：

```
<build>
    <resources>
        <resource>
            <!-- 指定资源文件的目录 -->
            <directory>${project.basedir}/src/main/resources</directory>
            <!-- 是否开启过滤替换配置，默认是不开启的 -->
            <filtering>true</filtering>
        </resource>
    </resources>
    <testResources>
        <testResource>
            <!-- 指定资源文件的目录 -->
            <directory>${project.basedir}/src/test/resources</directory>
            <!-- 是否开启过滤替换配置，默认是不开启的 -->
            <filtering>true</filtering>
        </testResource>
    </testResources>
</build>
```

> 注意上面开启动态替换的元素是`filtering`。
>
> 上面build元素中的`resources`和`testResources`是用来控制构建过程中资源文件配置信息的，比资源文件位于哪个目录，需要复制到那个目录，是否开启动态过滤等信息。
>
> `resources`元素中可以包含多个`resource`，每个`resource`表示一个资源的配置信息，一般使用来控制主资源的复制的。
>
> `testResources`元素和`testResources`类似，是用来控制测试资源复制的。

最终pom.xml内容如下：

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.javacode2018</groupId>
    <artifactId>b2b-account-service</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <!-- 指定资源文件复制过程中采用的编码方式 -->
        <encoding>UTF-8</encoding>
        <jdbc.url>jdbc:mysql://localhost:3306/javacode2018?characterEncoding=UTF-8</jdbc.url>
        <jdbc.username>root</jdbc.username>
        <jdbc.password>root</jdbc.password>
    </properties>

    <dependencies>
        <dependency>
            <groupId>com.javacode2018</groupId>
            <artifactId>b2b-account-api</artifactId>
            <version>${project.version}</version>
        </dependency>
    </dependencies>

    <build>
        <resources>
            <resource>
                <!-- 指定资源文件的目录 -->
                <directory>${project.basedir}/src/main/resources</directory>
                <!-- 是否开启过滤替换配置，默认是不开启的 -->
                <filtering>true</filtering>
            </resource>
        </resources>
        <testResources>
            <testResource>
                <!-- 指定资源文件的目录 -->
                <directory>${project.basedir}/src/test/resources</directory>
                <!-- 是否开启过滤替换配置，默认是不开启的 -->
                <filtering>true</filtering>
            </testResource>
        </testResources>
    </build>

</project>
```

运行下面命令看效果：

```
D:\code\IdeaProjects\b2b>mvn clean package -pl :b2b-account-service
[INFO] Scanning for projects...
[INFO]
[INFO] ----------------< com.javacode2018:b2b-account-service >----------------
[INFO] Building b2b-account-service 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ b2b-account-service ---
[INFO] Deleting D:\code\IdeaProjects\b2b\b2b-account\b2b-account-service\target
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ b2b-account-service ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 1 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ b2b-account-service ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ b2b-account-service ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 1 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ b2b-account-service ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ b2b-account-service ---
[INFO]
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ b2b-account-service ---
[INFO] Building jar: D:\code\IdeaProjects\b2b\b2b-account\b2b-account-service\target\b2b-account-service-1.0-SNAPSHOT.jar
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  1.798 s
[INFO] Finished at: 2019-11-21T17:53:48+08:00
[INFO] ------------------------------------------------------------------------
```

4个文件截图看一下效果，如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bb1IzXs5M1sFnGmhYXreCC54mbuMibXkV1SDgpGqMbicoicH8jvEaicYBnZriay4w4gUTViclf1F4bRD8A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

target中的2个资源文件内容被动态替换掉了。

上面会将资源文件中`${}`的内容使用maven属性中的值进行替换，`${}`中包含的内容默认会被替换，那么我们是否可以自定义`${}`这个格式，比如我希望被`##`包含内容进行替换，这个就涉及到替换中分隔符的指定了，需要设置插件的一些参数。

#### 自定义替换的分隔符

自定义分隔符，需要我们配置`maven-resources-plugin`插件的参数，如下：

```
<plugins>
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-resources-plugin</artifactId>
        <version>2.6</version>
        <configuration>
            <!-- 是否使用默认的分隔符，默认分隔符是${*}和@ ,这个地方设置为false，表示不启用默认分隔符配置-->
            <useDefaultDelimiters>false</useDefaultDelimiters>
            <!-- 自定义分隔符 -->
            <delimiters>
                <delimiter>$*$</delimiter>
                <delimiter>##</delimiter>
            </delimiters>
        </configuration>
    </plugin>
</plugins>
```

`delimiters`中可以配置多个`delimiter`，可以配置`#*#`,其中的`*`表示属性名称，那么资源文件中的`#属性名#`在复制的过程中会被替换掉，`*`前后都是#，表示前后分隔符都一样，那么可以简写为`#`，所以`#*#`和`#`写法是一样的，我们去看一下源码，delimiters的默认值如下：

```
this.delimiters.add("${*}");
this.delimiters.add("@");
```

##### 案例

我们将pom.xml修改成下面这样：

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.javacode2018</groupId>
    <artifactId>b2b-account-service</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <!-- 指定资源文件复制过程中采用的编码方式 -->
        <encoding>UTF-8</encoding>
        <jdbc.url>jdbc:mysql://localhost:3306/javacode2018?characterEncoding=UTF-8</jdbc.url>
        <jdbc.username>root</jdbc.username>
        <jdbc.password>root</jdbc.password>
    </properties>

    <dependencies>
        <dependency>
            <groupId>com.javacode2018</groupId>
            <artifactId>b2b-account-api</artifactId>
            <version>${project.version}</version>
        </dependency>
    </dependencies>

    <build>
        <resources>
            <resource>
                <!-- 指定资源文件的目录 -->
                <directory>${project.basedir}/src/main/resources</directory>
                <!-- 是否开启过滤替换配置，默认是不开启的 -->
                <filtering>true</filtering>
            </resource>
        </resources>
        <testResources>
            <testResource>
                <!-- 指定资源文件的目录 -->
                <directory>${project.basedir}/src/test/resources</directory>
                <!-- 是否开启过滤替换配置，默认是不开启的 -->
                <filtering>true</filtering>
            </testResource>
        </testResources>

        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-resources-plugin</artifactId>
                <version>2.6</version>
                <configuration>
                    <!-- 是否使用默认的分隔符，默认分隔符是${*}和@ ,这个地方设置为false，表示不启用默认分隔符配置-->
                    <useDefaultDelimiters>false</useDefaultDelimiters>
                    <!-- 自定义分隔符 -->
                    <delimiters>
                        <delimiter>$*$</delimiter>
                        <delimiter>##</delimiter>
                    </delimiters>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>
```

将两个jdbc.properties修改成下面这样：

```
jdbc.url=${jdbc.url}
jdbc.username=$jdbc.username$
jdbc.password=##jdbc.password##
```

再运行一下下面的命令：

```
mvn clean package -pl :b2b-account-service
```

看一下`target/classes`中的`jdbc.properties`文件，变成了下面这样：

```
jdbc.url=${jdbc.url}
jdbc.username=root
jdbc.password=root
```

> 可以看到后面两符合分隔符的要求，被替换了，第一个没有被替换。

#### 指定需要替换的资源文件

在`src/main/resources`中新增一个`const.properties`文件，内容如下：

```
email=javacode2018@163.com
jdbc.url=${jdbc.url}
jdbc.username=$jdbc.username$
jdbc.password=##jdbc.password##
```

执行下面命令：

```
D:\code\IdeaProjects\b2b>mvn clean package -pl :b2b-account-service
[INFO] Scanning for projects...
[INFO]
[INFO] ----------------< com.javacode2018:b2b-account-service >----------------
[INFO] Building b2b-account-service 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ b2b-account-service ---
[INFO] Deleting D:\code\IdeaProjects\b2b\b2b-account\b2b-account-service\target
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ b2b-account-service ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 2 resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ b2b-account-service ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ b2b-account-service ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 1 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ b2b-account-service ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ b2b-account-service ---
[INFO]
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ b2b-account-service ---
[INFO] Building jar: D:\code\IdeaProjects\b2b\b2b-account\b2b-account-service\target\b2b-account-service-1.0-SNAPSHOT.jar
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  1.667 s
[INFO] Finished at: 2019-11-21T18:26:01+08:00
[INFO] ------------------------------------------------------------------------
```

看一下`target/classes`中，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bb1IzXs5M1sFnGmhYXreCCqIDia17UJOfibyCO6VnibUxFaibpU9wlVGJUAhv4zrQVchibX569ChwSibYg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

`target/classes/const.properties`内容变成下面这样了：

```
email=javacode2018@163.com
jdbc.url=${jdbc.url}
jdbc.username=root
jdbc.password=root
```

说明`const.properteis`也被替换了。2个资源文件都被复制到了target中，如果我们不想让`cont.properties`被复制到`target/classes`目录，我们怎么做？我们需要在资源构建的过程中排除他，可以使用`exclude`元素信息进行排除操作。

修改pom.xml中`resources`元素配置如下：

```
<resources>
    <resource>
        <!-- 指定资源文件的目录 -->
        <directory>${project.basedir}/src/main/resources</directory>
        <!-- 是否开启过滤替换配置，默认是不开启的 -->
        <filtering>true</filtering>
        <includes>
            <include>**/jdbc.properties</include>
        </includes>
        <excludes>
            <exclude>**/const.properties</exclude>
        </excludes>
    </resource>
</resources>
```

> 上面使用`includes`列出需要被处理的，使用`excludes`排除需要被处理的资源文件列表，采用通配符的写法，**匹配任意深度的文件路径，*匹配任意个字符。

再执行下面命令：

```
mvn clean package -pl :b2b-account-service
```

再看一下`target/classes`中，如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bb1IzXs5M1sFnGmhYXreCCJiaa0ltjibRBXr9htCcb4719rTTiacQbdIibxKDdmjvoz3gyf6Ds12k4jw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

`const.properties`被排除了，确实没有被复制过来。

如果此时我想让`const.propertis`只是被复制到target下面，但是不要去替换里面的内容，该怎么做呢？此时需要配置多个`resouce`元素了，如下案例。

#### 多个resource元素的使用案例

修改pom.xml中`resources`，如下：

```
<resources>
    <resource>
        <!-- 指定资源文件的目录 -->
        <directory>${project.basedir}/src/main/resources</directory>
        <!-- 是否开启过滤替换配置，默认是不开启的 -->
        <filtering>true</filtering>
        <includes>
            <include>**/jdbc.properties</include>
        </includes>
    </resource>
    <resource>
        <directory>${project.basedir}/src/main/resources</directory>
        <includes>
            <include>**/const.properties</include>
        </includes>
    </resource>
</resources>
```

现在pom.xml内容如下：

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.javacode2018</groupId>
    <artifactId>b2b-account-service</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <!-- 指定资源文件复制过程中采用的编码方式 -->
        <encoding>UTF-8</encoding>
        <jdbc.url>jdbc:mysql://localhost:3306/javacode2018?characterEncoding=UTF-8</jdbc.url>
        <jdbc.username>root</jdbc.username>
        <jdbc.password>root</jdbc.password>
    </properties>

    <dependencies>
        <dependency>
            <groupId>com.javacode2018</groupId>
            <artifactId>b2b-account-api</artifactId>
            <version>${project.version}</version>
        </dependency>
    </dependencies>

    <build>
        <resources>
            <resource>
                <!-- 指定资源文件的目录 -->
                <directory>${project.basedir}/src/main/resources</directory>
                <!-- 是否开启过滤替换配置，默认是不开启的 -->
                <filtering>true</filtering>
                <includes>
                    <include>**/jdbc.properties</include>
                </includes>
            </resource>
            <resource>
                <directory>${project.basedir}/src/main/resources</directory>
                <includes>
                    <include>**/const.properties</include>
                </includes>
            </resource>
        </resources>
        <testResources>
            <testResource>
                <!-- 指定资源文件的目录 -->
                <directory>${project.basedir}/src/test/resources</directory>
                <!-- 是否开启过滤替换配置，默认是不开启的 -->
                <filtering>true</filtering>
            </testResource>
        </testResources>

        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-resources-plugin</artifactId>
                <version>2.6</version>
                <configuration>
                    <!-- 是否使用默认的分隔符，默认分隔符是${*}和@ ,这个地方设置为false，表示不启用默认分隔符配置-->
                    <useDefaultDelimiters>false</useDefaultDelimiters>
                    <!-- 自定义分隔符 -->
                    <delimiters>
                        <delimiter>$*$</delimiter>
                        <delimiter>##</delimiter>
                    </delimiters>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>
```

执行命令看效果：

```
D:\code\IdeaProjects\b2b>mvn clean package -pl :b2b-account-service
[INFO] Scanning for projects...
[INFO]
[INFO] ----------------< com.javacode2018:b2b-account-service >----------------
[INFO] Building b2b-account-service 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ b2b-account-service ---
[INFO] Deleting D:\code\IdeaProjects\b2b\b2b-account\b2b-account-service\target
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ b2b-account-service ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 1 resource
[INFO] Copying 1 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ b2b-account-service ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ b2b-account-service ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 1 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ b2b-account-service ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ b2b-account-service ---
[INFO]
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ b2b-account-service ---
[INFO] Building jar: D:\code\IdeaProjects\b2b\b2b-account\b2b-account-service\target\b2b-account-service-1.0-SNAPSHOT.jar
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  2.116 s
[INFO] Finished at: 2019-11-21T18:38:59+08:00
[INFO] ------------------------------------------------------------------------
```

上面有如下输出：

```
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ b2b-account-service ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 1 resource
[INFO] Copying 1 resource
```

插件处理了2个资源文件。

我们看一下`target/classes`目录下两个文件的内容。

jdbc.proerties:

```
jdbc.url=${jdbc.url}
jdbc.username=root
jdbc.password=root
```

const.properties：

```
email=javacode2018@163.com
jdbc.url=${jdbc.url}
jdbc.username=$jdbc.username$
jdbc.password=##jdbc.password##
```

2个资源都被复制到target/classes目录了，只有`jdbc.properties`中的内容被替换了。

**关于资源文件处理的，更详细的过程可以去看这个插件的源码，下面我们来说多环境处理的问题。**

### 使用profiles处理多环境构建问题

maven支持让我们配置多套环境，每套环境中可以指定自己的maven属性，mvn命令对模块进行构建的时候可以通过`-P`参数来指定具体使用哪个环境的配置，具体向下看。

profiles元素支持定义多套环境的配置信息，配置如下用法：

```
<profiles>
    <profile>测试环境配置信息</profile>
    <profile>开发环境配置信息</profile>
    <profile>线上环境配置信息</profile>
    <profile>环境n配置信息</profile>
</profiles>
```

profiles中包含多个profile元素，每个profile可以表示一套环境，profile示例如下：

```
<profile>
    <id>dev</id>
    <properties>
        <jdbc.url>dev jdbc url</jdbc.url>
        <jdbc.username>dev jdbc username</jdbc.username>
        <jdbc.password>dev jdbc password</jdbc.password>
    </properties>
</profile>
```

> id：表示这套环境的标识信息，properties可以定义环境中使用到的属性列表。
>
> 执行mvn命令编译的时候可以带上一个`-P profileid`来使用指定的环境进行构建。

#### 指定环境进行构建

在`b2b-account-service/pom.xml`加入下面配置：

```
<profiles>
    <!-- 开发环境使用的配置 -->
    <profile>
        <id>dev</id>
        <properties>
            <jdbc.url>dev jdbc url</jdbc.url>
            <jdbc.username>dev jdbc username</jdbc.username>
            <jdbc.password>dev jdbc password</jdbc.password>
        </properties>
    </profile>
    <!-- 测试环境使用的配置 -->
    <profile>
        <id>test</id>
        <properties>
            <jdbc.url>test jdbc url</jdbc.url>
            <jdbc.username>test jdbc username</jdbc.username>
            <jdbc.password>test jdbc password</jdbc.password>
        </properties>
    </profile>
    <!-- 线上环境使用的配置 -->
    <profile>
        <id>prod</id>
        <properties>
            <jdbc.url>test jdbc url</jdbc.url>
            <jdbc.username>test jdbc username</jdbc.username>
            <jdbc.password>test jdbc password</jdbc.password>
        </properties>
    </profile>
</profiles>
```

将pom.xml的`project->properties`中的下面几个元素干掉，这些和profile中的重复了，需要干掉：

```
<jdbc.url>jdbc:mysql://localhost:3306/javacode2018?characterEncoding=UTF-8</jdbc.url>
<jdbc.username>root</jdbc.username>
<jdbc.password>root</jdbc.password>
```

此时pom.xml内容如下，建议大家直接贴进去：

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.javacode2018</groupId>
    <artifactId>b2b-account-service</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <!-- 指定资源文件复制过程中采用的编码方式 -->
        <encoding>UTF-8</encoding>
    </properties>

    <!-- 配置多套环境 -->
    <profiles>
        <!-- 开发环境使用的配置 -->
        <profile>
            <id>dev</id>
            <properties>
                <jdbc.url>dev jdbc url</jdbc.url>
                <jdbc.username>dev jdbc username</jdbc.username>
                <jdbc.password>dev jdbc password</jdbc.password>
            </properties>
        </profile>
        <!-- 测试环境使用的配置 -->
        <profile>
            <id>test</id>
            <properties>
                <jdbc.url>test jdbc url</jdbc.url>
                <jdbc.username>test jdbc username</jdbc.username>
                <jdbc.password>test jdbc password</jdbc.password>
            </properties>
        </profile>
        <!-- 线上环境使用的配置 -->
        <profile>
            <id>prod</id>
            <properties>
                <jdbc.url>prod jdbc url</jdbc.url>
                <jdbc.username>prod jdbc username</jdbc.username>
                <jdbc.password>prod jdbc password</jdbc.password>
            </properties>
        </profile>
    </profiles>

    <dependencies>
        <dependency>
            <groupId>com.javacode2018</groupId>
            <artifactId>b2b-account-api</artifactId>
            <version>${project.version}</version>
        </dependency>
    </dependencies>

    <build>
        <resources>
            <resource>
                <!-- 指定资源文件的目录 -->
                <directory>${project.basedir}/src/main/resources</directory>
                <!-- 是否开启过滤替换配置，默认是不开启的 -->
                <filtering>true</filtering>
                <includes>
                    <include>**/jdbc.properties</include>
                </includes>
            </resource>
            <resource>
                <directory>${project.basedir}/src/main/resources</directory>
                <includes>
                    <include>**/const.properties</include>
                </includes>
            </resource>
        </resources>
        <testResources>
            <testResource>
                <!-- 指定资源文件的目录 -->
                <directory>${project.basedir}/src/test/resources</directory>
                <!-- 是否开启过滤替换配置，默认是不开启的 -->
                <filtering>true</filtering>
            </testResource>
        </testResources>

        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-resources-plugin</artifactId>
                <version>2.6</version>
                <configuration>
                    <!-- 是否使用默认的分隔符，默认分隔符是${*}和@ ,这个地方设置为false，表示不启用默认分隔符配置-->
                    <useDefaultDelimiters>false</useDefaultDelimiters>
                    <!-- 自定义分隔符 -->
                    <delimiters>
                        <delimiter>$*$</delimiter>
                        <delimiter>##</delimiter>
                    </delimiters>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>
```

修改`src/main/resource/jdbc.properties`内容如下：

```
jdbc.url=$jdbc.url$
jdbc.username=$jdbc.username$
jdbc.password=##jdbc.password##
```

运行下面的构建命令：

```
D:\code\IdeaProjects\b2b>mvn clean package -pl :b2b-account-service -Pdev
[INFO] Scanning for projects...
[INFO]
[INFO] ----------------< com.javacode2018:b2b-account-service >----------------
[INFO] Building b2b-account-service 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ b2b-account-service ---
[INFO] Deleting D:\code\IdeaProjects\b2b\b2b-account\b2b-account-service\target
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ b2b-account-service ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 1 resource
[INFO] Copying 1 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ b2b-account-service ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ b2b-account-service ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 1 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ b2b-account-service ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ b2b-account-service ---
[INFO]
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ b2b-account-service ---
[INFO] Building jar: D:\code\IdeaProjects\b2b\b2b-account\b2b-account-service\target\b2b-account-service-1.0-SNAPSHOT.jar
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  2.361 s
[INFO] Finished at: 2019-11-21T18:51:16+08:00
[INFO] ------------------------------------------------------------------------
```

> **注意上面命令中多了一个`-Pdev`参数，`-P后面跟的是pom.xml中profile的id`，表示需要使用那套环境进行构建。此时我们使用的是`dev`环境，即开发环境。**

看一下`target/classes/jdbc.properties`，内容变成了下面这样：

```
jdbc.url=dev jdbc url
jdbc.username=dev jdbc username
jdbc.password=dev jdbc password
```

上面文件的内容和pom.xml中dev的profile中的properties对比一下，内容是不是一样的，说明资源文件中内容被替换为了dev环境的配置，我们再来试试`test`环境，执行下面命令：

```
mvn clean package -pl :b2b-account-service -Ptest
```

看一下`target/classes/jdbc.properties`，内容变成了下面这样：

```
jdbc.url=test jdbc url
jdbc.username=test jdbc username
jdbc.password=test jdbc password
```

看到了么，神奇的效果出现了，test环境起效了。

#### 开启默认环境配置

我们在执行下面命令：

```
mvn clean package -pl :b2b-account-service
```

看一下`target/classes/jdbc.properties`，内容变成了下面这样：

```
jdbc.url=$jdbc.url$
jdbc.username=$jdbc.username$
jdbc.password=##jdbc.password##
```

内容没有被替换，因为我们没有通过-P来指定具体使用哪个环境进行构建，所以出现了这种现象。

但是我们可以指定一个默认开启的配置，我们默认开启dev的配置，修改dev的profile元素，在这个元素下面加上：

```
<activation>
    <activeByDefault>true</activeByDefault>
</activation>
```

> activeByDefault表示默认开启这个环境的配置，默认值是false，这个地方我们设置为true，表示开启默认配置。

此时dev的配置如下：

```
<profile>
    <id>dev</id>
    <activation>
        <activeByDefault>true</activeByDefault>
    </activation>
    <properties>
        <jdbc.url>dev jdbc url</jdbc.url>
        <jdbc.username>dev jdbc username</jdbc.username>
        <jdbc.password>dev jdbc password</jdbc.password>
    </properties>
</profile>
```

执行下面命令：

```
mvn clean package -pl :b2b-account-service
```

看一下`target/classes/jdbc.properties`，内容变成了下面这样：

```
jdbc.url=dev jdbc url
jdbc.username=dev jdbc username
jdbc.password=dev jdbc password
```

这次我们没有指定环境，默认使用了dev环境。

#### 通过maven属性来控制环境的开启

刚才上面说了通过-P profileId的方式来指定环境，现在我们想通过自定义的属性值来控制使用哪个环境。

可以在profile元素中加入下面配置

```
<activation>
    <property>
        <name>属性xx</name>
        <value>属性xx的值</value>
    </property>
</activation>
```

那么我们可以在mvn后面跟上下面的命令可以开启匹配的环境：

```
mvn ... -D属性xx=属性xx的值
```

> -D可以通过命令行指定一些属性的值，这个前面有讲过，-D后面的属性会和activation->properties中的name、value进行匹配，匹配成功的环境都会被开启。

将pom.xml中`profiles`元素修改成下面这样：

```
<!-- 配置多套环境 -->
<profiles>
    <!-- 开发环境使用的配置 -->
    <profile>
        <id>dev</id>
        <activation>
            <activeByDefault>true</activeByDefault>
            <property>
                <name>env</name>
                <value>env_dev</value>
            </property>
        </activation>
        <properties>
            <jdbc.url>dev jdbc url</jdbc.url>
            <jdbc.username>dev jdbc username</jdbc.username>
            <jdbc.password>dev jdbc password</jdbc.password>
        </properties>
    </profile>
    <!-- 测试环境使用的配置 -->
    <profile>
        <id>test</id>
        <activation>
            <property>
                <name>env</name>
                <value>env_test</value>
            </property>
        </activation>
        <properties>
            <jdbc.url>test jdbc url</jdbc.url>
            <jdbc.username>test jdbc username</jdbc.username>
            <jdbc.password>test jdbc password</jdbc.password>
        </properties>
    </profile>
    <!-- 线上环境使用的配置 -->
    <profile>
        <id>prod</id>
        <activation>
            <property>
                <name>env</name>
                <value>env_prod</value>
            </property>
        </activation>
        <properties>
            <jdbc.url>prod jdbc url</jdbc.url>
            <jdbc.username>prod jdbc username</jdbc.username>
            <jdbc.password>prod jdbc password</jdbc.password>
        </properties>
    </profile>
</profiles>
```

运行命令：

```
mvn clean package -pl :b2b-account-service -Denv=env_prod
```

`target/classes/jdbc.properties`内容如下：

```
jdbc.url=prod jdbc url
jdbc.username=prod jdbc username
jdbc.password=prod jdbc password
```

说明使用到了prod环境的配置。

#### 启动的时候指定多个环境

可以在`-P`参数后跟多个环境的id，多个之间用逗号隔开，当使用多套环境的时候，多套环境中的maven属性会进行合并，如果多套环境中属性有一样的，后面的会覆盖前面的。

运行下面命令看效果：

```
mvn clean package -pl :b2b-account-service -Pdev,test,prod
```

修改pom.xml中的profiles元素，如下：

```
<!-- 配置多套环境 -->
<profiles>
    <!-- 开发环境使用的配置 -->
    <profile>
        <id>dev</id>
        <activation>
            <activeByDefault>true</activeByDefault>
            <property>
                <name>env</name>
                <value>env_dev</value>
            </property>
        </activation>
        <properties>
            <jdbc.url>dev jdbc url</jdbc.url>
        </properties>
    </profile>
    <!-- 测试环境使用的配置 -->
    <profile>
        <id>test</id>
        <activation>
            <property>
                <name>env</name>
                <value>env_test</value>
            </property>
        </activation>
        <properties>
            <jdbc.username>test jdbc username</jdbc.username>
        </properties>
    </profile>
    <!-- 线上环境使用的配置 -->
    <profile>
        <id>prod</id>
        <activation>
            <property>
                <name>env</name>
                <value>env_prod</value>
            </property>
        </activation>
        <properties>
            <jdbc.password>prod jdbc password</jdbc.password>
        </properties>
    </profile>
</profiles>
```

> 注意看一下上面3个环境中都只有一个自定义属性了

下面我们同时使用3个环境，执行下面命令：

```
mvn clean package -pl :b2b-account-service -Pdev,test,prod
```

target中的jdbc.properties文件变成了这样：

```
jdbc.url=dev jdbc url
jdbc.username=test jdbc username
jdbc.password=prod jdbc password
```

#### 查看目前有哪些环境

##### 命令：

```
mvn help:all-profiles
```

##### 案例：

```
D:\code\IdeaProjects\b2b>mvn help:all-profiles -pl :b2b-account-service
[INFO] Scanning for projects...
[INFO]
[INFO] ----------------< com.javacode2018:b2b-account-service >----------------
[INFO] Building b2b-account-service 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-help-plugin:3.2.0:all-profiles (default-cli) @ b2b-account-service ---
[INFO] Listing Profiles for Project: com.javacode2018:b2b-account-service:jar:1.0-SNAPSHOT
  Profile Id: dev (Active: true , Source: pom)
  Profile Id: prod (Active: false , Source: pom)
  Profile Id: test (Active: false , Source: pom)

[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  1.527 s
[INFO] Finished at: 2019-11-21T19:16:34+08:00
[INFO] ------------------------------------------------------------------------
```

从输出中可以看到有3个环境，他们的id，默认激活的是那个等信息。

#### 查看目前激活的是哪些环境

##### 命令：

```
mvn help:active-profiles
```

##### 案例：

```
D:\code\IdeaProjects\b2b>mvn help:active-profiles -pl :b2b-account-service -Ptest,prod
[INFO] Scanning for projects...
[INFO]
[INFO] ----------------< com.javacode2018:b2b-account-service >----------------
[INFO] Building b2b-account-service 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-help-plugin:3.2.0:active-profiles (default-cli) @ b2b-account-service ---
[INFO]
Active Profiles for Project 'com.javacode2018:b2b-account-service:jar:1.0-SNAPSHOT':

The following profiles are active:

 - test (source: com.javacode2018:b2b-account-service:1.0-SNAPSHOT)
 - prod (source: com.javacode2018:b2b-account-service:1.0-SNAPSHOT)



[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  1.387 s
[INFO] Finished at: 2019-11-21T19:25:56+08:00
[INFO] ------------------------------------------------------------------------
```

从输出中可以看到启用了2套环境，分别是`test`和`prod`。

#### 新问题：配置太分散了

我们的系统中有2个模块需要用到数据库的配置，这两个模块是：

```
b2b-account-service
b2b-order-service
```

上面我们介绍了`b2b-account-service`不同环境的构建操作，是在`pom.xml`中进行配置的，`b2b-order-service`中数据的配置也可以这么做，如果以后有更多的模块都需要连接不同的数据库，是不是每个模块中都需要配置这样的`pom.xml`，那时候数据库的配置分散在几十个`pom.xml`文件中，如果运维需要修改数据库的配置的时候，需要去每个模块中去修改`pom.xml`中的属性，这种操作会让人疯掉的，我们可以怎么做？

我们可以将数据库所有的配置放在一个文件中，运维只用修改这一个文件就行了，然后执行构件操作，重新发布上线，就ok了。

maven支持我们这么做，可以在profile中指定一个外部属性文件`xx.properties`，文件内容是这种格式的：

```
key1=value1
key2=value2
keyn=value2
```

然后在profile元素中加入下面配置：

```
<build>
    <filters>
        <filter>xx.properties文件路径（相对路径或者完整路径）</filter>
    </filters>
</build>
```

上面的`filter`元素可以指定多个，当有多个外部属性配置文件的时候，可以指定多个来进行引用。

然后资源文件复制的时候就可以使用下面的方式引用外部资源文件的内容：

```
xxx=${key1}
```

##### 案例

新建文件`b2b/config/dev.properties`，内容如下：

```
# b2b-account-service jdbc配置信息
b2b-account-service.jdbc.url=dev_account_jdbc_url
b2b-account-service.jdbc.username=dev_account_jdbc_username
b2b-account-service.jdbc.password=dev_account_jdbc_password

# b2b-order-service jdbc配置信息
b2b-order-service.jdbc.url=dev_order_jdbc_url
b2b-order-service.jdbc.username=dev_order_jdbc_username
b2b-order-service.jdbc.password=dev_order_jdbc_password
```

新建文件`b2b/config/test.properties`，内容如下：

```
# b2b-account-service jdbc配置信息
b2b-account-service.jdbc.url=test_account_jdbc_url
b2b-account-service.jdbc.username=test_account_jdbc_username
b2b-account-service.jdbc.password=test_account_jdbc_password

# b2b-order-service jdbc配置信息
b2b-order-service.jdbc.url=test_order_jdbc_url
b2b-order-service.jdbc.username=test_order_jdbc_username
b2b-order-service.jdbc.password=test_order_jdbc_password
```

新建文件`b2b/config/prod.properties`，内容如下：

```
# b2b-account-service jdbc配置信息
b2b-account-service.jdbc.url=prod_account_jdbc_url
b2b-account-service.jdbc.username=prod_account_jdbc_username
b2b-account-service.jdbc.password=prod_account_jdbc_password

# b2b-order-service jdbc配置信息
b2b-order-service.jdbc.url=prod_order_jdbc_url
b2b-order-service.jdbc.username=prod_order_jdbc_username
b2b-order-service.jdbc.password=prod_order_jdbc_password
```

`b2b-account-service/src/main/resource/jdbc.properties`文件内容：

```
jdbc.url=${b2b-account-service.jdbc.url}
jdbc.username=${b2b-account-service.jdbc.username}
jdbc.password=${b2b-account-service.jdbc.password}
```

> 看到了没，这里面`${}`中的属性名称取的是上面`b2b/config`目录中资源文件中的属性名称。

`b2b-order-service/src/main/resource/jdbc.properties`文件内容：

```
jdbc.url=${b2b-order-service.jdbc.url}
jdbc.username=${b2b-order-service.jdbc.username}
jdbc.password=${b2b-order-service.jdbc.password}
```

`b2b-account/pom.xml`内容如下：

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.javacode2018</groupId>
    <artifactId>b2b-account-service</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <dependency>
            <groupId>com.javacode2018</groupId>
            <artifactId>b2b-account-api</artifactId>
            <version>${project.version}</version>
        </dependency>
    </dependencies>

    <properties>
        <!-- 指定资源文件复制过程中采用的编码方式 -->
        <encoding>UTF-8</encoding>
    </properties>

    <!-- 配置多套环境 -->
    <profiles>
        <!-- 开发环境使用的配置 -->
        <profile>
            <id>dev</id>
            <activation>
                <activeByDefault>true</activeByDefault>
                <property>
                    <name>env</name>
                    <value>env_dev</value>
                </property>
            </activation>
            <build>
                <filters>
                    <filter>../../config/dev.properties</filter>
                </filters>
            </build>
        </profile>
        <!-- 测试环境使用的配置 -->
        <profile>
            <id>test</id>
            <activation>
                <property>
                    <name>env</name>
                    <value>env_test</value>
                </property>
            </activation>
            <build>
                <filters>
                    <filter>../../config/test.properties</filter>
                </filters>
            </build>
        </profile>
        <!-- 线上环境使用的配置 -->
        <profile>
            <id>prod</id>
            <activation>
                <property>
                    <name>env</name>
                    <value>env_prod</value>
                </property>
            </activation>
            <build>
                <filters>
                    <filter>../../config/prod.properties</filter>
                </filters>
            </build>
        </profile>
    </profiles>

    <build>
        <resources>
            <resource>
                <!-- 指定资源文件的目录 -->
                <directory>${project.basedir}/src/main/resources</directory>
                <!-- 是否开启过滤替换配置，默认是不开启的 -->
                <filtering>true</filtering>
            </resource>
        </resources>
        <testResources>
            <testResource>
                <!-- 指定资源文件的目录 -->
                <directory>${project.basedir}/src/test/resources</directory>
                <!-- 是否开启过滤替换配置，默认是不开启的 -->
                <filtering>true</filtering>
            </testResource>
        </testResources>
    </build>

</project>
```

`b2b-order/pom.xml`内容如下：

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.javacode2018</groupId>
    <artifactId>b2b-order-service</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <dependency>
            <groupId>${project.groupId}</groupId>
            <artifactId>b2b-account-api</artifactId>
            <version>${project.version}</version>
        </dependency>
        <dependency>
            <groupId>${project.groupId}</groupId>
            <artifactId>b2b-order-api</artifactId>
            <version>${project.version}</version>
        </dependency>
    </dependencies>

    <properties>
        <!-- 指定资源文件复制过程中采用的编码方式 -->
        <encoding>UTF-8</encoding>
    </properties>

    <!-- 配置多套环境 -->
    <profiles>
        <!-- 开发环境使用的配置 -->
        <profile>
            <id>dev</id>
            <activation>
                <activeByDefault>true</activeByDefault>
                <property>
                    <name>env</name>
                    <value>env_dev</value>
                </property>
            </activation>
            <build>
                <filters>
                    <filter>../../config/dev.properties</filter>
                </filters>
            </build>
        </profile>
        <!-- 测试环境使用的配置 -->
        <profile>
            <id>test</id>
            <activation>
                <property>
                    <name>env</name>
                    <value>env_test</value>
                </property>
            </activation>
            <build>
                <filters>
                    <filter>../../config/test.properties</filter>
                </filters>
            </build>
        </profile>
        <!-- 线上环境使用的配置 -->
        <profile>
            <id>prod</id>
            <activation>
                <property>
                    <name>env</name>
                    <value>env_prod</value>
                </property>
            </activation>
            <build>
                <filters>
                    <filter>../../config/prod.properties</filter>
                </filters>
            </build>
        </profile>
    </profiles>

    <build>
        <resources>
            <resource>
                <!-- 指定资源文件的目录 -->
                <directory>${project.basedir}/src/main/resources</directory>
                <!-- 是否开启过滤替换配置，默认是不开启的 -->
                <filtering>true</filtering>
            </resource>
        </resources>
        <testResources>
            <testResource>
                <!-- 指定资源文件的目录 -->
                <directory>${project.basedir}/src/test/resources</directory>
                <!-- 是否开启过滤替换配置，默认是不开启的 -->
                <filtering>true</filtering>
            </testResource>
        </testResources>
    </build>

</project>
```

见证奇迹的时刻来了。

执行下面命令：

```
D:\code\IdeaProjects\b2b>mvn clean package -pl :b2b-account-service,:b2b-order-service -Pdev
[INFO] Scanning for projects...
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Build Order:
[INFO]
[INFO] b2b-account-service                                                [jar]
[INFO] b2b-order-service                                                  [jar]
[INFO]
[INFO] ----------------< com.javacode2018:b2b-account-service >----------------
[INFO] Building b2b-account-service 1.0-SNAPSHOT                          [1/2]
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ b2b-account-service ---
[INFO] Deleting D:\code\IdeaProjects\b2b\b2b-account\b2b-account-service\target
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ b2b-account-service ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 1 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ b2b-account-service ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ b2b-account-service ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 1 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ b2b-account-service ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ b2b-account-service ---
[INFO]
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ b2b-account-service ---
[INFO] Building jar: D:\code\IdeaProjects\b2b\b2b-account\b2b-account-service\target\b2b-account-service-1.0-SNAPSHOT.jar
[INFO]
[INFO] -----------------< com.javacode2018:b2b-order-service >-----------------
[INFO] Building b2b-order-service 1.0-SNAPSHOT                            [2/2]
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ b2b-order-service ---
[INFO] Deleting D:\code\IdeaProjects\b2b\b2b-order\b2b-order-service\target
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ b2b-order-service ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 1 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ b2b-order-service ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ b2b-order-service ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory D:\code\IdeaProjects\b2b\b2b-order\b2b-order-service\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ b2b-order-service ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ b2b-order-service ---
[INFO] No tests to run.
[INFO]
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ b2b-order-service ---
[INFO] Building jar: D:\code\IdeaProjects\b2b\b2b-order\b2b-order-service\target\b2b-order-service-1.0-SNAPSHOT.jar
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary for b2b-account-service 1.0-SNAPSHOT:
[INFO]
[INFO] b2b-account-service ................................ SUCCESS [  2.510 s]
[INFO] b2b-order-service .................................. SUCCESS [  0.257 s]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  3.026 s
[INFO] Finished at: 2019-11-21T20:11:32+08:00
[INFO] ------------------------------------------------------------------------
```

看一下两个模块target/classes下面jdbc.properties内容，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bb1IzXs5M1sFnGmhYXreCC7L8SJkoMianpyfia881k8B9Kbvjiaomn9uAdyZXK1sGPJ35zjGictVhricg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

使用了dev环境的配置，dev环境中处理资源文件替换的时候采用了`config/dev.properties`文件的内容，这部分配置如下，重点在于`build->filters元素`：

```
<profile>
    <id>dev</id>
    <activation>
        <activeByDefault>true</activeByDefault>
        <property>
            <name>env</name>
            <value>env_dev</value>
        </property>
    </activation>
    <build>
        <filters>
            <filter>../../config/dev.properties</filter>
        </filters>
    </build>
</profile>
```

我们再使用prod环境看一下效果，执行下面命令：

```
mvn clean package -pl :b2b-account-service,:b2b-order-service -Pprod
```

效果图如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bb1IzXs5M1sFnGmhYXreCCQwkW0fPrKy9f0tmuVJ8o5zS6OmGhE0loCaABNBpX5zviaOmODIVoc5Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

感受一下，这次使用了`b2b/config/prod.properties`文件的内容对资源文件进行了替换，这次将所有配置的信息集中在config目录的几个文件中进行处理了，对于运维和开发来说都非常方便了。

#### profile元素更强大的功能

profile元素可以用于对不同环境的构建进行配置，project中包含的元素，在profile元素中基本上都有，所以profile可以定制更复杂的构建过程，不同的环境依赖的构件、插件、build过程、测试过程都是不一样的，这些都可以在profile中进行指定，也就是说不同的环境所有的东西都可以通过profile元素来进行个性化的设置，这部分的功能有兴趣的大家可以自己去研究一下，截个图大家感受一下:

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bb1IzXs5M1sFnGmhYXreCCbPcYgcwWUcYhDrP74cVPPT85LWAGxvx6qqht3q8NLUrYA3L1Hvo59Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

从上图中可以看出这些属性都可以根据环境进行定制的。