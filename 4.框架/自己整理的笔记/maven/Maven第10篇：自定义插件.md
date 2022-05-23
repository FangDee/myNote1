# Maven第10篇：自定义插件

路人甲Java [程序员路人](javascript:void(0);) *2022-03-22 20:20*

**大家好，我是路人~~~**

**点击下方卡片，关注我，java干货及时送达**

![img](http://mmbiz.qpic.cn/mmbiz_png/kvBL17F8Dr1MFuX9tnHDHzic6yBpLSiaTibZLKcia9tHwm7aFueNjgoAIibdqXibicAUEia87xvDN77VhfDKbo59BJCwlw/0?wx_fmt=png)

**程序员路人**

专注Java相关技术：SSM、Spring全家桶、微服务、MySQL、MyCat、集群、分布式、中间件、Linux、网络、多线程，偶尔讲点运维Jenkins、Nexus、Docker、ELK，偶尔分享些技术干货，致力于Java全栈开发！

1篇原创内容



公众号

**这是 Maven 第 10 篇，点击上方卡片，即可翻阅前面章节。**

**整个maven系列的内容前后是有依赖的，如果之前没有接触过maven，建议从第一篇看起。**

Maven默认提供了很多插件，功能也非常强大，但是如果我们想自己开发一些插件，比如自定义一款自动打包并且发布到服务器然后重启服务器的插件；或者定义一款插件自动打包自动运行打包好的构件。各种好玩的东西只要你能想到，都可以通过maven插件去完成，不过我们需要先了解如何自定义maven插件。

### 本篇环境

1. jdk1.8
2. maven3.6.2
3. idea

### 本章内容

1. 自定义插件详细步骤
2. 自定义插件参数的使用
3. 自定义插件前缀的使用
4. 手动实现打包之后自动运行的插件

### 自定义插件详细步骤

maven中的插件是有很多目标（goal）组成的，开发插件，实际上就是去编写插件中目标的具体代码。每个目标对应一个java类，这个类在maven中叫做MOJO，maven提供了一个Mojo的接口，我们开发插件也就是去实现这个接口的方法，这个接口是：

```
org.apache.maven.plugin.Mojo
```

接口有3个方法：

```
void execute() throws MojoExecutionException, MojoFailureException;
void setLog( Log log );
Log getLog();
```

- **execute**：这个方法比较重要，目标的主要代码就在这个方法中实现，当使用mvn命令调用插件的目标的时候，最后具体调用的就是这个方法。
- **setLog**：注入一个标准的Maven日志记录器，允许这个Mojo向用户传递事件和反馈
- **getLog**：获取注入的日志记录器

说一下上面这个Log，这是一日志接口，里面定义了很多方法，主要用户向交互者输出日志，比如我们运行`mvn clean`，会输出很多提示信息，这些输出的信息就是通过Log来输出的。

Mojo接口有个默认的抽象类：

```
org.apache.maven.plugin.AbstractMojo
```

这个类中把`Mojo`接口中的`setLog`和`getLog`实现了，而`execute`方法没有实现，交给继承者去实现，这个类中Log默认可以向控制台输出日志信息，maven中自带的插件都继承这个类，一般情况下我们开发插件目标可以直接继承这个类，然后实现`execute`方法就可以了。

#### 实现一个插件的具体步骤

##### 1、 创建一个maven构件，这个构件的packaging比较特殊，必须为maven-plugin，表示这个构件是一个插件类型，如下：

> pom.xml中的packageing元素必须如下值：

```
<packaging>maven-plugin</packaging>
```

##### 2、导入maven插件依赖：

```
<dependency>
    <groupId>org.apache.maven</groupId>
    <artifactId>maven-plugin-api</artifactId>
    <version>3.0</version>
</dependency>

<dependency>
    <groupId>org.apache.maven.plugin-tools</groupId>
    <artifactId>maven-plugin-annotations</artifactId>
    <version>3.4</version>
    <scope>provided</scope>
</dependency>
```

##### 3、创建一个目标类，需要继承org.apache.maven.plugin.AbstractMojo

##### 4、目标类中添加注解@Mojo注解：

```
@org.apache.maven.plugins.annotations.Mojo(name="目标名称")
```

注意`@Mojo`注解用来标注这个类是一个目标类，maven对插件进行构建的时候会根据这个注解来找到这个插件的目标，这个注解中还有其他参数，后面在详细介绍。

##### 5、在目标类的execute方法中实现具体的逻辑

##### 6、安装插件到本地仓库：插件的pom.xml所在目录执行下面命令

```
mvn clean install
```

或者可以部署到私服仓库，部署方式和其他构件的方式一样，这个具体去看前面文章的私服的文章。

##### 7、让使用者去使用插件

#### 案例1

下面我们来实现我们第一个插件，插件构件信息：

```
<groupId>com.javacode2018</groupId>
<artifactId>demo1-maven-plugin</artifactId>
<version>1.0-SNAPSHOT</version>
<packaging>maven-plugin</packaging>
```

创建一个目标类demo1，调用这个目标的时候，希望他能够输出：

```
hello my first maven plugin!
```

##### 创建一个maven项目

打开idea，点击`File->New->Project`，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUKhbUD2Yh2Izib4aNQr2ScwuIotABIpfgKCrCOq29Nicv1TUN1pJFDukrDlHpAyDupLQNsEqDmS3g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

选择`Maven`，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUKhbUD2Yh2Izib4aNQr2ScRGWVOuXIHCeicribH90zDPw8nt9ibnmDJic1FnibGTq748nQflNO1zQb0Mw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击上图中的`Next`,如下图，输入项目坐标信息：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUKhbUD2Yh2Izib4aNQr2ScAeSSJneiaePfFicnNBkd9oZHiarVAaU6AJIlfmfsHibyuBfHib7oa4XAzlA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击上图中的`Next`，如下图，输入`Project name`为`maven-chat10`：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUKhbUD2Yh2Izib4aNQr2ScdrZDYow9oaibyibTt5uDbUx3xkOsLrBgdqRpK2sfjZgXGsO2w8f9uMkg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击上图中的`Finish`，完成创建，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUKhbUD2Yh2Izib4aNQr2ScKrOia6eibY4KY5fVgpJeIaMTJ1jBrFUJyGZT9ePkPqT4TUUBtPhdIZ2Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

配置一下idea的maven环境，点击`File->Settings`，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUKhbUD2Yh2Izib4aNQr2ScdsrgEEk9DjW2Q1sNg8pn47hfzWM9UtJ8ic16OjgSk6t0OBGIBHKLzbQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

删除下面2个无用的文件夹：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUKhbUD2Yh2Izib4aNQr2ScJhqpWUv5oYwvnT8ZZm52O5y63L1YUobmgC5aezQ0F3jypoNAKTxr3g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

##### 创建插件模块`demo1-maven-plugin`

这次用idea创建一个插件模块`demo1-maven-plugin`，具体过程如下。

在刚才的`maven-chat10`项目窗口中，点击`File->Project Structure`，如下图：

> 也可以使用快捷键`Ctrl+Alt+Shift+S`打开

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUKhbUD2Yh2Izib4aNQr2ScbNR2xQP5uuE9fJG79rPVYiaN7jxYQhBkCTCHbGkQWS9jTrPtsicvkicVw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUKhbUD2Yh2Izib4aNQr2Sc7ryu54fWgOGmLaeOiaX9kxibmFgTauGwmjYlhq6tWWo5dNXgH2bWKv2g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

选择上图找你的`Modules`，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUKhbUD2Yh2Izib4aNQr2ScDhictR2VElxIDlJjANWX6ib7g6HiaSjTtSslVUVbfPUibmHPtX9Xa3lXKg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击`+`，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUKhbUD2Yh2Izib4aNQr2SciaKCibetPVBjFXD2Xh0MuzSa5Gu9XuwoHGv2FC8icbYHoJzemibu0qJBMw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUKhbUD2Yh2Izib4aNQr2ScOhNpVWHlKunngsjVbibwib0APn6UCvOOicXzAUoByuPQJlqDI871wp3mQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

选择上图中的`New Module`，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUKhbUD2Yh2Izib4aNQr2ScGiaMdHFtc1Y3aaKbeibgicgK4pcTvaTvm5ZpRvKRDAfjhph23z36uxeAA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

选择上图左侧的`Maven`，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUKhbUD2Yh2Izib4aNQr2ScGjCM1AHJApJVibw6RM3jG3ffzMe6XldKTSicz3r5ZPlfRfVg2pDA9cIg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击上图中的`Next`，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUKhbUD2Yh2Izib4aNQr2ScyDXVeg60xia2GnhRrrrlJJsdfPNLPVNjia0Ef2Vj2AUgVFV0LL3lmZkA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

将`Add as module to`设置为`maven-chat10`，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUKhbUD2Yh2Izib4aNQr2ScIuFjm9c6wFR70hqSG9QG4T6Jn0Uh8eAiaKVKSeXgttXDPXM1YibLiaGgA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUKhbUD2Yh2Izib4aNQr2ScmTqDKjjR2MdicnCp3rUKNMa6lTpGibq0vY9OibY68qAEtGnLaM409GJfQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUKhbUD2Yh2Izib4aNQr2ScjwmgZKVw0DqaW4DxUbyYs86MUQF0VibWmlUYbSaqLLVrtV2iaae0WpGw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

上图中输入坐标信息，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUKhbUD2Yh2Izib4aNQr2ScRjByk3oMGlwDVsjam7Cov5MLkS2niaErIeplZOw1al3vZ5wApsoAHIQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击上图中的`Next`，默认如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUKhbUD2Yh2Izib4aNQr2ScKq6IkQ9tXbY85qLlicsw0kECwYNd4YKw3Qye5VqYoVftLUqiakEibAib4w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

修改上图中`Module name`为`demo1-maven-plugin`，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUKhbUD2Yh2Izib4aNQr2Sck9RzRZiaC0KQddDjrWMemUiaR95DWXs10ibIVI35Val5ljfmRNFxjuibhw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击上图的`Finish`，如下图:

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUKhbUD2Yh2Izib4aNQr2Scp7c2EHic93CVIQn5ia9T5Pbmk4aCnGibicZa0QqDLfSAicaNWU39pJUluUQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击上图中的`OK`按钮，完成创建工作，目前项目结构如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUKhbUD2Yh2Izib4aNQr2ScSGFuCZibZDcS9lXwbIfOqrWYjPoKQiczic6LDDibn5yj5yYAIoXdulYNXg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUKhbUD2Yh2Izib4aNQr2Sc0icdkibSKIVsw1HhicSvpcACiaCUNrhhfHyRlBrnvsDHM6YacVQic0qGcng/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

##### 设置demo1-maven-plugin/pom.xml中packaging的值为maven-plugin，如下

```
<packaging>maven-plugin</packaging>
```

##### demo1-maven-plugin/pom.xml引入插件需要的依赖

```
<dependency>
    <groupId>org.apache.maven</groupId>
    <artifactId>maven-plugin-api</artifactId>
    <version>3.0</version>
</dependency>

<dependency>
    <groupId>org.apache.maven.plugin-tools</groupId>
    <artifactId>maven-plugin-annotations</artifactId>
    <version>3.4</version>
    <scope>provided</scope>
</dependency>
```

最后`demo1-maven-plugin/pom.xml`内容如下

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.javacode2018</groupId>
    <artifactId>demo1-maven-plugin</artifactId>
    <version>1.0-SNAPSHOT</version>
    <!-- 插件必须为maven-plugin这个类型 -->
    <packaging>maven-plugin</packaging>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <!-- 配置maven编译的时候采用的编译器版本 -->
        <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
        <!-- 指定源代码是什么版本的，如果源码和这个版本不符将报错，maven中执行编译的时候会用到这个配置，默认是1.5，这个相当于javac命令后面的-source参数 -->
        <maven.compiler.source>1.8</maven.compiler.source>
        <!-- 该命令用于指定生成的class文件将保证和哪个版本的虚拟机进行兼容，maven中执行编译的时候会用到这个配置，默认是1.5，这个相当于javac命令后面的-target参数 -->
        <maven.compiler.target>1.8</maven.compiler.target>
    </properties>

    <dependencies>
        <!-- maven插件依赖 start -->
        <dependency>
            <groupId>org.apache.maven</groupId>
            <artifactId>maven-plugin-api</artifactId>
            <version>3.0</version>
        </dependency>

        <dependency>
            <groupId>org.apache.maven.plugin-tools</groupId>
            <artifactId>maven-plugin-annotations</artifactId>
            <version>3.4</version>
            <scope>provided</scope>
        </dependency>
        <!-- maven插件依赖 end -->
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-plugin-plugin</artifactId>
                <version>3.4</version>
            </plugin>
        </plugins>
    </build>

</project>
```

##### 创建目标类

在`demo-maven-plugin`中创建的目标类`com.javacode2018.Demo1Mojo`，需要继承`org.apache.maven.plugin.AbstractMojo`，需要实现`@Mojo注解`，如下：

```
package com.javacode2018;

import org.apache.maven.plugin.AbstractMojo;
import org.apache.maven.plugin.MojoExecutionException;
import org.apache.maven.plugin.MojoFailureException;
import org.apache.maven.plugins.annotations.Mojo;

/**
 * 工作10年的前阿里P7分享Java、算法、数据库方面的技术干货！坚信用技术改变命运，让家人过上更体面的生活！喜欢的请关注公众号：路人甲Java
 */
@Mojo(name = "demo1")
public class Demo1Mojo extends AbstractMojo {
    public void execute() throws MojoExecutionException, MojoFailureException {
    }
}
```

注意上面注解`@Mojo(name = "demo1")`，`name`使用来标注目标的名称为`demo1`。

##### 实现目标类的execute方法

我们在`execute`方法中输出一句话

```
this.getLog().info("hello my first maven plugin!");
```

目前execute方法代码如下：

```
public void execute() throws MojoExecutionException, MojoFailureException {
    this.getLog().info("hello my first maven plugin!");
}
```

##### 安装插件到本地仓库

在`maven-chat10/pom.xml`目录执行下面命令：

```
mvn clean install -pl :demo1-maven-plugin
```

> 注意上面命令和在`demo1-maven-plugin/pom`中执行`mvn clean install`效果是一样的，只是这个地方使用了maven裁剪的功能，对这块命令不熟悉的可以看：[Maven系列第8篇：大型Maven项目，快速按需任意构建，必备神技能！](https://mp.weixin.qq.com/s?__biz=MzA5MTkxMDQ4MQ==&mid=2648933693&idx=1&sn=b4a376e724d5735668f0d1bd27efd3a3&chksm=88621d03bf15941572902777407235154e2eb62e2d4655f6ec820c5e209994bbfe688c874726&token=1135746982&lang=zh_CN&scene=21#wechat_redirect)

上面命令效果如下：

```
D:\code\IdeaProjects\maven-chat10>mvn clean install -pl :demo1-maven-plugin
[INFO] Scanning for projects...
[INFO]
[INFO] ----------------< com.javacode2018:demo1-maven-plugin >-----------------
[INFO] Building demo1-maven-plugin 1.0-SNAPSHOT
[INFO] ----------------------------[ maven-plugin ]----------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ demo1-maven-plugin ---
[INFO] Deleting D:\code\IdeaProjects\maven-chat10\demo1-maven-plugin\target
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ demo1-maven-plugin ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ demo1-maven-plugin ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 1 source file to D:\code\IdeaProjects\maven-chat10\demo1-maven-plugin\target\classes
[INFO]
[INFO] --- maven-plugin-plugin:3.4:descriptor (default-descriptor) @ demo1-maven-plugin ---
[INFO] Using 'UTF-8' encoding to read mojo metadata.
[INFO] Mojo extractor with id: java-javadoc found 0 mojo descriptors.
[INFO] Mojo extractor with id: java-annotations found 1 mojo descriptors.
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ demo1-maven-plugin ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory D:\code\IdeaProjects\maven-chat10\demo1-maven-plugin\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ demo1-maven-plugin ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ demo1-maven-plugin ---
[INFO] No tests to run.
[INFO]
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ demo1-maven-plugin ---
[INFO] Building jar: D:\code\IdeaProjects\maven-chat10\demo1-maven-plugin\target\demo1-maven-plugin-1.0-SNAPSHOT.jar
[INFO]
[INFO] --- maven-plugin-plugin:3.4:addPluginArtifactMetadata (default-addPluginArtifactMetadata) @ demo1-maven-plugin ---
[INFO]
[INFO] --- maven-install-plugin:2.4:install (default-install) @ demo1-maven-plugin ---
[INFO] Installing D:\code\IdeaProjects\maven-chat10\demo1-maven-plugin\target\demo1-maven-plugin-1.0-SNAPSHOT.jar to C:\Users\Think\.m2\repository\com\javacode2018\demo1-maven-plugin\1.0-SNAPSHOT\demo1-maven-plugin-1.0-SNAPSHOT.jar
[INFO] Installing D:\code\IdeaProjects\maven-chat10\demo1-maven-plugin\pom.xml to C:\Users\Think\.m2\repository\com\javacode2018\demo1-maven-plugin\1.0-SNAPSHOT\demo1-maven-plugin-1.0-SNAPSHOT.pom
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  5.402 s
[INFO] Finished at: 2019-11-26T15:21:26+08:00
[INFO] ------------------------------------------------------------------------
```

##### 验证插件，调用插件的demo1目标看效果

`maven-chat10/pom.xml`所在目录执行：

```
mvn com.javacode2018:demo1-maven-plugin:demo1
```

效果如下：

```
D:\code\IdeaProjects\maven-chat10>mvn com.javacode2018:demo1-maven-plugin:demo1
[INFO] Scanning for projects...
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Build Order:
[INFO]
[INFO] demo1-maven-plugin                                        [maven-plugin]
[INFO] maven-chat10                                                       [pom]
[INFO]
[INFO] ----------------< com.javacode2018:demo1-maven-plugin >-----------------
[INFO] Building demo1-maven-plugin 1.0-SNAPSHOT                           [1/2]
[INFO] ----------------------------[ maven-plugin ]----------------------------
[INFO]
[INFO] --- demo1-maven-plugin:1.0-SNAPSHOT:demo1 (default-cli) @ demo1-maven-plugin ---
[INFO] hello my first maven plugin!
[INFO]
[INFO] -------------------< com.javacode2018:maven-chat10 >--------------------
[INFO] Building maven-chat10 1.0-SNAPSHOT                                 [2/2]
[INFO] --------------------------------[ pom ]---------------------------------
[INFO]
[INFO] --- demo1-maven-plugin:1.0-SNAPSHOT:demo1 (default-cli) @ maven-chat10 ---
[INFO] hello my first maven plugin!
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary for maven-chat10 1.0-SNAPSHOT:
[INFO]
[INFO] demo1-maven-plugin ................................. SUCCESS [  0.358 s]
[INFO] maven-chat10 ....................................... SUCCESS [  0.042 s]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  0.656 s
[INFO] Finished at: 2019-11-26T13:49:30+08:00
```

上面输出的东西比较多，我们主要看一下有这样的一句输出，如下：

```
[INFO] hello my first maven plugin!
```

上面这个输出就是我们在`execute`方法中输出的内容。

### 目标中参数的使用

上面我们介绍了开发一个插件目标详细的实现过程，然后写了一个简单的案例，比较简单。不过自定义的`Mojo`如果没有参数，那么这个`Mojo`基本上也实现不了什么复杂的功能，下面我们来看一下Mojo中如何使用参数。

#### 需要先在mojo中定义参数

定义参数就像在mojo中创建一个实例变量并添加适当的注释一样简单。下面列出了一个简单mojo的参数示例：

```
/**
 * 要显示的问候语。
 */
@Parameter( property = "sayhi.greeting", defaultValue = "Hello World!" )
private String greeting;
```

> @Parameter注解之前的部分是参数的描述，这个注解将变量标识为mojo参数。注解的defaultValue参数定义变量的默认值，此值maven的属性值，例如“${project.version}”（更多信息可以看上一篇文章中的[ target="_blank">maven属性部分](https://mp.weixin.qq.com/s?__biz=MzA5MTkxMDQ4MQ==&mid=2648933709&idx=1&sn=dcbb0185d88ce5e440c3102b0bd2e325&chksm=88621d73bf159465d06dc1cd651bd73c25c03aff3038d4938ddbb072a40df94d12b162ebe77a&token=2023299221&lang=zh_CN&scene=21#wechat_redirect)），property参数可用于通过引用用户通过-D选项设置的系统属性，即通过从命令行配置mojo参数，如`mvn ... -Dsayhi.greeting=路人甲Java`可以将`路人甲Java`的值传递给`greeting`参数，这个注解还有几个属性大家有兴趣的可以自己去研究一下。

#### 在pom.xml配置参数的值

```
<plugin>
  <groupId>com.javacode2018</groupId>
  <artifactId>demo1-maven-plugin</artifactId>
  <version>1.0-SNAPSHOT</version>
  <configuration>
    <greeting>欢迎您和【路人甲Java】一起学习Maven技术！</greeting>
  </configuration>
</plugin>
```

上面设置的是一个string类型的`greeting`参数的值，还有其他很多类型定义以及使用，我们也来看一下。

#### Boolean参数

```
/**
* My boolean.
*/
@Parameter
private boolean myBoolean;
<myBoolean>true</myBoolean>
```

#### 数字类型参数

数字类型包含：`byte`, `Byte`, `int`, `Integer`, `long`, `Long`, `short`, `Short`，读取配置时，XML文件中的文本将使用适当类的integer.parseInt（）或valueOf（）方法转换为整数值，这意味着字符串必须是有效的十进制整数值，仅由数字0到9组成，前面有一个可选的-表示负值。例子：

```
/**
* My Integer.
*/
@Parameter
private Integer myInteger;
<myInteger>10</myInteger>
```

#### File类型参数

读取配置时，XML文件中的文本用作所需文件或目录的路径。如果路径是相对的（不是以/或C:之类的驱动器号开头），则路径是相对于包含POM的目录的。例子：

```
/**
* My File.
*/
@Parameter
private File myFile;
<myFile>c:\temp</myFile>
```

#### 枚举类型参数

```
public enum Color {
      GREEN,
      RED,
      BLUE
}

/**
* My Enum
*/
@Parameter
private Color myColor;
<myColor>GREEN</myColor>
```

#### 数组类型参数

```
/**
* My Array.
*/
@Parameter
private String[] myArray;
<myArray>
  <param>value1</param>
  <param>value2</param>
</myArray>
```

#### Collections类型参数

```
/**
* My List.
*/
@Parameter
private List myList;
<myList>
  <param>value1</param>
  <param>value2</param>
</myList>
```

#### Maps类型参数

```
/**
* My Map.
*/
@Parameter
private Map myMap;
<myMap>
  <key1>value1</key1>
  <key2>value2</key2>
</myMap>
```

#### Properties类型参数

`java.util.Properties`的类型

```
/**
* My Properties.
*/
@Parameter
private Properties myProperties;
<myProperties>
  <property>
    <name>propertyName1</name>
    <value>propertyValue1</value>
  <property>
  <property>
    <name>propertyName2</name>
    <value>propertyValue2</value>
  <property>
</myProperties>
```

#### 自定义类型参数

```
/**
* My Object.
*/
@Parameter
private MyObject myObject;
<myObject>
  <myField>test</myField>
</myObject>
```

#### 案例2

##### 修改案例代码

我们将上面各种类型的参数都放到Demo1Mojo中，Demo1Mojo类如下：

```
package com.javacode2018;

import org.apache.maven.plugin.AbstractMojo;
import org.apache.maven.plugin.MojoExecutionException;
import org.apache.maven.plugin.MojoFailureException;
import org.apache.maven.plugins.annotations.Mojo;
import org.apache.maven.plugins.annotations.Parameter;

import java.io.File;
import java.lang.reflect.Field;
import java.util.Arrays;
import java.util.List;
import java.util.Map;
import java.util.Properties;

/**
 * 工作10年的前阿里P7分享Java、算法、数据库方面的技术干货！坚信用技术改变命运，让家人过上更体面的生活！喜欢的请关注公众号：路人甲Java
 */
@Mojo(name = "demo1")
public class Demo1Mojo extends AbstractMojo {

    /**
     * 要显示的问候语。
     */
    @Parameter(property = "sayhi.greeting", defaultValue = "Hello World!")
    private String greeting;

    /**
     * My boolean.
     */
    @Parameter
    private boolean myBoolean;

    /**
     * My Integer.
     */
    @Parameter
    private Integer myInteger;

    /**
     * My File.
     */
    @Parameter
    private File myFile;

    public enum Color {
        GREEN,
        RED,
        BLUE
    }

    /**
     * My Enum
     */
    @Parameter
    private Color myColor;

    /**
     * My Array.
     */
    @Parameter
    private String[] myArray;

    /**
     * My List.
     */
    @Parameter
    private List myList;

    /**
     * My Map.
     */
    @Parameter
    private Map myMap;

    /**
     * My Properties.
     */
    @Parameter
    private Properties myProperties;

    public static class Person {
        private String name;
        private int age;

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        public int getAge() {
            return age;
        }

        public void setAge(int age) {
            this.age = age;
        }

        @Override
        public String toString() {
            return "Person{" +
                    "name='" + name + '\'' +
                    ", age=" + age +
                    '}';
        }
    }

    @Parameter
    private Person person;

    public void execute() throws MojoExecutionException, MojoFailureException {
        this.getLog().info("hello my first maven plugin!");

        Field[] declaredFields = Demo1Mojo.class.getDeclaredFields();

        Arrays.stream(declaredFields).forEach(f -> {
            if (f.isAccessible()) {
                f.setAccessible(true);
            }
            try {
                this.getLog().info(f.getName() + ":" + f.get(this));
            } catch (IllegalAccessException e) {
                this.getLog().warn(e);
            }
        });
    }
}
```

##### 将`demo1-maven-plugin`安装到本地仓库

`maven-chat10/pom.xml`所在目录运行：

```
mvn clean install -pl :demo1-maven-plugin
```

##### 创建测试模块`demo1-maven-plugin-test`

使用idea创建，过程和`demo1-maven-plugin`过程类似，可以直接参考，创建好了，如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUKhbUD2Yh2Izib4aNQr2ScWOlJxXtXLUkI0qMIiaibqExgLpEBJ4ibnoEolTb4lbcOAWicicZsZFhnZcw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

修改`demo1-mavein-plugin-test/pom.xml`文件，加入下面内容：

```
<build>
    <plugins>
        <plugin>
            <groupId>com.javacode2018</groupId>
            <artifactId>demo1-maven-plugin</artifactId>
            <version>1.0-SNAPSHOT</version>
            <executions>
                <execution>
                    <id>demo1 plugin test</id>
                    <phase>pre-clean</phase>
                    <goals>
                        <goal>demo1</goal>
                    </goals>
                    <configuration>
                        <myBoolean>true</myBoolean>
                        <myInteger>30</myInteger>
                        <myFile>${project.basedir}</myFile>
                        <myColor>BLUE</myColor>
                        <myArray>
                            <array>maven</array>
                            <array>spring</array>
                            <array>mybatis</array>
                            <array>springboot</array>
                            <array>springcloud</array>
                        </myArray>
                        <myList>
                            <list>30</list>
                            <list>35</list>
                        </myList>
                        <myMap>
                            <name>路人甲Java</name>
                            <age>30</age>
                        </myMap>
                        <myProperties>
                            <property>
                                <name>name</name>
                                <value>javacode2018</value>
                            </property>
                            <property>
                                <name>age</name>
                                <value>30</value>
                            </property>
                        </myProperties>
                        <person>
                            <name>路人甲Java</name>
                            <age>32</age>
                        </person>
                    </configuration>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

上面是将生命周期的`pre-clean`阶段绑定插件`demo1-maven-plugin`的`demo1`目标，并且设置了`demo1`目标所需要的所有参数的值。

##### 验证效果

在`maven-chat10/pom.xml`所在目录执行：

```
D:\code\IdeaProjects\maven-chat10>mvn pre-clean -pl :demo1-maven-plugin-test -Dsayhi.greeting="欢迎和【路人甲Java】一起学习Maven!"
[INFO] Scanning for projects...
[INFO]
[INFO] --------------< com.javacode2018:demo1-maven-plugin-test >--------------
[INFO] Building demo1-maven-plugin-test 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- demo1-maven-plugin:1.0-SNAPSHOT:demo1 (demo1 plugin test) @ demo1-maven-plugin-test ---
[INFO] hello my first maven plugin!
[INFO] greeting:欢迎和【路人甲Java】一起学习Maven!
[INFO] myBoolean:true
[INFO] myInteger:30
[INFO] myFile:D:\code\IdeaProjects\maven-chat10\demo1-maven-plugin-test
[INFO] myColor:BLUE
[INFO] myArray:[Ljava.lang.String;@7bf9b098
[INFO] myList:[30, 35]
[INFO] myMap:{age=30, name=路人甲Java}
[INFO] myProperties:{age=30, name=javacode2018}
[INFO] person:Person{name='路人甲Java', age=32}
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  0.517 s
[INFO] Finished at: 2019-11-26T15:42:53+08:00
[INFO] ------------------------------------------------------------------------
```

### 插件前缀

在案例1中，我们使用下面命令调用的插件：

```
mvn com.javacode2018:demo1-maven-plugin:demo1
```

这种是采用下面这种格式：

```
mvn 插件groupId:插件artifactId[:插件版本]:插件目标名称
```

> 命令中插件版本是可以省略的，maven会自动找到这个插件最新的版本运行，不过最好我们不要省略版本号，每个版本的插件功能可能不一样，为了保证任何情况下运行效果的一致性，强烈建议指定版本号。

上面执行插件需要插件的坐标信息，一长串比较麻烦，maven也为了我们使用插件方便，提供了插件前缀来帮我们解决这个问题。

#### 自定义插件前缀的使用

##### 设置自定义插件的artifactId

自定义插件的`artifactId`满足下面的格式：

```
xxx-maven-plugin
```

如果采用这种格式的maven会自动将`xxx`指定为插件的前缀，其他格式也可以，不过此处我们只说这种格式，这个是最常用的格式。

如我们上面的`demo1-maven-plugin`插件，他的前缀就是`demo1`。

当我们配置了插件前缀，可以插件前缀来调用插件的目标了，命令如下：

```
mvn 插件前缀:插件目标
```

maven是如何通过插件前缀找到具体的插件的呢？

maven默认会在仓库`"org.apache.maven.plugins" 和 "org.codehaus.mojo"`2个位置查找插件，比如：

```
mvn clean:help
```

这个是调用`maven-clean-plugin`插件的`help`目标，`maven-clean-plugin`的前缀就是`clean`，他的`groupId`是`org.apache.maven.plugins`，所以能够直接找到。

但是我们自己定义的插件，如果也让maven能够找到，需要下面的配置。

##### 在`~/.m2/settings.xml`中配置自定义插件组

在`pluginGroups`中加入自定义的插件组`groupId`，如：

```
<pluginGroup>com.javacode2018</pluginGroup>
```

这样当我们通过前缀调用插件的时候，maven除了会在2个默认的组中查找，还会在这些自定义的插件组中找，一般情况下我们自定义的插件通常使用同样的`groupId`。

##### 使用插件前缀调用插件

```
mvn 插件前缀:插件目标
```

#### 案例3

在`~/.m2/settings.xml`中加入下面配置：

```
<pluginGroup>com.javacode2018</pluginGroup>
```

在`maven-chat10/pom.xml`所在目录执行：

```
mvn demo1:demo1 -pl demo1-maven-plugin-test
```

效果如下：

```
D:\code\IdeaProjects\maven-chat10>mvn demo1:demo1 -pl demo1-maven-plugin-test
[INFO] Scanning for projects...
[INFO]
[INFO] --------------< com.javacode2018:demo1-maven-plugin-test >--------------
[INFO] Building demo1-maven-plugin-test 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- demo1-maven-plugin:1.0-SNAPSHOT:demo1 (default-cli) @ demo1-maven-plugin-test ---
[INFO] hello my first maven plugin!
[INFO] greeting:Hello World!
[INFO] myBoolean:false
[INFO] myInteger:null
[INFO] myFile:null
[INFO] myColor:null
[INFO] myArray:null
[INFO] myList:null
[INFO] myMap:null
[INFO] myProperties:null
[INFO] person:null
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  0.786 s
[INFO] Finished at: 2019-11-26T16:10:41+08:00
[INFO] ------------------------------------------------------------------------
```

上面直接通过插件的前缀来调用插件的功能了，是不是很爽！

### 手动实现打包之后自动运行的插件

#### 实现思路

##### 1、将目标构件打包为可以执行jar包到target目录

maven中将构件打包为可以执行的jar的插件，maven已经帮我们提供了，如下：

```
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-shade-plugin</artifactId>
    <version>3.2.1</version>
    <executions>
        <execution>
            <phase>package</phase>
            <goals>
                <goal>shade</goal>
            </goals>
            <configuration>
                <transformers>
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                        <mainClass>启动类完整路径</mainClass>
                    </transformer>
                </transformers>
            </configuration>
        </execution>
    </executions>
</plugin>
```

上面使用到了maven官方提供的一个打包的插件，可以将构件打包为可以直接运行的jar包。

##### 2、自定义一个插件，然后执行上面打包好的插件

插件中需要通过java命令调用打包好的jar包，然后运行。

#### 具体实现如下

##### 创建自定义目标类

`demo1-maven-plugin`中创建一个插件目标类，如下：

```
package com.javacode2018;

import org.apache.maven.plugin.AbstractMojo;
import org.apache.maven.plugin.MojoExecutionException;
import org.apache.maven.plugin.MojoFailureException;
import org.apache.maven.plugins.annotations.Execute;
import org.apache.maven.plugins.annotations.LifecyclePhase;
import org.apache.maven.plugins.annotations.Mojo;
import org.apache.maven.plugins.annotations.Parameter;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

/**
 * 工作10年的前阿里P7分享Java、算法、数据库方面的技术干货！坚信用技术改变命运，让家人过上更体面的生活！喜欢的请关注公众号：路人甲Java
 */
@Mojo(name = "run", defaultPhase = LifecyclePhase.PACKAGE)
@Execute(phase = LifecyclePhase.PACKAGE)
public class RunMojo extends AbstractMojo {

    /**
     * 打包好的构件的路径
     */
    @Parameter(defaultValue = "${project.build.directory}\\${project.artifactId}-${project.version}.jar")
    private String jarPath;

    @Override
    public void execute() throws MojoExecutionException, MojoFailureException {
        try {
            this.getLog().info("Started:" + this.jarPath);
            ProcessBuilder builder = new ProcessBuilder("java", "-jar", this.jarPath);
            final Process process = builder.start();

            new Thread(() -> {
                BufferedReader reader = new BufferedReader(new InputStreamReader(process.getInputStream()));
                try {
                    String s;
                    while ((s = reader.readLine()) != null) {
                        System.out.println(s);
                    }
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }).start();
            Runtime.getRuntime().addShutdownHook(new Thread() {
                @Override
                public void run() {
                    RunMojo.this.getLog().info("Destroying...");
                    process.destroy();
                    RunMojo.this.getLog().info("Shutdown hook finished.");
                }
            });

            process.waitFor();
            this.getLog().info("Finished.");
        } catch (Exception e) {
            this.getLog().warn(e);
        }
    }
}
```

> 上面这个插件目标的名称为`run`
>
> 注意这个类上面多了一个注解`@Execute`，这个注解可以配置这个目标执行之前可以先执行的`生命周期的阶段`或者需要先执行的`插件目标`。
>
> 上面配置的是`phase = LifecyclePhase.PACKAGE`，也就是说当我们运行上面`run`目标的时候，会先执行构件的`package`阶段，也就是先执行项目的打包阶段，打包完成之后才会执行`run`目标。

##### 安装插件到本地仓库

`maven-chat10/pom.xml`所在目录运行：

```
mvn clean install -pl :demo1-maven-plugin
```

##### 创建测试模块`demo1-maven-plugin-run`

使用idea创建，过程和`demo1-maven-plugin`过程类似，可以直接参考，创建好了，如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUKhbUD2Yh2Izib4aNQr2ScF28DzsEx7eXqRfv7RBdibSdHE88qs0z3AfKSHxpZ9lnEqz5mSHicVrGA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

##### 创建`com.javacode2018.Demo`类

在demo1-maven-plugin-run\src\main\java创建下面类：

```
package com.javacode2018;

import java.util.Calendar;
import java.util.concurrent.TimeUnit;

/**
 * 工作10年的前阿里P7分享Java、算法、数据库方面的技术干货！坚信用技术改变命运，让家人过上更体面的生活！喜欢的请关注公众号：路人甲Java
 */
public class Demo1 {
    public static void main(String[] args) throws InterruptedException {
        for (int i = 0; i < 1000; i++) {
            System.out.println(Calendar.getInstance().getTime() + ":" + i);
            TimeUnit.SECONDS.sleep(1);
        }
    }
}
```

##### 修改`demo1-maven-plugin-run/pom.xml`，如下

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.javacode2018</groupId>
    <artifactId>demo1-maven-plugin-run</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <!-- 配置maven编译的时候采用的编译器版本 -->
        <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
        <!-- 指定源代码是什么版本的，如果源码和这个版本不符将报错，maven中执行编译的时候会用到这个配置，默认是1.5，这个相当于javac命令后面的-source参数 -->
        <maven.compiler.source>1.8</maven.compiler.source>
        <!-- 该命令用于指定生成的class文件将保证和哪个版本的虚拟机进行兼容，maven中执行编译的时候会用到这个配置，默认是1.5，这个相当于javac命令后面的-target参数 -->
        <maven.compiler.target>1.8</maven.compiler.target>
    </properties>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>3.2.1</version>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>shade</goal>
                        </goals>
                        <configuration>
                            <transformers>
                                <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                                    <mainClass>com.javacode2018.Demo1</mainClass>
                                </transformer>
                            </transformers>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
</project>
```

##### 验证效果见证奇迹的时刻

在`maven-chat10/pom.xml`所在目录执行下面的命令：

```
mvn clean demo1:run -pl demo1-maven-plugin-run
```

输出如下：

```
D:\code\IdeaProjects\maven-chat10>mvn clean demo1:run -pl demo1-maven-plugin-run
[INFO] Scanning for projects...
[INFO]
[INFO] --------------< com.javacode2018:demo1-maven-plugin-run >---------------
[INFO] Building demo1-maven-plugin-run 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ demo1-maven-plugin-run ---
[INFO] Deleting D:\code\IdeaProjects\maven-chat10\demo1-maven-plugin-run\target
[INFO]
[INFO] >>> demo1-maven-plugin:1.0-SNAPSHOT:run (default-cli) > package @ demo1-maven-plugin-run >>>
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ demo1-maven-plugin-run ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ demo1-maven-plugin-run ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 1 source file to D:\code\IdeaProjects\maven-chat10\demo1-maven-plugin-run\target\classes
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ demo1-maven-plugin-run ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory D:\code\IdeaProjects\maven-chat10\demo1-maven-plugin-run\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ demo1-maven-plugin-run ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ demo1-maven-plugin-run ---
[INFO] No tests to run.
[INFO]
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ demo1-maven-plugin-run ---
[INFO] Building jar: D:\code\IdeaProjects\maven-chat10\demo1-maven-plugin-run\target\demo1-maven-plugin-run-1.0-SNAPSHOT.jar
[INFO]
[INFO] --- maven-shade-plugin:3.2.1:shade (default) @ demo1-maven-plugin-run ---
[INFO] Replacing original artifact with shaded artifact.
[INFO] Replacing D:\code\IdeaProjects\maven-chat10\demo1-maven-plugin-run\target\demo1-maven-plugin-run-1.0-SNAPSHOT.jar with D:\code\IdeaProjects\maven-chat10\demo1-maven-plugin-run\target\demo1-maven-plugin-run-1.0-SNAPSHOT-shaded.jar
[INFO]
[INFO] <<< demo1-maven-plugin:1.0-SNAPSHOT:run (default-cli) < package @ demo1-maven-plugin-run <<<
[INFO]
[INFO]
[INFO] --- demo1-maven-plugin:1.0-SNAPSHOT:run (default-cli) @ demo1-maven-plugin-run ---
[INFO] Started:D:\code\IdeaProjects\maven-chat10\demo1-maven-plugin-run\target\demo1-maven-plugin-run-1.0-SNAPSHOT.jar
Tue Nov 26 17:24:47 CST 2019:0
Tue Nov 26 17:24:48 CST 2019:1
Tue Nov 26 17:24:49 CST 2019:2
Tue Nov 26 17:24:50 CST 2019:3
Tue Nov 26 17:24:51 CST 2019:4
Tue Nov 26 17:24:52 CST 2019:5
Tue Nov 26 17:24:53 CST 2019:6
Tue Nov 26 17:24:54 CST 2019:7
Tue Nov 26 17:24:55 CST 2019:8
Tue Nov 26 17:24:56 CST 2019:9
Tue Nov 26 17:24:57 CST 2019:10
Tue Nov 26 17:24:58 CST 2019:11
Tue Nov 26 17:24:59 CST 2019:12
Tue Nov 26 17:25:00 CST 2019:13
```

**是不是打包+运行很简单，一行命令就搞定了。**

后面我们需要学的springboot中也是一个maven命令就可以启动springboot项目，这个原理和我们上面的类似，是spring官方他们自己开发了一款maven插件，可以直接运行springboot项目，后面学习springboot的时候，我们会详细介绍。

### 总结

**本文的内容起到一个抛砖引玉的作用，大家如果有兴趣，可以去写很多更好的maven插件玩玩，maven默认提供了很多优秀的插件，大家可以去看他们的源码，借鉴他们的设计思路，开发出自己喜欢的插件使用，有问题的可以加我微信【itsoku】或者留言交流！**

**maven系列到此已经结束了，10篇如果都能够坚持看完，大家已经成为一等一的高手了。**

**后面将进行mybatis、springboot、springcloud系列，所有系列的目标都是让大家掌握从入门到高级开发所需要的所有技能。**