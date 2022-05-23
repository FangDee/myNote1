# Maven第7篇：聚合、继承、单继承问题详解，必备技能！

路人甲Java [程序员路人](javascript:void(0);) *2022-03-16 20:20*

**大家好，我是路人~~~**

**点击下方卡片，关注我，java干货及时送达**

![img](http://mmbiz.qpic.cn/mmbiz_png/kvBL17F8Dr1MFuX9tnHDHzic6yBpLSiaTibZLKcia9tHwm7aFueNjgoAIibdqXibicAUEia87xvDN77VhfDKbo59BJCwlw/0?wx_fmt=png)

**程序员路人**

专注Java相关技术：SSM、Spring全家桶、微服务、MySQL、MyCat、集群、分布式、中间件、Linux、网络、多线程，偶尔讲点运维Jenkins、Nexus、Docker、ELK，偶尔分享些技术干货，致力于Java全栈开发！

1篇原创内容



公众号

**这是 Maven 第 7 篇，点击上方卡片，即可翻阅前面章节。**

### 本篇内容

1. maven中聚合详解
2. maven中继承详解
3. pom.xml中parent元素的使用详解
4. pom.xml中dependencyManagement元素使用详解
5. pom.xml中pluginManagement元素使用详解
6. 单继承存在的问题及解决方案详解（springboot,springcloud中会常用）
7. 解答上一篇遗留的2个问题

### 聚合

#### 原始需求

我们需要使用java做一个电商网站，涉及到：pc端网站、h5微站、移动端接口部分，那么我们可以使用maven创建3个项目用于这3块业务的开发，3个项目名称如下：

```
javacode2018-pc
javacode2018-h5
javacode2018-api
```

这3个项目的groupId都是`com.javacode2018`，artifactId取上面的，我们使用maven来搭建项目结构。

#### 实现

##### 创建第一个javacode2018-pc项目

打开idea，点击`File->New->Project`，选择`Maven`，如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iayBT8yEpNfIIl2JVTzMqicJwwicW4W8ER6QAQ8JJbd4HKBKnwEXicGYvBA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击`Next`，输入项目坐标信息，如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iakN0Tz2KThZZV6wRa1EJHHNiaia5sLzbiaJEZ6skLZk2qCThIhFd1TsCtA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击`Next`，输入Project name 为`javacode2018-pc`，如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iaGdGJWq5ibxgZxyopJNYjO1ia1In5NIzqreflUpPlXTMT87gxibXFQUkKw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击`Finish`，创建成功，如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iaiaicV3VmGJTXt5XP7nf1BfamxzH34EbG394YOcA6oXWulXlDupJd8Uyg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

配置一下idea的maven环境，点击`File->Settings`，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iaqb9pP653CN9XWqWPpkzHmWJJtUYzlzsoDtNB4JNPVic7gicFefVBTSDA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击上面的`OK`完成配置。

大家再按照上面一样的操作，分别创建另外2个项目`javacode2018-h5`和`javacode2018-api`，创建完成之后，另外2个项目如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iazLQfh4MYnzss62JJ9Iwu1wou0Oibib6DkianMOvVIoQYVy3IToCYx6SjA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iavroiab11m5x3vIku26eqjtlGnShFHs2HzD8ycWAnQx8G44ttQp6ADYA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

上面3个项目由整个项目组6个人共同开发，这几个项目经常一起上线，上线过程，先由开发进行打包，然后将几个包发给运行进行发布。我们使用`mvn package`进行打包，需要在每个项目的`pom.xml`所在目录都去执行一次这个命令，也就是说需要执行3次，这个电商项目还会涉及到后台系统、bi系统、监控系统等等，可能最后会多达10个小项目，那时候我们每次上线都需要执行10次打包操作，这个过程是相当繁琐的。

那么maven有没有更好的办法来解决这个事情呢？

这个用到的就是我们本次要说的`maven中的聚合`。

整个电商我们可以作为一个大的系统，上面的pc端、h5微站、api接口、后台系统、bi系统、监控系统都可以作为里面的一个具体比较大一个模块。

我们使用`maven聚合`功能来实现上面的需求，我们需要创建一个额外的maven项目`javacode-aggregator`来管理上面3个项目，然后只用在`javacode-aggregator`项目中执行`mvn`命令，就会自动为其他3个项目自动执行同样的`mvn`命令。

#### Maven聚合

maven聚合需要创建一个新的maven项目， 用来管理其他的maven构件模块，新的maven项目中加入如下配置：

```
<modules>
    <module>模块1</module>
    <module>模块2</module>
    <module>模块n</module>
</modules>
<package>pom</package>
```

> 新的项目中执行任何`mvn`命令，都会`modules`中包含的所有模块执行同样的命令，而被包含的模块不需要做任何特殊的配置，正常的maven项目就行。
>
> 注意上面的`module`元素，这部分是被聚合的模块`pom.xml`所在目录的相对路径。
>
> package的值必须为pom，这个需要注意。
>
> 下面看案例。

#### 实操案例

> 下面过程仔细看了。

##### 创建项目`javacode-aggregator`

打开idea，点击`File->New->Project`，选择`Maven`，如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iaE1MyFc84aiaf3RvOAZiclicl32SkoQbhQX3sl9uw9UkaEy1zADibTvBOQA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击`Next`，输入项目坐标信息，如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iaQDjABKd1xFUDeAYTm0oOwdpXK6GquAGWFCQsicuYjdPQWZxeAhJ9eAQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击`Next`，输入Project name 为`javacode-aggregator`，如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iaLsa9C4nZaGgXK6O3Ywo9jicOwia36va0qAgx1FtibZfr6Tc7zF8Hw9X3w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击`Finish`，创建成功，如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iaFqAHicOCMDzdYa5IzS2qI6mmicHbdwDGZh2EwK0EpbtLEHuClU6EHfgw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

配置一下idea的maven环境，点击`File->Settings`，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iaVuSBbk8yoR8A83fWV62pic99hRLE3kAZQ86JJb5mJc2nZyBSQVaoyiaQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

删除下面无用的代码：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iaM2z8OTrjdpgcCe8UY8jibKDZ0GdnUkrjKNnicibr6kR17uOMM6weAC9KQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

项目变成了下面这样：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iahwiazjbeXkxib3nospB3lqTlMoYibzZ0aFRhGOic8cXUbxF2qhS6VmGWRw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

`pom.xml`中加入下面配置：

```
<packaging>pom</packaging>
```

最后pom.xml内容如下：

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.javacode2018</groupId>
    <artifactId>javacode-aggregator</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>pom</packaging>

</project>
```

##### 创建`javacode2018-pc` 模块

**注意这里说的是模块，不再是创建项目，过程仔细看。**

idea中选中`javacode-aggregator`

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iacXHhibDBYATibwCsVfTDD9Z3ociaIicPzKwIYI2rcJQCiaDQKWUJSiagBskQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击`File->New->Module`，准备创建模块，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iaFUUdKZNeWoDpzRicUh2BnACNmOUhAvCCk7hZz3h9KsHYQmQCeDw5v6w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

选择`Maven`，如图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iaAnjkicdAArN41Z1icosqxvY7xcs5TibOF20p0dSbVOcrXGrVpOPbxCcug/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击`Next`，如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iaw9kV2lmfvSWdmVgaZX3NfvMGOEtE1TvhvEgTZFSic3PIe5bQkS2NbIA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击下面的`...`按钮，选择`None`，如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iaFuaGaR48cGaoUWFyPP1LdfTu9He4M5mO2htFjE2sc5XiaZib3EJzg9kA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iaOKkm2koWEDokkxzdjugHozoQ93Sg3YhUwwDfLJYZL3WqZU06Ff4sVA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iaZxeMYZNChrhXzazG9yErzeQmvEfemN3nZDag7oCrG1SY73ILYmo9sA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

输入坐标信息，如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iawmsLZbxdXzQIDI3gqE8XctlOH0Tp3YRBqLJprn2D0zm1y1ayoCnvyA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击`Next`，输入Module name :`javacode2018-pc`，如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iatqw3LPDb8gEhsicuxxF4Hhzia4ZwJr4DTamLuQWluxeIS4dcJY70VIwQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击`Finish`，完成`javacode-2018`模块的创建，如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9ianLKGKmRnsLMBwc0icTqR9V58AGGogpU5XCia16S6y1RFs4QDTBCQsPEQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

我们来看一下`javacode-aggregator`的pom.xml，多了一部分，如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9ianMkJBtrGglfDwK8yFgYWYs9sHzUc1nwzpN78tBEvc09e338x8icqq6w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

> 注意上面的`<packaging>pom</packaging>`

##### 创建javacode2018-h5模块

创建步骤和上面`javacode2018-pc`步骤一样。

##### 创建javacode2018-api模块

创建步骤和上面`javacode2018-pc`步骤一样。

我们看一下项目最终结构，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iajEr7jHeWDUfichnC0LTOAUrW60RTbAad958ia25TZVae6Lb9gicbqC5DA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

看一下上图中的`javacode-aggregator`项目的`pom.xml`，`modules`元素中多出了3部分。

再来看看其他3个模块的pom文件，和普通的pom.xml一样，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iaGYfpKN1ne6yG8RIjbuRARaN4s0DmcFeMMeZwdL9aNF0LZD46dib5MZQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iaKfe1iaFOWGarFS4bByfLo7k8KqIyY9d4Gt3LrqLfDsicSssZ16dpl3yQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9ia7mIricgHqDoV1gnm1460gCZgYFKwUxqAuhibfxhQicj1VgibnO5anzCicag/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

在`javacode-aggregator/pom.xml`目录中执行`mvn package`感受一下效果：

```
D:\code\IdeaProjects\javacode-aggregator>mvn package
[INFO] Scanning for projects...
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Build Order:
[INFO]
[INFO] javacode2018-pc                                                    [jar]
[INFO] javacode2018-h5                                                    [jar]
[INFO] javacode2018-api                                                   [jar]
[INFO] javacode-aggregator                                                [pom]
[INFO]
[INFO] ------------------< com.javacode2018:javacode2018-pc >------------------
[INFO] Building javacode2018-pc 1.0-SNAPSHOT                              [1/4]
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ javacode2018-pc ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ javacode2018-pc ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ javacode2018-pc ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] skip non existing resourceDirectory D:\code\IdeaProjects\javacode-aggregator\javacode2018-pc\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ javacode2018-pc ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ javacode2018-pc ---
[INFO] No tests to run.
[INFO]
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ javacode2018-pc ---
[INFO] Building jar: D:\code\IdeaProjects\javacode-aggregator\javacode2018-pc\target\javacode2018-pc-1.0-SNAPSHOT.jar
[INFO]
[INFO] ------------------< com.javacode2018:javacode2018-h5 >------------------
[INFO] Building javacode2018-h5 1.0-SNAPSHOT                              [2/4]
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ javacode2018-h5 ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ javacode2018-h5 ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ javacode2018-h5 ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] skip non existing resourceDirectory D:\code\IdeaProjects\javacode-aggregator\javacode2018-h5\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ javacode2018-h5 ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ javacode2018-h5 ---
[INFO] No tests to run.
[INFO]
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ javacode2018-h5 ---
[INFO] Building jar: D:\code\IdeaProjects\javacode-aggregator\javacode2018-h5\target\javacode2018-h5-1.0-SNAPSHOT.jar
[INFO]
[INFO] -----------------< com.javacode2018:javacode2018-api >------------------
[INFO] Building javacode2018-api 1.0-SNAPSHOT                             [3/4]
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ javacode2018-api ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ javacode2018-api ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ javacode2018-api ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] skip non existing resourceDirectory D:\code\IdeaProjects\javacode-aggregator\javacode2018-api\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ javacode2018-api ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ javacode2018-api ---
[INFO] No tests to run.
[INFO]
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ javacode2018-api ---
[INFO] Building jar: D:\code\IdeaProjects\javacode-aggregator\javacode2018-api\target\javacode2018-api-1.0-SNAPSHOT.jar
[INFO]
[INFO] ----------------< com.javacode2018:javacode-aggregator >----------------
[INFO] Building javacode-aggregator 1.0-SNAPSHOT                          [4/4]
[INFO] --------------------------------[ pom ]---------------------------------
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary for javacode-aggregator 1.0-SNAPSHOT:
[INFO]
[INFO] javacode2018-pc .................................... SUCCESS [  2.542 s]
[INFO] javacode2018-h5 .................................... SUCCESS [  0.195 s]
[INFO] javacode2018-api ................................... SUCCESS [  0.218 s]
[INFO] javacode-aggregator ................................ SUCCESS [  0.024 s]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  3.223 s
[INFO] Finished at: 2019-11-19T13:28:00+08:00
[INFO] ------------------------------------------------------------------------
```

我们分析一下上面的输出：

```
[INFO] Reactor Build Order:
[INFO]
[INFO] javacode2018-pc                                                    [jar]
[INFO] javacode2018-h5                                                    [jar]
[INFO] javacode2018-api                                                   [jar]
[INFO] javacode-aggregator                                                [pom]
```

> 上面这个列出了需要构件的maven构件列表及顺序，共有4个构件，3个jar包，1个pom类型的构件。

然后开始按照上面列出的顺序，一个个开始执行`mvn package`命令，最后4个都执行成功了，我们来看一下最终产生的效果，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iasWG0W9ZnSeicWmN1yVcPSjPpTREb8QPuoBkvpL6Pyja15bliaOSnGPSA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

从上图中我们可以看到3个模块都生成了jar包，说明我们执行了一次`mvn package`，分别在3个模块中都运行了，我们在来执行一下`mvn clean`清理代码，感受一下最终效果：

```
D:\code\IdeaProjects\javacode-aggregator>mvn clean
[INFO] Scanning for projects...
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Build Order:
[INFO]
[INFO] javacode2018-pc                                                    [jar]
[INFO] javacode2018-h5                                                    [jar]
[INFO] javacode2018-api                                                   [jar]
[INFO] javacode-aggregator                                                [pom]
[INFO]
[INFO] ------------------< com.javacode2018:javacode2018-pc >------------------
[INFO] Building javacode2018-pc 1.0-SNAPSHOT                              [1/4]
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ javacode2018-pc ---
[INFO] Deleting D:\code\IdeaProjects\javacode-aggregator\javacode2018-pc\target
[INFO]
[INFO] ------------------< com.javacode2018:javacode2018-h5 >------------------
[INFO] Building javacode2018-h5 1.0-SNAPSHOT                              [2/4]
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ javacode2018-h5 ---
[INFO] Deleting D:\code\IdeaProjects\javacode-aggregator\javacode2018-h5\target
[INFO]
[INFO] -----------------< com.javacode2018:javacode2018-api >------------------
[INFO] Building javacode2018-api 1.0-SNAPSHOT                             [3/4]
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ javacode2018-api ---
[INFO] Deleting D:\code\IdeaProjects\javacode-aggregator\javacode2018-api\target
[INFO]
[INFO] ----------------< com.javacode2018:javacode-aggregator >----------------
[INFO] Building javacode-aggregator 1.0-SNAPSHOT                          [4/4]
[INFO] --------------------------------[ pom ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ javacode-aggregator ---
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary for javacode-aggregator 1.0-SNAPSHOT:
[INFO]
[INFO] javacode2018-pc .................................... SUCCESS [  0.250 s]
[INFO] javacode2018-h5 .................................... SUCCESS [  0.067 s]
[INFO] javacode2018-api ................................... SUCCESS [  0.068 s]
[INFO] javacode-aggregator ................................ SUCCESS [  0.034 s]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  0.605 s
[INFO] Finished at: 2019-11-19T13:34:42+08:00
[INFO] ------------------------------------------------------------------------
```

项目结构如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iaiaKcBYWHV7ZYj0Yaiah52gbU2RGJicbFia3hLFOqODtq7NM54yK9UDHVQA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

3个项目中的target都被清理掉了，是不是感觉很爽。

**上面介绍了pom.xml中的`module`元素的值为被聚合的模块pom.xml所在的目录路径，可以是相对路径，也可以是绝对路径，上面演示的是相对路径，大家可以自己去玩一下绝对路径的情况。**

聚合的功能中，聚合模块的pom.xml中通过`modules->module`来引用被聚合的模块，被聚合的模块是不用感知自己被聚合了，所以被聚合的模块中`pom.xml`中是不知道`javacode-aggregator`的存在的。

### 继承

#### 需求

细心的朋友已经发现了`javacode2018-pc、javacode2018-h5、javacode2018-api`3个项目的`groupId、version`都是一样的。还有这几个项目准备都是用springmvc、mybatis开发的，所以需要引入依赖如下：

```
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-web</artifactId>
    <version>5.2.1.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>2.0.3</version>
</dependency>
```

3个项目中出现了同样的配置，一个配置出现了多份，作为开发者，我想你们和我一样，是无法接受的，必须要寻求一些方法来解决，需要将公共的部分提取出来共用。

maven也为我们考虑到了这种情况，maven中使用继承来解决这个问题。

#### 继承的实现

操作分为3步骤：

1. 创建一个父maven构件，将依赖信息放在pom.xml中

   ```
   <dependencies>
      <dependency>依赖的构件的坐标信息</dependency>
      <dependency>依赖的构件的坐标信息</dependency>
      <dependency>依赖的构件的坐标信息</dependency>
   </dependencies>
   ```

2. 将父构件的package元素的值置为pom

   ```
   <packaging>pom</packaging>
   ```

3. 在子构件的pom.xml引入父构件的配置：

   ```
   <parent>
      <groupId>父构件groupId</groupId>
      <artifactId>父构件artifactId</artifactId>
      <version>父构件的版本号</version>
      <relativePath>父构件pom.xml路径</relativePath>
   </parent>
   ```

> relativePath表示父构件pom.xml相对路径，默认是`../pom.xml`，所以一般情况下父子结构的maven构件在目录结构上一般也采用父子关系。

#### 实操案例

先创建一个父项目`javacode-parent`，坐标信息如下：

```
<groupId>com.javacode2018</groupId>
<artifactId>javacode2018-parent</artifactId>
<version>1.0-SNAPSHOT</version>
```

创建过程大家已经轻车熟路了，就不演示了，最终如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iaSZgSbzmuPyDPWKnicQIz1kvbEu4uANU5bCtId20ibZFHxzhMKicUHW9hw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

删除无用的2目录`.idea和src`

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iaZmYPpq7xovIwHhyv7r41DS4gia9tEAzB9DibkibQBXqic5vxnsrvZz2F8g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

##### 创建子模块`javacode2018-pc` 模块

**注意这里说的是模块了，不在是创建项目了，过程仔细看。**

idea中选中`javacode-parent`，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iaE5NfGELdH6CMsABjwJlL58iah0xsS8gDR28H7UibDLuN48z9lSRXvvWA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击`File->New->Module`，准备创建模块，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iaRribjKhZZupYPuJLIWrVpWW7Ot0lcpEM2pL7Jnyl7RHaRqxGvibZRpZQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

选中`Maven`，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iayHXVibo0hW1GVwj7WynpAicxGvk0j4rqWw3xKcbpmaau0tpKK86lAcTA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击`Next`，这个页面需要注意了，默认如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iaSBy5iaCp2JXqibde1SKkqh0ddv05MVBxNYyaTmW1UOynicdJnuNiaAlzaw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

此时我们点击第一个`...`按钮，这个按钮是设置聚合的，所以选中`None`，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iay5WvlT2o9Y2N6tQECfiaIAtdXl0HUYrf2EHPwQSz12JJcR8sooQ4agQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9ia3LFZkC4YCpkic8Q6TpBicbT7iaYiciaj15zicN2FjbBtJdfe4DLatz9v7mIQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

变成了下面这样：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iaAFogcwIcYPgXXqCpEKeEZ4icGnhPeI89McoTdLqqUlECXFgwB2Rlwtw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

输入构件信息，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iax0dQ9vohxRh2W9dsdSUrVrKibhnWrFbZK7BFzULU9Kb42fZWk7WxAkw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击`Next`，输入`Module-name`的值为`javacode2018-pc`，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9ia6bq2upGn9XWCWXtlQbqQ0SLIzqoxYYP2rSichgKCddLCAy4RtbKhwIg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击`Finish`完成创建操作，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iaVKuSFrjFwYhAup0AuDEbk3d6ZCcgkTGgHzSkGXZDbibnQ5cBDhXqVbg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

上图中，看一下`javacode2018-pc`的pom.xml，多了个`parent`元素，并且这个pom.xml中构件的`groupId、version`都没有了，这个`pom.xml`继承了`javacode2018-parent/pom.xml`中的内容，他们的`groupId、version`都是一样的，子构件可以从父pom.xml中继承这些内容，所以如果是一样的情况，可以不写。

##### 创建另外两个子模块javacode2018-h5、javacode2018-api

步骤参考`javacode2018-pc`模块的创建过程。

最终项目如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iaM0QXBu2wtex0X1PvRl9poYL48gIVIAhPZWMs4nwNHibJ6eYwdEqiazIw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

下面我们来看一下4个pom.xml的内容。

javacode2018-parent/pom.xml的内容：

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.javacode2018</groupId>
    <artifactId>javacode2018-parent</artifactId>
    <version>1.0-SNAPSHOT</version>

</project>
```

javacode2018-pc/pom.xml的内容：

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>javacode2018-parent</artifactId>
        <groupId>com.javacode2018</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>javacode2018-pc</artifactId>

</project>
```

javacode2018-h5/pom.xml的内容：

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>javacode2018-parent</artifactId>
        <groupId>com.javacode2018</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>javacode2018-h5</artifactId>

</project>
```

javacode2018-api/pom.xml的内容：

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>javacode2018-parent</artifactId>
        <groupId>com.javacode2018</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>javacode2018-api</artifactId>

</project>
```

我们在父构件javacode2018-parent/pom.xml中加入下面配置：

```
<packaging>pom</packaging>
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-web</artifactId>
        <version>5.2.1.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis-spring</artifactId>
        <version>2.0.3</version>
    </dependency>
</dependencies>
```

使用下面命令在javacode2018-parent/pom.xml所在目录看一下javacode2018-parent构件依赖情况:

```
D:\code\IdeaProjects\javacode2018-parent>mvn dependency:tree
[INFO] Scanning for projects...
[INFO]
[INFO] ----------------< com.javacode2018:javacode2018-parent >-----------------
[INFO] Building javacode2018-parent 1.0-SNAPSHOT
[INFO] --------------------------------[ pom ]---------------------------------
[INFO]
[INFO] --- maven-dependency-plugin:2.8:tree (default-cli) @ javacode2018-parent ---
[INFO] com.javacode2018:javacode2018-parent:pom:1.0-SNAPSHOT
[INFO] +- org.springframework:spring-web:jar:5.2.1.RELEASE:compile
[INFO] |  +- org.springframework:spring-beans:jar:5.2.1.RELEASE:compile
[INFO] |  \- org.springframework:spring-core:jar:5.2.1.RELEASE:compile
[INFO] |     \- org.springframework:spring-jcl:jar:5.2.1.RELEASE:compile
[INFO] \- org.mybatis:mybatis-spring:jar:2.0.3:compile
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  2.544 s
[INFO] Finished at: 2019-11-19T14:58:21+08:00
[INFO] ------------------------------------------------------------------------
```

> mvn dependency:tree 这个插件可以根据pom.xml的配置，列出构件的依赖树信息。

我们再来看看javacode2018-pc/pom.xml构件的依赖信息：

```
D:\code\IdeaProjects\javacode2018-parent>cd javacode2018-pc

D:\code\IdeaProjects\javacode2018-parent\javacode2018-pc>mvn dependency:tree
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------< com.javacode2018:javacode2018-pc >-------------------
[INFO] Building javacode2018-pc 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-dependency-plugin:2.8:tree (default-cli) @ javacode2018-pc ---
[INFO] com.javacode2018:javacode2018-pc:jar:1.0-SNAPSHOT
[INFO] +- org.springframework:spring-web:jar:5.2.1.RELEASE:compile
[INFO] |  +- org.springframework:spring-beans:jar:5.2.1.RELEASE:compile
[INFO] |  \- org.springframework:spring-core:jar:5.2.1.RELEASE:compile
[INFO] |     \- org.springframework:spring-jcl:jar:5.2.1.RELEASE:compile
[INFO] \- org.mybatis:mybatis-spring:jar:2.0.3:compile
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  2.093 s
[INFO] Finished at: 2019-11-19T14:59:34+08:00
[INFO] ------------------------------------------------------------------------
```

> 从上图中可以看到`javacode2018-pc`依赖的jar和父构件`javacode2019-parent`的一样，说明从父类继承了这些依赖。
>
> 其他2个子构件，大家也可以去看一下，依赖关系都从父构件继承过来了。

#### relativePath元素

上面演示的父构件和子构件的目录结构刚好符合父子关系，如果父构件和子构件的目录不是父子关系，比如都位于同等级别的目录或者位于更复杂的目录的时候，此时我们需要在子`pom.xml`的`parent`元素中使用`relativePath`元素来指定父`pom.xml`相对路径位置，这个值我们上面没有指定，默认是`../pom.xml`，表示父pom.xml位于子pom.xml的上一级目录，我们的模块刚好符合这种关系，所以这个值省略了。

正确的设置`relativePath`是非常重要的，这个需要注意，子模块中执行`mvn`命令的时候，会去找父`pom.xml`的配置，会先通过`relativePath`指定的路径去找，如果找不到，会尝试通过坐标在本地仓库进行查找，如果本地找不到，会去远程仓库找，如果远程仓库也没有，会报错。

#### 可以通过继承的元素有以下这些

上面我们看到了`groupId、version、dependency中的依赖`在子`pom.xml`中都没有写，这些都是从父`pom.xml`中继承过来的，还有很多元素也可以被继承过来，下面我们列个清单：

- groupId：项目组ID，项目坐标的核心元素
- version：项目版本，项目坐标的核心元素
- description：项目的描述信息
- organization：项目的组织信息
- inceptionYear：项目的创始年份
- url：项目的url地址
- developers：项目的开发者信息
- contributors：项目的贡献者信息
- distributionManagement：项目的部署配置信息
- issueManagement：项目的缺陷跟踪系统信息
- ciManagement：项目的持续集成系统信息
- scm：项目的版本控制系统信息
- mailingLists：项目的邮件列表信息
- properties：自定义的maven属性配置信息
- dependencyManagement：项目的依赖管理配置
- repositories：项目的仓库配置
- build：包括项目的源码目录配置、输出目录配置、插件管理配置等信息
- reporting：包括项目的报告输出目录配置、报告插件配置等信息

### 依赖管理(dependencyManagement)

大家是否发现了，上面的继承存在的一个问题，如果我在新增一个子构件，都会默认从父构件中继承依赖的一批构建，父pom.xml中配置的这些依赖的构建可能是其他项目不需要的，可能某个子项目只是想使用其中一个构件，但是上面的继承关系却把所有的依赖都给传递到子构件中了，这种显然是不合适的。

maven中也考虑到了这种情况，可以使用`dependencyManagement`元素来解决这个问题。

maven提供的dependencyManagement元素既能让子模块继承到父模块的依赖配置，又能保证子模块依赖使用的灵活性，**在dependencyManagement元素下声明的依赖不会引入实际的依赖，他只是声明了这些依赖，不过它可以对`dependencies`中使用的依赖起到一些约束作用。**

修改`javacode2018-parent/pom.xml`配置如下：

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.javacode2018</groupId>
    <artifactId>javacode2018-parent</artifactId>
    <version>1.0-SNAPSHOT</version>

    <packaging>pom</packaging>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-web</artifactId>
                <version>5.2.1.RELEASE</version>
            </dependency>
            <dependency>
                <groupId>org.mybatis</groupId>
                <artifactId>mybatis-spring</artifactId>
                <version>2.0.3</version>
            </dependency>
        </dependencies>
    </dependencyManagement>

</project>
```

可以看到我们将`dependencies`配置移到`dependencyManagement`下面了。

我们使用下面命令看一下项目的依赖情况：

```
D:\code\IdeaProjects\javacode2018-parent>mvn dependency:tree
[INFO] Scanning for projects...
[INFO]
[INFO] ----------------< com.javacode2018:javacode2018-parent >-----------------
[INFO] Building javacode2018-parent 1.0-SNAPSHOT
[INFO] --------------------------------[ pom ]---------------------------------
[INFO]
[INFO] --- maven-dependency-plugin:2.8:tree (default-cli) @ javacode2018-parent ---
[INFO] com.javacode2018:javacode2018-parent:pom:1.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  1.756 s
[INFO] Finished at: 2019-11-19T15:44:52+08:00
[INFO] ------------------------------------------------------------------------

D:\code\IdeaProjects\javacode2018-parent>cd javacode2018-pc

D:\code\IdeaProjects\javacode2018-parent\javacode2018-pc>mvn dependency:tree
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------< com.javacode2018:javacode2018-pc >-------------------
[INFO] Building javacode2018-pc 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-dependency-plugin:2.8:tree (default-cli) @ javacode2018-pc ---
[INFO] com.javacode2018:javacode2018-pc:jar:1.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  1.676 s
[INFO] Finished at: 2019-11-19T15:45:08+08:00
[INFO] ------------------------------------------------------------------------
```

> 父子构件中都看不到依赖的jar包了，说明父pom.xml中dependencyManagement这些依赖的构建没有被子模块依赖进去。

子模块如果想用到这些配置，可以`dependencies`进行引用，引用之后，依赖才会真正的起效。

在在3个子模块的pom.xml中加入下面配置：

```
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis-spring</artifactId>
    </dependency>
</dependencies>
```

我们在运行一下上面的3个命令，看效果：

```
D:\code\IdeaProjects\javacode2018-parent>mvn dependency:tree
[INFO] Scanning for projects...
[INFO]
[INFO] ----------------< com.javacode2018:javacode2018-parent >-----------------
[INFO] Building javacode2018-parent 1.0-SNAPSHOT
[INFO] --------------------------------[ pom ]---------------------------------
[INFO]
[INFO] --- maven-dependency-plugin:2.8:tree (default-cli) @ javacode2018-parent ---
[INFO] com.javacode2018:javacode2018-parent:pom:1.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  1.759 s
[INFO] Finished at: 2019-11-19T15:48:28+08:00
[INFO] ------------------------------------------------------------------------

D:\code\IdeaProjects\javacode2018-parent>cd javacode2018-pc

D:\code\IdeaProjects\javacode2018-parent\javacode2018-pc>mvn dependency:tree
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------< com.javacode2018:javacode2018-pc >-------------------
[INFO] Building javacode2018-pc 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-dependency-plugin:2.8:tree (default-cli) @ javacode2018-pc ---
[INFO] com.javacode2018:javacode2018-pc:jar:1.0-SNAPSHOT
[INFO] +- org.springframework:spring-web:jar:5.2.1.RELEASE:compile
[INFO] |  +- org.springframework:spring-beans:jar:5.2.1.RELEASE:compile
[INFO] |  \- org.springframework:spring-core:jar:5.2.1.RELEASE:compile
[INFO] |     \- org.springframework:spring-jcl:jar:5.2.1.RELEASE:compile
[INFO] \- org.mybatis:mybatis-spring:jar:2.0.3:compile
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  1.875 s
[INFO] Finished at: 2019-11-19T15:48:38+08:00
[INFO] ------------------------------------------------------------------------
```

> javacode2018-parent构件中没有列出依赖信息，而javacode2018-pc列出了依赖信息。

大家看一下下面的截图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iawuHFuZlUeTMNY3hpu3LIibB1xVlFesfj4GTvuia5ZzhSg9Adnf0icSkCw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

> 注意上面的红框中依赖的构建没有`version`，左边2个红圈中的2个向上的箭头，表示这个是从父pom.xml中传递过来的，所以version可以省略。

再看一下父pom.xml截图效果：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9ia1zzGraEdicY1l96D5XSxUy8lsBibaUfUOIYy6mAZiaUm8pjowLS6Kh1fQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

> dependencyManagement中定义了依赖的构建，2个向下的箭头表示被子模块有继承。

**dependencyManagement不会引入实际的依赖，只有在子类中使用`dependency`来引入父`dependencyManagement`声明的依赖之后，依赖的构建才会被真正的引入。**

**使用dependencyManagement来解决继承的问题，子pom.xml中只用写`groupId,artifactId`就可以了，其他信息都会从父`dependencyManagement`中声明的依赖关系中传递过来，通常我们使用这种方式将所有依赖的构建在父pom.xml中定义好，子构件中只需要通过`groupId,artifactId`就可以引入依赖的构建，而不需要写`version`，可以很好的确保多个子项目中依赖构件的版本的一致性，对应依赖构件版本的升级也非常方便，只需要在父pom.xml中修改一下就可以了。**

### 单继承问题

#### 存在的问题及解决方案

上面讲解了dependencyManagement的使用，但是有个问题，只有使用继承的时候，dependencyManagement中声明的依赖才可能被子pom.xml用到，如果我的项目本来就有父pom.xml了，但是我现在想使用另外一个项目dependencyManagement中声明的依赖，此时我们怎么办？这就是单继承的问题，这种情况在spring-boot、spring-cloud中会遇到，所以大家需要注意，这块一定需要玩转。

当我们想在项目中使用另外一个构件中dependencyManagement声明的依赖，而又不想继承这个项目的时候，可以在我们的项目中使用加入下面配置：

```
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>com.javacode2018</groupId>
            <artifactId>javacode2018-parent</artifactId>
            <version>1.0-SNAPSHOT</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
        <dependency>构件2</dependency>
        <dependency>构件3</dependency>
        <dependency>构件n</dependency>
    </dependencies>
</dependencyManagement>
```

上面这个配置会将`javacode2018-parent`构件中`dependencyManagement`元素中声明的所有依赖导入到当前pom.xml的`dependencyManagement`中，相当于把下面部分的内容：

```
<dependency>
    <groupId>com.javacode2018</groupId>
    <artifactId>javacode2018-parent</artifactId>
    <version>1.0-SNAPSHOT</version>
    <type>pom</type>
    <scope>import</scope>
</dependency>
```

替换成了`javacode2018-parent/pom.xml`中dependencyManagement元素内容，替换之后变成：

```
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
            <version>5.2.1.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>2.0.3</version>
        </dependency>
        <dependency>构件2</dependency>
        <dependency>构件3</dependency>
        <dependency>构件n</dependency>
    </dependencies>
</dependencyManagement>
```

#### 实操案例

创建项目`javacode2018-pom-import`，具体过程不演示了，坐标信息：

```
<groupId>com.javacode2018</groupId>
<artifactId>javacode2018-pom-import</artifactId>
<version>1.0-SNAPSHOT</version>
```

创建完毕之后，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iaopTkzCojkKt3gFdtmV9bRoibP0RfIt9VdIUYpYMMU3oNvMq6d8S03fw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

修改`javacode2018-pom-import/pom.xml`内容，如下：

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.javacode2018</groupId>
    <artifactId>javacode2018-pom-import</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>com.alibaba</groupId>
                <artifactId>fastjson</artifactId>
                <version>1.2.62</version>
            </dependency>
            <dependency>
                <groupId>junit</groupId>
                <artifactId>junit</artifactId>
                <version>4.12</version>
                <scope>test</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

</project>
```

> 上面我们在`dependencyManagement`中声明了2个依赖：fastjson和junit。

在`javacode2018-pom-import/pom.xml`中执行`mvn install`将其安装到本地仓库

```
D:\code\IdeaProjects\javacode2018-pom-import>mvn install
[INFO] Scanning for projects...
[INFO]
[INFO] --------------< com.javacode2018:javacode2018-pom-import >--------------
[INFO] Building javacode2018-pom-import 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ javacode2018-pom-import ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ javacode2018-pom-import ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ javacode2018-pom-import ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] skip non existing resourceDirectory D:\code\IdeaProjects\javacode2018-pom-import\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ javacode2018-pom-import ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ javacode2018-pom-import ---
[INFO] No tests to run.
[INFO]
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ javacode2018-pom-import ---
[INFO] Building jar: D:\code\IdeaProjects\javacode2018-pom-import\target\javacode2018-pom-import-1.0-SNAPSHOT.jar
[INFO]
[INFO] --- maven-install-plugin:2.4:install (default-install) @ javacode2018-pom-import ---
[INFO] Installing D:\code\IdeaProjects\javacode2018-pom-import\target\javacode2018-pom-import-1.0-SNAPSHOT.jar to C:\Users\Think\.m2\repository\com\javacode2018\javacode2018-pom-import\1.0-SNAPSHOT\javacode2018-pom-import-1.0-SNAPSHOT.jar
[INFO] Installing D:\code\IdeaProjects\javacode2018-pom-import\pom.xml to C:\Users\Think\.m2\repository\com\javacode2018\javacode2018-pom-import\1.0-SNAPSHOT\javacode2018-pom-import-1.0-SNAPSHOT.pom
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  2.692 s
[INFO] Finished at: 2019-11-19T17:23:16+08:00
[INFO] ------------------------------------------------------------------------
```

现在我们想在`javacode2018-pc/pom.xml`中使用`javacode2018-pom-import/pom.xml`的`dependencyManagement`元素中定义的`fastjson和junit的依赖`，而我们的`javacode2019-pc`已经继承了`javacode-parent`了，maven中只能单继承，所以没法通过继承的方式来实现了，那么我们可以在`javacode2019-pc/pom.xml`中加入下面配置：

```
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>com.javacode2018</groupId>
            <artifactId>javacode2018-pom-import</artifactId>
            <version>1.0-SNAPSHOT</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

