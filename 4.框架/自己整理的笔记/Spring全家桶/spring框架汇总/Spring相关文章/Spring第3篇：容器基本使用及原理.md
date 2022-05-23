# Spring第3篇：容器基本使用及原理

路人甲Java [程序员路人](javascript:void(0);) *2022-03-26 20:20*

**大家好，我是路人~~~
**

**点击下方卡片，关注我，java干货及时送达**

![img](http://mmbiz.qpic.cn/mmbiz_png/kvBL17F8Dr1MFuX9tnHDHzic6yBpLSiaTibZLKcia9tHwm7aFueNjgoAIibdqXibicAUEia87xvDN77VhfDKbo59BJCwlw/0?wx_fmt=png)

**程序员路人**

专注Java相关技术：SSM、Spring全家桶、微服务、MySQL、MyCat、集群、分布式、中间件、Linux、网络、多线程，偶尔讲点运维Jenkins、Nexus、Docker、ELK，偶尔分享些技术干货，致力于Java全栈开发！

1篇原创内容



公众号

**这是 Spring 第 3 篇，点击上方卡片，即可翻阅前面章节。**

### 环境 

\1. jdk1.8

\2. idea

\3. maven-3.6.1

\4. spring-5.2.3.RELEASE

### IOC容器

IOC容器是具有依赖注入功能的容器，负责**对象的实例化、对象的初始化，对象和对象之间依赖关系配置、对象的销毁、对外提供对象的查找**等操作，对象的整个生命周期都是由容器来控制。我们需要使用的对象都由ioc容器进行管理，不需要我们再去手动通过new的方式去创建对象，由ioc容器直接帮我们组装好，当我们需要使用的时候直接从ioc容器中直接获取就可以了。

**那么spring ioc容器是如何知道需要管理哪些对象呢？**

需要我们给ioc容器提供一个配置清单，这个配置**支持xml格式**和**java注解的方式**，在配置文件中列出需要让ioc容器管理的对象，以及可以指定让ioc容器如何构建这些对象，当spring容器启动的时候，就会去加载这个配置文件，然后将这些对象给组装好以供外部访问者使用。

这里所说的IOC容器也叫spring容器。

### Bean概念

由spring容器管理的对象统称为Bean对象。Bean就是普通的java对象，和我们自己new的对象其实是一样的，只是这些对象是由spring去创建和管理的，我们需要在配置文件中告诉spring容器需要创建哪些bean对象，所以需要先在配置文件中定义好需要创建的bean对象，这些配置统称为bean定义配置元数据信息，spring容器通过读取这些bean配置元数据信息来构建和组装我们需要的对象。

### Spring容器使用步骤

\1. 引入spring相关的maven配置

\2. 创建bean配置文件，比如bean xml配置文件

\3. 在bean xml文件中定义好需要spring容器管理的bean对象

\4. 创建spring容器，并给容器指定需要装载的bean配置文件，当spring容器启动之后，会加载这些配置文件，然后创建好配置文件中定义好的bean对象，将这些对象放在容器中以供使用

\5. 通过容器提供的方法获取容器中的对象，然后使用

### Spring容器对象

spring内部提供了很多表示spring容器的接口和对象，我们来看看比较常见的几个容器接口和具体的实现类。

#### BeanFactory接口

```
org.springframework.beans.factory.BeanFactory
```

spring容器中具有代表性的容器就是BeanFactory接口，这个是spring容器的顶层接口，提供了容器最基本的功能。

##### 常用的几个方法

```
//按bean的id或者别名查找容器中的bean
Object getBean(String name) throws BeansException

//这个是一个泛型方法，按照bean的id或者别名查找指定类型的bean，返回指定类型的bean对象
<T> T getBean(String name, Class<T> requiredType) throws BeansException;

//返回容器中指定类型的bean对象
<T> T getBean(Class<T> requiredType) throws BeansException;

//获取指定类型bean对象的获取器，这个方法比较特别，以后会专门来讲
<T> ObjectProvider<T> getBeanProvider(Class<T> requiredType);
```

#### ApplicationContext接口

```
org.springframework.context.ApplicationContext
```

这个接口继承了BeanFactory接口，所以内部包含了BeanFactory所有的功能，并且在其上进行了扩展，增加了很多企业级功能，比如AOP、国际化、事件支持等等。。

#### ClassPathXmlApplicationContext类

```
org.springframework.context.support.ClassPathXmlApplicationContext
```

这个类实现了ApplicationContext接口，注意一下这个类名称包含了ClassPath Xml，说明这个容器类可以从classpath中加载bean xml配置文件，然后创建xml中配置的bean对象，一会后面的案例就会用到这个类。

#### AnnotationConfigApplicationContext类

```
org.springframework.context.annotation.AnnotationConfigApplicationContext
```

这个类也实现了ApplicationContext接口，注意其类名包含了Annotation和config两个单词，上面我们有说过，bean的定义支持xml的方式和注解的方式，当我们使用注解的方式定义bean的时候，就需要用到这个容器来装载了，这个容器内部会解析注解来构建构建和管理需要的bean。

注解的方式相对于xml方式更方便一些，也是我们比较推荐的方式，后面我们会大量使用这种方式，具体会详解。

### 案例

来个helloworld来详细看一下spring如何使用。

#### 创建项目spring-series

使用idea创建maven项目spring-series，项目坐标：

```
<groupId>com.javacode2018</groupId>
<artifactId>spring-series</artifactId>
<packaging>pom</packaging>
<version>1.0-SNAPSHOT</version>
```

spring-series项目中创建一个子模块`lesson-001`，项目maven父子结构，如下图：

1581221060218

#### spring-series/pom.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.javacode2018</groupId>
    <artifactId>spring-series</artifactId>
    <packaging>pom</packaging>
    <version>1.0-SNAPSHOT</version>
    <modules>
        <module>lesson-001</module>
    </modules>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <!-- 配置maven编译的时候采用的编译器版本 -->
        <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
        <!-- 指定源代码是什么版本的，如果源码和这个版本不符将报错，maven中执行编译的时候会用到这个配置，默认是1.5，这个相当于javac命令后面的-source参数 -->
        <maven.compiler.source>1.8</maven.compiler.source>
        <!-- 该命令用于指定生成的class文件将保证和哪个版本的虚拟机进行兼容，maven中执行编译的时候会用到这个配置，默认是1.5，这个相当于javac命令后面的-target参数 -->
        <maven.compiler.target>1.8</maven.compiler.target>
        <spring.version>5.2.3.RELEASE</spring.version>
    </properties>

    <dependencyManagement>
       <dependencies>
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-core</artifactId>
               <version>${spring.version}</version>
           </dependency>
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-context</artifactId>
               <version>${spring.version}</version>
           </dependency>
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-beans</artifactId>
               <version>${spring.version}</version>
           </dependency>
       </dependencies>
    </dependencyManagement>
</project>
```

目前我们使用spring最新的版本`5.2.3.RELEASE`，需要引入spring提供的3个构件

```
spring-core、spring-context、spring-beans
```

#### lesson-001\pom.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>spring-series</artifactId>
        <groupId>com.javacode2018</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>lesson-001</artifactId>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
        </dependency>
    </dependencies>

</project>
```

#### lesson-001中创建HelloWord类

```
package com.javacode2018.lesson001.demo1;

/**
 * 公众号：程序员路人
 */
public class HelloWorld {
    public void say() {
        System.out.println("hello,欢迎和【程序员路人】一起学spring!");
    }
}
```

HelloWord中我们创建了一个say方法，里面会输一段文字。

#### 使用spring容器

下面我们通过spring容器来创建HelloWord对象，并从容器中获取这个对象，然后调用其say方法输出文字。

#### 创建bean xml配置文件

新建一个文件，文件路径如下：

```
spring-series\lesson-001\src\main\resources\com\javacode2018\lesson001\demo1\bean.xml
```

bean.xml内容如下：

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">

    <!--
    定义一个bean
    id：bean的唯一标识，可以通过这个标识从容器中获取这个bean对象
    clss：这个bean的类型，完整类名称
    -->
    <bean id="helloWorld" class="com.javacode2018.lesson001.demo1.HelloWorld"/>

</beans>
```

上面就是bean的定义文件，每个xml中可以定义多个bean元素，通过bean元素定义需要spring容器管理的对象，bean元素需指定id和class属性

- id表示这个bean的标识，在容器中需要唯一，可以通过这个id从容器中获取这个对象；
- class用来指定这个bean的完整类名

上面的配置文件中我们定义了一个`helloWorld`标识的`HellWorld类型`的bean对象。

#### 创建测试类

创建一个Client类，如下：

```
package com.javacode2018.lesson001.demo1;

import org.springframework.context.support.ClassPathXmlApplicationContext;

/**
 * 公众号：程序员路人
 */
public class Client {

    public static void main(String[] args) {
        //1.bean配置文件位置
        String beanXml = "classpath:/com/javacode2018/lesson001/demo1/beans.xml";

        //2.创建ClassPathXmlApplicationContext容器，给容器指定需要加载的bean配置文件
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext(beanXml);

        //3.从容器中获取需要的bean
        HelloWorld helloWorld = context.getBean("helloWorld", HelloWorld.class);

        //4.使用对象
        helloWorld.say();

    }
}
```

上面main方法中有容器的详细使用步骤，需要先创建容器对象，创建容器的对象的时候需要指定bean xml文件的位置，容器启动之后会加载这些配文件，然后将这些对象构建好。

代码中通过容器提供的getBean方法从容器中获取了HellWorld对象，第一个参数就是xml中bean的id，第二个参数为bean对应的Class对象。

#### 运行输出

```
hello,欢迎和【程序员路人】一起学spring!
```

### 总结

本文主要介绍了spring容器的概念、bean的概念、常见的spring容器，以及spring容器的使用步骤；下一篇我们将详解bean的定义。