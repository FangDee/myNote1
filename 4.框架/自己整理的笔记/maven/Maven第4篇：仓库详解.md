# Maven第4篇：仓库详解

路人甲Java [程序员路人](javascript:void(0);) *2022-03-10 20:20*

**大家好，我是路人~~~**

**点击下方卡片，关注我，java干货及时送达**

![img](http://mmbiz.qpic.cn/mmbiz_png/kvBL17F8Dr1MFuX9tnHDHzic6yBpLSiaTibZLKcia9tHwm7aFueNjgoAIibdqXibicAUEia87xvDN77VhfDKbo59BJCwlw/0?wx_fmt=png)

**程序员路人**

专注Java相关技术：SSM、Spring全家桶、微服务、MySQL、MyCat、集群、分布式、中间件、Linux、网络、多线程，偶尔讲点运维Jenkins、Nexus、Docker、ELK，偶尔分享些技术干货，致力于Java全栈开发！

1篇原创内容



公众号

**这是 Maven 第 4 篇，点击上方卡片，即可翻阅前面章节。**

### 环境

1. maven3.6.1
2. 开发工具idea
3. jdk1.8

### 本篇内容

1. maven是如何找到我们依赖的jar的
2. 什么是仓库
3. 仓库的分类
4. 各种类型仓库详解
5. maven中远程仓库配置详解
6. 关于构件版本问题说明
7. 构件文件的布局

在maven出现之前，项目中用到第三方jar包时，我们会把这些依赖的jar包拷贝到项目的lib目录中，如果我们开发了多个项目，这些jar包在各个项目目录中都有一份拷贝，这存在的一些问题：

1. 不方便jar包的管理，比如jar的升级、删除等操作
2. 对磁盘空间的重复占用

主要还是不方便jar包的管理，maven很好的解决了这些问题，我们来看看maven管理依赖jar包的效果。

我来提几个问题，带着问题来看本篇内容

1. maven是如何将依赖的jar引入项目的？
2. maven项目中依赖的jar是从哪里获取的？
3. 我们如何掌控这些jar的获取方式？
4. maven是如何组织管理构件的？

### 先来看一下maven项目案例

创建一个maven项目，打开idea，点击`File->New->Project`，选择`Maven`，如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06CUhvEzIVFvkgibRJE1zOxpIiazaM5gU1B4b8MkBicFcHHWzQ2EcCDDXx3ic0iczPX6q52J4DeUNBe5anw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击`Next`，输入项目坐标信息，如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06CUhvEzIVFvkgibRJE1zOxpIDkYm8rm8sA01Lk6SicX8s857BKcCd2PZjzdJQndYb0Ol2Bz5evMCYCA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击`Next`，输入Project name 为`maven-chat03`，如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06CUhvEzIVFvkgibRJE1zOxpIvcUYdhOdROCuo5pOEWz0UU1gqaVppSUsqaGwta1plwZkMzJACicQRSQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击`Finish`，创建成功，如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06CUhvEzIVFvkgibRJE1zOxpIpJRN3kgXErhPfXJiatbEdCIUibfu6a6v74Ilq72x3h7vW8RoDkjwHPag/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

我们看一下这个项目占用的磁盘大小：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06CUhvEzIVFvkgibRJE1zOxpIcfW1ExSUQ3z6A6Ib69Heq2b37MeMYlm4HhrWYH0AX9E58vxOkW42nA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

占用 29.9KB，下面我们在pom.xml中引入fastjson的依赖：

```
<dependencies>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>fastjson</artifactId>
        <version>1.2.62</version>
    </dependency>
</dependencies>
```

创建一个Demo.java，如下

```
package com.javacode2018;

import com.alibaba.fastjson.JSON;

public class Demo1 {
    public static void main(String[] args) {
        System.out.println(JSON.class);
    }
}
```

运行一下Demo1，输出：

```
class com.alibaba.fastjson.JSON
```

说明fastjson在项目中起效了，我们再来看一下项目的大小

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06CUhvEzIVFvkgibRJE1zOxpIqAO2icjpzd5BaqZLx6q8mcX5T9aiamZjichR9Cvico6l4fd87XOpDFdoww/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

上面一次是39kb，这次是49kb，大小增加了10KB，我们来看一下fastjson.jar的大小

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06CUhvEzIVFvkgibRJE1zOxpIN0RBia6jPJamTVoEchmiaUDEvUIngZOy2oM4afhvD8FpWZ98V1GBDDgg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

这个jar包643kb，但是项目才49kb，这说明了什么？

说明了项目目录中没有包含这个jar包，只是对这个jar包做了一个引用。

如果系统中有很多项目，都采用同一个maven来引用依赖的jar包，那么这些jar只会在磁盘中存储一份，这些jar可以被其他所有的maven项目共享，项目只需要在pom.xml中通过maven坐标的方式来对这些jar进行引用，而不用再拷贝至项目中，若对jar包进行删除、升级版本直接修改pom.xml就可以了，非常方便。

**结论：maven采用引用的方式将依赖的jar引入进来，不对真实的jar进行拷贝，但是打包的时候，运行需要用到的jar都会被拷贝到安装包中。**

### Maven寻找依赖的jar

我们可以看到，当我们项目中需要使用某些jar时，只需要将这些jar的maven坐标添加到pom.xml中就可以了，这背后maven是如何找到这些jar的呢？

maven官方为我们提供了一个站点，这个站点中存放了很多第三方常用的构建（jar、war、zip、pom等等），当我们需要使用这些构件时，只需将其坐标加入到pom.xml中，此时maven会自动将这些构建下载到本地一个目录，然后进行自动引用。

上面提到的maven站点，我们叫做maven中央仓库，本地目录叫做本地仓库。

默认情况下，当项目中引入依赖的jar包时，maven先在本地仓库检索jar，若本地仓库没有，maven再去从中央仓库寻找，然后从中央仓库中将依赖的构件下载到本地仓库，然后才可以使用，如果2个地方都没有，maven会报错。

下面我们来看看什么是仓库？

### Maven 仓库

在 Maven 中，任何一个依赖、插件或者项目构建的输出，都可以称之为构件。

在 Maven 中，仓库是一个位置，这个位置是用来存放各种第三方构件的，所有maven项目可以共享这个仓库中的构件。

Maven 仓库能帮助我们管理构件（主要是jar包），它就是放置所有jar文件（jar、war、zip、pom等等）的地方。

#### 仓库的分类

主要分为2大类：

1. **本地仓库**
2. **远程仓库**

**而远程仓库又分为：中央仓库、私服、其他公共远程仓库**

当maven根据坐标寻找构件的时候，会首先查看本地仓库，如果本地仓库存在，则直接使用；如果本地不存在，maven会去远程仓库中查找，如果找到了，会将其下载到本地仓库中进行使用，如果本地和远程仓库都没有找到构件，maven会报错，构件只有在本地仓库中存在了，才能够被maven项目使用。

#### 本地仓库

默认情况下，maven本地仓库默认地址是`~/.m2/respository`目录，这个默认我们也可以在`~/.m2/settings.xml`文件中进行修改：

```
<localRepository>本地仓库地址</localRepository>
```

当我们使用maven的时候，依赖的构件都会从远程仓库下载到本地仓库目录中。

Maven 的本地仓库，在安装 Maven 后并不会创建，当我们执行第一条 maven 命令的时候本地仓库才会创建，此时会从远程仓库下载构建到本地仓库给maven项目使用。

需要我们注意，默认情况下，`~/.m2/settings.xml`这个文件是不存在的（`~`是指用户目录，前面的文章中有介绍过，此处不再做说明），我们需要从Maven安装目录中拷贝`conf/settings.xml`文件，将`M2_HOME/conf/settings.xml`拷贝到`~/.m2`目录中，然后对`~/.m2/settings.xml`进行编辑，`M2_HOME/config/settings.xml`这个文件其实也是可以使用的，不过我们不建议直接使用，这个修改可能会影响其他所有使用者，还有修改了这个文件，也不利于以后maven的升级，如果我们使用`~/.m2/settings.xml`，而maven安装目录中的配置不动，升级的时候只需要替换一下安装包就好了，所以我们建议将maven安装目录中的`settings.xml`拷贝到`~/.m2`中进行编辑，这个是用户级别的，只会影响当前用户。

#### 远程仓库

最开始我们使用maven的时候，本地仓库中的构件是空的，此时maven必须要提供一种功能，要能够从外部获取这些构件，这个外部就是所谓的远程仓库，远程仓库可以有多个，当本地仓库找不到构件时，可以去远程仓库找，然后放置到本地仓库中进行使用。

#### 中央仓库

由于maven刚安装好的时候，本地仓库是空的，此时我们什么都没有配置，去执行maven命令的时候，我们会看到maven默认执行了一些下载操作，这个下载地址就是中央仓库的地址，这个地址是maven社区为我们提供的，是maven内置的一个默认的远程仓库地址，不需要用户去配置。

这个地址在maven安装包的什么地方呢？

我们使用的是3.6.1，在下面这个位置

```
apache-maven-3.6.1\lib\maven-model-builder-3.6.1.jar\org\apache\maven\model\pom-4.0.0.xml
```

在pom-4.0.0.xml中，如下：

```
<repositories>
    <repository>
      <id>central</id>
      <name>Central Repository</name>
      <url>https://repo.maven.apache.org/maven2</url>
      <layout>default</layout>
      <snapshots>
        <enabled>false</enabled>
      </snapshots>
    </repository>
  </repositories>
```

就是：

```
https://repo.maven.apache.org/maven2
```

可以去访问一下，如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06CUhvEzIVFvkgibRJE1zOxpIZDes2laMmDokvXXSRKEDe4rmIlKov5VX2EkOv2TzFpOD4qLjCkMicmw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

上面站点中包含了很多常用的构建。

中央仓库有几个特点：

1. 中央仓库是由maven官方社区提供给大家使用的
2. 不需要我们手动去配置，maven内部集成好了
3. 使用中央仓库时，机器必须是联网状态，需要可以访问中央仓库的地址

中央仓库还为我们提供了一个检索构件的站点：

```
https://search.maven.org/
```

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06CUhvEzIVFvkgibRJE1zOxpI1DOD7tibJXMb5fibdl3GB9tMCCic1ZEKxSoxngNVJIZChBviaynqPLiay4Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

非常方便我们查找需要依赖的构件，大家可以去体验一下。

中央仓库中包含了这个世界上大多数流行的开源java构件，基本上所有的jave开发者都会使用这个仓库，一般我们需要的第三方构件在这里都可以找到。

#### 私服

私服也是远程仓库中的一种，我们为什么需要私服呢？

如果我们一个团队中有几百个人在开发一些项目，都是采用maven的方式来组织项目，那么我们每个人都需要从远程仓库中把需要依赖的构件下载到本地仓库，这对公司的网络要求也比较高，为了节省这个宽带和加快下载速度，我们在公司内部局域网内部可以架设一台服务器，这台服务器起到一个代理的作用，公司里面的所有开发者去访问这个服务器，这台服务器将需要的构建返回给我们，如果这台服务器中也没有我们需要的构建，那么这个代理服务器会去远程仓库中查找，然后将其先下载到代理服务器中，然后再返回给开发者本地的仓库。

还有公司内部有很多项目之间会相互依赖，你可能是架构组的，你需要开发一些jar包给其他组使用，此时，我们可以将自己jar发布到私服中给其他同事使用，如果没有私服，可能需要我们手动发给别人或者上传到共享机器中，不过管理起来不是很方便。

**总体上来说私服有以下好处：**

1. 加速maven构件的下载速度
2. 节省宽带
3. 方便部署自己的构件以供他人使用
4. 提高maven的稳定性，中央仓库需要本机能够访问外网，而如果采用私服的方式，只需要本机可以访问内网私服就可以了

**关于私服，后面会专门有一篇文章会做详细介绍。**

#### 其他远程仓库

中央仓库是在国外的，访问速度不是特别快，所以有很多比较大的公司做了一些好事，自己搭建了一些maven仓库服务器，公开出来给其他开发者使用，比如像阿里、网易等等，他们对外提供了一些maven仓库给全球开发者使用，在国内的访问速度相对于maven中央仓库来说还是快了不少。

还有一些公司比较牛，只在自己公开的仓库中发布构件，这种情况如果要使用他们的构件时，需要去访问他们提供的远程仓库地址。

#### 构建文件的布局

我们来看一下构件在仓库的文件结构中是如何组成的？

这块我们以本地仓库来做说明，远程仓库中组织构件的方式和本地仓库是一样的，以fastjson在本地仓库中的信息为例来做说明，如下：

```
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.62</version>
</dependency>
```

上面是fastjson 1.2.62这个jar，我们看一下这个jar在本地仓库中的位置，如下图

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06CUhvEzIVFvkgibRJE1zOxpIXiakfG0Qyxn6Z397vlxHtaE5Xmc0sUV6KnFqAT7Z4GK9sv5eiagg8Aeg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

fastjson这个jar的地址是：

```
~\.m2\repository\com\alibaba\fastjson\1.2.62\fastjson-1.2.62.jar
```

`~\.m2\repository\`是仓库的目录，所有本地构件都位于该目录中，我们主要看一下后面的部分，是怎么构成的。

构件所在目录的构成如下：

```
groupId+"."+artifactId+"."+版本号
```

通过上面获取一个字符串，字符串由`groupId、artifactId、版本号`之间用`.`连接，然后将这个字符串中的`.`替换为文件目录分隔符然后创建多级目录。

而构件文件名称的组成如下：

```
[artifactId][-verion][-classifier].[type]
```

上面的fastjson-1.2.62.jar信息如下：

```
artifactId为fastjson
version为1.2.62
classifier为空
type没有指定，默认为jar
```

所以构件文件名称为`fastjson-1.2.62.jar`。

### 关于构件版本问题

平时我们开发项目的时候，打包测试，或者将自己开发的构建提供给他人使用时，中间我们反反复复的打包测试，会给使用方提供很多不稳定的版本，最终经过同事和测试反复验证修改，我们会发布一个稳定的版本。

在发布稳定版本之前，会有很多个不稳定的测试版本，我们版本我们称为快照版本，用SNAPSHOT表示，回头去看看本文开头搭建的`maven-cha03`的pom.xml文件，默认是快照版本的，如下：

```
<version>1.0-SNAPSHOT</version>
```

version以`-SNAPSHOT`结尾的，表示这是一个不稳定的版本，这个版本我们最好只在公司内部测试的时候使用，最终发布的时候，我们需要将`-SNAPSHOT`去掉，然后发布一个稳定的版本，表示这个版本是稳定的，可以直接使用，这种稳定的版本我们叫做`release`版本。

当我们想控制构件获取的远程地址时，我们该怎么做呢？此时需要使用远程仓库的配置功能。

### Maven中远程仓库的配置

此处我们讲解2种方式。

#### 方式1

##### pom.xml中配置远程仓库，语法如下：

```
<project>
    <repositories>
        <repository>
            <id>aliyun-releases</id>
            <url>https://maven.aliyun.com/repository/public</url>
            <releases>
                <enabled>true</enabled>
            </releases>
            <snapshots>
                <enabled>false</enabled>
            </snapshots>
        </repository>
    </repositories>
</project>
```

在repositories元素下，可以使用repository子元素声明一个或者多个远程仓库。

repository元素说明：

- id：远程仓库的一个标识，中央仓库的id是`central`，所以添加远程仓库的时候，id不要和中央仓库的id重复，会把中央仓库的覆盖掉
- url：远程仓库地址
- releases：主要用来配置是否需要从这个远程仓库下载稳定版本构建
- snapshots：主要用来配置是否需要从这个远程仓库下载快照版本构建

releases和snapshots中有个`enabled`属性，是个boolean值，默认为true，表示是否需要从这个远程仓库中下载稳定版本或者快照版本的构建，一般使用第三方的仓库，都是下载稳定版本的构建。

快照版本的构建以`-SNAPSHOT`结尾，稳定版没有这个标识。

##### 示例

来感受一下pom方式配置远程仓库的效果。

文本编辑器打开`maven-chat03/pom.xml`，将下面内容贴进去：

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.javacode2018</groupId>
    <artifactId>maven-chat03</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.62</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>2.2.1.RELEASE</version>
        </dependency>
    </dependencies>

    <repositories>
        <repository>
            <id>aliyun-releases</id>
            <url>https://maven.aliyun.com/repository/public</url>
            <releases>
                <enabled>true</enabled>
            </releases>
            <snapshots>
                <enabled>false</enabled>
            </snapshots>
        </repository>
    </repositories>

</project>
```

> 上面我们配置了一个远程仓库，地址是阿里云的maven仓库地址，`releases`的`enabled`为`true`，`snapshots`的`enabled`为`false`，表示这个远程仓库我们只允许下载稳定版本的构件，而不能从这个仓库中下载快照版本的构建。

删除本地仓库中以下几个目录：

```
~\.m2\repository\org\springframework
~\.m2\repository\com\alibaba
```

maven-chat03项目目录中打开cmd运行：

```
mvn compile
```

输出如下：

```
D:\code\IdeaProjects\maven-chat03>mvn compile
[INFO] Scanning for projects...
[INFO]
[INFO] -------------------< com.javacode2018:maven-chat03 >--------------------
[INFO] Building maven-chat03 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
Downloading from aliyun-releases: https://maven.aliyun.com/repository/public/org/springframework/boot/spring-boot-starter-web/2.2.1.RELEASE/spring-boot-starter-web-2.2.1.RELEASE.pom
Downloaded from aliyun-releases: https://maven.aliyun.com/repository/public/org/springframework/boot/spring-boot-starter-web/2.2.1.RELEASE/spring-boot-starter-web-2.2.1.RELEASE.pom (3.3 kB at 5.1 kB/s)
Downloading from aliyun-releases: https://maven.aliyun.com/repository/public/org/springframework/boot/spring-boot-starters/2.2.1.RELEASE/spring-boot-starters-2.2.1.RELEASE.pom
Downloaded from aliyun-releases: https://maven.aliyun.com/repository/public/org/springframework/boot/spring-boot-starters/2.2.1.RELEASE/spring-boot-starters-2.2.1.RELEASE.pom (1.8 kB at 8.0 kB/s)
Downloading from aliyun-releases: https://maven.aliyun.com/repository/public/org/springframework/boot/spring-boot-parent/2.2.1.RELEASE/spring-boot-parent-2.2.1.RELEASE.pom
Downloaded from aliyun-releases: https://maven.aliyun.com/repository/public/org/springframework/boot/spring-boot-parent/2.2.1.RELEASE/spring-boot-parent-2.2.1.RELEASE.pom (1.8 kB at 8.6 kB/s)
Downloaded from aliyun-releases: https://maven.aliyun.com/repository/public/org/springframework/spring-expression/5.2.1.RELEASE/spring-expression-5.2.1.RELEASE.jar (282 kB at 82 kB/s)
Downloaded from aliyun-releases: https://maven.aliyun.com/repository/public/org/springframework/spring-webmvc/5.2.1.RELEASE/spring-webmvc-5.2.1.RELEASE.jar (946 kB at 219 kB/s)
Downloaded from aliyun-releases: https://maven.aliyun.com/repository/public/org/springframework/spring-beans/5.2.1.RELEASE/spring-beans-5.2.1.RELEASE.jar (684 kB at 56 kB/s)
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ maven-chat03 ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ maven-chat03 ---
[INFO] Changes detected - recompiling the module!
[WARNING] File encoding has not been set, using platform encoding GBK, i.e. build is platform dependent!
[INFO] Compiling 1 source file to D:\code\IdeaProjects\maven-chat03\target\classes
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  24.079 s
[INFO] Finished at: 2019-11-12T15:33:27+08:00
[INFO] ------------------------------------------------------------------------
```

输出中有很多`Downloaded from aliyun-releases`，`Downloaded from`后面跟的`aliyun-releases`就是上面我们在pom.xml中配置的远程仓库repository元素中的id，后面还可以看到很多下载地址，这个地址就是我们上面在pom.xml中指定的远程仓库的地址，可以看到项目中依赖的构建从我们指定的远程仓库中下载了。

pom中配置远程仓库的方式只对当前项目起效，如果我们需要对所有项目起效，我们可以下面的方式2，向下看。

#### 方式2

##### 镜像的方式

如果仓库X可以提供仓库Y所有的内容，那么我们就可以认为X是Y的一个镜像，通俗点说，可以从Y获取的构件都可以从他的镜像中进行获取。

可以采用镜像的方式配置远程仓库，镜像在`settings.xml`中进行配置，对所有使用该配置的maven项目起效，配置方式如下：

```
<mirror>
  <id>mirrorId</id>
  <mirrorOf>repositoryId</mirrorOf>
  <name>Human Readable Name for this Mirror.</name>
  <url>http://my.repository.com/repo/path</url>
</mirror>
```

mirrors元素下面可以有多个mirror元素，每个mirror元素表示一个远程镜像，元素说明：

- id：镜像的id，是一个标识
- name：镜像的名称，这个相当于一个描述信息，方便大家查看
- url：镜像对应的远程仓库的地址
- mirrorOf：指定哪些远程仓库的id使用这个镜像，这个对应pom.xml文件中repository元素的id，就是表示这个镜像是给哪些pom.xml文章中的远程仓库使用的，这里面需要列出远程仓库的id，多个之间用逗号隔开，`*`表示给所有远程仓库做镜像

这里主要对mirrorOf再做一下说明，上面我们在项目中定义远程仓库的时候，pom.xml文件的repository元素中有个id，这个id就是远程仓库的id，而mirrorOf就是用来配置哪些远程仓库会走这个镜像去下载构件。

mirrorOf的配置有以下几种:

```
<mirrorOf>*</mirrorOf> 
```

> 上面匹配所有远程仓库id，这些远程仓库都会走这个镜像下载构件

```
<mirrorOf>远程仓库1的id,远程仓库2的id</mirrorOf> 
```

> 上面匹配指定的仓库，这些指定的仓库会走这个镜像下载构件

```
<mirrorOf>*,! repo1</mirrorOf> 
```

> 上面匹配所有远程仓库，repo1除外，使用感叹号将仓库从匹配中移除。

需要注意镜像仓库完全屏蔽了被镜像的仓库，所以当镜像仓库无法使用的时候，maven是无法自动切换到被镜像的仓库的，此时下载构件会失败，这个需要了解。

##### 示例

将maven-chat03中的pom.xml修改为：

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.javacode2018</groupId>
    <artifactId>maven-chat03</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.62</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>2.2.1.RELEASE</version>
        </dependency>
    </dependencies>

</project>
```

修改~/.m2/settings.xml，加入镜像配置，如下：

```
<mirrors>
    <mirror>
        <id>mirror-aliyun-releases</id>
        <mirrorOf>*</mirrorOf>
        <name>阿里云maven镜像</name>
        <url>https://maven.aliyun.com/repository/public</url>
    </mirror>
</mirrors>
```

> 上面配置了一个阿里云的镜像，注意镜像的id是`mirror-aliyun-releases`
>
> 下面我们就来验证一下镜像的效果。

删除本地仓库中的以下几个目录：

```
~\.m2\repository\org\springframework
~\.m2\repository\com\alibaba
```

maven-chat03项目目录中打开cmd运行：

```
mvn compile
```

输出如下：

```
D:\code\IdeaProjects\maven-chat03>mvn compile
[INFO] Scanning for projects...
[INFO]
[INFO] -------------------< com.javacode2018:maven-chat03 >--------------------
[INFO] Building maven-chat03 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
Downloading from mirror-aliyun-releases: https://maven.aliyun.com/repository/public/com/alibaba/fastjson/1.2.62/fastjson-1.2.62.pom
Downloaded from mirror-aliyun-releases: https://maven.aliyun.com/repository/public/com/alibaba/fastjson/1.2.62/fastjson-1.2.62.pom (9.7 kB at 17 kB/s)
Downloading from mirror-aliyun-releases: https://maven.aliyun.com/repository/public/org/springframework/boot/spring-boot-starter-web/2.2.1.RELEASE/spring-boot-starter-web-2.2.1.RELEASE.pom
Downloaded from mirror-aliyun-releases: https://maven.aliyun.com/repository/public/org/springframework/boot/spring-boot-starter-web/2.2.1.RELEASE/spring-boot-starter-web-2.2.1.RELEASE.pom (3.3 kB at 15 kB/s)
Downloading from mirror-aliyun-releases: https://maven.aliyun.com/repository/public/org/springframework/boot/spring-boot-starters/2.2.1.RELEASE/spring-boot-starters-2.2.1.RELEASE.pom
Downloaded from mirror-aliyun-releases: https://maven.aliyun.com/repository/public/org/springframework/boot/spring-boot-starters/2.2.1.RELEASE/spring-boot-starters-2.2.1.RELEASE.pom (1.8 kB at 8.3 kB/s)
Downloading from mirror-aliyun-releases: https://maven.aliyun.com/repository/public/org/springframework/boot/spring-boot-parent/2.2.1.RELEASE/spring-boot-parent-2.2.1.RELEASE.pom
Downloaded from mirror-aliyun-releases: https://maven.aliyun.com/repository/public/org/springframework/boot/spring-boot-parent/2.2.1.RELEASE/spring-boot-parent-2.2.1.RELEASE.pom (1.8 kB at 8.2 kB/s)
Downloading from mirror-aliyun-releases: https://maven.aliyun.com/repository/public/org/springframework/boot/spring-boot-dependencies/2.2.1.RELEASE/spring-boot-dependencies-2.2.1.RELEASE.pom
Downloaded from mirror-aliyun-releases: https://maven.aliyun.com/repository/public/org/springframework/boot/spring-boot-dependencies/2.2.1.RELEASE/spring-boot-dependencies-2.2.1.RELEASE.pom (127 kB at 311 kB/s)
Downloading from mirror-aliyun-releases: https://maven.aliyun.com/repository/public/org/springframework/spring-framework-bom/5.2.1.RELEASE/spring-framework-bom-5.2.1.RELEASE.pom
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ maven-chat03 ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ maven-chat03 ---
[INFO] Nothing to compile - all classes are up to date
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  15.967 s
[INFO] Finished at: 2019-11-12T16:44:57+08:00
[INFO] ------------------------------------------------------------------------
```

上面复制了部分内容，大家仔细看一下`Downloaded from`后面显示的是`mirror-aliyun-releases`，这个和settings.xml中镜像的id一致，表示我们配置的镜像起效了，所有依赖的构建都从镜像来获取了。

**关于镜像一个比较常用的用法是结合私服一起使用，由于私服可以代理所有远程仓库（包含中央仓库），因此对于maven用来来说，只需通过访问一个私服就可以间接访问所有外部远程仓库了，这块后面我们会在私服中做具体说明。**