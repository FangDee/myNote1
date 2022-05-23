# Maven第8篇：按需构建，太强了

路人甲Java [程序员路人](javascript:void(0);) *2022-03-17 20:20*

**大家好，我是路人~~~**

**点击下方卡片，关注我，java干货及时送达**

![img](http://mmbiz.qpic.cn/mmbiz_png/kvBL17F8Dr1MFuX9tnHDHzic6yBpLSiaTibZLKcia9tHwm7aFueNjgoAIibdqXibicAUEia87xvDN77VhfDKbo59BJCwlw/0?wx_fmt=png)

**程序员路人**

专注Java相关技术：SSM、Spring全家桶、微服务、MySQL、MyCat、集群、分布式、中间件、Linux、网络、多线程，偶尔讲点运维Jenkins、Nexus、Docker、ELK，偶尔分享些技术干货，致力于Java全栈开发！

1篇原创内容



公众号

**这是 Maven 第 8 篇，点击上方卡片，即可翻阅前面章节。**

本篇涉及到的内容属于神技能，多数使用maven的人都经常想要的一种功能，但是大多数人都不知道如何使用，废话不多说，上干货。

### 需求背景

我们需要做一个电商项目，一般都会做成微服务的形式，按业务进行划分，本次我们主要以**账户业务**和**订单业务**为例，我们将这两块业务分别作为2个大的模块来开发，而订单模块又会调用账户模块中的接口，所以对账户模块接口部分会有依赖。

我们以maven来搭建项目，项目的目录结构如下：

```
b2b
    b2b-account
        b2b-account-api
        b2b-account-service
    b2b-order
        b2b-order-api
        b2b-order-service
```

#### b2b-account

账户模块，其中包含2个小模块：b2b-account-api和b2b-account-service

##### b2b-account-api

账户模块对外暴露的接口部分，以供外部调用

##### b2b-account-service

账户模块具体业务实现，依赖于`b2b-account-api`模块

#### b2b-order

订单模块，也是包含2个小模块：b2b-order-api和b2b-order-service

##### b2b-order-api

订单模块对外暴露的接口部分，以供外部调用

##### b2b-order-service

订单模块具体业务实现，依赖2个模块`b2b-order-api、b2b-account-api`

### 搭建项目结构

我们按照上面的需求，使用maven搭建项目结构，具体过程如下。

#### 创建b2b项目

打开idea，点击`File->New->Project`，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bm8MIia33rFfhn4a3CGtEEGsFGxC15G3MPNZ4Oda3TmHhrFoPeJHALyQobR4femqlFAobDVoUIVMg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

选择`Maven`，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bm8MIia33rFfhn4a3CGtEEGafRSAtia8tlLwibgAuOSoBVGNKFCt3JCIlmLvl4H5GnnQicmwSXEns9Tg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击上图的`Next`，输入坐标信息，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bm8MIia33rFfhn4a3CGtEEGPmMzT3T1R1DPibOBNCaPKR0WP1DBE38c4PW9Z04DtlOWLeduGhJ6BXg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击上图的`Next`，输入`Project Name`为`b2b`，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bm8MIia33rFfhn4a3CGtEEGIpyGsKeFicdlq7cC5ZicdWcGs3At9P4p1rEsxdqcb5N9G1kcEcFDUQrA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击上图的`Finish`，完成项目的创建，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bm8MIia33rFfhn4a3CGtEEG6235P8mqfZuOVW0PznWh8Jib4icTofuNr9Koe1tlMdbjTp0FiaicZ6RckA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

删除下图中无用的代码：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bm8MIia33rFfhn4a3CGtEEGn0H0R023occyA3Zo2kDia7MiasJmr9X0MnIABfaDKVnmd3xiaR9wUwCmw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

变成了下面这样：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bm8MIia33rFfhn4a3CGtEEGibX7iafBIc5oCXZqEZgQEfZptWOuTp7MVQvlx60DUNo4RqhFYbz8BdnQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

配置一下idea的maven环境，点击`File->Settings`，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bm8MIia33rFfhn4a3CGtEEGDwP9QicKO6Ciag56Ku8mWZ8uQdaMPhHUIbEdxRyjKPUo4Y0tn0kWXgiag/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击上面的`OK`完成配置。

#### 在b2b下创建b2b-account模块

> 注意这里是创建模块，不是项目了，注意下面的操作过程。

选中b2b项目目录，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bm8MIia33rFfhn4a3CGtEEGOkiaGsW3VH9C9Fl08dibnY1EmQl4mcSnVicWib8ribd7JMwBVkldQ6w0tMg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击`File->New->Module`，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bm8MIia33rFfhn4a3CGtEEGQ26jWcpnvSe6FTlGVvibBZ9nYnm1NOzdeXs8AYZb0pic2SWriaNheOMoA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

选择`Maven`，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bm8MIia33rFfhn4a3CGtEEG9zJKWS0pqjq1icJA2S0pEDkfhtjhnVQjbFUte5nNTSM1EGpHasXfsmw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击`Next`，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bm8MIia33rFfhn4a3CGtEEGibicr4XbvHbXkRyt7jdtjxgW95LoVJT4gSVAMxrSRXRfFC8JeBiaGK1ZA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击上面第二个`...`，将Parent置为`None`，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bm8MIia33rFfhn4a3CGtEEG22fouzibS7zIFzIUibGHnn1ex7O80LC2U257NFpia76uusQOe1zg8qs7Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bm8MIia33rFfhn4a3CGtEEGQOLb1VvyD5J1rKsDGNvn6y0tuGv3hCL0wZvGnqlXicMtibKceo7TQsIQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bm8MIia33rFfhn4a3CGtEEGib47J69UURMXZclic2PH49wr6icUrbVDmjYNM3J4gSzvIMIQDHNUDtNWQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

输入坐标信息，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bm8MIia33rFfhn4a3CGtEEG54giaZVbzmSKJndynOH7Q68e08uzribJq1NRfacZGE5laOybdIuejyicw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击上图中的`Next`，输入`Project name`为`b2b-account`，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bm8MIia33rFfhn4a3CGtEEGdicXQqBWoN5XZNdOopHjzgwNklN3VAHEBQ2y6X2hBDD34ObOX6gcXRA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击`Finish`，完成`b2b-account`模块的创建，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bm8MIia33rFfhn4a3CGtEEGibkvibSbdd8plx92VKS1CaKp95S7Ighh5iacY9M2cOO0xWhE85x306QQw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

删除`b2b-account`中`src`目录，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bm8MIia33rFfhn4a3CGtEEGUf3WWXIYM1EmcaOSSIeObfOp8wmCyNiaM7ajMHfkdReViafMncjKpPvw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

项目结构变成了下面这样：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bm8MIia33rFfhn4a3CGtEEGaraNqlRBJBUlzTs002icPCKoIB0icRcBe4dIP7wguVB1gjtXiaF0x0VAw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

#### 在b2b-account模块下面创建b2b-account-api模块

选中`b2b-account`，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bm8MIia33rFfhn4a3CGtEEGBLdTGvo7ibTCf1H2h91PUpYw2nu72PLQr1Gz01jUvjKQIacGPk4Ua9Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击`File->New->Module`，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bm8MIia33rFfhn4a3CGtEEG5IXK3YAMK39micCEFuFkKrNqKVMqfg6BmsGG2YjWUSrpMJ03V8upw4w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

选择`Maven`，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bm8MIia33rFfhn4a3CGtEEGejhJzUSU41LWSLyTX7sAyfxJLUl1tQgH85uFwvHL0RjbwCryB0GiaPg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击上图中的`Next`，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bm8MIia33rFfhn4a3CGtEEGvNNRWFj0aF7ziavELTcXVOdMGtk4PAuly0GJLFxTVicUIaZ8icWkOY3GQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

将上面的`Add as module to`设置为`b2b-account`模块，表示新增的这个模块被`b2b-account`聚合进行管理。

点击`Parent`后面的`...`，选择`None`，将`Parent`对应的值置为`None`。

如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bm8MIia33rFfhn4a3CGtEEGaV3kqCcwW6n6aJibb4yicMTDjkz4z0XzresulXSyjia6ianlTgqVmOlhxw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bm8MIia33rFfhn4a3CGtEEGxgnyT9fwBRR5gEqEIWQKoZkPEouWCc44CcbhEb9BJp7VGsR76U7uqQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bm8MIia33rFfhn4a3CGtEEG71urVWTm6G6ooMNLPjXHTiadXv7cJu43QMuxicZ18otvrAIlsRVru1SA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

输入坐标信息，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bm8MIia33rFfhn4a3CGtEEG7JW0I7TCvMMomKNl1zeUBCic6LljERAZdlWviaeqs5171ib4JDXiaTfIzQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击`Next`，默认如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bm8MIia33rFfhn4a3CGtEEGmIFjnAq5xBagwhucvyf94H2TV2EJdEMkRnbyC4Bhsln0gB0Czsg2Hg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

我们需要对上面3个输入框的值进行修改，修改成下面这样：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bm8MIia33rFfhn4a3CGtEEGsYMKd7JfsruNEtot69meiceJ3S5gIZQyCU5osCGyticD0GdIK2TWQATQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击`Finish`，完成创建，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bm8MIia33rFfhn4a3CGtEEGhWC9rjjNXhR91gGILbWjPHgmTyn8iaWvvA9yDbCrGHjDFTFcGLv2VEg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

#### 在b2b-account模块下创建b2b-account-service模块

这个过程参考`在b2b-account模块下面创建b2b-account-api模块`。

#### 在b2b下创建b2b-order模块

这个过程参考`在b2b下创建b2b-account模块`。

#### 在b2b-order模块下面创建b2b-order-api模块

这个过程参考`在b2b-account模块下面创建b2b-account-api模块`。

#### 在b2b-order模块下面创建b2b-order-service模块

这个过程参考`在b2b-account模块下面创建b2b-account-api模块`。

#### 项目结构创建成功

上面都创建成功之后，项目结构如下图

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06Bm8MIia33rFfhn4a3CGtEEGSAvVMeUpvQP3xCVb6oI7haQ39483eDDIObl0QNKsMXS5df0qialfEibw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

参与过大型maven项目开发的是不是很熟悉很亲切，多数互联网项目的maven结构就是这样一大片maven模块。

#### 引入项目依赖

b2b-account-service依赖于`b2b-account-api`模块，所以在`b2b-account-service/pom.xml`中加入下面配置：

```
<dependencies>
    <dependency>
        <groupId>${project.groupId}</groupId>
        <artifactId>b2b-account-api</artifactId>
        <version>${project.version}</version>
    </dependency>
</dependencies>
```

> 由于项目的`groupId,version`都是一样的，所以直接通过`${}`这种方式引用了。

b2b-order-service依赖2个模块`b2b-order-api、b2b-account-api`，所以在`b2b-order-service/pom.xml`中加入下面配置：

```
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
```

#### 此时每个模块pom.xml如下

##### b2b/pom.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.javacode2018</groupId>
    <artifactId>b2b</artifactId>
    <packaging>pom</packaging>
    <version>1.0-SNAPSHOT</version>
    <modules>
        <module>b2b-account</module>
        <module>b2b-order</module>
    </modules>

</project>
```

##### b2b-account/pom.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.javacode2018</groupId>
    <artifactId>b2b-account</artifactId>
    <packaging>pom</packaging>
    <version>1.0-SNAPSHOT</version>
    <modules>
        <module>b2b-account-api</module>
        <module>b2b-account-service</module>
    </modules>

</project>
```

##### b2b-account-api/pom.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.javacode2018</groupId>
    <artifactId>b2b-account-api</artifactId>
    <version>1.0-SNAPSHOT</version>

</project>
```

##### b2b-account-service/pom.xml

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
            <groupId>${project.groupId}</groupId>
            <artifactId>b2b-account-api</artifactId>
            <version>${project.version}</version>
        </dependency>
    </dependencies>

</project>
```

##### b2b-order/pom.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.javacode2018</groupId>
    <artifactId>b2b-order</artifactId>
    <packaging>pom</packaging>
    <version>1.0-SNAPSHOT</version>
    <modules>
        <module>b2b-order-api</module>
        <module>b2b-order-service</module>
    </modules>

</project>
```

##### b2b-order-api/pom.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.javacode2018</groupId>
    <artifactId>b2b-order-api</artifactId>
    <version>1.0-SNAPSHOT</version>

</project>
```

##### b2b-order-service/pom.xml

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

</project>
```

> 注意：上面项目的结构属于父子结构，使用maven中聚合的方式进行管理的，对应聚合不是太了解的，可以看上篇文章有详细介绍。
>
> b2b中包含了2个子模块`b2b_account`和`b2b_order`，这3个package都是pom。
>
> b2b_account中包含了2个子模块，2个子模块package是默认的jar类型。
>
> b2b_order中包含了2个子模块，2个子模块package是默认的jar类型。

### 反应堆

上面项目都开发好了，我们需要安装到本地仓库，一般情况下，我们会这么做，在`b2b/pom.xml`所在目录执行下面命令：

```
mvn clean install
```

我们来感受一下这个命令的效果：

```
D:\code\IdeaProjects\b2b>mvn clean install
[INFO] Scanning for projects...
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Build Order:
[INFO]
[INFO] b2b-account-api                                                    [jar]
[INFO] b2b-account-service                                                [jar]
[INFO] b2b-account                                                        [pom]
[INFO] b2b-order-api                                                      [jar]
[INFO] b2b-order-service                                                  [jar]
[INFO] b2b-order                                                          [pom]
[INFO] b2b                                                                [pom]
[INFO]
[INFO] ------------------< com.javacode2018:b2b-account-api >------------------
[INFO] Building b2b-account-api 1.0-SNAPSHOT                              [1/7]
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ b2b-account-api ---
[INFO] Deleting D:\code\IdeaProjects\b2b\b2b-account\b2b-account-api\target
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ b2b-account-api ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ b2b-account-api ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ b2b-account-api ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] skip non existing resourceDirectory D:\code\IdeaProjects\b2b\b2b-account\b2b-account-api\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ b2b-account-api ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ b2b-account-api ---
[INFO] No tests to run.
[INFO]
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ b2b-account-api ---
[INFO] Building jar: D:\code\IdeaProjects\b2b\b2b-account\b2b-account-api\target\b2b-account-api-1.0-SNAPSHOT.jar
[INFO]
[INFO] --- maven-install-plugin:2.4:install (default-install) @ b2b-account-api ---
[INFO] Installing D:\code\IdeaProjects\b2b\b2b-account\b2b-account-api\target\b2b-account-api-1.0-SNAPSHOT.jar to C:\Users\Think\.m2\repository\com\javacode2018\b2b-account-api\1.0-SNAPSHOT\b2b-account-api-1.0-SNAPSHOT.jar
[INFO] Installing D:\code\IdeaProjects\b2b\b2b-account\b2b-account-api\pom.xml to C:\Users\Think\.m2\repository\com\javacode2018\b2b-account-api\1.0-SNAPSHOT\b2b-account-api-1.0-SNAPSHOT.pom
[INFO]
[INFO] ----------------< com.javacode2018:b2b-account-service >----------------
[INFO] Building b2b-account-service 1.0-SNAPSHOT                          [2/7]
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ b2b-account-service ---
[INFO] Deleting D:\code\IdeaProjects\b2b\b2b-account\b2b-account-service\target
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ b2b-account-service ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ b2b-account-service ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ b2b-account-service ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] skip non existing resourceDirectory D:\code\IdeaProjects\b2b\b2b-account\b2b-account-service\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ b2b-account-service ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ b2b-account-service ---
[INFO] No tests to run.
[INFO]
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ b2b-account-service ---
[INFO] Building jar: D:\code\IdeaProjects\b2b\b2b-account\b2b-account-service\target\b2b-account-service-1.0-SNAPSHOT.jar
[INFO]
[INFO] --- maven-install-plugin:2.4:install (default-install) @ b2b-account-service ---
[INFO] Installing D:\code\IdeaProjects\b2b\b2b-account\b2b-account-service\target\b2b-account-service-1.0-SNAPSHOT.jar to C:\Users\Think\.m2\repository\com\javacode2018\b2b-account-service\1.0-SNAPSHOT\b2b-account-service-1.0-SNAPSHOT.jar
[INFO] Installing D:\code\IdeaProjects\b2b\b2b-account\b2b-account-service\pom.xml to C:\Users\Think\.m2\repository\com\javacode2018\b2b-account-service\1.0-SNAPSHOT\b2b-account-service-1.0-SNAPSHOT.pom
[INFO]
[INFO] --------------------< com.javacode2018:b2b-account >--------------------
[INFO] Building b2b-account 1.0-SNAPSHOT                                  [3/7]
[INFO] --------------------------------[ pom ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ b2b-account ---
[INFO]
[INFO] --- maven-install-plugin:2.4:install (default-install) @ b2b-account ---
[INFO] Installing D:\code\IdeaProjects\b2b\b2b-account\pom.xml to C:\Users\Think\.m2\repository\com\javacode2018\b2b-account\1.0-SNAPSHOT\b2b-account-1.0-SNAPSHOT.pom
[INFO]
[INFO] -------------------< com.javacode2018:b2b-order-api >-------------------
[INFO] Building b2b-order-api 1.0-SNAPSHOT                                [4/7]
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ b2b-order-api ---
[INFO] Deleting D:\code\IdeaProjects\b2b\b2b-order\b2b-order-api\target
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ b2b-order-api ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ b2b-order-api ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ b2b-order-api ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] skip non existing resourceDirectory D:\code\IdeaProjects\b2b\b2b-order\b2b-order-api\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ b2b-order-api ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ b2b-order-api ---
[INFO] No tests to run.
[INFO]
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ b2b-order-api ---
[INFO] Building jar: D:\code\IdeaProjects\b2b\b2b-order\b2b-order-api\target\b2b-order-api-1.0-SNAPSHOT.jar
[INFO]
[INFO] --- maven-install-plugin:2.4:install (default-install) @ b2b-order-api ---
[INFO] Installing D:\code\IdeaProjects\b2b\b2b-order\b2b-order-api\target\b2b-order-api-1.0-SNAPSHOT.jar to C:\Users\Think\.m2\repository\com\javacode2018\b2b-order-api\1.0-SNAPSHOT\b2b-order-api-1.0-SNAPSHOT.jar
[INFO] Installing D:\code\IdeaProjects\b2b\b2b-order\b2b-order-api\pom.xml to C:\Users\Think\.m2\repository\com\javacode2018\b2b-order-api\1.0-SNAPSHOT\b2b-order-api-1.0-SNAPSHOT.pom
[INFO]
[INFO] -----------------< com.javacode2018:b2b-order-service >-----------------
[INFO] Building b2b-order-service 1.0-SNAPSHOT                            [5/7]
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ b2b-order-service ---
[INFO] Deleting D:\code\IdeaProjects\b2b\b2b-order\b2b-order-service\target
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ b2b-order-service ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ b2b-order-service ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ b2b-order-service ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
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
[INFO]
[INFO] --- maven-install-plugin:2.4:install (default-install) @ b2b-order-service ---
[INFO] Installing D:\code\IdeaProjects\b2b\b2b-order\b2b-order-service\target\b2b-order-service-1.0-SNAPSHOT.jar to C:\Users\Think\.m2\repository\com\javacode2018\b2b-order-service\1.0-SNAPSHOT\b2b-order-service-1.0-SNAPSHOT.jar
[INFO] Installing D:\code\IdeaProjects\b2b\b2b-order\b2b-order-service\pom.xml to C:\Users\Think\.m2\repository\com\javacode2018\b2b-order-service\1.0-SNAPSHOT\b2b-order-service-1.0-SNAPSHOT.pom
[INFO]
[INFO] ---------------------< com.javacode2018:b2b-order >---------------------
[INFO] Building b2b-order 1.0-SNAPSHOT                                    [6/7]
[INFO] --------------------------------[ pom ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ b2b-order ---
[INFO]
[INFO] --- maven-install-plugin:2.4:install (default-install) @ b2b-order ---
[INFO] Installing D:\code\IdeaProjects\b2b\b2b-order\pom.xml to C:\Users\Think\.m2\repository\com\javacode2018\b2b-order\1.0-SNAPSHOT\b2b-order-1.0-SNAPSHOT.pom
[INFO]
[INFO] ------------------------< com.javacode2018:b2b >------------------------
[INFO] Building b2b 1.0-SNAPSHOT                                          [7/7]
[INFO] --------------------------------[ pom ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ b2b ---
[INFO]
[INFO] --- maven-install-plugin:2.4:install (default-install) @ b2b ---
[INFO] Installing D:\code\IdeaProjects\b2b\pom.xml to C:\Users\Think\.m2\repository\com\javacode2018\b2b\1.0-SNAPSHOT\b2b-1.0-SNAPSHOT.pom
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary for b2b 1.0-SNAPSHOT:
[INFO]
[INFO] b2b-account-api .................................... SUCCESS [  1.806 s]
[INFO] b2b-account-service ................................ SUCCESS [  0.298 s]
[INFO] b2b-account ........................................ SUCCESS [  0.082 s]
[INFO] b2b-order-api ...................................... SUCCESS [  0.229 s]
[INFO] b2b-order-service .................................. SUCCESS [  0.254 s]
[INFO] b2b-order .......................................... SUCCESS [  0.058 s]
[INFO] b2b ................................................ SUCCESS [  0.069 s]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  3.072 s
[INFO] Finished at: 2019-11-20T16:03:27+08:00
[INFO] ------------------------------------------------------------------------
```

注意上面有这样的输出：

```
[INFO] Reactor Build Order:
[INFO]
[INFO] b2b-account-api                                                    [jar]
[INFO] b2b-account-service                                                [jar]
[INFO] b2b-account                                                        [pom]
[INFO] b2b-order-api                                                      [jar]
[INFO] b2b-order-service                                                  [jar]
[INFO] b2b-order                                                          [pom]
[INFO] b2b                                                                [pom]
```

maven会根据模块之间的依赖关系，然后会得到所有模块的构建顺序，上面共有7个pom.xml文件，也就是7个maven构件，按照上面列出的顺序依次进行构建。

可以看到`b2b-account-api`是被其他一些模块依赖的，所以这个放在了第一个。

`mvn`命令对多模块构件时，会根据模块的依赖关系而得到模块的构建顺序，这个功能就是maven的反应堆（reactor）做的事情，反应堆会根据模块之间的依赖关系、聚合关系、继承关系等等，从而计算得出一个合理的模块构建顺序，所以反应堆的功能是相当强大的。

### 按需随意构建

有这样的一种场景：`b2b-account-api`被`b2b-account-service`和`b2b-order-service`依赖了，所以当`b2b-account-api`有修改的时候，我们希望他们3个都能够被重新构建一次，而不是去对所有的模块都进行重新构建，我们只希望被影响的模块都能够参与重新构建，大家有什么好的办法？

上面列出的只是2个模块的功能，真正的电商项目还有很多模块，如果每次修改一个模块，我们都去重新打包所有的模块，这个构建过程耗时是非常久的，只能干等着，我们需要的是按需构建，需要构建哪些模块让我们自己能够随意指定，这样也可以加快构建的速度，所以我们需要这样的功能

maven反应堆帮我们考虑到了这种情况，mvn命令提供了一些功能可以帮我们实现这些操作，我们看一下mvn的帮助文档，`mvn -h`可以查看帮助，如下：

```
D:\code\IdeaProjects\b2b>mvn -h

usage: mvn [options] [<goal(s)>] [<phase(s)>]

Options:
 -am,--also-make                        If project list is specified, also
                                        build projects required by the
                                        list
 -amd,--also-make-dependents            If project list is specified, also
                                        build projects that depend on
                                        projects on the list
 -B,--batch-mode                        Run in non-interactive (batch)
                                        mode (disables output color)
 -b,--builder <arg>                     The id of the build strategy to
                                        use
 -C,--strict-checksums                  Fail the build if checksums don't
                                        match
 -c,--lax-checksums                     Warn if checksums don't match
 -cpu,--check-plugin-updates            Ineffective, only kept for
                                        backward compatibility
 -D,--define <arg>                      Define a system property
 -e,--errors                            Produce execution error messages
 -emp,--encrypt-master-password <arg>   Encrypt master security password
 -ep,--encrypt-password <arg>           Encrypt server password
 -f,--file <arg>                        Force the use of an alternate POM
                                        file (or directory with pom.xml)
 -fae,--fail-at-end                     Only fail the build afterwards;
                                        allow all non-impacted builds to
                                        continue
 -ff,--fail-fast                        Stop at first failure in
                                        reactorized builds
 -fn,--fail-never                       NEVER fail the build, regardless
                                        of project result
 -gs,--global-settings <arg>            Alternate path for the global
                                        settings file
 -gt,--global-toolchains <arg>          Alternate path for the global
                                        toolchains file
 -h,--help                              Display help information
 -l,--log-file <arg>                    Log file where all build output
                                        will go (disables output color)
 -llr,--legacy-local-repository         Use Maven 2 Legacy Local
                                        Repository behaviour, ie no use of
                                        _remote.repositories. Can also be
                                        activated by using
                                        -Dmaven.legacyLocalRepo=true
 -N,--non-recursive                     Do not recurse into sub-projects
 -npr,--no-plugin-registry              Ineffective, only kept for
                                        backward compatibility
 -npu,--no-plugin-updates               Ineffective, only kept for
                                        backward compatibility
 -nsu,--no-snapshot-updates             Suppress SNAPSHOT updates
 -ntp,--no-transfer-progress            Do not display transfer progress
                                        when downloading or uploading
 -o,--offline                           Work offline
 -P,--activate-profiles <arg>           Comma-delimited list of profiles
                                        to activate
 -pl,--projects <arg>                   Comma-delimited list of specified
                                        reactor projects to build instead
                                        of all projects. A project can be
                                        specified by [groupId]:artifactId
                                        or by its relative path
 -q,--quiet                             Quiet output - only show errors
 -rf,--resume-from <arg>                Resume reactor from specified
                                        project
 -s,--settings <arg>                    Alternate path for the user
                                        settings file
 -t,--toolchains <arg>                  Alternate path for the user
                                        toolchains file
 -T,--threads <arg>                     Thread count, for instance 2.0C
                                        where C is core multiplied
 -U,--update-snapshots                  Forces a check for missing
                                        releases and updated snapshots on
                                        remote repositories
 -up,--update-plugins                   Ineffective, only kept for
                                        backward compatibility
 -v,--version                           Display version information
 -V,--show-version                      Display version information
                                        WITHOUT stopping build
 -X,--debug                             Produce execution debug output
```

上面列出了`mvn`命令后面的一些选项，有几个选项本次我们需要用到，如下：

#### -pl,--projects <arg>

构件指定的模块，arg表示多个模块，之间用逗号分开，模块有两种写法

```
-pl 模块1相对路径 [,模块2相对路径] [,模块n相对路径]
-pl [模块1的groupId]:模块1的artifactId [,[模块2的groupId]:模块2的artifactId] [,[模块n的groupId]:模块n的artifactId]
```

举例：

> 下面命令都是在b2b/pom.xml来运行

```
mvn clean install -pl b2b-account
mvn clean install -pl b2b-account/b2b-account-api
mvn clean install -pl b2b-account/b2b-account-api,b2b-account/b2b-account-service
mvn clean install -pl :b2b-account-api,b2b-order/b2b-order-api
mvn clean install -pl :b2b-account-api,:b2b-order-service
```

#### -rf,--resume-from <arg>

从指定的模块恢复反应堆

#### -am,--also-make

同时构建所列模块的依赖模块

#### -amd,--also-make-dependents

同时构件依赖于所列模块的模块

下面我们通过6个案例细说上面这些用法。

**注意：下面所有案例都在`b2b/pom.xml`所在目录执行。**

### 案例1

只构建`p-account`模块，运行命令

```
mvn clean install -pl b2b-account
```

效果如下：

```
D:\code\IdeaProjects\b2b>mvn clean install -pl b2b-account
[INFO] Scanning for projects...
[INFO]
[INFO] --------------------< com.javacode2018:b2b-account >--------------------
[INFO] Building b2b-account 1.0-SNAPSHOT
[INFO] --------------------------------[ pom ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ b2b-account ---
[INFO]
[INFO] --- maven-install-plugin:2.4:install (default-install) @ b2b-account ---
[INFO] Installing D:\code\IdeaProjects\b2b\b2b-account\pom.xml to C:\Users\Think\.m2\repository\com\javacode2018\b2b-account\1.0-SNAPSHOT\b2b-account-1.0-SNAPSHOT.pom
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  0.907 s
[INFO] Finished at: 2019-11-20T16:12:51+08:00
[INFO] ------------------------------------------------------------------------
```

可以看到只构建了`b2b-account`。

### 案例2

只构建b2b-account-api模块，运行命令：

```
mvn clean install -pl b2b-account/b2b-account-api
```

效果如下：

```
D:\code\IdeaProjects\b2b>mvn clean install -pl b2b-account/b2b-account-api
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------< com.javacode2018:b2b-account-api >------------------
[INFO] Building b2b-account-api 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ b2b-account-api ---
[INFO] Deleting D:\code\IdeaProjects\b2b\b2b-account\b2b-account-api\target
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ b2b-account-api ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ b2b-account-api ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ b2b-account-api ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] skip non existing resourceDirectory D:\code\IdeaProjects\b2b\b2b-account\b2b-account-api\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ b2b-account-api ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ b2b-account-api ---
[INFO] No tests to run.
[INFO]
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ b2b-account-api ---
[INFO] Building jar: D:\code\IdeaProjects\b2b\b2b-account\b2b-account-api\target\b2b-account-api-1.0-SNAPSHOT.jar
[INFO]
[INFO] --- maven-install-plugin:2.4:install (default-install) @ b2b-account-api ---
[INFO] Installing D:\code\IdeaProjects\b2b\b2b-account\b2b-account-api\target\b2b-account-api-1.0-SNAPSHOT.jar to C:\Users\Think\.m2\repository\com\javacode2018\b2b-account-api\1.0-SNAPSHOT\b2b-account-api-1.0-SNAPSHOT.jar
[INFO] Installing D:\code\IdeaProjects\b2b\b2b-account\b2b-account-api\pom.xml to C:\Users\Think\.m2\repository\com\javacode2018\b2b-account-api\1.0-SNAPSHOT\b2b-account-api-1.0-SNAPSHOT.pom
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  2.853 s
[INFO] Finished at: 2019-11-20T16:14:37+08:00
[INFO] ------------------------------------------------------------------------
```

上面使用的是相对路径的写法，还有2种写法，大家可以去试试，如下：

```
mvn clean install -pl :b2b-account-api
mvn clean install -pl com.javacode2018:b2b-account-api
```

### 案例3

构建b2b-account-api和b2b-order-api，运行下面命令：

```
mvn clean install -pl b2b-account/b2b-account-api,b2b-order/b2b-order-api
```

效果如下：

```
D:\code\IdeaProjects\b2b>mvn clean install -pl b2b-account/b2b-account-api,b2b-order/b2b-order-api
[INFO] Scanning for projects...
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Build Order:
[INFO]
[INFO] b2b-account-api                                                    [jar]
[INFO] b2b-order-api                                                      [jar]
[INFO]
[INFO] ------------------< com.javacode2018:b2b-account-api >------------------
[INFO] Building b2b-account-api 1.0-SNAPSHOT                              [1/2]
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ b2b-account-api ---
[INFO] Deleting D:\code\IdeaProjects\b2b\b2b-account\b2b-account-api\target
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ b2b-account-api ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ b2b-account-api ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ b2b-account-api ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] skip non existing resourceDirectory D:\code\IdeaProjects\b2b\b2b-account\b2b-account-api\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ b2b-account-api ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ b2b-account-api ---
[INFO] No tests to run.
[INFO]
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ b2b-account-api ---
[INFO] Building jar: D:\code\IdeaProjects\b2b\b2b-account\b2b-account-api\target\b2b-account-api-1.0-SNAPSHOT.jar
[INFO]
[INFO] --- maven-install-plugin:2.4:install (default-install) @ b2b-account-api ---
[INFO] Installing D:\code\IdeaProjects\b2b\b2b-account\b2b-account-api\target\b2b-account-api-1.0-SNAPSHOT.jar to C:\Users\Think\.m2\repository\com\javacode2018\b2b-account-api\1.0-SNAPSHOT\b2b-account-api-1.0-SNAPSHOT.jar
[INFO] Installing D:\code\IdeaProjects\b2b\b2b-account\b2b-account-api\pom.xml to C:\Users\Think\.m2\repository\com\javacode2018\b2b-account-api\1.0-SNAPSHOT\b2b-account-api-1.0-SNAPSHOT.pom
[INFO]
[INFO] -------------------< com.javacode2018:b2b-order-api >-------------------
[INFO] Building b2b-order-api 1.0-SNAPSHOT                                [2/2]
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ b2b-order-api ---
[INFO] Deleting D:\code\IdeaProjects\b2b\b2b-order\b2b-order-api\target
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ b2b-order-api ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ b2b-order-api ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ b2b-order-api ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] skip non existing resourceDirectory D:\code\IdeaProjects\b2b\b2b-order\b2b-order-api\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ b2b-order-api ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ b2b-order-api ---
[INFO] No tests to run.
[INFO]
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ b2b-order-api ---
[INFO] Building jar: D:\code\IdeaProjects\b2b\b2b-order\b2b-order-api\target\b2b-order-api-1.0-SNAPSHOT.jar
[INFO]
[INFO] --- maven-install-plugin:2.4:install (default-install) @ b2b-order-api ---
[INFO] Installing D:\code\IdeaProjects\b2b\b2b-order\b2b-order-api\target\b2b-order-api-1.0-SNAPSHOT.jar to C:\Users\Think\.m2\repository\com\javacode2018\b2b-order-api\1.0-SNAPSHOT\b2b-order-api-1.0-SNAPSHOT.jar
[INFO] Installing D:\code\IdeaProjects\b2b\b2b-order\b2b-order-api\pom.xml to C:\Users\Think\.m2\repository\com\javacode2018\b2b-order-api\1.0-SNAPSHOT\b2b-order-api-1.0-SNAPSHOT.pom
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary for b2b-account-api 1.0-SNAPSHOT:
[INFO]
[INFO] b2b-account-api .................................... SUCCESS [  2.874 s]
[INFO] b2b-order-api ...................................... SUCCESS [  0.393 s]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  3.554 s
[INFO] Finished at: 2019-11-20T16:15:57+08:00
[INFO] ------------------------------------------------------------------------
```

> 上面输出中可以看到只构建了2个目标模块。

### 案例4

我们只修改了b2b-account-api代码，所以我希望对这个重新打包，并且对这个有依赖的也重新打包，所以需要打包下面3个模块：

```
b2b-account-api
b2b-account-service
b2b-order-service
```

上面列表中的后面两个是依赖于`b2b-account-api`的，可以在b2b/pom.xml中执行下面命令：

```
mvn clean install -pl b2b-account/b2b-account-api -amd
```

我们看一下效果：

```
D:\code\IdeaProjects\b2b>mvn clean install -pl b2b-account/b2b-account-api -amd
[INFO] Scanning for projects...
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Build Order:
[INFO]
[INFO] b2b-account-api                                                    [jar]
[INFO] b2b-account-service                                                [jar]
[INFO] b2b-order-service                                                  [jar]
[INFO]
[INFO] ------------------< com.javacode2018:b2b-account-api >------------------
[INFO] Building b2b-account-api 1.0-SNAPSHOT                              [1/3]
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ b2b-account-api ---
[INFO] Deleting D:\code\IdeaProjects\b2b\b2b-account\b2b-account-api\target
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ b2b-account-api ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ b2b-account-api ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ b2b-account-api ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] skip non existing resourceDirectory D:\code\IdeaProjects\b2b\b2b-account\b2b-account-api\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ b2b-account-api ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ b2b-account-api ---
[INFO] No tests to run.
[INFO]
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ b2b-account-api ---
[INFO] Building jar: D:\code\IdeaProjects\b2b\b2b-account\b2b-account-api\target\b2b-account-api-1.0-SNAPSHOT.jar
[INFO]
[INFO] --- maven-install-plugin:2.4:install (default-install) @ b2b-account-api ---
[INFO] Installing D:\code\IdeaProjects\b2b\b2b-account\b2b-account-api\target\b2b-account-api-1.0-SNAPSHOT.jar to C:\Users\Think\.m2\repository\com\javacode2018\b2b-account-api\1.0-SNAPSHOT\b2b-account-api-1.0-SNAPSHOT.jar
[INFO] Installing D:\code\IdeaProjects\b2b\b2b-account\b2b-account-api\pom.xml to C:\Users\Think\.m2\repository\com\javacode2018\b2b-account-api\1.0-SNAPSHOT\b2b-account-api-1.0-SNAPSHOT.pom
[INFO]
[INFO] ----------------< com.javacode2018:b2b-account-service >----------------
[INFO] Building b2b-account-service 1.0-SNAPSHOT                          [2/3]
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ b2b-account-service ---
[INFO] Deleting D:\code\IdeaProjects\b2b\b2b-account\b2b-account-service\target
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ b2b-account-service ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ b2b-account-service ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ b2b-account-service ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] skip non existing resourceDirectory D:\code\IdeaProjects\b2b\b2b-account\b2b-account-service\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ b2b-account-service ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ b2b-account-service ---
[INFO] No tests to run.
[INFO]
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ b2b-account-service ---
[INFO] Building jar: D:\code\IdeaProjects\b2b\b2b-account\b2b-account-service\target\b2b-account-service-1.0-SNAPSHOT.jar
[INFO]
[INFO] --- maven-install-plugin:2.4:install (default-install) @ b2b-account-service ---
[INFO] Installing D:\code\IdeaProjects\b2b\b2b-account\b2b-account-service\target\b2b-account-service-1.0-SNAPSHOT.jar to C:\Users\Think\.m2\repository\com\javacode2018\b2b-account-service\1.0-SNAPSHOT\b2b-account-service-1.0-SNAPSHOT.jar
[INFO] Installing D:\code\IdeaProjects\b2b\b2b-account\b2b-account-service\pom.xml to C:\Users\Think\.m2\repository\com\javacode2018\b2b-account-service\1.0-SNAPSHOT\b2b-account-service-1.0-SNAPSHOT.pom
[INFO]
[INFO] -----------------< com.javacode2018:b2b-order-service >-----------------
[INFO] Building b2b-order-service 1.0-SNAPSHOT                            [3/3]
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ b2b-order-service ---
[INFO] Deleting D:\code\IdeaProjects\b2b\b2b-order\b2b-order-service\target
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ b2b-order-service ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ b2b-order-service ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ b2b-order-service ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
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
[INFO]
[INFO] --- maven-install-plugin:2.4:install (default-install) @ b2b-order-service ---
[INFO] Installing D:\code\IdeaProjects\b2b\b2b-order\b2b-order-service\target\b2b-order-service-1.0-SNAPSHOT.jar to C:\Users\Think\.m2\repository\com\javacode2018\b2b-order-service\1.0-SNAPSHOT\b2b-order-service-1.0-SNAPSHOT.jar
[INFO] Installing D:\code\IdeaProjects\b2b\b2b-order\b2b-order-service\pom.xml to C:\Users\Think\.m2\repository\com\javacode2018\b2b-order-service\1.0-SNAPSHOT\b2b-order-service-1.0-SNAPSHOT.pom
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary for b2b-account-api 1.0-SNAPSHOT:
[INFO]
[INFO] b2b-account-api .................................... SUCCESS [  2.440 s]
[INFO] b2b-account-service ................................ SUCCESS [  0.381 s]
[INFO] b2b-order-service .................................. SUCCESS [  0.364 s]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  3.508 s
[INFO] Finished at: 2019-11-20T16:05:00+08:00
[INFO] ------------------------------------------------------------------------
```

> 从上面输出中看一下反应堆列出的构建顺序：b2b-account-api、b2b-account-service、b2b-order-service。

上面过程给大家捋一捋：

上面命令先会运行`-pl b2b-account-api`得到一个反应堆列表，如下，只有一个模块：

```
b2b-account-api
```

然后后面又会执行`amd`，这个命令会找到对`-pl b2b-account-api`有依赖的构件，也就是：

```
b2b-account-service
b2b-order-serivce
```

然后反应堆会对3个构件进行排序，得到一个正确的构件顺序，如下：

```
[INFO] Reactor Build Order:
[INFO]
[INFO] b2b-account-api                                                    [jar]
[INFO] b2b-account-service                                                [jar]
[INFO] b2b-order-service                                                  [jar]
```

然后maven会依次对他们执行：

```
mvn clean install
```

### 案例5

需求：我们来构建b2b-order-service，希望b2b-account-service依赖的构件也能被同时构建，b2b-account-service依赖于`b2b-account-api`和`b2b-order-api`，可能b2b-account-service会依赖的更多。

可以使用下面命令：

```
mvn clean install -pl b2b-order/b2b-order-service -am
```

效果如下：

```
D:\code\IdeaProjects\b2b>mvn clean install -pl b2b-order/b2b-order-service -am
[INFO] Scanning for projects...
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Build Order:
[INFO]
[INFO] b2b-account-api                                                    [jar]
[INFO] b2b-order-api                                                      [jar]
[INFO] b2b-order-service                                                  [jar]
[INFO]
[INFO] ------------------< com.javacode2018:b2b-account-api >------------------
[INFO] Building b2b-account-api 1.0-SNAPSHOT                              [1/3]
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ b2b-account-api ---
[INFO] Deleting D:\code\IdeaProjects\b2b\b2b-account\b2b-account-api\target
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ b2b-account-api ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ b2b-account-api ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ b2b-account-api ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] skip non existing resourceDirectory D:\code\IdeaProjects\b2b\b2b-account\b2b-account-api\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ b2b-account-api ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ b2b-account-api ---
[INFO] No tests to run.
[INFO]
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ b2b-account-api ---
[INFO] Building jar: D:\code\IdeaProjects\b2b\b2b-account\b2b-account-api\target\b2b-account-api-1.0-SNAPSHOT.jar
[INFO]
[INFO] --- maven-install-plugin:2.4:install (default-install) @ b2b-account-api ---
[INFO] Installing D:\code\IdeaProjects\b2b\b2b-account\b2b-account-api\target\b2b-account-api-1.0-SNAPSHOT.jar to C:\Users\Think\.m2\repository\com\javacode2018\b2b-account-api\1.0-SNAPSHOT\b2b-account-api-1.0-SNAPSHOT.jar
[INFO] Installing D:\code\IdeaProjects\b2b\b2b-account\b2b-account-api\pom.xml to C:\Users\Think\.m2\repository\com\javacode2018\b2b-account-api\1.0-SNAPSHOT\b2b-account-api-1.0-SNAPSHOT.pom
[INFO]
[INFO] -------------------< com.javacode2018:b2b-order-api >-------------------
[INFO] Building b2b-order-api 1.0-SNAPSHOT                                [2/3]
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ b2b-order-api ---
[INFO] Deleting D:\code\IdeaProjects\b2b\b2b-order\b2b-order-api\target
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ b2b-order-api ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ b2b-order-api ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ b2b-order-api ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] skip non existing resourceDirectory D:\code\IdeaProjects\b2b\b2b-order\b2b-order-api\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ b2b-order-api ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ b2b-order-api ---
[INFO] No tests to run.
[INFO]
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ b2b-order-api ---
[INFO] Building jar: D:\code\IdeaProjects\b2b\b2b-order\b2b-order-api\target\b2b-order-api-1.0-SNAPSHOT.jar
[INFO]
[INFO] --- maven-install-plugin:2.4:install (default-install) @ b2b-order-api ---
[INFO] Installing D:\code\IdeaProjects\b2b\b2b-order\b2b-order-api\target\b2b-order-api-1.0-SNAPSHOT.jar to C:\Users\Think\.m2\repository\com\javacode2018\b2b-order-api\1.0-SNAPSHOT\b2b-order-api-1.0-SNAPSHOT.jar
[INFO] Installing D:\code\IdeaProjects\b2b\b2b-order\b2b-order-api\pom.xml to C:\Users\Think\.m2\repository\com\javacode2018\b2b-order-api\1.0-SNAPSHOT\b2b-order-api-1.0-SNAPSHOT.pom
[INFO]
[INFO] -----------------< com.javacode2018:b2b-order-service >-----------------
[INFO] Building b2b-order-service 1.0-SNAPSHOT                            [3/3]
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ b2b-order-service ---
[INFO] Deleting D:\code\IdeaProjects\b2b\b2b-order\b2b-order-service\target
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ b2b-order-service ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ b2b-order-service ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ b2b-order-service ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
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
[INFO]
[INFO] --- maven-install-plugin:2.4:install (default-install) @ b2b-order-service ---
[INFO] Installing D:\code\IdeaProjects\b2b\b2b-order\b2b-order-service\target\b2b-order-service-1.0-SNAPSHOT.jar to C:\Users\Think\.m2\repository\com\javacode2018\b2b-order-service\1.0-SNAPSHOT\b2b-order-service-1.0-SNAPSHOT.jar
[INFO] Installing D:\code\IdeaProjects\b2b\b2b-order\b2b-order-service\pom.xml to C:\Users\Think\.m2\repository\com\javacode2018\b2b-order-service\1.0-SNAPSHOT\b2b-order-service-1.0-SNAPSHOT.pom
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary for b2b-account-api 1.0-SNAPSHOT:
[INFO]
[INFO] b2b-account-api .................................... SUCCESS [  2.509 s]
[INFO] b2b-order-api ...................................... SUCCESS [  0.343 s]
[INFO] b2b-order-service .................................. SUCCESS [  0.340 s]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  3.455 s
[INFO] Finished at: 2019-11-20T16:46:45+08:00
[INFO] ------------------------------------------------------------------------
```

> 从上面输出中看一下反应堆列出的构建顺序：b2b-account-api、b2b-order-api、b2b-order-service。

上面过程给大家捋一捋：

上面命令先会运行`-pl b2b-order-service`得到一个反应堆列表：

```
b2b-order-service
```

然后后面又会执行`am`，这个命令会找到`-pl b2b-order-service`依赖的构件，也就是：

```
b2b-account-api
b2b-order-api
```

然后反应堆会对3个构件进行排序，得到一个正确的构件顺序，如下：

```
[INFO] Reactor Build Order:
[INFO]
[INFO] b2b-account-api                                                    [jar]
[INFO] b2b-order-api                                                      [jar]
[INFO] b2b-order-service                                                  [jar]
```

然后maven会依次对他们执行：

```
mvn clean install
```

### 案例6

分别执行下面2条mvn命令，先看一下效果：

```
mvn clean install
mvn clean install -rf b2b-order/b2b-order-service
```

输出中我们取出部分内容，如下。

第一条命令，反应堆产生的构建顺序是：

```
[INFO] Reactor Build Order:
[INFO]
[INFO] b2b-account-api                                                    [jar]
[INFO] b2b-account-service                                                [jar]
[INFO] b2b-account                                                        [pom]
[INFO] b2b-order-api                                                      [jar]
[INFO] b2b-order-service                                                  [jar]
[INFO] b2b-order                                                          [pom]
[INFO] b2b                                                                [pom]
```

第2条命令，反应堆产生的构建顺序是：

```
[INFO] Reactor Build Order:
[INFO]
[INFO] b2b-order-service                                                  [jar]
[INFO] b2b-order                                                          [pom]
[INFO] b2b                                                                [pom]
```

在仔细看一下上面2条命令的差别，后面的命令多了`-rf b2b-order/b2b-order-service`，具体过程如下：

会先执行下面命令

```
mvn clean install
```

反应堆会计算出需要构件的模块顺序，如下：

```
[INFO] Reactor Build Order:
[INFO]
[INFO] b2b-account-api                                                    [jar]
[INFO] b2b-account-service                                                [jar]
[INFO] b2b-account                                                        [pom]
[INFO] b2b-order-api                                                      [jar]
[INFO] b2b-order-service                                                  [jar]
[INFO] b2b-order                                                          [pom]
[INFO] b2b                                                                [pom]
```

`-rf b2b-order/b2b-order-service`对上面的反应堆构件顺序进行裁剪，将`b2b-order/b2b-order-service`前面的部分干掉，从`b2b-order/b2b-order-service`开始执行构建操作，所以剩下了3个需要构建的模块。