此时可以使用fastjson的依赖了，`javacode2018-pc/pom.xml`中`project->dependencies`元素中加入下面配置：

```
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
</dependency>
```

看一下效果：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iaIEZPZ3iab0mTwFgjyGc2XhGSHicpBQI60NCpJUMarLZDwbCiaTJ5ibMicCA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

> 上面的fastjson没有指定版本号，直接可以使用，说明从`javacode2018-pom-import`传递过来了。

可以点击一下上面红圈中的箭头，会跳到本地仓库的`javacode2018-pom-import-1.0-SNAPSHOT.pom`文件，如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iam0rP0k2jmstC79TvjCx7B2HLdyV0tCoVHnIQdqXTpxvicNPSVDOOgZw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

### 插件管理(pluginManagement)

maven中提供了dependencyManagement来解决继承的问题，同样也提供了解决插件继承问题的`pluginManagement`元素，在父pom中可以在这个元素中声明插件的配置信息，但是子pom.xml中不会引入此插件的配置信息，只有在子pom.xml中使用`plugins->plugin`元素正在引入这些声明的插件的时候，插件才会起效，子插件中只需要写`groupId、artifactId`，其他信息都可以从父构件中传递过来。

#### 实操案例

在`javacode2018-parent/pom.xml`中加入下面配置：

