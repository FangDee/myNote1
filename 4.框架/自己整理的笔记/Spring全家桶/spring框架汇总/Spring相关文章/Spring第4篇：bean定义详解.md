# Spring第4篇：bean定义详解

路人甲Java [程序员路人](javascript:void(0);) *2022-03-27 10:44*

**大家好，我是路人~~~
**

**点击下方卡片，关注我，java干货及时送达**

![img](http://mmbiz.qpic.cn/mmbiz_png/kvBL17F8Dr1MFuX9tnHDHzic6yBpLSiaTibZLKcia9tHwm7aFueNjgoAIibdqXibicAUEia87xvDN77VhfDKbo59BJCwlw/0?wx_fmt=png)

**程序员路人**

专注Java相关技术：SSM、Spring全家桶、微服务、MySQL、MyCat、集群、分布式、中间件、Linux、网络、多线程，偶尔讲点运维Jenkins、Nexus、Docker、ELK，偶尔分享些技术干货，致力于Java全栈开发！

1篇原创内容



公众号

**这是 Spring 第 4 篇，点击上方卡片，即可翻阅前面章节。**

### 本文内容 

\1. bean xml配置文件

\2. bean元素详解

\2. 名称和别名详解

\4. alias元素详解

\5. 通过import元素引入外部配置

### 环境

\1. jdk1.8

\2. idea

\3. maven-3.6.1

\4. spring-5.2.3.RELEASE

### bean概念回顾

我们再来回顾一下，被spring管理的对象统称为bean，我们程序中需要用到很多对象，我们将这些对象让spring去帮我们创建和管理，我们可以通过bean xml配置文件告诉spring容器需要管理哪些bean，spring帮我们创建和组装好这些bean对象；那么我们如何从spring中获取想要的bean对象呢，我们需要给bean定义一个名称，spring内部将这些名称和具体的bean对象进行绑定，然后spring容器可以通过这个的名称找对我们需要的对象，这个名称叫做bean的名称，在一个spring容器中需要是唯一的。

### bean xml配置文件格式

bean xml文件用于定义spring容器需要管理的bean，常见的格式如下：

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">

    <import resource="引入其他bean xml配置文件" />
    <bean id="bean标识" class="玩转类型名称"/>
    <alias name="bean标识" alias="别名" />

</beans>
```

beans是根元素，下面可以包含任意数量的import、bean、alias元素，下面我们对每个元素进行详解。

### bean元素

用来定义一个bean对象。

#### 格式

```
<bean id="bean唯一标识" name="bean名称" class="完整类型名称" factory-bean="工厂bean名称" factory-method="工厂方法"/>
```

#### bean名称

每个bean都有一个名称，叫做bean名称，bean名称在一个spring容器中必须唯一，否则会报错，通过bean名称可以从spring容器获取对应的bean对象。

#### bean别名

先来说一下什么是别名？

> 相当于人的外号一样，一个人可能有很多外号，当别人喊这个人的名称或者外号的时候，都可以找到这个人。那么bean也一样，也可以给bean起几个外号，这个外号在spring中叫做bean的别名，spring容器允许使用者通过名称或者别名获取对应的bean对象。

#### bean名称别名定义规则

名称和别名可以通过bean元素中的id和name来定义，具体定义规则如下：

\1. 当id存在的时候，不管name有没有，取id为bean的名称

\2. 当id不存在，此时需要看name，name的值可以通过`,;或者空格`分割，最后会按照分隔符得到一个String数组，数组的第一个元素作为bean的名称，其他的作为bean的别名

\3. 当id和name都存在的时候，id为bean名称，name用来定义多个别名

\4. 当id和name都不指定的时候，bean名称自动生成，生成规则下面详细说明

#### 案例

下面演示一下bean名称和别名的各种写法。

```
<!-- 通过id定义bean名称：user1 -->
<bean id="user1" class="com.javacode2018.lesson001.demo2.UserModel"/>

<!-- 通过name定义bean名称：user2 -->
<bean name="user2" class="com.javacode2018.lesson001.demo2.UserModel"/>

<!-- id为名称，name为别名；bean名称：user3，1个别名：[user3_1] -->
<bean id="user3" name="user3_1" class="com.javacode2018.lesson001.demo2.UserModel"/>

<!-- bean名称：user4，多个别名：[user4_1,user4_2,user4_3,user4_4] -->
<bean id="user4" name="user4_1,user4_2;user4_3 user4_4" class="com.javacode2018.lesson001.demo2.UserModel"/>

<!-- bean名称：user5，别名：[user5_1,user5_2,user5_3,user5_4] -->
<bean name="user5,user5_1,user5_2;user5_3 user5_4" class="com.javacode2018.lesson001.demo2.UserModel"/>
```

我们来写个java来输出上面所有bean的名称和其别名，如下：

```
package com.javacode2018.lesson001.demo2;

import org.springframework.beans.factory.BeanFactory;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import java.util.Arrays;

/**
 * 公众号：程序员路人
 */
public class Client {

    public static void main(String[] args) {
        //1.bean配置文件位置
        String beanXml = "classpath:/com/javacode2018/lesson001/demo2/beans.xml";

        //2.创建ClassPathXmlApplicationContext容器，给容器指定需要加载的bean配置文件
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext(beanXml);

        for (String beanName : Arrays.asList("user1", "user2", "user3", "user4", "user5")) {
            //获取bean的别名
            String[] aliases = context.getAliases(beanName);
            System.out.println(String.format("beanName:%s,别名:[%s]", beanName, String.join(",", aliases)));
        }

        System.out.println("spring容器中所有bean如下：");

        //getBeanDefinitionNames用于获取容器中所有bean的名称
        for (String beanName : context.getBeanDefinitionNames()) {
            //获取bean的别名
            String[] aliases = context.getAliases(beanName);
            System.out.println(String.format("beanName:%s,别名:[%s]", beanName, String.join(",", aliases)));
        }

    }
}
```

> 上面有2个新的方法：
>
> getAliases：通过bean名称获取这个bean的所有别名
>
> getBeanDefinitionNames：返回spring容器中定义的所有bean的名称

运行输出：

```
beanName:user1,别名:[]
beanName:user2,别名:[]
beanName:user3,别名:[user3_1]
beanName:user4,别名:[user4_1,user4_4,user4_2,user4_3]
beanName:user5,别名:[user5_3,user5_4,user5_1,user5_2]
spring容器中所有bean如下：
beanName:user1,别名:[]
beanName:user2,别名:[]
beanName:user3,别名:[user3_1]
beanName:user4,别名:[user4_1,user4_4,user4_2,user4_3]
beanName:user5,别名:[user5_3,user5_4,user5_1,user5_2]
```

#### id和name都未指定

当id和name都未指定的时候，bean的名称和别名又是什么呢？此时由spring自动生成，bean名称为：

```
bean的class的完整类名#编号
```

上面的编号是从0开始的，同种类型的没有指定名称的依次递增。

如：

下面定义了2个UserModel和2个String类型的bean，这些bean都没有指定id和name。

```
<bean class="com.javacode2018.lesson001.demo2.UserModel" />
<bean class="com.javacode2018.lesson001.demo2.UserModel" />

<bean class="java.lang.String" />
<bean class="java.lang.String" />
```

我们再次运行一下上面Client类的main方法，输出：

```
beanName:user1,别名:[]
beanName:user2,别名:[]
beanName:user3,别名:[user3_1]
beanName:user4,别名:[user4_1,user4_4,user4_2,user4_3]
beanName:user5,别名:[user5_3,user5_4,user5_1,user5_2]
spring容器中所有bean如下：
beanName:user1,别名:[]
beanName:user2,别名:[]
beanName:user3,别名:[user3_1]
beanName:user4,别名:[user4_1,user4_4,user4_2,user4_3]
beanName:user5,别名:[user5_3,user5_4,user5_1,user5_2]
beanName:com.javacode2018.lesson001.demo2.UserModel#0,别名:[com.javacode2018.lesson001.demo2.UserModel]
beanName:com.javacode2018.lesson001.demo2.UserModel#1,别名:[]
beanName:java.lang.String#0,别名:[java.lang.String]
beanName:java.lang.String#1,别名:[]
```

注意看一下上面最后4行的输出，bean名称和别名都是自动生成的，未指定id和name的bean对象，第一个会有别名，别名为完整的类名。bean名称为完整类名#编号。

### alias元素

alias元素也可以用来给某个bean定义别名，语法：

```
<alias name="需要定义别名的bean" alias="别名" />
```

如：

```
<bean id="user6" class="com.javacode2018.lesson001.demo2.UserModel" />
<alias name="user6" alias="user6_1" />
<alias name="user6" alias="user6_2" />
```

上面第一行通过bean元素定义了一个名称为user6的UserModel对象，后面2行给user6这个bean定义了2个别名，分别是user6_1和user6_2。

运行Client，会输出一行：

```
beanName:user6,别名:[user6_2,user6_1]
```

### import元素

当我们的系统比较大的时候，会分成很多模块，每个模块会对应一个bean xml文件，我们可以在一个总的bean xml中对其他bean xml进行汇总，相当于把多个bean xml的内容合并到一个里面了，可以通过import元素引入其他bean配置文件。

语法：

```
<import resource="其他配置文件的位置" />
```

如：

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">

    <import resource="user.xml" />
    <import resource="order.xml" />

</beans>
```

### 总结

本文主要介绍了xml bean的定义，名称和别名的使用，通过import元素引入外部配置文件。