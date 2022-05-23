# Maven第5篇：手把手教你搭建私服

路人甲Java [程序员路人](javascript:void(0);) *2022-03-14 20:20*

**大家好，我是路人~~~**

**点击下方卡片，关注我，java干货及时送达**

![img](http://mmbiz.qpic.cn/mmbiz_png/kvBL17F8Dr1MFuX9tnHDHzic6yBpLSiaTibZLKcia9tHwm7aFueNjgoAIibdqXibicAUEia87xvDN77VhfDKbo59BJCwlw/0?wx_fmt=png)

**程序员路人**

专注Java相关技术：SSM、Spring全家桶、微服务、MySQL、MyCat、集群、分布式、中间件、Linux、网络、多线程，偶尔讲点运维Jenkins、Nexus、Docker、ELK，偶尔分享些技术干货，致力于Java全栈开发！

1篇原创内容



公众号

***\*这是 Maven 第 5 篇，点击上方卡片，即可翻阅前面章节。\****

### 环境

1. maven3.6.1
2. 开发工具idea
3. jdk1.8

### 本篇内容

1. 私服介绍
2. windows中安装nexus私服
3. linux中安装nexus私服
4. nexus私服中各种仓库详解
5. 配置本地Maven从nexus下载构件
6. 将本地构件发布到私服的2种方式详解
7. 总结

上一篇文章中有提到了私服，我们再来回顾一下私服相关的内容。

### 私服

私服也是远程仓库中的一种，我们为什么需要私服呢？

如果我们一个团队中有几百个人在开发一些项目，都是采用maven的方式来组织项目，那么我们每个人都需要从远程仓库中把需要依赖的构件下载到本地仓库，这对公司的网络要求也比较高，为了节省这个宽带和加快下载速度，我们在公司内部局域网内部可以架设一台服务器，这台服务器起到一个代理的作用，公司里面的所有开发者去访问这个服务器，这台服务器将需要的构件返回给我们，如果这台服务器中也没有我们需要的构件，那么这个代理服务器会去远程仓库中查找，然后将其先下载到代理服务器中，然后再返回给开发者本地的仓库。

还有公司内部有很多项目之间会相互依赖，你可能是架构组的，你需要开发一些jar包给其他组使用，此时，我们可以将自己jar发布到私服中给其他同事使用，如果没有私服，可能需要我们手动发给别人或者上传到共享机器中，不过管理起来不是很方便。

**总体上来说私服有以下好处：**

1. 加速maven构件的下载速度
2. 节省宽带，加速项目构建速度
3. 方便部署自己的构件以供他人使用
4. 提高maven的稳定性，中央仓库需要本机能够访问外网，而如果采用私服的方式，只需要本机可以访问内网私服就可以了

有3种专门的maven仓库管理软件可以用来帮助我们搭建私服：

1. Apache基金会的archiva

   ```
   http://archiva.apache.org/
   ```

2. JFrog的Artifactory

   ```
   https://jfrog.com/artifactory/
   ```

3. Sonatype的Nexus

   ```
   https://my.sonatype.com/
   ```

这些都是开源的私服软件，都可以自由使用。用的最多的是第三种Nexus，本文我们主要以这个来讲解，其他2种有兴趣的朋友可以去研究一下。

### Windows10中安装Nexus私服

nexus是java开发的，所以运行的时候需要有java环境的支持。

#### 安装jdk

安装jdk1.8，可以参考[Maven系列第2篇：安装、配置、mvn运行过程详解](https://mp.weixin.qq.com/s?__biz=MzA5MTkxMDQ4MQ==&mid=2648933498&idx=1&sn=2829ed934929e893309e6ad24ca59820&chksm=88621c44bf159552d910e093c83d147437c3121c039118d6981895ac24402fbcebc16ceb29ce&token=272569675&lang=zh_CN&scene=21#wechat_redirect)这篇文章中有介绍

#### 下载nexus

> 下面提供两种下载方式：官网下载、百度网盘下载，百度网盘速度会快一些。
>
> 官网和百度网盘中都包含了windows、linux、mac版本nexus安装文件。
>
> 建议大家使用网盘中的资源，保持和本文环境一致，可以避免出错。

##### nexus下载地址

```
https://help.sonatype.com/repomanager3/download
```

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUwG78vXxbDgGTwiamMsBgMZZMUiaTFwrGpniakOzMSIq7vknUjGyR48lm9NsSCREyZoLGTL087f21g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

##### 百度网盘下载地址

```
链接：https://pan.baidu.com/s/1N8A4JH1Ykvetn3xs05LszA 
提取码：vig6
复制这段内容后打开百度网盘手机App，操作更方便哦
```

#### 解压latest-win64.zip

> latest-win64.zip解压之后会产生两个文件目录nexus-3.19.1-01和sonatyp-work

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUwG78vXxbDgGTwiamMsBgMxeuibCGo7QoLPefmj2e5e3qriakJHH7l9icicibGmVRwNHhayicgf8Lhpcrw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

#### 启动nexus

cmd中直接运行`nexus-3.19.1-01/bin/nexus.exe /run` ，如下：

```
D:\installsoft\maven\nexus\nexus-3.19.1-01\bin>nexus.exe /run
```

如果输出中出现了下面的异常请忽略

```
java.io.UnsupportedEncodingException: Encoding GBK is not supported yet (feel free to submit a patch)
```

浏览器中打开

```
http://localhost:8081/
```

效果如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUwG78vXxbDgGTwiamMsBgMpmEMUxJYliamz1X1XhM7gdNWf4DbdEyrPOfLXZUUVprmEQvTL19QElA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

#### 登录Nexus

点击上图右上角的`Sign in`，输入用户名和密码，nexus默认用户名是`admin`

nexus这个版本的密码是第一次启动的时候生成的，密码位于下面的文件中：

```
安装目录/sonatype-work/nexus3/admin.password
```

登录成功后会弹出一些设置，如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUwG78vXxbDgGTwiamMsBgMNqAcjFAIN3WQQric9Rd8seialibqVUBPBBPO4mocp2tk0C9hDgJ9ZJqmg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击`Next`，设置新的登录密码（新密码要保存好），如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUwG78vXxbDgGTwiamMsBgMqX4VaWVMicW8ulw560KC4icIhn4icG3d3zRjyE5icEicBcaq2HbOprDOmBw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击`Next`->`Finish`完成设置。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUwG78vXxbDgGTwiamMsBgMRfHF82VyJOSzUkVzwW1ZDFiaWWvRJZezXG8f6XUApaYFGvICB4dG5oQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

#### 其他一些常见的操作

##### 停止Nexus的命令

启动的cmd窗口中按：`ctrl+c`，可以停止Nexus。

##### 修改启动端口

默认端口是8081，如果和本机有冲突，可以在下面的文件中修改：

```
nexus-3.19.1-01\etc\nexus-default.properties
```

> nexus使用java开发的web项目，内置了jetty web容器，所以可以直接运行。

### Linux安装Nexus私服

#### 下载安装包

百度网盘中下载linux版本的nexus安装包，选择`latest-unix.tar.gz`文件，下载地址如下：

```
链接：https://pan.baidu.com/s/1N8A4JH1Ykvetn3xs05LszA 
提取码：vig6
复制这段内容后打开百度网盘手机App，操作更方便哦
```

将上面的安装包放在`/opt/nexus/`目录。

#### 解压

```
[root@test1117 nexus]# tar -zvxf latest-unix.tar.gz
[root@test1117 nexus]# ls
latest-unix.tar.gz  nexus-3.19.1-01  sonatype-work
```

#### 启动

```
[root@test1117 bin]# /opt/nexus/nexus-3.19.1-01/bin/nexus start
WARNING: ************************************************************
WARNING: Detected execution as "root" user.  This is NOT recommended!
WARNING: ************************************************************
Starting nexus
```

> 我上面使用的是root用户操作的，为了安全性，你们最好自己创建个用户来操作。

#### 开放端口

在`/etc/sysconfig/iptables`文件中加入下面内容：

```
-A INPUT -p tcp -m state --state NEW -m tcp --dport 8081 -j ACCEPT
```

执行下面命令，让上面配置生效：

```
[root@test1117 bin]# service iptables restart
Redirecting to /bin/systemctl restart  iptables.service
```

#### 验证效果

访问

```
http://nexus私服所在的机器ip:8081/
```

出现下面效果表示一切ok。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUwG78vXxbDgGTwiamMsBgM3vbpr0L8kxAr084iadZ1BYM304Axnib2ialCW5D0OGuSFK6mTZ1xcc3GA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

#### 登录

用户名为`admin`，密码在

```
/opt/nexus/sonatype-work/nexus3/admin.password
```

登录之后请请立即修改密码。

### Nexus中仓库分类

前面我们说过，用户可以通过nexus去访问远程仓库，可以将本地的构件发布到nexus中，nexus是如何支撑这些操作的呢？

nexus中有个仓库列表，里面包含了各种各样的仓库，有我们说的被代理的第三方远程仓库，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUwG78vXxbDgGTwiamMsBgMRMORGjI42X34aSZhywlLVic1siaDXHs7kfzPa2uic2xZfFg2RxVP2U4oQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

上图中是nexus安装好默认自带的仓库列表，主要有3种类型：

1. 代理仓库
2. 宿主仓库
3. 仓库组

#### 代理仓库

代理仓库主要是让使用者通过代理仓库来间接访问外部的第三方远程仓库的，如通过代理仓库访问maven中央仓库、阿里的maven仓库等等。代理仓库会从被代理的仓库中下载构件，缓存在代理仓库中以供maven用户使用。

我们在nexus中创建一个阿里云的maven代理仓库来看下过程如下。

Nexus仓库列表中点击`Create repository`按钮，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUwG78vXxbDgGTwiamMsBgMs0qIngIH7ovdbfS5vsOibmhFQArcrKia80AIs6lz6htkCiaFSz2IyKT0w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

进入添加页面，选择`maven2(proxy)`，这个表示`代理仓库`，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUwG78vXxbDgGTwiamMsBgMpKWMW93AyxWLkHPghyv42N8sSQaBxqUNoxudJyQp613iaWCeCw1zmgg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

输入远程仓库的信息，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUwG78vXxbDgGTwiamMsBgMWuZHvbqfyWGOGmWU7ic2SbWNhV3mEzfSsAnCYPicQLx4eJNhPTJwcGkw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

```
第一个红框中输入仓库名称：maven-aliyun

第二个红框选择：Release，表示从这个仓库中下载稳定版的构件

第三个红框输入阿里云仓库地址：https://maven.aliyun.com/repository/public
```

点击底部的`Create repository`按钮，创建完成，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUwG78vXxbDgGTwiamMsBgMrKSTpibp9yNyMLjaH70I0ocWacGVmd85JYhCGXnVXFvfjvJ5ky0OcjQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

#### 宿主仓库

宿主仓库主要是给我们自己用的，主要有2点作用

1. 将私有的一些构件通过nexus中网页的方式上传到宿主仓库中给其他同事使用
2. 将自己开发好一些构件发布到nexus的宿主仓库中以供其他同事使用

上面这2种操作，一会稍后会详解以及演示操作过程。

#### 仓库组

maven用户可以从代理仓库和宿主仓库中下载构件至本地仓库，为了方便从多个代理仓库和宿主仓库下载构件，maven提供了仓库组，仓库组中可以有多个代理仓库和宿主仓库，而maven用户只用访问一个仓库组就可以间接的访问这个组内所有的仓库，仓库组中多个仓库是有顺序的，当maven用户从仓库组下载构件时，仓库组会按顺序依次在组内的仓库中查找组件，查找到了立即返回给本地仓库，所以一般情况我们会将速度快的放在前面。

仓库组内部实际上是没有构件内容的，他只是起到一个请求转发的作用，将maven用户下载构件的请求转发给组内的其他仓库处理。

nexus默认有个仓库组`maven-public`，如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUwG78vXxbDgGTwiamMsBgMAns6EbDnL0UIP2NibdeQdsFrb8Vvk5UVTf9DcStNZEQB3Ry8mwLkic9Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击一下`maven-public`这行记录，进去看一下，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUwG78vXxbDgGTwiamMsBgMheGsyps7Od7HSckjyB9brlHfhfMg2QyVZnHO60CdtTwQ8IgwGfGIqg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

> 上图中第一个红框是这个仓库组对外的一个url，我们本地的maven可以通过这个url来从仓库组中下载构件至本地仓库。
>
> 第二个红框中是这个仓库组中的成员，目前包含了3个仓库，第1个是宿主的releases版本仓库，第1个是宿主快照版本的仓库，第3个是代理仓库（maven社区中央仓库的代理）。
>
> 刚才我们新增的`maven-aliyun`在左边，我们将其也加到右边的仓库成员（`Members`）列表，然后将`maven-aliyun`这个仓库放在第3个位置，这个仓库的速度比`maven-central`要快一些，能加速我们下载maven构件的速度，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUwG78vXxbDgGTwiamMsBgM8QYFbNuyz5IiasNcAZxn9cwrMPhU5fStJdn1WjRWU9TTrxRvzbq2bxw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

### 配置本地Maven从nexus下载构件

介绍2种方式

#### 方式1：pom.xml的方式

本次我们就从nexus默认仓库组中下载构件，先获取仓库组对外的地址，点击下图中的`copy`按钮，获取仓库组的地址：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUwG78vXxbDgGTwiamMsBgM50ZTmzbXuc4ZZHcIp8HFrdVSRj1ePZIcmY8nEtMtkJzoPjhUmEgegg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUwG78vXxbDgGTwiamMsBgMhNQ2tR6mcEMBdEbTf55kOnVcefCqATkrHrIMRZ48hhwTLKIy7qlrWQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



修改pom.xml，加入如下内容：

> 注意下面`url`的地址为上面复制的地址。

```
<repositories>
    <repository>
        <id>maven-nexus</id>
        <url>http://localhost:8081/repository/maven-public/</url>
        <releases>
            <enabled>true</enabled>
        </releases>
        <snapshots>
            <enabled>true</enabled>
        </snapshots>
    </repository>
</repositories>
```

由于nexus私服需要有用户名和密码登录才能访问，所以需要有凭证，凭证需要在settings.xml文件中配置，在`~/.m2/settings.xml`文件的`servers`元素中加入如下内容：

```
<server>
  <id>maven-nexus</id>
  <username>admin</username>
  <password>admin123</password>
</server>
```

> 注意上面的`server->id`的值和`pom.xml中repository->id`的值一致，通过这个id关联找到凭证的。
>
> server元素中的username和password你们根据自己的去编辑，我这边密码设置的是admin123

#### 方式1示例

创建一个maven项目，打开idea，点击`File->New->Project`，选择`Maven`，如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUwG78vXxbDgGTwiamMsBgMhOl5uaYhHH4kWPjvGAibibdhu2KlZG0cUrLFvlOslrzsqnUZd9L3jZXg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击`Next`，输入项目坐标信息，如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUwG78vXxbDgGTwiamMsBgM9iaTbTibRsH5dmahEy9CLLSITOoVSdGJOAmntX1yEEED9uQvwXV1mGZw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击`Next`，输入Project name 为`maven-chat05`，如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUwG78vXxbDgGTwiamMsBgMsIVzDibaRITEWfvvpp8boUOXlJjhFqx8KazABaUARmytURibiaFm3lsibg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击`Finish`，创建成功，如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUwG78vXxbDgGTwiamMsBgMW4DjLYrIvwDDjdB3rdCoNnrkMVaa98ibHiaSgFQNVXXjDl8Apf1KMTzA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

配置一下idea的maven环境，点击`File->Settings`，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUwG78vXxbDgGTwiamMsBgMGb0tpTkO4oJuMT5M8uh56hNVMhUFv1NcFySgRtM7LxWTbiblQwSnSIQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击上面的`OK`完成配置。

还原`~/.m2/settings.xml`的配置到初始状态，操作如下：

```
将M2_HOME/conf/settings.xml复制熬~/.m2/settings.xml目录，如果存在先备份一个，然后进行覆盖。
```

修改上面idea项目中的pom.xml文件，内容如下：

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.javacode2018</groupId>
    <artifactId>maven-chat05</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.62</version>
        </dependency>
    </dependencies>

    <repositories>
        <repository>
            <id>maven-nexus</id>
            <url>http://localhost:8081/repository/maven-public/</url>
            <releases>
                <enabled>true</enabled>
            </releases>
            <snapshots>
                <enabled>true</enabled>
            </snapshots>
        </repository>
    </repositories>

</project>
```

删除本地仓库中以下几个目录：

```
~\.m2\repository\com\alibaba
```

maven-chat05项目目录中打开cmd运行：

```
mvn compile
```

见证奇迹的时刻，输出如下：

```
D:\code\IdeaProjects\maven-chat05>mvn compile
[INFO] Scanning for projects...
[INFO]
[INFO] -------------------< com.javacode2018:maven-chat05 >--------------------
[INFO] Building maven-chat05 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
Downloading from maven-nexus: http://localhost:8081/repository/maven-public/com/alibaba/fastjson/1.2.62/fastjson-1.2.62.pom
Downloaded from maven-nexus: http://localhost:8081/repository/maven-public/com/alibaba/fastjson/1.2.62/fastjson-1.2.62.pom (9.7 kB at 5.1 kB/s)
Downloading from maven-nexus: http://localhost:8081/repository/maven-public/com/alibaba/fastjson/1.2.62/fastjson-1.2.62.jar
Downloaded from maven-nexus: http://localhost:8081/repository/maven-public/com/alibaba/fastjson/1.2.62/fastjson-1.2.62.jar (658 kB at 70 kB/s)
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ maven-chat05 ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ maven-chat05 ---
[INFO] Nothing to compile - all classes are up to date
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  16.083 s
[INFO] Finished at: 2019-11-13T15:45:44+08:00
[INFO] ------------------------------------------------------------------------
```

从输出中可以看出fastjson这个jar包从`maven-nexus`中下载了，下载地址正是我们nexus私服中的那个地址。

#### 方式2：镜像方式

关于什么是镜像，这里就不再做说明了，上篇文章中有介绍，可以移步过去看一下：[仓库详解](https://mp.weixin.qq.com/s?__biz=MzA5MTkxMDQ4MQ==&mid=2648933541&idx=1&sn=8617c73b82d8aa4517a6357261a882b4&chksm=88621c9bbf15958d96ec7ebd83571245e765f1df07ac7dca028e93a656c2e91d943dc7dab08f&token=428347706&lang=zh_CN&scene=21#wechat_redirect)

镜像的方式主要修改`~/.m2/setting.xml`文件，需要修改2处理。

##### 第1处：setting.xml的mirrors元素中加入如下配置：

```
<mirror>
  <id>mirror-nexus</id>
  <mirrorOf>*</mirrorOf>
  <name>nexus镜像</name>
  <url>http://localhost:8081/repository/maven-public/</url>
</mirror>
```

> 上面`mirrorOf`配置的`*`，说明所有远程仓库都通过该镜像下载构件。
>
> url：这个为nexus中仓库组的地址，上面方式一中有说过。

##### 第2处：由于nexus的url是需要用户名和密码才可以访问的，所以需要配置访问凭证，在`~/.m2/settings.xml`文件的`servers`元素中加入如下内容：

```
<server>
  <id>mirror-nexus</id>
  <username>admin</username>
  <password>admin123</password>
</server>
```

> 注意上面的`server->id`的值和`mirror->id`的值需要一致，这样才能找到对应的凭证。

#### 方式2示例

还是以方式1中的maven项目maven-chat05为例。

修改pom.xml，内容如下：

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.javacode2018</groupId>
    <artifactId>maven-chat05</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.62</version>
        </dependency>
    </dependencies>

</project>
```

`~/.m2/settings.xml`的`servers`元素中加入下面内容：

```
<server>
  <id>mirror-nexus</id>
  <username>admin</username>
  <password>admin123</password>
</server>
```

> 注意上面username、password根据你们自己的进行配置。

`~/.m2/settings.xml`的`mirrors`元素中加入下面内容：

```
<mirror>
  <id>mirror-nexus</id>
  <mirrorOf>*</mirrorOf>
  <name>nexus镜像</name>
  <url>http://localhost:8081/repository/maven-public/</url>
</mirror>
```

删除本地仓库中以下几个目录：

```
~\.m2\repository\com\alibaba
```

maven-chat05项目目录中打开cmd运行：

```
mvn compile
```

见证奇迹的时刻，输出如下：

```
D:\code\IdeaProjects\maven-chat05>mvn compile
[INFO] Scanning for projects...
[INFO]
[INFO] -------------------< com.javacode2018:maven-chat05 >--------------------
[INFO] Building maven-chat05 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
Downloading from mirror-nexus: http://localhost:8081/repository/maven-public/org/apache/maven/plugins/maven-resources-plugin/2.6/maven-resources-plugin-2.6.pom
Downloaded from mirror-nexus: http://localhost:8081/repository/maven-public/org/apache/maven/plugins/maven-resources-plugin/2.6/maven-resources-plugin-2.6.pom (0 B at 0 B/s)
Downloading from mirror-nexus: http://localhost:8081/repository/maven-public/org/apache/maven/plugins/maven-plugins/23/maven-plugins-23.pom
Downloaded from mirror-nexus: http://localhost:8081/repository/maven-public/org/apache/maven/plugins/maven-plugins/23/maven-plugins-23.pom (0 B at 0 B/s)
Downloading from mirror-nexus: http://localhost:8081/repository/maven-public/org/apache/maven/maven-parent/22/maven-parent-22.pom
Downloaded from mirror-nexus: http://localhost:8081/repository/maven-public/org/apache/maven/maven-parent/22/maven-parent-22.pom (0 B at 0 B/s)
Downloading from mirror-nexus: http://localhost:8081/repository/maven-public/org/apache/apache/11/apache-11.pom
Downloaded from mirror-nexus: http://localhost:8081/repository/maven-public/org/apache/apache/11/apache-11.pom (0 B at 0 B/s)
Downloading from mirror-nexus: http://localhost:8081/repository/maven-public/org/apache/maven/plugins/maven-resources-plugin/2.6/maven-resources-plugin-2.6.jar
Downloaded from mirror-nexus: http://localhost:8081/repository/maven-public/org/apache/maven/plugins/maven-resources-plugin/2.6/maven-resources-plugin-2.6.jar (0 B at 0 B/s)
Downloading from mirror-nexus: http://localhost:8081/repository/maven-public/org/apache/maven/plugins/maven-compiler-plugin/3.1/maven-compiler-plugin-3.1.pom
Downloaded from mirror-nexus: http://localhost:8081/repository/maven-public/org/apache/maven/plugins/maven-compiler-plugin/3.1/maven-compiler-plugin-3.1.pom (0 B at 0 B/s)
[INFO] Nothing to compile - all classes are up to date
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  02:23 min
[INFO] Finished at: 2019-11-13T16:05:43+08:00
[INFO] ------------------------------------------------------------------------
```

输出内容比较多，只截取了部分输出，从输出中可以看出fastjson这个jar包从`mirror-nexus`中下载了，下载地址正是我们nexus私服中的那个地址，镜像的方式起效了，是不是感觉很爽，哈哈！

### 本地构件发布到私服

#### 经常用到的有2种

1. **使用maven部署构件至nexus私服**
2. **手动部署第三方构件至nexus私服**：比如我们第三方发给我们的一个包，比如短信发送商的jar包，这个包远程仓库是不存在的，我们要把这个包上传到私服供所有开发使用。

下面我们来看一下这两种如何操作。

#### 使用maven部署构件至nexus私服

我们创建maven项目的时候，会有一个pom.xml文件，里面有个version元素，这个是这个构件的版本号，可以去看一下上面我们刚创建的`maven-chat05`，默认是`1.0-SNAPSHOT`，这个以`-SNAPSHOT`结尾的表示是个快照版本，叫做`SNAPSHOT`版本，快照版本一般是不稳定的，会反复发布、测试、修改、发布。而最终会有一个稳定的可以发布的版本，是没有`-SNAPSHOT`后缀的，这个叫做`release`版本。

而nexus私服中存储用户的构件是使用的宿主仓库，这个我们上面也有说过，nexus私服中提供了两个默认的宿主仓库分别用来存放`SNAPSHOT`版本和`release`版本，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUwG78vXxbDgGTwiamMsBgMwBM4q4YxGatwwSLNsLx1wOSicYh3ianibBkT84AKWibFYPMzdSwSxx4DicQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

上图中第1个红框的`maven-releases`宿主仓库用来存放用户自己release版本的构件。

第2个红框的`maven-snapshots`宿主仓库用来存放用户snapshot版本的构件。

上面两个仓库的地址可以点击后面的`copy`按钮获取。

##### 操作分为3步

##### 第一步：修改pom.xml配置

我们需要将本地maven项目的构件发布到上面宿主仓库中，需要修改项目中pom.xml的配置，加入下面内容：

```
<distributionManagement>
    <repository>
        <id>release-nexus</id>
        <url>http://localhost:8081/repository/maven-releases/</url>
        <name>nexus私服中宿主仓库->存放/下载稳定版本的构件</name>
    </repository>
    <snapshotRepository>
        <id>snapshot-nexus</id>
        <url>http://localhost:8081/repository/maven-snapshots/</url>
        <name>nexus私服中宿主仓库->存放/下载快照版本的构件</name>
    </snapshotRepository>
</distributionManagement>
```

> 上面2个url分别是上图中两个宿主仓库的地址。

##### 第二步：修改settings.xml

上面地址需要登录才可以访问，所以需要配置凭证，这个需要在`~/.m2/settings.xml`中进行配置，在这个文件的`servers`元素中加入：

```
<server>
  <id>release-nexus</id>
  <username>admin</username>
  <password>admin123</password>
</server>

<server>
  <id>snapshot-nexus</id>
  <username>admin</username>
  <password>admin123</password>
</server>
```

> 注意上面第1个`server->id`的值需要和pom.xml中的`distributionManagement->repository->id`的值一致。
>
> 第2个`server->id`的值需要和pom.xml中的`distributionManagement->snapshotRepository->id`的值一致。

##### 第三步：执行`mvn deploy`命令

执行这个命令的时候，会对构件进行打包，然后上传到私服中。

这命令的原理，后面的文章中会具体介绍。

##### 示例效果

我们来感受一下效果。

按照上面的配置修改`maven-chat03/pom.xml`文件和本地的`~/.m2/settings.xml`文件。

maven-chat05项目目录中打开cmd运行：

```
mvn deploy
```

见证奇迹的时刻，输出如下：

```
D:\code\IdeaProjects\maven-chat05>mvn deploy
[INFO] Scanning for projects...
[INFO]
[INFO] -------------------< com.javacode2018:maven-chat05 >--------------------
[INFO] Building maven-chat05 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ maven-chat05 ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ maven-chat05 ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ maven-chat05 ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] skip non existing resourceDirectory D:\code\IdeaProjects\maven-chat05\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ maven-chat05 ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ maven-chat05 ---
[INFO] No tests to run.
[INFO]
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ maven-chat05 ---
[INFO]
[INFO] --- maven-install-plugin:2.4:install (default-install) @ maven-chat05 ---
[INFO] Installing D:\code\IdeaProjects\maven-chat05\target\maven-chat05-1.0-SNAPSHOT.jar to C:\Users\Think\.m2\repository\com\javacode2018\maven-chat05\1.0-SNAPSHOT\maven-chat05-1.0-SNAPSHOT.jar
[INFO] Installing D:\code\IdeaProjects\maven-chat05\pom.xml to C:\Users\Think\.m2\repository\com\javacode2018\maven-chat05\1.0-SNAPSHOT\maven-chat05-1.0-SNAPSHOT.pom
[INFO]
[INFO] --- maven-deploy-plugin:2.7:deploy (default-deploy) @ maven-chat05 ---
Downloading from snapshot-nexus: http://localhost:8081/repository/maven-snapshots/com/javacode2018/maven-chat05/1.0-SNAPSHOT/maven-metadata.xml
Uploading to snapshot-nexus: http://localhost:8081/repository/maven-snapshots/com/javacode2018/maven-chat05/1.0-SNAPSHOT/maven-chat05-1.0-20191113.083820-1.jar
Uploaded to snapshot-nexus: http://localhost:8081/repository/maven-snapshots/com/javacode2018/maven-chat05/1.0-SNAPSHOT/maven-chat05-1.0-20191113.083820-1.jar (1.8 kB at 11 kB/s)
Uploading to snapshot-nexus: http://localhost:8081/repository/maven-snapshots/com/javacode2018/maven-chat05/1.0-SNAPSHOT/maven-chat05-1.0-20191113.083820-1.pom
Uploaded to snapshot-nexus: http://localhost:8081/repository/maven-snapshots/com/javacode2018/maven-chat05/1.0-SNAPSHOT/maven-chat05-1.0-20191113.083820-1.pom (1.2 kB at 10 kB/s)
Downloading from snapshot-nexus: http://localhost:8081/repository/maven-snapshots/com/javacode2018/maven-chat05/maven-metadata.xml
Uploading to snapshot-nexus: http://localhost:8081/repository/maven-snapshots/com/javacode2018/maven-chat05/1.0-SNAPSHOT/maven-metadata.xml
Uploaded to snapshot-nexus: http://localhost:8081/repository/maven-snapshots/com/javacode2018/maven-chat05/1.0-SNAPSHOT/maven-metadata.xml (772 B at 9.2 kB/s)
Uploading to snapshot-nexus: http://localhost:8081/repository/maven-snapshots/com/javacode2018/maven-chat05/maven-metadata.xml
Uploaded to snapshot-nexus: http://localhost:8081/repository/maven-snapshots/com/javacode2018/maven-chat05/maven-metadata.xml (286 B at 3.4 kB/s)
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  6.156 s
[INFO] Finished at: 2019-11-13T16:38:20+08:00
[INFO] ------------------------------------------------------------------------
```

输出内容中有`Uploading to snapshot-nexus、Uploaded to snapshot-nexus` ，`snapshot-nexus`正是我们在pom.xml配置的快照版本的地址，上面输出内容中有具体的地址，和快照的地址也是一样的，上传成功了。

我们去nexus私服中看一下，访问nexus私服中快照版本仓库的地址：

```
http://localhost:8081/repository/maven-snapshots/
```

出现如下页面：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUwG78vXxbDgGTwiamMsBgMZpWcIf35IOcQ0CQ2UG0CyNklPzfkbUpQsG0JWxqPXnEoF1Ufia1ziauQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击上面的`bowse`连接，如下图，我们的构件上传成功了：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUwG78vXxbDgGTwiamMsBgMJHrKADdso7JxialI5Xib5pic5BqFtt9n6nFK6yJ6yh59u26eZuicNOia0xQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

如果其他同事需要使用上面这个构件，只需要点击下图中的pom文件，右边会显示构件的坐标，然后可以拿去使用了，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUwG78vXxbDgGTwiamMsBgM5vD8xUpt6VzDsUuVbDticibhbWjr3Q5KUgF46ruaqRNMHPbMjiazWZNrQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

上面是将快照版本的发布到了nexus私服的快照宿主仓库了，下面我们再来操作一下将release稳定版本的发布到nexus私服，如下：

修改pom.xml文件的版本号，将`-SNAPSHOT`后缀去掉，去掉之后表示是release版本的了，如下：

```
<version>1.0</version>
```

cmd命令中执行：

```
mvn deploy
```

输出：

```
D:\code\IdeaProjects\maven-chat05>mvn deploy
[INFO] Scanning for projects...
[INFO]
[INFO] -------------------< com.javacode2018:maven-chat05 >--------------------
[INFO] Building maven-chat05 1.0
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ maven-chat05 ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ maven-chat05 ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ maven-chat05 ---
[WARNING] Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] skip non existing resourceDirectory D:\code\IdeaProjects\maven-chat05\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ maven-chat05 ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ maven-chat05 ---
[INFO] No tests to run.
[INFO]
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ maven-chat05 ---
[INFO] Building jar: D:\code\IdeaProjects\maven-chat05\target\maven-chat05-1.0.jar
[INFO]
[INFO] --- maven-install-plugin:2.4:install (default-install) @ maven-chat05 ---
[INFO] Installing D:\code\IdeaProjects\maven-chat05\target\maven-chat05-1.0.jar to C:\Users\Think\.m2\repository\com\javacode2018\maven-chat05\1.0\maven-chat05-1.0.jar
[INFO] Installing D:\code\IdeaProjects\maven-chat05\pom.xml to C:\Users\Think\.m2\repository\com\javacode2018\maven-chat05\1.0\maven-chat05-1.0.pom
[INFO]
[INFO] --- maven-deploy-plugin:2.7:deploy (default-deploy) @ maven-chat05 ---
Uploading to release-nexus: http://localhost:8081/repository/maven-releases/com/javacode2018/maven-chat05/1.0/maven-chat05-1.0.jar
Uploaded to release-nexus: http://localhost:8081/repository/maven-releases/com/javacode2018/maven-chat05/1.0/maven-chat05-1.0.jar (1.7 kB at 9.6 kB/s)
Uploading to release-nexus: http://localhost:8081/repository/maven-releases/com/javacode2018/maven-chat05/1.0/maven-chat05-1.0.pom
Uploaded to release-nexus: http://localhost:8081/repository/maven-releases/com/javacode2018/maven-chat05/1.0/maven-chat05-1.0.pom (1.2 kB at 12 kB/s)
Downloading from release-nexus: http://localhost:8081/repository/maven-releases/com/javacode2018/maven-chat05/maven-metadata.xml
Uploading to release-nexus: http://localhost:8081/repository/maven-releases/com/javacode2018/maven-chat05/maven-metadata.xml
Uploaded to release-nexus: http://localhost:8081/repository/maven-releases/com/javacode2018/maven-chat05/maven-metadata.xml (304 B at 4.3 kB/s)
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  5.646 s
[INFO] Finished at: 2019-11-13T16:48:44+08:00
[INFO] ------------------------------------------------------------------------
```

输出中有`release-nexus`，说明使用了`pom.xml`中的`distributionManagement->repository->id`的值，上传地址是`http://localhost:8081/repository/maven-releases`。

打开nexus私服中release地址：

```
http://localhost:8081/repository/maven-releases/
```

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUwG78vXxbDgGTwiamMsBgM5XdGoXh31FSvj1dkVVpo8RjTFV3uOicmZ0fSI4ne8CgicQwRfKVlDu1A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击上面的`browse`连接，如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUwG78vXxbDgGTwiamMsBgMZuvHzjNZ6nhtpibyc6IweOdNGeTGusDKRCUPnqSeaJhYYsnHzK7mgvg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

成功了，是不是感觉很爽。

#### 手动部署构件至nexus私服

##### 操作步骤

**手动上传只支持发布稳定版本的构件**，操作过程如下图：

登录nexus，按照下图的步骤依次点击：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUwG78vXxbDgGTwiamMsBgMYuCUV4uwRuxTEtiaRPadVic0ZxwM9m80hEPDNdtrdb2v9AvIVPjCuAgw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

图中第一行`maven-releases`宿主仓库就是存放用户自己构件的仓库，点击上图中列表中的第一行，进入上传页面，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUwG78vXxbDgGTwiamMsBgMlVKym197FMtcrm6dMXkpkrRF56ibHk4kfFX26CHtyDd8VG7PBR0t1yQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

上面页面中点击`Browse`选择本地的构件，然后输入其他坐标信息，点击`Upload`完成上传操作。

##### 示例效果

我们把`maven-chat05\target\maven-chat05-1.0.jar`上传上去，如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUwG78vXxbDgGTwiamMsBgMJN6N5Q27uxAgx7JibJS6hyFl1lLUHSKjzas2K4oLtl54oKvQNwDoKOg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击`Upload`完成上传操作。

访问如下地址：

```
http://localhost:8081/#browse/browse:maven-releases
```

可以看到上传好的构件，如下图：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06AUwG78vXxbDgGTwiamMsBgMm2KOUglbAvhqhLz0kKPgx728eqTpWfljJbBHjDbsUI41DWuAU1qEtQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

### 总结