```
<build>
    <pluginManagement>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-source-plugin</artifactId>
                <version>3.2.0</version>
                <executions>
                    <execution>
                        <id>attach-source</id>
                        <phase>verify</phase>
                        <goals>
                            <goal>jar-no-fork</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </pluginManagement>
</build>
```

> maven-source-plugin这个插件在上一章有介绍过，源码打包的。
>
> 上面是将插件`maven-source-plugin`的`jar-no-fork`目标绑定在`default`生命周期的`verify`阶段，`verify`阶段会在default生命周期的`install`周期前面执行。
>
> 下面我们看效果。

在javacode2018-pc/pom.xml所在目录运行下面命令

```
D:\code\IdeaProjects\javacode2018-parent\javacode2018-pc>mvn clean install
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------< com.javacode2018:javacode2018-pc >-------------------
[INFO] Building javacode2018-pc 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ javacode2018-pc ---
[INFO] Deleting D:\code\IdeaProjects\javacode2018-parent\javacode2018-pc\target
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ javacode2018-pc ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ javacode2018-pc ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ javacode2018-pc ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] skip non existing resourceDirectory D:\code\IdeaProjects\javacode2018-parent\javacode2018-pc\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ javacode2018-pc ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ javacode2018-pc ---
[INFO] No tests to run.
[INFO]
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ javacode2018-pc ---
[INFO] Building jar: D:\code\IdeaProjects\javacode2018-parent\javacode2018-pc\target\javacode2018-pc-1.0-SNAPSHOT.jar
[INFO]
[INFO] --- maven-install-plugin:2.4:install (default-install) @ javacode2018-pc ---
[INFO] Installing D:\code\IdeaProjects\javacode2018-parent\javacode2018-pc\target\javacode2018-pc-1.0-SNAPSHOT.jar to C:\Users\Think\.m2\repository\com\javacode2018\javacode2018-pc\1.0-SNAPSHOT\javacode2018-pc-1.0-SNAPSHOT.jar
[INFO] Installing D:\code\IdeaProjects\javacode2018-parent\javacode2018-pc\pom.xml to C:\Users\Think\.m2\repository\com\javacode2018\javacode2018-pc\1.0-SNAPSHOT\javacode2018-pc-1.0-SNAPSHOT.pom
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  2.254 s
[INFO] Finished at: 2019-11-19T16:24:39+08:00
[INFO] ------------------------------------------------------------------------
```

> mvn clean install 会清理代码，然后将打包构件，将构建安装到本地仓库，从输入中可以看到`javacode2018-pc-1.0-SNAPSHOT.jar`被安装到本地仓库了。
>
> 但是没有看到打包源码的插件的运行，说明了`javacode2018-pc`没有从父pom.xml中继承插件的配置信息，所以插件配置没有起效，现在我们要让插件起效，继续看：

在javacode2018-pc/pom.xml中加入下面配置：

```
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-source-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

截图看效果：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06BCEy1baic5EkBqicXicBBFe9iacK5ozMn2V8F8z92dZpLD873Ag0xwvG2Pc5RBCIib5rtV0YEHlwic5Vww/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

又一个线上的箭头，说明这个是从父pom.xml中传递过来了，大家仔细看一下上面的配置，插件只写了`groupId、artifactId`，其他信息都没有写，其他信息都可以从父pom.xml中传递过来，下面我们看一下插件是否起效了，运行下面命令，见证奇迹：

```
D:\code\IdeaProjects\javacode2018-parent\javacode2018-pc>mvn clean install
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------< com.javacode2018:javacode2018-pc >-------------------
[INFO] Building javacode2018-pc 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ javacode2018-pc ---
[INFO] Deleting D:\code\IdeaProjects\javacode2018-parent\javacode2018-pc\target
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ javacode2018-pc ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ javacode2018-pc ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ javacode2018-pc ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] skip non existing resourceDirectory D:\code\IdeaProjects\javacode2018-parent\javacode2018-pc\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ javacode2018-pc ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ javacode2018-pc ---
[INFO] No tests to run.
[INFO]
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ javacode2018-pc ---
[INFO] Building jar: D:\code\IdeaProjects\javacode2018-parent\javacode2018-pc\target\javacode2018-pc-1.0-SNAPSHOT.jar
[INFO]
[INFO] --- maven-source-plugin:3.2.0:jar-no-fork (attach-source) @ javacode2018-pc ---
[INFO] Building jar: D:\code\IdeaProjects\javacode2018-parent\javacode2018-pc\target\javacode2018-pc-1.0-SNAPSHOT-sources.jar
[INFO]
[INFO] --- maven-install-plugin:2.4:install (default-install) @ javacode2018-pc ---
[INFO] Installing D:\code\IdeaProjects\javacode2018-parent\javacode2018-pc\target\javacode2018-pc-1.0-SNAPSHOT.jar to C:\Users\Think\.m2\repository\com\javacode2018\javacode2018-pc\1.0-SNAPSHOT\javacode2018-pc-1.0-SNAPSHOT.jar
[INFO] Installing D:\code\IdeaProjects\javacode2018-parent\javacode2018-pc\pom.xml to C:\Users\Think\.m2\repository\com\javacode2018\javacode2018-pc\1.0-SNAPSHOT\javacode2018-pc-1.0-SNAPSHOT.pom
[INFO] Installing D:\code\IdeaProjects\javacode2018-parent\javacode2018-pc\target\javacode2018-pc-1.0-SNAPSHOT-sources.jar to C:\Users\Think\.m2\repository\com\javacode2018\javacode2018-pc\1.0-SNAPSHOT\javacode2018-pc-1.0-SNAPSHOT-sources.jar
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  2.477 s
[INFO] Finished at: 2019-11-19T16:28:18+08:00
[INFO] ------------------------------------------------------------------------
```

> 注意上面的输出中有`attach-source`，这个就是上面我们插件任务的id，输出中还有`javacode2018-pc-1.0-SNAPSHOT-sources.jar`信息，说明源码也是打包成功了，最后也是上传到了本地仓库了。

上面演示了只用在子pom.xml中写上插件的`groupId、artifactId`就可以了，其他信息会从父pom.xml中插件的定义中传递过来，而子pom.xml中也可以自定义插件的这些配置，修改`javacode2018-pc/pom.xml`配置，如下：

```
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-source-plugin</artifactId>
            <executions>
                <execution>
                    <id>attach-source</id>
                    <goals>
                        <goal>help</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

再看一下效果：

```
D:\code\IdeaProjects\javacode2018-parent\javacode2018-pc>mvn clean install
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------< com.javacode2018:javacode2018-pc >-------------------
[INFO] Building javacode2018-pc 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ javacode2018-pc ---
[INFO] Deleting D:\code\IdeaProjects\javacode2018-parent\javacode2018-pc\target
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ javacode2018-pc ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ javacode2018-pc ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ javacode2018-pc ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] skip non existing resourceDirectory D:\code\IdeaProjects\javacode2018-parent\javacode2018-pc\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ javacode2018-pc ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ javacode2018-pc ---
[INFO] No tests to run.
[INFO]
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ javacode2018-pc ---
[INFO] Building jar: D:\code\IdeaProjects\javacode2018-parent\javacode2018-pc\target\javacode2018-pc-1.0-SNAPSHOT.jar
[INFO]
[INFO] --- maven-source-plugin:3.2.0:help (attach-source) @ javacode2018-pc ---
[INFO] Apache Maven Source Plugin 3.2.0
  The Maven Source Plugin creates a JAR archive of the source files of the
  current project.

This plugin has 7 goals:

source:aggregate
  Aggregate sources for all modules in an aggregator project.

source:generated-test-jar
  This plugin bundles all the test sources into a jar archive.

source:help
  Display help information on maven-source-plugin.
  Call mvn source:help -Ddetail=true -Dgoal=<goal-name> to display parameter
  details.

source:jar
  This plugin bundles all the sources into a jar archive.

source:jar-no-fork
  This goal bundles all the sources into a jar archive. This goal functions the
  same as the jar goal but does not fork the build and is suitable for attaching
  to the build lifecycle.

source:test-jar
  This plugin bundles all the test sources into a jar archive.

source:test-jar-no-fork
  This goal bundles all the test sources into a jar archive. This goal functions
  the same as the test-jar goal but does not fork the build, and is suitable for
  attaching to the build lifecycle.


[INFO]
[INFO] --- maven-source-plugin:3.2.0:jar-no-fork (attach-source) @ javacode2018-pc ---
[INFO] Building jar: D:\code\IdeaProjects\javacode2018-parent\javacode2018-pc\target\javacode2018-pc-1.0-SNAPSHOT-sources.jar
[INFO]
[INFO] --- maven-install-plugin:2.4:install (default-install) @ javacode2018-pc ---
[INFO] Installing D:\code\IdeaProjects\javacode2018-parent\javacode2018-pc\target\javacode2018-pc-1.0-SNAPSHOT.jar to C:\Users\Think\.m2\repository\com\javacode2018\javacode2018-pc\1.0-SNAPSHOT\javacode2018-pc-1.0-SNAPSHOT.jar
[INFO] Installing D:\code\IdeaProjects\javacode2018-parent\javacode2018-pc\pom.xml to C:\Users\Think\.m2\repository\com\javacode2018\javacode2018-pc\1.0-SNAPSHOT\javacode2018-pc-1.0-SNAPSHOT.pom
[INFO] Installing D:\code\IdeaProjects\javacode2018-parent\javacode2018-pc\target\javacode2018-pc-1.0-SNAPSHOT-sources.jar to C:\Users\Think\.m2\repository\com\javacode2018\javacode2018-pc\1.0-SNAPSHOT\javacode2018-pc-1.0-SNAPSHOT-sources.jar
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  2.752 s
[INFO] Finished at: 2019-11-19T16:41:21+08:00
[INFO] ------------------------------------------------------------------------
```

输出中有下面2行：

```
[INFO] --- maven-source-plugin:3.2.0:help (attach-source) @ javacode2018-pc ---
[INFO] --- maven-source-plugin:3.2.0:jar-no-fork (attach-source) @ javacode2018-pc ---
```

说明`maven-source-plugin`插件执行了2个目标：`help`和`jar-no-fork`。此时父子pom.xml中插件配置信息合并了，所以出现了2个目标。具体最终`javacode2018-pc/pom.xml`中的内容是什么样的，可以通过下面这个命令看到：

```
mvn help:effective-pom
```

上面这个命令大家最好去看一下效果，当pom.xml中存在复杂的关系的时候，可以通过这个命令解析得到这个构件最终pom.xml的内容。

### 聚合与继承的关系

前面已经详解了聚合和继承，想必大家对这块也有了自己的理解。

聚合主要是为了方便多模块快速构建。

而继承主要是为了重用相同的配置。

对于聚合来说，聚合模块是知道被聚合模块的存在的，而被聚合模块是感知不到聚合模块的存在。

对于继承来说，父构件是感知不到子构件的存在，而子构件需要使用`parent`来引用父构件。

两者的共同点是，聚合模块和继承中的父模块的package属性都必须是pom类型的，同时，聚合模块和父模块中的除了pom.xml，一般都是没有什么内容的。

实际使用是，我们经常将聚合和继承一起使用，能同时使用到两者的优点。

下面我们在`javacode2018-parent`中加入聚合的配置：

```
<modules>
    <module>javacode2018-pc</module>
    <module>javacode2018-h5</module>
    <module>javacode2018-api</module>
</modules>
```

javacode20180-parent/pom.xml目录运行一下mvn install，看看效果：

```
D:\code\IdeaProjects\javacode2018-parent>mvn install
[INFO] Scanning for projects...
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Build Order:
[INFO]
[INFO] javacode2018-parent                                                [pom]
[INFO] javacode2018-pc                                                    [jar]
[INFO] javacode2018-h5                                                    [jar]
[INFO] javacode2018-api                                                   [jar]
[INFO]
[INFO] ----------------< com.javacode2018:javacode2018-parent >----------------
[INFO] Building javacode2018-parent 1.0-SNAPSHOT                          [1/4]
[INFO] --------------------------------[ pom ]---------------------------------
[INFO]
[INFO] --- maven-install-plugin:2.4:install (default-install) @ javacode2018-parent ---
[INFO] Installing D:\code\IdeaProjects\javacode2018-parent\pom.xml to C:\Users\Think\.m2\repository\com\javacode2018\javacode2018-parent\1.0-SNAPSHOT\javacode2018-parent-1.0-SNAPSHOT.pom
[INFO]
[INFO] ------------------< com.javacode2018:javacode2018-pc >------------------
[INFO] Building javacode2018-pc 1.0-SNAPSHOT                              [2/4]
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ javacode2018-pc ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ javacode2018-pc ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ javacode2018-pc ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] skip non existing resourceDirectory D:\code\IdeaProjects\javacode2018-parent\javacode2018-pc\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ javacode2018-pc ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ javacode2018-pc ---
[INFO] No tests to run.
[INFO]
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ javacode2018-pc ---
[INFO]
[INFO] --- maven-source-plugin:3.2.0:help (attach-source) @ javacode2018-pc ---
[INFO] Apache Maven Source Plugin 3.2.0
  The Maven Source Plugin creates a JAR archive of the source files of the
  current project.

This plugin has 7 goals:

source:aggregate
  Aggregate sources for all modules in an aggregator project.

source:generated-test-jar
  This plugin bundles all the test sources into a jar archive.

source:help
  Display help information on maven-source-plugin.
  Call mvn source:help -Ddetail=true -Dgoal=<goal-name> to display parameter
  details.

source:jar
  This plugin bundles all the sources into a jar archive.

source:jar-no-fork
  This goal bundles all the sources into a jar archive. This goal functions the
  same as the jar goal but does not fork the build and is suitable for attaching
  to the build lifecycle.

source:test-jar
  This plugin bundles all the test sources into a jar archive.

source:test-jar-no-fork
  This goal bundles all the test sources into a jar archive. This goal functions
  the same as the test-jar goal but does not fork the build, and is suitable for
  attaching to the build lifecycle.


[INFO]
[INFO] --- maven-source-plugin:3.2.0:jar-no-fork (attach-source) @ javacode2018-pc ---
[INFO]
[INFO] --- maven-install-plugin:2.4:install (default-install) @ javacode2018-pc ---
[INFO] Installing D:\code\IdeaProjects\javacode2018-parent\javacode2018-pc\target\javacode2018-pc-1.0-SNAPSHOT.jar to C:\Users\Think\.m2\repository\com\javacode2018\javacode2018-pc\1.0-SNAPSHOT\javacode2018-pc-1.0-SNAPSHOT.jar
[INFO] Installing D:\code\IdeaProjects\javacode2018-parent\javacode2018-pc\pom.xml to C:\Users\Think\.m2\repository\com\javacode2018\javacode2018-pc\1.0-SNAPSHOT\javacode2018-pc-1.0-SNAPSHOT.pom
[INFO] Installing D:\code\IdeaProjects\javacode2018-parent\javacode2018-pc\target\javacode2018-pc-1.0-SNAPSHOT-sources.jar to C:\Users\Think\.m2\repository\com\javacode2018\javacode2018-pc\1.0-SNAPSHOT\javacode2018-pc-1.0-SNAPSHOT-sources.jar
[INFO]
[INFO] ------------------< com.javacode2018:javacode2018-h5 >------------------
[INFO] Building javacode2018-h5 1.0-SNAPSHOT                              [3/4]
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ javacode2018-h5 ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ javacode2018-h5 ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ javacode2018-h5 ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] skip non existing resourceDirectory D:\code\IdeaProjects\javacode2018-parent\javacode2018-h5\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ javacode2018-h5 ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ javacode2018-h5 ---
[INFO] No tests to run.
[INFO]
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ javacode2018-h5 ---
[INFO] Building jar: D:\code\IdeaProjects\javacode2018-parent\javacode2018-h5\target\javacode2018-h5-1.0-SNAPSHOT.jar
[INFO]
[INFO] --- maven-install-plugin:2.4:install (default-install) @ javacode2018-h5 ---
[INFO] Installing D:\code\IdeaProjects\javacode2018-parent\javacode2018-h5\target\javacode2018-h5-1.0-SNAPSHOT.jar to C:\Users\Think\.m2\repository\com\javacode2018\javacode2018-h5\1.0-SNAPSHOT\javacode2018-h5-1.0-SNAPSHOT.jar
[INFO] Installing D:\code\IdeaProjects\javacode2018-parent\javacode2018-h5\pom.xml to C:\Users\Think\.m2\repository\com\javacode2018\javacode2018-h5\1.0-SNAPSHOT\javacode2018-h5-1.0-SNAPSHOT.pom
[INFO]
[INFO] -----------------< com.javacode2018:javacode2018-api >------------------
[INFO] Building javacode2018-api 1.0-SNAPSHOT                             [4/4]
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ javacode2018-api ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ javacode2018-api ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ javacode2018-api ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] skip non existing resourceDirectory D:\code\IdeaProjects\javacode2018-parent\javacode2018-api\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ javacode2018-api ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ javacode2018-api ---
[INFO] No tests to run.
[INFO]
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ javacode2018-api ---
[INFO] Building jar: D:\code\IdeaProjects\javacode2018-parent\javacode2018-api\target\javacode2018-api-1.0-SNAPSHOT.jar
[INFO]
[INFO] --- maven-install-plugin:2.4:install (default-install) @ javacode2018-api ---
[INFO] Installing D:\code\IdeaProjects\javacode2018-parent\javacode2018-api\target\javacode2018-api-1.0-SNAPSHOT.jar to C:\Users\Think\.m2\repository\com\javacode2018\javacode2018-api\1.0-SNAPSHOT\javacode2018-api-1.0-SNAPSHOT.jar
[INFO] Installing D:\code\IdeaProjects\javacode2018-parent\javacode2018-api\pom.xml to C:\Users\Think\.m2\repository\com\javacode2018\javacode2018-api\1.0-SNAPSHOT\javacode2018-api-1.0-SNAPSHOT.pom
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary for javacode2018-parent 1.0-SNAPSHOT:
[INFO]
[INFO] javacode2018-parent ................................ SUCCESS [  0.416 s]
[INFO] javacode2018-pc .................................... SUCCESS [  2.654 s]
[INFO] javacode2018-h5 .................................... SUCCESS [  0.262 s]
[INFO] javacode2018-api ................................... SUCCESS [  0.225 s]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  3.827 s
[INFO] Finished at: 2019-11-19T17:51:42+08:00
[INFO] ------------------------------------------------------------------------
```

### 上篇文章中留下的2个问题

#### 问题1

**下面这个配置是干什么的？给哪个插件使用的？**

```
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
</properties>
```

编译代码的时候，涉及到资源文件和测试资源文件的拷贝，拷贝文件的时候涉及到文件的编码，这个是设置文件的编码为`UTF-8`格式的。

我们在上篇文章的`maven-chat06`项目的目录中执行`mvn compile`命令，效果如下：

```
D:\code\IdeaProjects\maven-chat06>mvn compile
[INFO] Scanning for projects...
[INFO]
[INFO] -------------------< com.javacode2018:maven-chat06 >--------------------
[INFO] Building maven-chat06 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ maven-chat06 ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ maven-chat06 ---
[INFO] Nothing to compile - all classes are up to date
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  1.179 s
[INFO] Finished at: 2019-11-19T17:57:27+08:00
[INFO] ------------------------------------------------------------------------
```

可以看到上面有这样的输出：

```
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ maven-chat06 ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
```

从上面可以看出`maven-resources-plugin`插件的`resources`目标中会使用这个编码的配置，我们来看一下这个目标具体参数配置，我们截取了部分输出，如下：

```
D:\code\IdeaProjects\maven-chat06>mvn help:describe -Dplugin=resources -Dgoal=resources  -Ddetail
[INFO] Scanning for projects...
[INFO]
[INFO] -------------------< com.javacode2018:maven-chat06 >--------------------
[INFO] Building maven-chat06 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-help-plugin:3.2.0:describe (default-cli) @ maven-chat06 ---
[INFO] Mojo: 'resources:resources'
resources:resources
  Description: Copy resources for the main source code to the main output
    directory. Always uses the project.build.resources element to specify the
    resources to copy.
  Implementation: org.apache.maven.plugin.resources.ResourcesMojo
  Language: java
  Bound to phase: process-resources

  Available parameters:

    delimiters
      Set of delimiters for expressions to filter within the resources. These
      delimiters are specified in the form 'beginToken*endToken'. If no '*' is
      given, the delimiter is assumed to be the same for start and end.

      So, the default filtering delimiters might be specified as:

      <delimiters>
       <delimiter>${*}</delimiter>
       <delimiter>@</delimiter>
      </delimiters>

      Since the '@' delimiter is the same on both ends, we don't need to
      specify '@*@' (though we can).

    encoding (Default: ${project.build.sourceEncoding})
      User property: encoding
      The character encoding scheme to be applied when filtering resources.
```

注意上面输出中最后的一部分，如下：

```
    encoding (Default: ${project.build.sourceEncoding})
      User property: encoding
      The character encoding scheme to be applied when filtering resources.
```

encoding这个参数用来指定编码的，默认值是`${project.build.sourceEncoding}`，也可以通过`encoding`用户属性来设置。

所以这个设置编码的还有下面3种写法，共四种：

```
pom.xml中2种：
<encoding>UTF-8</encoding>
<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>

mvn命令中2种：
mvn compile -Dencoding=UTF-8
mvn compile -Dproject.build.sourceEncoding=UTF-8
```

#### 问题2

**mvn test运行测试用例的时候，测试用例类名的写法默认是有规则的，这些规则有人知道么？从哪里可以看到这些规则？如何自定义？**

我们在上一篇中的`maven-chat06`项目中执行`mvn test`，看看效果：

```
D:\code\IdeaProjects\maven-chat06>mvn test
[INFO] Scanning for projects...
[INFO]
[INFO] -------------------< com.javacode2018:maven-chat06 >--------------------
[INFO] Building maven-chat06 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ maven-chat06 ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ maven-chat06 ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ maven-chat06 ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ maven-chat06 ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ maven-chat06 ---
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  1.411 s
[INFO] Finished at: 2019-11-19T18:07:52+08:00
[INFO] ------------------------------------------------------------------------
```

可以看到下面这行：

```
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ maven-chat06 ---
```

表示运行测试使用的是插件`maven-surefire-plugin`的`test`目标。

我们看看这个目标详细参数，只列出部分信息，如下：

```
D:\code\IdeaProjects\maven-chat06>mvn help:describe -Dplugin=surefire -Dgoal=test -Ddetail
[INFO] Scanning for projects...
[INFO]
[INFO] -------------------< com.javacode2018:maven-chat06 >--------------------
[INFO] Building maven-chat06 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-help-plugin:3.2.0:describe (default-cli) @ maven-chat06 ---
[INFO] Mojo: 'surefire:test'
surefire:test
  Description: Run tests using Surefire.
  Implementation: org.apache.maven.plugin.surefire.SurefirePlugin
  Language: java
  Bound to phase: test

  Available parameters:

    includes
      A list of <include> elements specifying the tests (by pattern) that
      should be included in testing. When not specified and when the test
      parameter is not specified, the default includes will be
      <includes>
       <include>**/IT*.java</include>
       <include>**/*IT.java</include>
       <include>**/*ITCase.java</include>
      </includes>
      Each include item may also contain a comma-separated sublist of items,
      which will be treated as multiple  <include> entries.
      This parameter is ignored if the TestNG suiteXmlFiles parameter is
      specified.


[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  4.458 s
[INFO] Finished at: 2019-11-19T18:10:09+08:00
[INFO] ------------------------------------------------------------------------
```

可以看到上面有个`includes`参数，可以用来配置需要运行的测试用例，可以配置通配符的方式。

上面还有一段信息：

```
 Implementation: org.apache.maven.plugin.surefire.SurefirePlugin
```

上面这部分列出了这个目标的具体实现类是`SurefirePlugin`。

那么我们可以到本地仓库中看一下这个构件的源码，构件的坐标是：

```
<groupId>org.apache.maven.plugins</groupId>
<artifactId>maven-surefire-plugin</artifactId>
<version>2.12.4</version>
```

打开`org.apache.maven.plugin.surefire.SurefirePlugin`的源码，会找到下面代码：

```
protected String[] getDefaultIncludes()
{
    return new String[]{ "**/Test*.java", "**/*Test.java", "**/*TestCase.java" };
}
```

这部分代码就是我们测试用例默认需要满足的格式，你创建的测试用例默认情况下必须满足上面这3种格式，否则，测试用例不会被`mvn test`执行。