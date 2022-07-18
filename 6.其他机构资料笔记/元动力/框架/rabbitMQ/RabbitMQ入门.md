# RabbitMQ入门

学习目标

- 能够说出什么是消息中间件
- 能够安装RabbitMQ
- 能够编写RabbitMQ的入门程序
- 能够说出RabbitMQ的5种模式特征-重点
- 能够使用Spring整合RabbitMQ

## 第一章 消息中间件概述-了解

### 1、 什么是消息中间件

#### （1）MQ是什么

MQ全称为Message Queue，消息队列是应用程序和应用程序之间的通信方法。

先进先出

原先：ip:port/order/add?goodsId=1

![image-20220408204615842](https://www.ydlclass.com/doc21xnv/assets/image-20220408204615842.7d54a462.png)

现在：异步调用

![image-20220408205148101](https://www.ydlclass.com/doc21xnv/assets/image-20220408205148101.8e02d37e.png)

#### （2）为什么使用MQ--面试

在项目中，可将一些无需即时返回且耗时的操作提取出来，进行**异步处理**，而这种异步处理的方式大大的节省了服务器的请求响应时间，从而**提高**了**系统**的**吞吐量**。

现在，应用开发和部署---微服务。

#### **（3）MQ优势**

##### a、**应用解耦**

MQ相当于一个中介，生产方通过MQ与消费方交互，它将应用程序进行解耦合。

![image-20220408210051290](https://www.ydlclass.com/doc21xnv/assets/image-20220408210051290.62f0af15.png)

##### b、**异步提速**

将不需要同步处理的并且耗时长的操作由消息队列通知消息接收方进行异步处理。提高了应用程序的响应时间。

![image-20220408210805836](https://www.ydlclass.com/doc21xnv/assets/image-20220408210805836.e96c44ed.png)

order_table status 0 已下单 status 1 支付成功 status 2 已通知商家发货 status 3 商家发货 status 4 已经收货

##### c、**削峰填谷**

如订单系统，在下单的时候就会往数据库写数据。但是数据库只能支撑每秒1000左右的并发写入，并发量再高就容易宕机。低峰期的时候并发也就100多个，但是在高峰期时候，并发量会突然激增到5000以上，这个时候数据库肯定卡死了。

消息被MQ保存起来了，然后系统就可以按照自己的消费能力来消费，比如每秒1000个数据，这样慢慢写入数据库，这样就不会卡死数据库了。

![image-20220408211341420](https://www.ydlclass.com/doc21xnv/assets/image-20220408211341420.d3b8bb87.png)

但是使用了MQ之后，限制消费消息的速度为1000，但是这样一来，高峰期产生的数据势必会被积压在MQ中，高峰就被“削”掉了。但是因为消息积压，在高峰期过后的一段时间内，消费消息的速度还是会维持在1000QPS，直到消费完积压的消息,这就叫做“填谷”

![img](https://www.ydlclass.com/doc21xnv/assets/03.2066487d.jpg)

#### （4）**MQ劣势**

- **系统可用性降低**

系统引入的外部依赖越多，系统稳定性越差。一旦 MQ 宕机，就会对业务造成影响。如何保证MQ的高可用？

- **系统复杂度提高**

MQ 的加入大大增加了系统的复杂度，以前系统间是同步的远程调用，现在是通过 MQ 进行异步调用。如何

保证消息没有被重复消费？怎么处理消息丢失情况？那么保证消息传递的顺序性？

- **一致性问题**

A 系统处理完业务，通过 MQ 给B、C、D三个系统发消息数据，如果 B 系统、C 系统处理成功，D 系统处理

失败。如何保证消息数据处理的一致性。

最终一致性。

#### （5）什么时候用MQ

① 生产者不需要从消费者处获得反馈。引入消息队列之前的直接调用，其接口的返回值应该为空，这才让明 明下层的动作还没做，上层却当成动作做完了继续往后走，即所谓异步成为了可能。

② 容许短暂的不一致性。

③ 确实是用了有效果。即解耦、提速、削峰这些方面的收益，超过加入MQ，管理MQ这些成本、

### 2、AMQP 和 JMS

MQ是消息通信的模型；实现MQ的大致有两种主流方式：AMQP、JMS。

#### （1）AMQP

AMQP是一种协议，更准确的说是一种binary wire-level protocol（链接协议）。这是其和JMS的本质差别，AMQP不从API层进行限定，而是直接定义网络交换的数据格式。

#### （2）JMS

JMS即Java消息服务（JavaMessage Service）应用程序接口，是一个Java平台中关于面向消息中间件（MOM）的API，用于在两个应用程序之间，或分布式系统中发送消息，进行异步通信。

#### （3）AMQP 与 JMS 区别

- JMS是定义了统一的接口，来对消息操作进行统一；AMQP是通过规定协议来统一数据交互的格式
- JMS限定了必须使用Java语言；AMQP只是协议，不规定实现方式，因此是跨语言的。
- JMS规定了两种消息模式；而AMQP的消息模式更加丰富。

### 3、消息队列产品

市场上常见的消息队列有如下：

- ActiveMQ：基于JMS
- ZeroMQ：基于C语言开发
- RabbitMQ：基于AMQP协议，erlang语言开发，稳定性好
- RocketMQ：基于JMS，阿里巴巴产品
- Kafka：类似MQ的产品；分布式消息系统，高吞吐量

![image-20200320111345727](https://www.ydlclass.com/doc21xnv/assets/image-20200320111345727.041941a7.png)

### 4、 RabbitMQ

#### （1）RabbitMQ**特点**：

- 1、使用简单，功能强大。
- 2、基于**AMQP**协议。 跨语言 c node.js->mq->java python
- 3、社区活跃，文档完善。
- 4、高并发性能好，这主要得益于**Erlang**语言。 c 底层语言，性能强。java 好开发。构建一个web。
- 5、Spring Boot默认已集成RabbitMQ

#### （2）其它相关术语

AMQP：

![image-20200320174327969](https://www.ydlclass.com/doc21xnv/assets/image-20200320174327969.9152399e.png)

JMS ：

![image-20200320174346261](https://www.ydlclass.com/doc21xnv/assets/image-20200320174346261.ff48f62e.png)

总结：

JMS是java提供的一套消息服务API标准，其目的是为所有的java应用程序提供统一的消息通信的标准，类似java的jdbc，只要遵循jms标准的应用程序之间都可以进行消息通信。

它和AMQP有什么 不同，jms是java语言专属的消息服务标准，它是在api层定义标准，并且只能用于java应用；而AMQP是在协议层定义的标准，是跨语言的 。

#### （3）工作模式

RabbitMQ提供了6种模式：简单模式，work模式，Publish/Subscribe发布与订阅模式，**Routing**路由模式，**Topics**主题模式，RPC远程调用模式（远程调用，不太算MQ；暂不作介绍）；

官网对应模式介绍：https://www.rabbitmq.com/getstarted.html

![1555988678324](https://www.ydlclass.com/doc21xnv/assets/1555988678324.a0be1434.png)

## 第二章 RabbitMQ工作原理--重点！

**重点看这张图**：

![image-20200320180217097](https://www.ydlclass.com/doc21xnv/assets/image-20200320180217097.84d99cd3.png)

http 三次握手 四次挥手 一个长连接

**一个消费者监听“一个”“队列”**

- Broker：消息队列服务进程，此进程包括两个部分：Exchange和Queue。
- Exchange：消息队列交换机，按一定的规则将消息路由转发到某个队列，对消息进行过虑。
- Queue：消息队列，存储消息的队列，消息到达队列并转发给指定的消费方。
- Producer：消息生产者，即生产方客户端，生产方客户端将消息发送到MQ。
- Consumer：消息消费者，即消费方客户端，接收MQ转发的消息。

消息发布接收流程：

-----发送消息-----

1、生产者和Broker建立TCP连接。

2、生产者和Broker建立通道。

3、生产者通过通道消息发送给Broker，由Exchange将消息进行转发。

4、Exchange将消息转发到指定的Queue（队列）

----接收消息-----

1、消费者和Broker建立TCP连接

2、消费者和Broker建立通道

3、消费者监听指定的Queue（队列）

4、当有消息到达Queue时Broker默认将消息推送给消费者。

5、消费者接收到消息。

### 1、相关概念介绍

AMQP 一个提供统一消息服务的应用层标准高级消息队列协议，是应用层协议的一个开放标准，为面向消息的中间件设计。

AMQP是一个二进制协议，拥有一些现代化特点：多信道、协商式，异步，安全，扩平台，中立，高效。

RabbitMQ是AMQP协议的Erlang的实现。

| 概念           | 说明                                                         |
| -------------- | ------------------------------------------------------------ |
| 连接Connection | 一个网络连接，比如TCP/IP套接字连接。                         |
| 会话Session    | 端点之间的命名对话。在一个会话上下文中，保证“恰好传递一次”。 |
| 信道Channel    | 多路复用连接中的一条独立的双向数据流通道。为会话提供物理传输介质。 |
| 客户端Client   | AMQP连接或者会话的发起者。AMQP是非对称的，客户端生产和消费消息，服务器存储和路由这些消息。 |
| 服务节点Broker | 消息中间件的服务节点；一般情况下可以将一个RabbitMQ Broker看作一台RabbitMQ 服务器。 |
| 端点           | AMQP对话的任意一方。一个AMQP连接包括两个端点（一个是客户端，一个是服务器）。 |
| 消费者Consumer | 一个从消息队列里请求消息的客户端程序。                       |
| 生产者Producer | 一个向交换机发布消息的客户端应用程序。                       |

### 2、RabbitMQ运转流程

在入门案例中：

- 生产者发送消息
  1. 生产者创建连接（Connection），开启一个信道（Channel），连接到RabbitMQ Broker；
  2. 声明队列并设置属性；如是否排它，是否持久化，是否自动删除；
  3. 将路由键（空字符串）与队列绑定起来；
  4. 发送消息至RabbitMQ Broker；
  5. 关闭信道；
  6. 关闭连接；
- 消费者接收消息
  1. 消费者创建连接（Connection），开启一个信道（Channel），连接到RabbitMQ Broker
  2. 向Broker 请求消费相应队列中的消息，设置相应的回调函数；
  3. 等待Broker回应闭关投递响应队列中的消息，消费者接收消息；
  4. 确认（ack，自动确认）接收到的消息；
  5. RabbitMQ从队列中删除相应已经被确认的消息；
  6. 关闭信道；
  7. 关闭连接；

![1565105223969](https://www.ydlclass.com/doc21xnv/assets/1565105223969.976e339d.png)

### 3、生产者流转过程说明

1. 客户端与代理服务器Broker建立连接。会调用newConnection() 方法,这个方法会进一步封装Protocol Header 0-9-1 的报文头发送给Broker ，以此通知Broker 本次交互采用的是AMQPO-9-1 协议，紧接着Broker 返回Connection.Start 来建立连接，在连接的过程中涉及Connection.Start/.Start-OK 、Connection.Tune/.Tune-Ok ，Connection.Open/ .Open-Ok 这6 个命令的交互。
2. 客户端调用connection.createChannel方法。此方法开启信道，其包装的channel.open命令发送给Broker,等待channel.basicPublish方法，对应的AMQP命令为Basic.Publish,这个命令包含了content Header 和content Body()。content Header 包含了消息体的属性，例如:投递模式，优先级等，content Body 包含了消息体本身。
3. 客户端发送完消息需要关闭资源时，涉及到Channel.Close和Channl.Close-Ok 与Connetion.Close和Connection.Close-Ok的命令交互。

### 4、消费者流转过程说明

1. 消费者客户端与代理服务器Broker建立连接。会调用newConnection() 方法,这个方法会进一步封装Protocol Header 0-9-1 的报文头发送给Broker ，以此通知Broker 本次交互采用的是AMQPO-9-1 协议，紧接着Broker 返回Connection.Start 来建立连接，在连接的过程中涉及Connection.Start/.Start-OK 、Connection.Tune/.Tune-Ok ，Connection.Open/ .Open-Ok 这6 个命令的交互。
2. 消费者客户端调用connection.createChannel方法。和生产者客户端一样，协议涉及Channel . Open/Open-Ok命令。
3. 在真正消费之前，消费者客户端需要向Broker 发送Basic.Consume 命令(即调用channel.basicConsume 方法〉将Channel 置为接收模式，之后Broker 回执Basic . Consume - Ok 以告诉消费者客户端准备好消费消息。
4. Broker 向消费者客户端推送(Push) 消息，即Basic.Deliver 命令，这个命令和Basic.Publish 命令一样会携带Content Header 和Content Body。
5. 消费者接收到消息并正确消费之后，向Broker 发送确认，即Basic.Ack 命令。
6. 客户端发送完消息需要关闭资源时，涉及到Channel.Close和Channl.Close-Ok 与Connetion.Close和Connection.Close-Ok的命令交互。

## 第三章 安装及配置RabbitMQ

### 1、下载安装

 RabbitMQ由Erlang语言开发，Erlang语言用于并发及分布式系统的开发，在电信领域应用广泛，OTP（Open Telecom Platform）作为Erlang语言的一部分，包含了很多基于Erlang开发的中间件及工具库，安装RabbitMQ需要安装Erlang/OTP，并保持版本匹配，如下图：

RabbitMQ的下载地址：http://www.rabbitmq.com/download.html

![image-20220408215304890](https://www.ydlclass.com/doc21xnv/assets/image-20220408215304890.e1f46c2f.png)

本项目使用Erlang/OTP 20.3版本和RabbitMQ3.7.3版本。

### 2、下载erlang

安装软件：路径不要有中文和空格。**管理员**

1地址如下：

https://www.erlang.org/downloads

或去老师提供的软件包中找到 otp_win64_20.3.exe，以**管理员**方式运行此文件，安装。

2erlang安装完成需要配置erlang环境变量：

 ERLANG_HOME=D:\soft\erl9.3

 在path中添加%ERLANG_HOME%\bin

### 3、安装RabbitMQ

https://github.com/rabbitmq/rabbitmq-server/releases/tag/v3.7.3

或去老师提供的软件包中找到 rabbitmq-server-3.7.3.exe，以**管理员**方式运行此文件，安装。

![image-20220408220632620](https://www.ydlclass.com/doc21xnv/assets/image-20220408220632620.dcd30a93.png)

### 4、启动

安装成功后会自动创建RabbitMQ服务并且启动。

（1）从开始菜单启动RabbitMQ

完成在开始菜单找到RabbitMQ的菜单：

![image-20200320174855134](https://www.ydlclass.com/doc21xnv/assets/image-20200320174855134.7aa6274f.png)

RabbitMQ Service-install :安装服务

RabbitMQ Service-remove 删除服务

RabbitMQ Service-start 启动

RabbitMQ Service-stop 启动

（2）如果没有开始菜单则进入安装目录下sbin目录手动启动：

![image-20200320174919367](https://www.ydlclass.com/doc21xnv/assets/image-20200320174919367.09c74d6b.png)

（3）安装并运行服务

rabbitmq-service.bat install 安装服务

rabbitmq-service.bat stop 停止服务

rabbitmq-service.bat start 启动服务

（4）安装管理插件

安装rabbitMQ的管理插件，方便在浏览器端管理RabbitMQ

在sbin目录下，**管理员身份**运行cmd

```text
rabbitmq-plugins.bat enable rabbitmq_management
```

### 5、启动成功 登录RabbitMQ

进入浏览器，输入：http://localhost:15672

![image-20200320175052306](https://www.ydlclass.com/doc21xnv/assets/image-20200320175052306.a9c088c5.png)

初始账号和密码：guest/guest

![image-20220408221008693](https://www.ydlclass.com/doc21xnv/assets/image-20220408221008693.bd953c3f.png)

### 6、注意事项

1、安装erlang和rabbitMQ以管理员身份运行。

2、当卸载重新安装时会出现RabbitMQ服务注册失败，此时需要进入注册表清理erlang

搜索RabbitMQ、ErlSrv，将对应的项全部删除。

### 7、查看管理控制台

![image-20200320175339750](https://www.ydlclass.com/doc21xnv/assets/image-20200320175339750.e00ee8bc.png)

尝试新增用户、新增虚拟机。

## 第四章 RabbitMQ入门

**简单模式**

![image-20200320180411894](https://www.ydlclass.com/doc21xnv/assets/image-20200320180411894.bbf971ee.png)

### [#](https://www.ydlclass.com/doc21xnv/distribute/rabbitmq/#_1、搭建示例工程)1、搭建示例工程

#### [#](https://www.ydlclass.com/doc21xnv/distribute/rabbitmq/#_1-创建工程)（1）创建工程

![image-20220408221608751](https://www.ydlclass.com/doc21xnv/assets/image-20220408221608751.f881d76e.png)

rabbitmqtest父工程下，新建两个模块：producer和consumer

![image-20220408221703947](https://www.ydlclass.com/doc21xnv/assets/image-20220408221703947.3fefbc02.png)

#### （2）添加依赖

往rabbitmqtest的pom.xml文件中添加如下依赖：

```xml
    <dependencies>
        <dependency>
            <groupId>com.rabbitmq</groupId>
            <artifactId>amqp-client</artifactId>
            <version>5.6.0</version>
        </dependency>
    </dependencies>
```

### 2、 编写生产者

producer工程中，编写消息生产者com.ydlclass.rabbitmq.simple.Producer

```java
package com.ydlclass.rabbitmq.simple;

import com.rabbitmq.client.AMQP;
import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;

import java.io.IOException;
import java.util.concurrent.TimeoutException;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
public class Producer {

    public static void main(String[] args) throws Exception {
        //1创建连接工厂
        ConnectionFactory connectionFactory=new ConnectionFactory();
        //连接的ip
        connectionFactory.setHost("localhost");
        //连接的端口
        connectionFactory.setPort(5672);
        //设置虚拟主机
        connectionFactory.setVirtualHost("/");
        //设置用户名
        connectionFactory.setUsername("itlils");
        //设置密码
        connectionFactory.setPassword("itlils");

        //2创建长连接
        Connection connection = connectionFactory.newConnection();
        //3创建channel
        Channel channel = connection.createChannel();
        //声明队列
        //String queue,  队列名
        // boolean durable, 持久化
        // boolean exclusive, 排他的
        // boolean autoDelete, 自动删除
        // Map<String, Object> arguments 属性
        channel.queueDeclare("ydlqueue",true,false,false,null);

        //4发消息
        // String exchange,  交换机
        // String routingKey, 路由键
        // AMQP.BasicProperties props, 属性
        // byte[] body 消息      string byte[] char[]如何相互转换的？
        String msg="hello rabbitmq!";
        channel.basicPublish("","ydlqueue",null,msg.getBytes());

        //5关闭连接  资源关闭的顺序，先关后出来的资源，最后关，第一个资源
        channel.close();
        connection.close();
    }
}
```

在执行上述的消息发送之后；可以登录rabbitMQ的管理控制台，可以发现队列和其消息：

![image-20220408223540630](https://www.ydlclass.com/doc21xnv/assets/image-20220408223540630.119923ff.png)

查看消息

![image-20220408223601408](https://www.ydlclass.com/doc21xnv/assets/image-20220408223601408.a29a49a6.png)

**注意**：关闭连接

### 3、 编写消费者

consumer工程，编写消息的消费者com.ydlclass.rabbitmq.simple.Consumer

```java
package com.ydlclass.rabbitmq.simple;

import com.ydlclass.rabbitmq.util.ConnectionUtil;
import com.rabbitmq.client.*;

import java.io.IOException;

public class Consumer {

    static final String QUEUE_NAME = "simple_queue";
    public static void main(String[] args) throws Exception {
        //1创建连接工厂
        ConnectionFactory connectionFactory = new ConnectionFactory();
        //主机地址;默认为 localhost
        connectionFactory.setHost("localhost");
        //连接端口;默认为 5672
        connectionFactory.setPort(5672);
        //虚拟主机名称;默认为 /
        connectionFactory.setVirtualHost("/");
        //连接用户名；默认为guest
        connectionFactory.setUsername("itlils");
        //连接密码；默认为guest
        connectionFactory.setPassword("itlils");

        //2创建连接
        Connection connection = connectionFactory.newConnection();

        //3创建频道
        Channel channel = connection.createChannel();

        //6声明（创建）队列
        /**
         * 参数1：队列名称
         * 参数2：是否定义持久化队列
         * 参数3：是否独占本次连接
         * 参数4：是否在不使用的时候自动删除队列
         * 参数5：队列其它参数
         */
        channel.queueDeclare(QUEUE_NAME, true, false, false, null);

        //5创建消费者；并设置消息处理
        DefaultConsumer consumer = new DefaultConsumer(channel){
            @Override
            /**
             * consumerTag 消息者标签，在channel.basicConsume时候可以指定
             * envelope 消息包的内容，可从中获取消息id，消息routingkey，交换机，消息和重传标志(收到消息失败后是否需要重新发送)
             * properties 属性信息
             * body 消息
             */
            public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                //消费者标签
                System.out.println("消费者标签为：" + consumerTag);
                //路由key
                System.out.println("路由key为：" + envelope.getRoutingKey());
                //交换机
                System.out.println("交换机为：" + envelope.getExchange());
                //消息id
                System.out.println("消息id为：" + envelope.getDeliveryTag());
                //收到的消息
                System.out.println("接收到的消息为：" + new String(body, "utf-8"));
            }
        };
        //4监听消息
        /**
         * 参数1：队列名称
         * 参数2：是否自动确认，设置为true为表示消息接收到自动向mq回复接收到了，mq接收到回复会删除消息，设置为false则需要手动确认
         * 参数3：消息接收到后回调
         */
        channel.basicConsume(QUEUE_NAME, true, consumer);

        //不关闭资源，应该一直监听消息
        //channel.close();
        //connection.close();
    }
}
```

**有能力的同学**，抽取创建connection的工具类com.ydlclass.rabbitmq.util.ConnectionUtil；

```java
package com.ydlclass.rabbitmq.util;

import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;

public class ConnectionUtil {

    public static Connection getConnection() throws Exception {
        //创建连接工厂
        ConnectionFactory connectionFactory = new ConnectionFactory();
        //主机地址;默认为 localhost
        connectionFactory.setHost("localhost");
        //连接端口;默认为 5672
        connectionFactory.setPort(5672);
        //虚拟主机名称;默认为 /
        connectionFactory.setVirtualHost("/");
        //连接用户名；默认为guest
        connectionFactory.setUsername("itlils");
        //连接密码；默认为guest
        connectionFactory.setPassword("itlils");

        //创建连接
        return connectionFactory.newConnection();
    }
}
```

### 4、小结

上述的入门案例中中其实使用的是如下的简单模式：

![1555991074575](https://www.ydlclass.com/doc21xnv/assets/1555991074575.c86a7dcf.png)

在上图的模型中，有以下概念：

- P：生产者，也就是要发送消息的程序
- C：消费者：消息的接受者，会一直等待消息到来。
- queue：消息队列，图中红色部分。类似一个邮箱，可以缓存消息；生产者向其中投递消息，消费者从其中取出消息。

## 第五章 RabbitMQ工作模式--重点

### 1、Work queues工作队列模式（包工头）

#### （1） 模式说明

![1556009144848](https://www.ydlclass.com/doc21xnv/assets/1556009144848.1e306c6c.png)

`Work Queues`与入门程序的`简单模式`相比，多了一个或一些消费端，多个消费端共同消费同一个队列中的消息。

**应用场景**：对于 任务过重或任务较多情况使用工作队列可以提高任务处理的速度。

#### （2）代码

复制生产者消费者各一份

![image-20220411210359456](https://www.ydlclass.com/doc21xnv/assets/image-20220411210359456.c4ca6df9.png)

##### a、生产者 修改队列名

```java
static final String QUEUE_NAME = "workqueue";

for (int i = 0; i < 10; i++) {
            String msg="hello rabbitmq!workqueue"+i;
            channel.basicPublish("",QUEUE,null,msg.getBytes());
}
```

##### b、消费者1 监听同一个队列

```java
static final String QUEUE_NAME = "workqueue";
```

##### c、消费者2 消费者再起一份

```java
static final String QUEUE_NAME = "workqueue";
```

### （3）测试

生产者，多运行几次，观察连个消费者

设置，消费者能启多份：

![image-20220411210105591](https://www.ydlclass.com/doc21xnv/assets/image-20220411210105591.b88f5e92.png)

在一个队列中如果有多个消费者，那么消费者之间对于同一个队列消息的关系是**竞争**的关系。

第一个消费者

![image-20220411210449520](https://www.ydlclass.com/doc21xnv/assets/image-20220411210449520.b8782f8d.png)

第二个消费者:

![image-20220411210501744](https://www.ydlclass.com/doc21xnv/assets/image-20220411210501744.c194803f.png)

### 2、publish/subscribe 订阅发布模式类型（微博）

atm取100元，短信，邮件。

订阅模式示例图：

![1556014499573](https://www.ydlclass.com/doc21xnv/assets/1556014499573.535fc6ca.png)

前面2个案例中，只有3个角色：

- P：生产者，也就是要发送消息的程序
- C：消费者：消息的接受者，会一直等待消息到来。
- queue：消息队列，图中红色部分

而在订阅模型中，多了一个exchange角色，而且过程略有变化：

- P：生产者，也就是要发送消息的程序，但是不再发送到队列中，而是发给X（交换机）
- C：消费者，消息的接受者，会一直等待消息到来。
- Queue：消息队列，接收消息、缓存消息。
- Exchange：交换机，图中的X。一方面，接收生产者发送的消息。另一方面，知道如何处理消息，例如递交给某个特别队列、递交给所有队列、或是将消息丢弃。到底如何操作，取决于Exchange的类型。Exchange有常见以下3种类型：
  - Fanout：广播，将消息交给所有绑定到交换机的队列
  - Direct：定向，把消息交给符合指定routing key 的队列
  - Topic：通配符，把消息交给符合routing pattern（路由模式） 的队列

**Exchange（交换机）只负责转发消息，不具备存储消息的能力**，因此如果没有任何队列与Exchange绑定，或者没有符合路由规则的队列，那么消息会丢失！

### 3、 Publish/Subscribe发布与订阅模式（微博）

#### （ 1）模式说明

![1556010329032](https://www.ydlclass.com/doc21xnv/assets/1556010329032.4888b21e.png)

发布订阅模式： 1、每个消费者监听自己的队列。 2、生产者将消息发给broker，由交换机将消息转发到绑定此交换机的每个队列，每个绑定交换机的队列都将接收 到消息

#### （2）代码

##### a、生产者

```java
package com.ydlclass.rabbitmq.simple;

import com.rabbitmq.client.BuiltinExchangeType;
import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;

/**
 * creste by ydlclass.
 */
public class Producer_pubsub {
    //交换机名称
    static final String FANOUT_EXCHAGE = "fanout_exchange";
    //队列名称
    static final String FANOUT_QUEUE_1 = "fanout_queue_1";
    //队列名称
    static final String FANOUT_QUEUE_2 = "fanout_queue_2";

    public static void main(String[] args) throws Exception {

        //1创建连接工厂
        ConnectionFactory connectionFactory = new ConnectionFactory();
        //主机地址;默认为 localhost
        connectionFactory.setHost("localhost");
        //连接端口;默认为 5672
        connectionFactory.setPort(5672);
        //虚拟主机名称;默认为 /
        connectionFactory.setVirtualHost("/");
        //连接用户名；默认为guest
        connectionFactory.setUsername("itlils");
        //连接密码；默认为guest
        connectionFactory.setPassword("itlils");

        //2创建连接
        Connection connection = connectionFactory.newConnection();

        //3创建频道
        Channel channel = connection.createChannel();

        /**
         * 声明交换机
         * 参数1：交换机名称
         * 参数2：交换机类型，fanout、topic、direct、headers
         * 参数3：是否定义持久化
         * 参数4：是否在不使用的时候自动删除
         */
        channel.exchangeDeclare(FANOUT_EXCHAGE, BuiltinExchangeType.FANOUT,true,true,null);

        // 声明（创建）队列
        /**
         * 参数1：队列名称
         * 参数2：是否定义持久化队列
         * 参数3：是否独占本次连接
         * 参数4：是否在不使用的时候自动删除队列
         * 参数5：队列其它参数
         */
        channel.queueDeclare(FANOUT_QUEUE_1, true, false, false, null);
        channel.queueDeclare(FANOUT_QUEUE_2, true, false, false, null);

        //队列绑定交换机
        channel.queueBind(FANOUT_QUEUE_1, FANOUT_EXCHAGE, "");
        channel.queueBind(FANOUT_QUEUE_2, FANOUT_EXCHAGE, "");

        for (int i = 1; i <= 10; i++) {
            // 发送信息
            String message = "你好；小兔子！发布订阅模式--" + i;
            /**
             * 参数1：交换机名称，如果没有指定则使用默认Default Exchage
             * 参数2：路由key,简单模式可以传递队列名称
             * 参数3：消息其它属性
             * 参数4：消息内容
             */
            channel.basicPublish(FANOUT_EXCHAGE, "", null, message.getBytes());
            System.out.println("已发送消息：" + message);
        }

        // 关闭资源
        channel.close();
        connection.close();
    }
}
```

##### b、消费者1

```java
package com.ydlclass.rabbitmq.publish;

import com.rabbitmq.client.*;

import java.io.IOException;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
public class Consumer1 {
    //交换机名称
    static final String FANOUT_EXCHAGE = "fanout_exchange";
    //队列名称
    static final String FANOUT_QUEUE_1 = "fanout_queue_1";
    //队列名称
    static final String FANOUT_QUEUE_2 = "fanout_queue_2";

    public static void main(String[] args) throws Exception {
        //1创建连接工厂
        ConnectionFactory connectionFactory=new ConnectionFactory();
        //连接的ip
        connectionFactory.setHost("localhost");
        //连接的端口
        connectionFactory.setPort(5672);
        //设置虚拟主机
        connectionFactory.setVirtualHost("/");
        //设置用户名
        connectionFactory.setUsername("itlils");
        //设置密码
        connectionFactory.setPassword("itlils");

        //2创建长连接
        Connection connection = connectionFactory.newConnection();
        //3创建channel
        Channel channel = connection.createChannel();

        //声明队列
        //String queue,  队列名
        // boolean durable, 持久化
        // boolean exclusive, 排他的
        // boolean autoDelete, 自动删除
        // Map<String, Object> arguments 属性
        channel.queueDeclare(FANOUT_QUEUE_1,true,false,false,null);
        channel.queueDeclare(FANOUT_QUEUE_2,true,false,false,null);

        // 声明交换机
        // String exchange,  交换机名称
        // BuiltinExchangeType type, 交换机类型
        // boolean durable,  持久化
        // boolean autoDelete, 自动删除
        // Map<String, Object> arguments 属性
        channel.exchangeDeclare(FANOUT_EXCHAGE, BuiltinExchangeType.FANOUT,true,false,null);

        //队列绑定交换机
        // String queue, 队列名称
        // String exchange, 交换机名称
        // String routingKey 路由键
        channel.queueBind(FANOUT_QUEUE_1,FANOUT_EXCHAGE,"");
        channel.queueBind(FANOUT_QUEUE_2,FANOUT_EXCHAGE,"");


        //4监听某个队列
        // String queue, 监听的队列名
        // boolean autoAck, 是否自动应答
        // Consumer callback 回调函数，收到消息，我要干啥
        com.rabbitmq.client.Consumer consumer=new DefaultConsumer(channel){
            // 回调函数，收到消息，我要干啥
            //  String consumerTag, 消费者标签
            // Envelope envelope, 信封 保存很多信息
            // AMQP.BasicProperties properties, 属性
            // byte[] body  消息字节数组
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                //业务逻辑

                //现在的业务逻辑就是打印
//                System.out.println("consumerTag:"+consumerTag);
//                System.out.println("Exchange:"+envelope.getExchange());
//                System.out.println("RoutingKey:"+envelope.getRoutingKey());
//                System.out.println("DeliveryTag:"+envelope.getDeliveryTag()); //消息id

                System.out.println(new String(body));
            }
        };
        channel.basicConsume(FANOUT_QUEUE_1,true,consumer);

        //5 千万别关闭连接，要不然queue有了消息 推不过来了
//        channel.close();
//        connection.close();



    }
}
```

##### c、消费者2

```java
package com.ydlclass.rabbitmq.publish;

import com.rabbitmq.client.*;

import java.io.IOException;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
public class Consumer2 {
    //交换机名称
    static final String FANOUT_EXCHAGE = "fanout_exchange";
    //队列名称
    static final String FANOUT_QUEUE_1 = "fanout_queue_1";
    //队列名称
    static final String FANOUT_QUEUE_2 = "fanout_queue_2";

    public static void main(String[] args) throws Exception {
        //1创建连接工厂
        ConnectionFactory connectionFactory=new ConnectionFactory();
        //连接的ip
        connectionFactory.setHost("localhost");
        //连接的端口
        connectionFactory.setPort(5672);
        //设置虚拟主机
        connectionFactory.setVirtualHost("/");
        //设置用户名
        connectionFactory.setUsername("itlils");
        //设置密码
        connectionFactory.setPassword("itlils");

        //2创建长连接
        Connection connection = connectionFactory.newConnection();
        //3创建channel
        Channel channel = connection.createChannel();

        //声明队列
        //String queue,  队列名
        // boolean durable, 持久化
        // boolean exclusive, 排他的
        // boolean autoDelete, 自动删除
        // Map<String, Object> arguments 属性
        channel.queueDeclare(FANOUT_QUEUE_1,true,false,false,null);
        channel.queueDeclare(FANOUT_QUEUE_2,true,false,false,null);

        // 声明交换机
        // String exchange,  交换机名称
        // BuiltinExchangeType type, 交换机类型
        // boolean durable,  持久化
        // boolean autoDelete, 自动删除
        // Map<String, Object> arguments 属性
        channel.exchangeDeclare(FANOUT_EXCHAGE, BuiltinExchangeType.FANOUT,true,false,null);

        //队列绑定交换机
        // String queue, 队列名称
        // String exchange, 交换机名称
        // String routingKey 路由键
        channel.queueBind(FANOUT_QUEUE_1,FANOUT_EXCHAGE,"");
        channel.queueBind(FANOUT_QUEUE_2,FANOUT_EXCHAGE,"");


        //4监听某个队列
        // String queue, 监听的队列名
        // boolean autoAck, 是否自动应答
        // Consumer callback 回调函数，收到消息，我要干啥
        Consumer consumer=new DefaultConsumer(channel){
            // 回调函数，收到消息，我要干啥
            //  String consumerTag, 消费者标签
            // Envelope envelope, 信封 保存很多信息
            // AMQP.BasicProperties properties, 属性
            // byte[] body  消息字节数组
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                //业务逻辑

                //现在的业务逻辑就是打印
//                System.out.println("consumerTag:"+consumerTag);
//                System.out.println("Exchange:"+envelope.getExchange());
//                System.out.println("RoutingKey:"+envelope.getRoutingKey());
//                System.out.println("DeliveryTag:"+envelope.getDeliveryTag()); //消息id

                System.out.println(new String(body));
            }
        };
        channel.basicConsume(FANOUT_QUEUE_2,true,consumer);

        //5 千万别关闭连接，要不然queue有了消息 推不过来了
//        channel.close();
//        connection.close();



    }
}
```

#### （3） 测试

启动所有消费者，然后使用生产者发送消息；在每个消费者对应的控制台可以查看到生产者发送的所有消息；到达**广播**的效果。

在执行完测试代码后，其实到RabbitMQ的管理后台找到`Exchanges`选项卡，点击 `fanout_exchange` 的交换机，可以查看到如下的绑定：

![image-20220411212807072](https://www.ydlclass.com/doc21xnv/assets/image-20220411212807072.d93ba322.png)

测试发送一条消息：

两个消费者都接受到同样的消息

![image-20220411212727005](https://www.ydlclass.com/doc21xnv/assets/image-20220411212727005.4caee19c.png)

![image-20220411212734150](https://www.ydlclass.com/doc21xnv/assets/image-20220411212734150.3bcc1d8a.png)

#### （4）小结

交换机需要与队列进行绑定，绑定之后；一个消息可以被多个消费者都收到。

**发布订阅模式与工作队列模式的区别**

1、工作队列模式不用定义交换机，而发布/订阅模式需要定义交换机。

2、发布/订阅模式的生产方是面向交换机发送消息，工作队列模式的生产方是面向队列发送消息(底层使用默认交换机)。

3、发布/订阅模式需要设置队列和交换机的绑定，工作队列模式不需要设置，实际上工作队列模式会将队列绑 定到默认的交换机 。

### 4、Routing路由模式（分布式日志收集系统）

#### （1）模式说明

路由模式特点：

- 队列与交换机的绑定，不能是任意绑定了，而是要指定一个`RoutingKey`（路由key）
- 消息的发送方在 向 Exchange发送消息时，也必须指定消息的 `RoutingKey`。
- Exchange不再把消息交给每一个绑定的队列，而是根据消息的`Routing Key`进行判断，只有队列的`Routingkey`与消息的 `Routing key`完全一致，才会接收到消息

![1556029284397](https://www.ydlclass.com/doc21xnv/assets/1556029284397.8f3e1d3c.png)

图解：

- P：生产者，向Exchange发送消息，发送消息时，会指定一个routing key。
- X：Exchange（交换机），接收生产者的消息，然后把消息递交给 与routing key完全匹配的队列
- C1：消费者，其所在队列指定了需要routing key 为 error 的消息
- C2：消费者，其所在队列指定了需要routing key 为 info、error、warning 的消息

#### （2）代码

在编码上与 `Publish/Subscribe发布与订阅模式` 的区别是交换机的类型为：Direct，还有队列绑定交换机的时候需要指定routing key。

##### a、生产者

```java
package com.ydlclass.rabbitmq.routing;

import com.rabbitmq.client.BuiltinExchangeType;
import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
public class Producer {

    //交换机名称
    static final String DIRECT_EXCHAGE = "direct_exchange";
    //队列名称
    static final String DIRECT_QUEUE_1 = "direct_queue_1";
    //队列名称
    static final String DIRECT_QUEUE_2 = "direct_queue_2";

    public static void main(String[] args) throws Exception {
        //1创建连接工厂
        ConnectionFactory connectionFactory=new ConnectionFactory();
        //连接的ip
        connectionFactory.setHost("localhost");
        //连接的端口
        connectionFactory.setPort(5672);
        //设置虚拟主机
        connectionFactory.setVirtualHost("/");
        //设置用户名
        connectionFactory.setUsername("itlils");
        //设置密码
        connectionFactory.setPassword("itlils");

        //2创建长连接
        Connection connection = connectionFactory.newConnection();
        //3创建channel
        Channel channel = connection.createChannel();
        //声明队列
        //String queue,  队列名
        // boolean durable, 持久化
        // boolean exclusive, 排他的
        // boolean autoDelete, 自动删除
        // Map<String, Object> arguments 属性
        channel.queueDeclare(DIRECT_QUEUE_1,true,false,false,null);
        channel.queueDeclare(DIRECT_QUEUE_2,true,false,false,null);

        // 声明交换机
        // String exchange,  交换机名称
        // BuiltinExchangeType type, 交换机类型
        // boolean durable,  持久化
        // boolean autoDelete, 自动删除
        // Map<String, Object> arguments 属性
        channel.exchangeDeclare(DIRECT_EXCHAGE, BuiltinExchangeType.DIRECT,true,false,null);

        //队列绑定交换机
        // String queue, 队列名称
        // String exchange, 交换机名称
        // String routingKey 路由键
        channel.queueBind(DIRECT_QUEUE_1,DIRECT_EXCHAGE,"error");
        channel.queueBind(DIRECT_QUEUE_2,DIRECT_EXCHAGE,"info");
        channel.queueBind(DIRECT_QUEUE_2,DIRECT_EXCHAGE,"error");
        channel.queueBind(DIRECT_QUEUE_2,DIRECT_EXCHAGE,"warning");

        //4发消息
        // String exchange,  交换机
        // String routingKey, 路由键
        // AMQP.BasicProperties props, 属性
        // byte[] body 消息      string byte[] char[]如何相互转换的？
        String msg="hello rabbitmq!routing error";
        channel.basicPublish(DIRECT_EXCHAGE,"error",null,msg.getBytes());

        String msg1="hello rabbitmq!routing info";
        channel.basicPublish(DIRECT_EXCHAGE,"info",null,msg1.getBytes());

        String msg2="hello rabbitmq!routing warning";
        channel.basicPublish(DIRECT_EXCHAGE,"warning",null,msg2.getBytes());

        //5关闭连接  资源关闭的顺序，先关后出来的资源，最后关，第一个资源
        channel.close();
        connection.close();
    }
}
```

##### b、消费者1

```java
package com.ydlclass.rabbitmq.routing;

import com.rabbitmq.client.*;

import java.io.IOException;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
public class Consumer1 {
    //交换机名称
    static final String DIRECT_EXCHAGE = "direct_exchange";
    //队列名称
    static final String DIRECT_QUEUE_1 = "direct_queue_1";
    //队列名称
    static final String DIRECT_QUEUE_2 = "direct_queue_2";

    public static void main(String[] args) throws Exception {
        //1创建连接工厂
        ConnectionFactory connectionFactory=new ConnectionFactory();
        //连接的ip
        connectionFactory.setHost("localhost");
        //连接的端口
        connectionFactory.setPort(5672);
        //设置虚拟主机
        connectionFactory.setVirtualHost("/");
        //设置用户名
        connectionFactory.setUsername("itlils");
        //设置密码
        connectionFactory.setPassword("itlils");

        //2创建长连接
        Connection connection = connectionFactory.newConnection();
        //3创建channel
        Channel channel = connection.createChannel();

        //声明队列
        //String queue,  队列名
        // boolean durable, 持久化
        // boolean exclusive, 排他的
        // boolean autoDelete, 自动删除
        // Map<String, Object> arguments 属性
        channel.queueDeclare(DIRECT_QUEUE_1,true,false,false,null);
        channel.queueDeclare(DIRECT_QUEUE_2,true,false,false,null);

        // 声明交换机
        // String exchange,  交换机名称
        // BuiltinExchangeType type, 交换机类型
        // boolean durable,  持久化
        // boolean autoDelete, 自动删除
        // Map<String, Object> arguments 属性
        channel.exchangeDeclare(DIRECT_EXCHAGE, BuiltinExchangeType.DIRECT,true,false,null);

        //队列绑定交换机
        // String queue, 队列名称
        // String exchange, 交换机名称
        // String routingKey 路由键
        channel.queueBind(DIRECT_QUEUE_1,DIRECT_EXCHAGE,"error");
        channel.queueBind(DIRECT_QUEUE_2,DIRECT_EXCHAGE,"info");
        channel.queueBind(DIRECT_QUEUE_2,DIRECT_EXCHAGE,"error");
        channel.queueBind(DIRECT_QUEUE_2,DIRECT_EXCHAGE,"warning");


        //4监听某个队列
        // String queue, 监听的队列名
        // boolean autoAck, 是否自动应答
        // Consumer callback 回调函数，收到消息，我要干啥
        com.rabbitmq.client.Consumer consumer=new DefaultConsumer(channel){
            // 回调函数，收到消息，我要干啥
            //  String consumerTag, 消费者标签
            // Envelope envelope, 信封 保存很多信息
            // AMQP.BasicProperties properties, 属性
            // byte[] body  消息字节数组
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                //业务逻辑

                //现在的业务逻辑就是打印
//                System.out.println("consumerTag:"+consumerTag);
//                System.out.println("Exchange:"+envelope.getExchange());
//                System.out.println("RoutingKey:"+envelope.getRoutingKey());
//                System.out.println("DeliveryTag:"+envelope.getDeliveryTag()); //消息id

                System.out.println(new String(body));
            }
        };
        channel.basicConsume(DIRECT_QUEUE_1,true,consumer);

        //5 千万别关闭连接，要不然queue有了消息 推不过来了
//        channel.close();
//        connection.close();

    }
}
```

##### c、消费者2

```java
package com.ydlclass.rabbitmq.routing;

import com.rabbitmq.client.*;

import java.io.IOException;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
public class Consumer2 {
    //交换机名称
    static final String DIRECT_EXCHAGE = "direct_exchange";
    //队列名称
    static final String DIRECT_QUEUE_1 = "direct_queue_1";
    //队列名称
    static final String DIRECT_QUEUE_2 = "direct_queue_2";

    public static void main(String[] args) throws Exception {
        //1创建连接工厂
        ConnectionFactory connectionFactory=new ConnectionFactory();
        //连接的ip
        connectionFactory.setHost("localhost");
        //连接的端口
        connectionFactory.setPort(5672);
        //设置虚拟主机
        connectionFactory.setVirtualHost("/");
        //设置用户名
        connectionFactory.setUsername("itlils");
        //设置密码
        connectionFactory.setPassword("itlils");

        //2创建长连接
        Connection connection = connectionFactory.newConnection();
        //3创建channel
        Channel channel = connection.createChannel();

        //声明队列
        //String queue,  队列名
        // boolean durable, 持久化
        // boolean exclusive, 排他的
        // boolean autoDelete, 自动删除
        // Map<String, Object> arguments 属性
        channel.queueDeclare(DIRECT_QUEUE_1,true,false,false,null);
        channel.queueDeclare(DIRECT_QUEUE_2,true,false,false,null);

        // 声明交换机
        // String exchange,  交换机名称
        // BuiltinExchangeType type, 交换机类型
        // boolean durable,  持久化
        // boolean autoDelete, 自动删除
        // Map<String, Object> arguments 属性
        channel.exchangeDeclare(DIRECT_EXCHAGE, BuiltinExchangeType.DIRECT,true,false,null);

        //队列绑定交换机
        // String queue, 队列名称
        // String exchange, 交换机名称
        // String routingKey 路由键
        channel.queueBind(DIRECT_QUEUE_1,DIRECT_EXCHAGE,"error");
        channel.queueBind(DIRECT_QUEUE_2,DIRECT_EXCHAGE,"info");
        channel.queueBind(DIRECT_QUEUE_2,DIRECT_EXCHAGE,"error");
        channel.queueBind(DIRECT_QUEUE_2,DIRECT_EXCHAGE,"warning");


        //4监听某个队列
        // String queue, 监听的队列名
        // boolean autoAck, 是否自动应答
        // Consumer callback 回调函数，收到消息，我要干啥
        Consumer consumer=new DefaultConsumer(channel){
            // 回调函数，收到消息，我要干啥
            //  String consumerTag, 消费者标签
            // Envelope envelope, 信封 保存很多信息
            // AMQP.BasicProperties properties, 属性
            // byte[] body  消息字节数组
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                //业务逻辑

                //现在的业务逻辑就是打印
//                System.out.println("consumerTag:"+consumerTag);
//                System.out.println("Exchange:"+envelope.getExchange());
//                System.out.println("RoutingKey:"+envelope.getRoutingKey());
//                System.out.println("DeliveryTag:"+envelope.getDeliveryTag()); //消息id

                System.out.println(new String(body));
            }
        };
        channel.basicConsume(DIRECT_QUEUE_2,true,consumer);

        //5 千万别关闭连接，要不然queue有了消息 推不过来了
//        channel.close();
//        connection.close();



    }
}
```

#### （3）测试

启动所有消费者，然后使用生产者发送消息；在消费者对应的控制台可以查看到生产者发送对应routing key对应队列的消息；到达**按照需要接收**的效果。

在执行完测试代码后，其实到RabbitMQ的管理后台找到`Exchanges`选项卡，点击 `direct_exchange` 的交换机，可以查看到如下的绑定：

![image-20220411214231000](https://www.ydlclass.com/doc21xnv/assets/image-20220411214231000.1e1170ad.png)

测试：

第一个消费者受到一条消息

![image-20220411214259476](https://www.ydlclass.com/doc21xnv/assets/image-20220411214259476.edf4f109.png)

第二个消费者受到三条消息

![image-20220411214316485](https://www.ydlclass.com/doc21xnv/assets/image-20220411214316485.00473d19.png)

#### （4）小结

Routing模式要求队列在绑定交换机时要指定routing key，消息会转发到符合routing key的队列。

### 5、 Topics通配符模式

#### （1） 模式说明

```text
ydlclass.taiyuan.caiwubu.info "本月工资大家涨两千！"
ydlclass.taiyuan.renshi.error "李老师携款潜逃！"
ydlclass.beijing.caiwubu.error "因为李老师逃了，全国所有校区降薪两千。不行就毕业！"
ydlclass.lasa.caiwubu.info "lasa校区成立了！"
ydlclass.taiyuan.shitangbu.info "太原校区学生吃饭免费！"
```

`Topic`类型与`Direct`相比，都是可以根据`RoutingKey`把消息路由到不同的队列。只不过`Topic`类型`Exchange`可以让队列在绑定`Routing key` 的时候**使用通配符**！

```
Routingkey` 一般都是有一个或多个单词组成，多个单词之间以”.”分割，例如： `item.insert
```

通配符规则：

`#`：匹配一个或多个词

`*`：匹配不多不少恰好1个词

举例：

```text
我是太原校区校长 ydlclass.taiyuan.*.*
itlils 我是总部财务主管 ydlclass.*.caiwubu.*
```

![1556031362048](https://www.ydlclass.com/doc21xnv/assets/1556031362048.6cfc1105.png)

![1556031519931](https://www.ydlclass.com/doc21xnv/assets/1556031519931.49fe6b22.png)

图解：

- 红色Queue：绑定的是`usa.#` ，因此凡是以 `usa.`开头的`routing key` 都会被匹配到
- 黄色Queue：绑定的是`#.news` ，因此凡是以 `.news`结尾的 `routing key` 都会被匹配

#### （2） 代码

##### a、生产者

```java
package com.ydlclass.rabbitmq.topic;

import com.rabbitmq.client.BuiltinExchangeType;
import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
public class Producer {

    //交换机名称
    static final String TOPIC_EXCHAGE = "topic_exchange";
    //队列名称
    static final String TOPIC_QUEUE_1 = "topic_queue_1";
    //队列名称
    static final String TOPIC_QUEUE_2 = "topic_queue_2";

    public static void main(String[] args) throws Exception {
        //1创建连接工厂
        ConnectionFactory connectionFactory=new ConnectionFactory();
        //连接的ip
        connectionFactory.setHost("localhost");
        //连接的端口
        connectionFactory.setPort(5672);
        //设置虚拟主机
        connectionFactory.setVirtualHost("/");
        //设置用户名
        connectionFactory.setUsername("itlils");
        //设置密码
        connectionFactory.setPassword("itlils");

        //2创建长连接
        Connection connection = connectionFactory.newConnection();
        //3创建channel
        Channel channel = connection.createChannel();
        //声明队列
        //String queue,  队列名
        // boolean durable, 持久化
        // boolean exclusive, 排他的
        // boolean autoDelete, 自动删除
        // Map<String, Object> arguments 属性
        channel.queueDeclare(TOPIC_QUEUE_1,true,false,false,null);
        channel.queueDeclare(TOPIC_QUEUE_2,true,false,false,null);

        // 声明交换机
        // String exchange,  交换机名称
        // BuiltinExchangeType type, 交换机类型
        // boolean durable,  持久化
        // boolean autoDelete, 自动删除
        // Map<String, Object> arguments 属性
        channel.exchangeDeclare(TOPIC_EXCHAGE, BuiltinExchangeType.TOPIC,true,false,null);

        //队列绑定交换机
        // String queue, 队列名称
        // String exchange, 交换机名称
        // String routingKey 路由键
        channel.queueBind(TOPIC_QUEUE_1,TOPIC_EXCHAGE,"ydlclass.taiyuan.*.*"); //我是太原校区校长的队列
        channel.queueBind(TOPIC_QUEUE_2,TOPIC_EXCHAGE,"ydlclass.*.caiwubu.*");//我是总部财务主管的队列

        //4发消息
        // String exchange,  交换机
        // String routingKey, 路由键
        // AMQP.BasicProperties props, 属性
        // byte[] body 消息      string byte[] char[]如何相互转换的？
        String msg1="hello rabbitmq!topic  本月工资大家涨两千！";
        channel.basicPublish(TOPIC_EXCHAGE,"ydlclass.taiyuan.caiwubu.info",null,msg1.getBytes());

        String msg2="hello rabbitmq!topic 李老师携款潜逃！";
        channel.basicPublish(TOPIC_EXCHAGE,"ydlclass.taiyuan.renshi.error",null,msg2.getBytes());

        String msg3="hello rabbitmq!topic  因为李老师逃了，全国所有校区降薪两千。不行就毕业！";
        channel.basicPublish(TOPIC_EXCHAGE,"ydlclass.beijing.caiwubu.error",null,msg3.getBytes());

        //5关闭连接  资源关闭的顺序，先关后出来的资源，最后关，第一个资源
        channel.close();
        connection.close();
    }
}
```

##### b、消费者1

接收两种类型的消息：更新商品和删除商品

```java
package com.ydlclass.rabbitmq.topic;

import com.rabbitmq.client.*;

import java.io.IOException;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
public class Consumer1 {
    //交换机名称
    static final String TOPIC_EXCHAGE = "topic_exchange";
    //队列名称
    static final String TOPIC_QUEUE_1 = "topic_queue_1";
    //队列名称
    static final String TOPIC_QUEUE_2 = "topic_queue_2";

    public static void main(String[] args) throws Exception {
        //1创建连接工厂
        ConnectionFactory connectionFactory=new ConnectionFactory();
        //连接的ip
        connectionFactory.setHost("localhost");
        //连接的端口
        connectionFactory.setPort(5672);
        //设置虚拟主机
        connectionFactory.setVirtualHost("/");
        //设置用户名
        connectionFactory.setUsername("itlils");
        //设置密码
        connectionFactory.setPassword("itlils");

        //2创建长连接
        Connection connection = connectionFactory.newConnection();
        //3创建channel
        Channel channel = connection.createChannel();

        //声明队列
        //String queue,  队列名
        // boolean durable, 持久化
        // boolean exclusive, 排他的
        // boolean autoDelete, 自动删除
        // Map<String, Object> arguments 属性
        channel.queueDeclare(TOPIC_QUEUE_1,true,false,false,null);
        channel.queueDeclare(TOPIC_QUEUE_2,true,false,false,null);

        // 声明交换机
        // String exchange,  交换机名称
        // BuiltinExchangeType type, 交换机类型
        // boolean durable,  持久化
        // boolean autoDelete, 自动删除
        // Map<String, Object> arguments 属性
        channel.exchangeDeclare(TOPIC_EXCHAGE, BuiltinExchangeType.TOPIC,true,false,null);

        //队列绑定交换机
        // String queue, 队列名称
        // String exchange, 交换机名称
        // String routingKey 路由键
        channel.queueBind(TOPIC_QUEUE_1,TOPIC_EXCHAGE,"ydlclass.taiyuan.*.*"); //我是太原校区校长的队列
        channel.queueBind(TOPIC_QUEUE_2,TOPIC_EXCHAGE,"ydlclass.*.caiwubu.*");//我是总部财务主管的队列


        //4监听某个队列
        // String queue, 监听的队列名
        // boolean autoAck, 是否自动应答
        // Consumer callback 回调函数，收到消息，我要干啥
        Consumer consumer=new DefaultConsumer(channel){
            // 回调函数，收到消息，我要干啥
            //  String consumerTag, 消费者标签
            // Envelope envelope, 信封 保存很多信息
            // AMQP.BasicProperties properties, 属性
            // byte[] body  消息字节数组
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                //业务逻辑

                //现在的业务逻辑就是打印
//                System.out.println("consumerTag:"+consumerTag);
//                System.out.println("Exchange:"+envelope.getExchange());
//                System.out.println("RoutingKey:"+envelope.getRoutingKey());
//                System.out.println("DeliveryTag:"+envelope.getDeliveryTag()); //消息id

                System.out.println(new String(body));
            }
        };
        channel.basicConsume(TOPIC_QUEUE_1,true,consumer);

        //5 千万别关闭连接，要不然queue有了消息 推不过来了
//        channel.close();
//        connection.close();



    }
}
```

##### c、消费者2

接收所有类型的消息：新增商品，更新商品和删除商品。

```java
package com.ydlclass.rabbitmq.topic;

import com.rabbitmq.client.*;

import java.io.IOException;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
public class Consumer2 {
    //交换机名称
    static final String TOPIC_EXCHAGE = "topic_exchange";
    //队列名称
    static final String TOPIC_QUEUE_1 = "topic_queue_1";
    //队列名称
    static final String TOPIC_QUEUE_2 = "topic_queue_2";

    public static void main(String[] args) throws Exception {
        //1创建连接工厂
        ConnectionFactory connectionFactory=new ConnectionFactory();
        //连接的ip
        connectionFactory.setHost("localhost");
        //连接的端口
        connectionFactory.setPort(5672);
        //设置虚拟主机
        connectionFactory.setVirtualHost("/");
        //设置用户名
        connectionFactory.setUsername("itlils");
        //设置密码
        connectionFactory.setPassword("itlils");

        //2创建长连接
        Connection connection = connectionFactory.newConnection();
        //3创建channel
        Channel channel = connection.createChannel();

        //声明队列
        //String queue,  队列名
        // boolean durable, 持久化
        // boolean exclusive, 排他的
        // boolean autoDelete, 自动删除
        // Map<String, Object> arguments 属性
        channel.queueDeclare(TOPIC_QUEUE_1,true,false,false,null);
        channel.queueDeclare(TOPIC_QUEUE_2,true,false,false,null);

        // 声明交换机
        // String exchange,  交换机名称
        // BuiltinExchangeType type, 交换机类型
        // boolean durable,  持久化
        // boolean autoDelete, 自动删除
        // Map<String, Object> arguments 属性
        channel.exchangeDeclare(TOPIC_EXCHAGE, BuiltinExchangeType.TOPIC,true,false,null);

        //队列绑定交换机
        // String queue, 队列名称
        // String exchange, 交换机名称
        // String routingKey 路由键
        channel.queueBind(TOPIC_QUEUE_1,TOPIC_EXCHAGE,"ydlclass.taiyuan.*.*"); //我是太原校区校长的队列
        channel.queueBind(TOPIC_QUEUE_2,TOPIC_EXCHAGE,"ydlclass.*.caiwubu.*");//我是总部财务主管的队列


        //4监听某个队列
        // String queue, 监听的队列名
        // boolean autoAck, 是否自动应答
        // Consumer callback 回调函数，收到消息，我要干啥
        Consumer consumer=new DefaultConsumer(channel){
            // 回调函数，收到消息，我要干啥
            //  String consumerTag, 消费者标签
            // Envelope envelope, 信封 保存很多信息
            // AMQP.BasicProperties properties, 属性
            // byte[] body  消息字节数组
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                //业务逻辑

                //现在的业务逻辑就是打印
//                System.out.println("consumerTag:"+consumerTag);
//                System.out.println("Exchange:"+envelope.getExchange());
//                System.out.println("RoutingKey:"+envelope.getRoutingKey());
//                System.out.println("DeliveryTag:"+envelope.getDeliveryTag()); //消息id

                System.out.println(new String(body));
            }
        };
        channel.basicConsume(TOPIC_QUEUE_2,true,consumer);

        //5 千万别关闭连接，要不然queue有了消息 推不过来了
//        channel.close();
//        connection.close();



    }
}
```

#### （3）测试

启动所有消费者，然后使用生产者发送消息；在消费者对应的控制台可以查看到生产者发送对应routing key对应队列的消息；到达**按照需要接收**的效果；并且这些routing key可以使用通配符。

在执行完测试代码后，其实到RabbitMQ的管理后台找到`Exchanges`选项卡，点击 `topic_exchange` 的交换机，可以查看到如下的绑定：

![image-20220411220413658](https://www.ydlclass.com/doc21xnv/assets/image-20220411220413658.af621fe7.png)

测试：

消费者1 接受2条消息

![image-20220411220433328](https://www.ydlclass.com/doc21xnv/assets/image-20220411220433328.8c0bb510.png)

消费者2 接受2条消息

![image-20220411220453524](https://www.ydlclass.com/doc21xnv/assets/image-20220411220453524.41faca02.png)

#### （4）小结

Topic主题模式可以实现 `Publish/Subscribe发布与订阅模式` 和 `Routing路由模式` 的功能；只是Topic在配置routing key 的时候可以使用通配符，显得更加灵活。

### 6、 模式总结

RabbitMQ工作模式：

#### **（1）简单模式 HelloWorld**

一个生产者、一个消费者，不需要设置交换机（使用默认的交换机）。

#### **（2）工作队列模式 Work Queue**

一个生产者、多个消费者（竞争关系），不需要设置交换机（使用默认的交换机）。

#### **（3）发布订阅模式 Publish/subscribe**

需要设置类型为fanout的交换机，并且交换机和队列进行绑定，当发送消息到交换机后，交换机会将消息发送到绑定的队列。

#### **（4）路由模式 Routing**

需要设置类型为direct的交换机，交换机和队列进行绑定，并且指定routing key，当发送消息到交换机后，交换机会根据routing key将消息发送到对应的队列。

#### **（5）通配符模式 Topic**

需要设置类型为topic的交换机，交换机和队列进行绑定，并且指定通配符方式的routing key，当发送消息到交换机后，交换机会根据routing key将消息发送到对应的队列。

## 第六章 Spring 整合RabbitMQ-了解

### 1、搭建生产者工程

#### （1）创建工程

spring-rabbitmq-producer

![image-20200320222056543](https://www.ydlclass.com/doc21xnv/assets/image-20200320222056543.22a9b177.png)

#### （2）添加依赖

修改pom.xml文件内容为如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.ydlclass</groupId>
    <artifactId>spring-rabbitmq-producer</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.1.7.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.springframework.amqp</groupId>
            <artifactId>spring-rabbit</artifactId>
            <version>2.1.8.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>5.1.7.RELEASE</version>
        </dependency>
    </dependencies>

        <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.0</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

#### （3） 配置整合

1. 创建`resources\rabbitmq.properties`连接参数等配置文件；

```properties
rabbitmq.host=localhost
rabbitmq.port=5672
rabbitmq.username=itlils
rabbitmq.password=itlils
rabbitmq.virtual-host=/
```

1. 创建 `resources\spring-rabbitmq.xml` 整合配置文件；

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:rabbit="http://www.springframework.org/schema/rabbit"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/rabbit
       http://www.springframework.org/schema/rabbit/spring-rabbit.xsd">
    <!--加载配置文件-->
    <context:property-placeholder location="classpath:rabbitmq.properties"/>

    <!-- 定义rabbitmq connectionFactory -->
    <rabbit:connection-factory id="connectionFactory"
                               host="${rabbitmq.host}"
                               port="${rabbitmq.port}"
                               username="${rabbitmq.username}"
                               password="${rabbitmq.password}"
                               virtual-host="${rabbitmq.virtual-host}"/>
    <!--定义管理交换机、队列-->
    <rabbit:admin connection-factory="connectionFactory"/>

    <!--定义持久化队列，不存在则自动创建；不绑定到交换机则绑定到默认交换机
    默认交换机类型为direct，名字为：""，路由键为队列的名称
    -->
    <rabbit:queue id="ydlqueue" name="ydlqueue" auto-declare="true"/>

    <!-- ~~~~~~~~~~~~~~~~~~~~~~~~~~~~广播；所有队列都能收到消息~~~~~~~~~~~~~~~~~~~~~~~~~~~~ -->
    <!--定义广播交换机中的持久化队列，不存在则自动创建-->
    <rabbit:queue id="spring_fanout_queue_1" name="spring_fanout_queue_1" auto-declare="true" auto-delete="false" durable="true"/>
    <!--定义广播交换机中的持久化队列，不存在则自动创建-->
    <rabbit:queue id="spring_fanout_queue_2" name="spring_fanout_queue_2" auto-declare="true"/>

    <!--定义广播类型交换机；并绑定上述两个队列-->
    <rabbit:fanout-exchange id="spring_fanout_exchange" name="spring_fanout_exchange" auto-declare="true">
        <rabbit:bindings>
            <rabbit:binding queue="spring_fanout_queue_1"/>
            <rabbit:binding queue="spring_fanout_queue_2"/>
        </rabbit:bindings>
    </rabbit:fanout-exchange>

    <!-- routing 模式-->
    <rabbit:queue id="spring_direct_queue_1" name="spring_direct_queue_1" auto-declare="true"/>
    <rabbit:queue id="spring_direct_queue_2" name="spring_direct_queue_2" auto-declare="true"/>

    <rabbit:direct-exchange id="spring_direct_exchange" name="spring_direct_exchange" auto-declare="true">
        <rabbit:bindings>
            <rabbit:binding queue="spring_direct_queue_1" key="error"></rabbit:binding>
            <rabbit:binding queue="spring_direct_queue_2" key="info"></rabbit:binding>
            <rabbit:binding queue="spring_direct_queue_2" key="error"></rabbit:binding>
            <rabbit:binding queue="spring_direct_queue_2" key="warning"></rabbit:binding>
        </rabbit:bindings>
    </rabbit:direct-exchange>


    <!-- ~~~~~~~~~~~~~~~~~~~~~~~~~~~~通配符；*匹配一个单词，#匹配多个单词 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~ -->
    <rabbit:queue id="spring_topic_queue_1" name="spring_topic_queue_1" auto-declare="true"/>
    <rabbit:queue id="spring_topic_queue_2" name="spring_topic_queue_2" auto-declare="true"/>

    <rabbit:topic-exchange id="spring_topic_exchange" name="spring_topic_exchange">
        <rabbit:bindings>
            <rabbit:binding queue="spring_topic_queue_1" pattern="ydlclass.taiyuan.*.*"></rabbit:binding>
            <rabbit:binding queue="spring_topic_queue_2" pattern="ydlclass.*.caiwubu.*"></rabbit:binding>
        </rabbit:bindings>
    </rabbit:topic-exchange>

    <!--定义rabbitTemplate对象操作可以在代码中方便发送消息-->
    <rabbit:template id="rabbitTemplate" connection-factory="connectionFactory"/>
</beans>
```

**注意**：

xxxtemplate作用就是帮我们和第三方组件键连接，断链接。我们只需要关注核心业务逻辑即可。

#### （4）发送消息

![image-20220413210312813](https://www.ydlclass.com/doc21xnv/assets/image-20220413210312813.86100d12.png)

创建测试文件 test\java\com\ydlclass\rabbitmq\ProducerTest.java

```java
package com.ydlclass.rabbitmq;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.amqp.rabbit.core.RabbitTemplate;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "classpath:spring-rabbitmq.xml")
public class ProducerTest {
    @Autowired
    RabbitTemplate rabbitTemplate;

    //简单模式发送
    @Test
    public void helloTest(){
        // String exchange,  交换机
        // String routingKey,  路由键
        // Object message     Object消息体 User car lunchuan
        String msg="hello rabbitmq!";
        rabbitTemplate.convertAndSend("","ydlqueue",msg);
    }

    //测试发布订阅模式
    @Test
    public void publishTest(){
        String msg="hello rabbitmq! publish";
        rabbitTemplate.convertAndSend("spring_fanout_exchange","",msg);
    }

    //测试routing模式
    @Test
    public void routingTest(){
        String msg="hello rabbitmq!routing error";
        rabbitTemplate.convertAndSend("spring_direct_exchange","error",msg);

        String msg1="hello rabbitmq!routing info";
        rabbitTemplate.convertAndSend("spring_direct_exchange","info",msg1);

        String msg2="hello rabbitmq!routing warning";
        rabbitTemplate.convertAndSend("spring_direct_exchange","warning",msg2);
    }

    //测试topic模式
    @Test
    public void topicTest(){
        String msg1="hello rabbitmq!topic  本月工资大家涨两千！";
        rabbitTemplate.convertAndSend("spring_topic_exchange","ydlclass.taiyuan.caiwubu.info",msg1);

        String msg2="hello rabbitmq!topic 李老师携款潜逃！";
        rabbitTemplate.convertAndSend("spring_topic_exchange","ydlclass.taiyuan.renshi.error",msg2);

        String msg3="hello rabbitmq!topic  因为李老师逃了，全国所有校区降薪两千。不行就毕业！";
        rabbitTemplate.convertAndSend("spring_topic_exchange","ydlclass.beijing.caiwubu.error",msg3);
    }

}
```

发布订阅：

发送者

![image-20220413211130569](https://www.ydlclass.com/doc21xnv/assets/image-20220413211130569.cbb13308.png)

队列：两个队列都有一条消息

![image-20220413211231511](https://www.ydlclass.com/doc21xnv/assets/image-20220413211231511.7448120c.png)

路由模式：

![image-20220413212246642](https://www.ydlclass.com/doc21xnv/assets/image-20220413212246642.12850ddb.png)

队列1只有1条消息

![image-20220413212324693](https://www.ydlclass.com/doc21xnv/assets/image-20220413212324693.ee3bfce9.png)

队列2有3条消息

![image-20220413212349991](https://www.ydlclass.com/doc21xnv/assets/image-20220413212349991.7afe9e22.png)

topic模式：

![image-20220413213316120](https://www.ydlclass.com/doc21xnv/assets/image-20220413213316120.9a5638cf.png)

队列1 2条消息

![image-20220413213334943](https://www.ydlclass.com/doc21xnv/assets/image-20220413213334943.f6c4e0da.png)

队列2 2条消息

![image-20220413213345957](https://www.ydlclass.com/doc21xnv/assets/image-20220413213345957.8a6ac58c.png)

### 2、搭建消费者工程

#### （1）创建工程

spring-rabbitmq-consumer

![image-20220413215506816](https://www.ydlclass.com/doc21xnv/assets/image-20220413215506816.413652ef.png)

#### （2）添加依赖

修改pom.xml文件内容为如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.ydlclass</groupId>
    <artifactId>spring-rabbitmq-consumer</artifactId>
    <version>1.0-SNAPSHOT</version>
    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.1.7.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.springframework.amqp</groupId>
            <artifactId>spring-rabbit</artifactId>
            <version>2.1.8.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>5.1.7.RELEASE</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.0</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

#### （3） 配置整合

1. 创建`resources\rabbitmq.properties`连接参数等配置文件；

```properties
rabbitmq.host=localhost
rabbitmq.port=5672
rabbitmq.username=itlils
rabbitmq.password=itlils
rabbitmq.virtual-host=/
```

1. 创建 resources\spring-rabbitmq.xml` 整合配置文件；

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:rabbit="http://www.springframework.org/schema/rabbit"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/rabbit
       http://www.springframework.org/schema/rabbit/spring-rabbit.xsd">
    <!--加载配置文件-->
    <context:property-placeholder location="classpath:rabbitmq.properties"/>

    <!-- 定义rabbitmq connectionFactory -->
    <rabbit:connection-factory id="connectionFactory"
                               host="${rabbitmq.host}"
                               port="${rabbitmq.port}"
                               username="${rabbitmq.username}"
                               password="${rabbitmq.password}"
                               virtual-host="${rabbitmq.virtual-host}"/>

    <bean id="springQueueListener" class="com.ydlclass.rabbitmq.listener.SpringQueueListener"/>
<!--    <bean id="fanoutListener1" class="com.ydlclass.rabbitmq.listener.FanoutListener1"/>-->
<!--    <bean id="fanoutListener2" class="com.ydlclass.rabbitmq.listener.FanoutListener2"/>-->
<!--    <bean id="topicListenerStar" class="com.ydlclass.rabbitmq.listener.TopicListenerStar"/>-->
<!--    <bean id="topicListenerWell" class="com.ydlclass.rabbitmq.listener.TopicListenerWell"/>-->
<!--    <bean id="topicListenerWell2" class="com.ydlclass.rabbitmq.listener.TopicListenerWell2"/>-->

    <rabbit:listener-container connection-factory="connectionFactory" auto-declare="true">
        <rabbit:listener ref="springQueueListener" queue-names="ydlqueue"/>
<!--        <rabbit:listener ref="fanoutListener1" queue-names="spring_fanout_queue_1"/>-->
<!--        <rabbit:listener ref="fanoutListener2" queue-names="spring_fanout_queue_2"/>-->
<!--        <rabbit:listener ref="topicListenerStar" queue-names="spring_topic_queue_star"/>-->
<!--        <rabbit:listener ref="topicListenerWell" queue-names="spring_topic_queue_well"/>-->
<!--        <rabbit:listener ref="topicListenerWell2" queue-names="spring_topic_queue_well2"/>-->
    </rabbit:listener-container>
</beans>
```

#### （4） 消息监听器

##### a、队列监听器

创建 `spring-rabbitmq-consumer\src\main\java\com\ydlclass\rabbitmq\listener\SpringQueueListener.java`

```java
package com.ydlclass.rabbitmq.listener;

import org.springframework.amqp.core.Message;
import org.springframework.amqp.core.MessageListener;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
//缺啥补啥 干就完事
public class SpringQueueListener implements MessageListener {
    //有了消息 做什么
    @Override
    public void onMessage(Message message) {
        //业务逻辑
        byte[] body = message.getBody();
        System.out.println(new String(body));
    }
}
```

为了让spring容器一直运行，我们需要在测试类中，启动spring容器：com.ydlclass.rabbimq

```java
package com.ydlclass.rabbitmq;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "classpath:spring-rabbitmq.xml")
public class ConsumerTest {

    @Test
    public void consumerTest(){
        while (true){

        }
    }
}
```

##### b、广播监听器1

创建 `spring-rabbitmq-consumer\src\main\java\com\ydlclass\rabbitmq\listener\FanoutListener1.java`

##### c、广播监听器2

创建 `spring-rabbitmq-consumer\src\main\java\com\ydlclass\rabbitmq\listener\FanoutListener2.java`

##### d、星号通配符监听器

创建 `spring-rabbitmq-consumer\src\main\java\com\ydlclass\rabbitmq\listener\TopicListenerStar.java`

##### e、井号通配符监听器

创建 `spring-rabbitmq-consumer\src\main\java\com\ydlclass\rabbitmq\listener\TopicListenerWell.java`

##### f、井号通配符监听器2

创建 `spring-rabbitmq-consumer\src\main\java\com\ydlclass\rabbitmq\listener\TopicListenerWell2.java`

测试：

![image-20220413215534196](https://www.ydlclass.com/doc21xnv/assets/image-20220413215534196.0ac570bd.png)

## 第七章 Spring Boot整合RabbitMQ

### 1、 简介

在Spring项目中，可以使用Spring-Rabbit去操作RabbitMQ https://github.com/spring-projects/spring-amqp

尤其是在spring boot项目中只需要引入对应的amqp启动器依赖即可，方便的使用RabbitTemplate发送消息，使用注解接收消息。

*一般在开发过程中*：

**生产者工程：**

1. application.yml文件配置RabbitMQ相关信息；
2. 在生产者工程中编写配置类，用于创建交换机和队列，并进行绑定
3. 注入RabbitTemplate对象，通过RabbitTemplate对象发送消息到交换机

**消费者工程：**

1. application.yml文件配置RabbitMQ相关信息
2. 创建消息处理类，用于接收队列中的消息并进行处理

### 2、搭建生产者工程

#### （1） 创建工程

创建生产者工程springboot-rabbitmq-producer

![image-20220413222931239](https://www.ydlclass.com/doc21xnv/assets/image-20220413222931239.b56a680b.png)

#### （2） 添加依赖

修改pom.xml文件内容为如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.4.RELEASE</version>
    </parent>
    <groupId>com.ydlclass</groupId>
    <artifactId>springboot-rabbitmq-producer</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-amqp</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
        </dependency>
    </dependencies>
</project>
```

#### （3） 启动类

```java
package com.ydlclass.rabbitmq;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
@SpringBootApplication
public class ProducerApp {
    public static void main(String[] args) {
        SpringApplication.run(ProducerApp.class,args);
    }
}
```

#### （4）配置RabbitMQ

##### a、配置文件

创建application.yml，内容如下：

```yaml
spring:
  rabbitmq:
    host: localhost
    port: 5672
    virtual-host: /
    username: itlils
    password: itlils
```

##### b、绑定交换机和队列

创建RabbitMQ队列与交换机绑定的配置类com.ydlclass.rabbitmq.config.RabbitMQConfig

```java
package com.ydlclass.rabbitmq.config;

import org.springframework.amqp.core.*;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
@Configuration
public class RabbitConfig {

    @Bean("boot_hello_queue")
    public Queue queue(){
        //String queue,  队列名
        // boolean durable, 持久化
        // boolean exclusive, 排他的
        // boolean autoDelete, 自动删除
        // Map<String, Object> arguments 属性
        return new Queue("boot_hello_queue",true,false,false,null);
    }

    //发布订阅模式
    //交换机名称
   public static final String FANOUT_EXCHAGE = "boot_fanout_exchange";
    //队列名称
    public static final String FANOUT_QUEUE_1 = "boot_fanout_queue_1";
    //队列名称
    public static final String FANOUT_QUEUE_2 = "boot_fanout_queue_2";

    @Bean(FANOUT_QUEUE_1)
    public Queue FANOUT_QUEUE_1(){
        return new Queue(FANOUT_QUEUE_1,true,false,false,null);
    }
    @Bean(FANOUT_QUEUE_2)
    public Queue FANOUT_QUEUE_2(){
        return new Queue(FANOUT_QUEUE_2,true,false,false,null);
    }
    @Bean(FANOUT_EXCHAGE)
    public Exchange FANOUT_EXCHAGE(){
        return ExchangeBuilder.fanoutExchange(FANOUT_EXCHAGE).durable(true).build();
    }
    @Bean
    public Binding FANOUT_QUEUE_1_FANOUT_EXCHAGE(@Qualifier(FANOUT_QUEUE_1) Queue queue,
                                                 @Qualifier(FANOUT_EXCHAGE) Exchange exchange){
        return BindingBuilder.bind(queue).to(exchange).with("").noargs();
    }
    @Bean
    public Binding FANOUT_QUEUE_2_FANOUT_EXCHAGE(@Qualifier(FANOUT_QUEUE_2) Queue queue,
                                                 @Qualifier(FANOUT_EXCHAGE) Exchange exchange){
        return BindingBuilder.bind(queue).to(exchange).with("").noargs();
    }

    //作业
    //routing模式

    //topic模式


}
```

测试类

```text
package com.ydlclass.rabbitmq;

import com.ydlclass.rabbitmq.config.RabbitConfig;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.amqp.rabbit.core.RabbitTemplate;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
@RunWith(SpringRunner.class)
@SpringBootTest(classes = ProducerApp.class)
public class ProducerTest {

    @Autowired
    RabbitTemplate rabbitTemplate;

    @Test
    public void helloTest(){
        String msg="hello rabbitmq!";
        rabbitTemplate.convertAndSend("","boot_hello_queue",msg);
    }

    @Test
    public void publishTest(){
        String msg="hello rabbitmq!publishTest";
        rabbitTemplate.convertAndSend(RabbitConfig.FANOUT_EXCHAGE,"",msg);
    }

    //作业 routing
    //topic
}
```

简单模式：

![image-20220413221046286](https://www.ydlclass.com/doc21xnv/assets/image-20220413221046286.35eb38b7.png)

发布订阅：

![image-20220413222539201](https://www.ydlclass.com/doc21xnv/assets/image-20220413222539201.769338b4.png)

每个队列都有一条消息了

![image-20220413222558184](https://www.ydlclass.com/doc21xnv/assets/image-20220413222558184.c6db4b62.png)

 ![image-20220413222609681](https://www.ydlclass.com/doc21xnv/assets/image-20220413222609681.d010ab71.png)

### 3、搭建消费者工程

#### （1） 创建工程

创建消费者工程springboot-rabbitmq-consumer

![image-20220413233208158](https://www.ydlclass.com/doc21xnv/assets/image-20220413233208158.458a7865.png)

#### （2） 添加依赖

修改pom.xml文件内容为如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.4.RELEASE</version>
    </parent>
    <groupId>com.ydlclass</groupId>
    <artifactId>springboot-rabbitmq-consumer</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-amqp</artifactId>
        </dependency>
    </dependencies>

</project>
```

#### （3） 启动类

```java
package com.ydlclass.rabbitmq;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
@SpringBootApplication
public class ConsumerApp {
    public static void main(String[] args) {
        SpringApplication.run(ConsumerApp.class,args);
    }
}
```

#### （4） 配置RabbitMQ

创建application.yml，内容如下：

```yaml
spring:
  rabbitmq:
    host: localhost
    port: 5672
    virtual-host: /
    username: itlils
    password: itlils
```

#### （5） 消息监听处理类

编写消息监听器com.ydlclass.rabbitmq.listener.MyListener

```java
package com.ydlclass.rabbitmq.listener;

import com.ydlclass.rabbitmq.config.RabbitConfig;
import org.springframework.amqp.core.Message;
import org.springframework.amqp.core.MessageProperties;
import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.stereotype.Component;

/**
 * @Created by IT李老师
 * 公主号 “元动力课堂”
 * 个人微 itlils
 */
@Component
public class MyListener {

    @RabbitListener(queues = RabbitConfig.FANOUT_QUEUE_1)
    public void receiveMsg(Message message){
        //业务逻辑
        byte[] body = message.getBody();
        System.out.println(new String(body));

        MessageProperties messageProperties = message.getMessageProperties(); //参数
        System.out.println(messageProperties.getMessageId());
        System.out.println(messageProperties.getDeliveryTag());
        System.out.println(messageProperties.getReceivedRoutingKey());
        System.out.println(messageProperties.getConsumerTag());
    }

}
```

### 4、 测试

先运行上述测试程序（交换机和队列才能先被声明和绑定），然后启动消费者；在消费者工程springboot-rabbitmq-consumer中控制台查看是否接收到对应消息。

另外；也可以在RabbitMQ的管理控制台中查看到交换机与队列的绑定。

# RabbitMQ高级 学习目标

- 掌握RabbitMQ 高级特性

- 理解RabbitMQ 应用问题

- 能够搭建RabbitMQ 集群

  ![image-20200320230112442](https://www.ydlclass.com/doc21xnv/assets/image-20200320230112442.9919b2e6.png)

## 第一章 RabbitMQ 高级特性

![image-20200616093631383](https://www.ydlclass.com/doc21xnv/assets/image-20200616093631383.0e0db4ef.png)

### 1、消息可靠性投递

在使用 RabbitMQ 的时候，作为消息发送方希望杜绝任何消息丢失或者投递失败场景。RabbitMQ 为我们提供了两种方式用来控制消息的投递可靠性模式。

- confirm **确认模式**
- return **退回模式**

rabbitmq 整个消息投递的路径为：

 **producer ---> rabbitmq broker ---> exchange ---> queue ---> consumer**

- 消息从 producer 到 exchange 则会返回一个 confirmCallback 。
- 消息从 exchange 到 queue 投递失败则会返回一个 returnCallback 。

我们将利用这两个 callback 控制消息的可靠性投递

#### （1）confirm确认模式代码实现

1. 创建maven工程，消息的生产者工程，项目模块名称：rabbitmq-producer-spring

2. 添加依赖

   ```xml
   <dependencies>
       <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-context</artifactId>
           <version>5.1.7.RELEASE</version>
       </dependency>
   
       <dependency>
           <groupId>org.springframework.amqp</groupId>
           <artifactId>spring-rabbit</artifactId>
           <version>2.1.8.RELEASE</version>
       </dependency>
       <dependency>
           <groupId>junit</groupId>
           <artifactId>junit</artifactId>
           <version>4.12</version>
       </dependency>
       <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-test</artifactId>
           <version>5.1.7.RELEASE</version>
       </dependency>
   </dependencies>
   
   <build>
       <plugins>
           <plugin>
               <groupId>org.apache.maven.plugins</groupId>
               <artifactId>maven-compiler-plugin</artifactId>
               <version>3.8.0</version>
               <configuration>
                   <source>1.8</source>
                   <target>1.8</target>
               </configuration>
           </plugin>
       </plugins>
   </build>
   ```

   

3. 在 resources 目录下创建 rabbitmq.properties 配置文件，添加连接RabbitMQ相关信息

   ```properties
   rabbitmq.host=localhost
   rabbitmq.port=5672
   rabbitmq.username=guest
   rabbitmq.password=guest
   rabbitmq.virtual-host=/
   ```

4. 在 resources 目录下创建 spring-rabbitmq-producer.xml 配置文件，添加以下配置

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:rabbit="http://www.springframework.org/schema/rabbit"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
                    http://www.springframework.org/schema/beans/spring-beans.xsd
                    http://www.springframework.org/schema/context
                    https://www.springframework.org/schema/context/spring-context.xsd
                    http://www.springframework.org/schema/rabbit
                    http://www.springframework.org/schema/rabbit/spring-rabbit.xsd">
       <!--加载配置文件-->
       <context:property-placeholder location="classpath:rabbitmq.properties"/>
   
       <!-- 定义rabbitmq connectionFactory  1. 设置  publisher-confirms="true" -->
       <rabbit:connection-factory id="connectionFactory" host="${rabbitmq.host}"
                                  port="${rabbitmq.port}"
                                  username="${rabbitmq.username}"
                                  password="${rabbitmq.password}"
                                  virtual-host="${rabbitmq.virtual-host}"
                                  
                                  publisher-confirms="true"
                                  />
       <!--定义管理交换机、队列-->
       <rabbit:admin connection-factory="connectionFactory"/>
   
       <!--定义rabbitTemplate对象操作可以在代码中方便发送消息-->
       <rabbit:template id="rabbitTemplate" connection-factory="connectionFactory"/>
   
       <!--2. 消息可靠性投递（生产端）-->
      <rabbit:queue id="test_queue_confirm" name="test_queue_confirm"></rabbit:queue>
       <rabbit:direct-exchange name="test_exchange_confirm">
           <rabbit:bindings>
               <rabbit:binding queue="test_queue_confirm" key="confirm">			               </rabbit:binding>
           </rabbit:bindings>
       </rabbit:direct-exchange>
       
   </beans>
   ```

5. 编写测试代码 com.ydlclass.producer

   ```java
   @RunWith(SpringJUnit4ClassRunner.class)
   @ContextConfiguration(locations = "classpath:spring-rabbitmq-producer.xml")
   public class ProducerTest {
   
       @Autowired
       private RabbitTemplate rabbitTemplate;
   
       /**
        * 确认模式：
        * 步骤：
        * 1. 确认模式开启：ConnectionFactory中开启publisher-confirms="true"
        * 2. 在rabbitTemplate定义ConfirmCallBack回调函数
        */
       @Test
       public void testConfirm() {
   
           //2. 定义回调 **
           rabbitTemplate.setConfirmCallback(new RabbitTemplate.ConfirmCallback() {
               /**
                *
                * @param correlationData 相关配置信息
                * @param ack   exchange交换机 是否成功收到了消息。true 成功，false代表失败
                * @param cause 失败原因
                */
               @Override
               public void confirm(CorrelationData correlationData, boolean ack, String cause) {
                   System.out.println("confirm方法被执行了....");
   
                   if (ack) {
                       //接收成功
                       System.out.println("接收成功消息" + cause);
                   } else {
                       //接收失败
                       System.out.println("接收失败消息" + cause);
                       //做一些处理，让消息再次发送。
                   }
               }
           });
   
           //3. 发送消息
           rabbitTemplate.convertAndSend("test_exchange_confirm111", "confirm", "message confirm....");
       }
   }
   ```

6. 测试结果

   成功：

   ![image-20220714120818639](https://www.ydlclass.com/doc21xnv/assets/image-20220714120818639.d5d2bd3b.png)

   失败：

   ![image-20220714120929607](https://www.ydlclass.com/doc21xnv/assets/image-20220714120929607.e9b31074.png)

#### （2） return退回模式代码实现

回退模式： 当消息发送给Exchange后，Exchange路由到Queue失败是 才会执行 ReturnCallBack，具体实现如下：

1. 在 spring-rabbitmq-producer.xml 配置文件，在 **rabbit:connection-factory**节点 添加配置：

   ```text
   publisher-returns="true"
   ```

   1

2. 编写测试方法

   ```java
   /**
    * 步骤：
    * 1. 开启回退模式:publisher-returns="true"
    * 2. 设置ReturnCallBack
    * 3. 设置Exchange处理消息的模式：
    *  1. 如果消息没有路由到Queue，则丢弃消息（默认）
    *  2. 如果消息没有路由到Queue，返回给消息发送方ReturnCallBack
    */
   
   @Test
   public void testReturn() {
   
       //设置交换机处理失败消息的模式
       rabbitTemplate.setMandatory(true);
   
       //2.设置ReturnCallBack
       rabbitTemplate.setReturnCallback(new RabbitTemplate.ReturnCallback() {
           /**
            *
            * @param message   消息对象
            * @param replyCode 错误码
            * @param replyText 错误信息
            * @param exchange  交换机
            * @param routingKey 路由键
            */
           @Override
           public void returnedMessage(Message message, int replyCode, String replyText, String exchange, String routingKey) {
               System.out.println("return 执行了....");
   
               System.out.println(message);
               System.out.println(replyCode);
               System.out.println(replyText);
               System.out.println(exchange);
               System.out.println(routingKey);
   
               //处理
           }
       });
   
   
       //3. 发送消息   
       rabbitTemplate.convertAndSend("test_exchange_confirm", "confirm", "message confirm....");
   }
   ```

   设置 routingKey 为一个不符合规则的key，观察控制台打印结果。

   成功：

   ![image-20220714133207266](https://www.ydlclass.com/doc21xnv/assets/image-20220714133207266.a43b3bda.png)

   失败：

   ![image-20220714133352530](https://www.ydlclass.com/doc21xnv/assets/image-20220714133352530.c340e440.png)

#### （3）小结

对于确认模式：

- 设置ConnectionFactory的publisher-confirms="true" 开启 确认模式。
- 使用rabbitTemplate.setConfirmCallback设置回调函数。当消息发送到exchange后回调confirm方法。在方法中判断ack，如果为true，则发送成功，如果为false，则发送失败，需要处理。

对于退回模式

- 设置ConnectionFactory的publisher-returns="true" 开启 退回模式。
- 使用rabbitTemplate.setReturnCallback设置退回函数，当消息从exchange路由到queue失败后，如果设置了rabbitTemplate.setMandatory(true)参数，则会将消息退回给producer。并执行回调函数returnedMessage。

> 在RabbitMQ中也提供了事务机制，但是性能较差，此处不做讲解。
>
> 使用channel列方法，完成事务控制：
>
> txSelect(), 用于将当前channel设置成transaction模式
>
> txCommit()，用于提交事务
>
> txRollback(),用于回滚事务

### 2、Consumer ACK

ack指 **Acknowledge**，确认。 表示消费端收到消息后的确认方式。

有三种确认方式：

• 自动确认：acknowledge="**none**"

• 手动确认：acknowledge="**manual**"

• 根据异常情况确认：acknowledge="**auto**"，（这种方式使用麻烦，不作讲解）

其中自动确认是指，当消息一旦被Consumer接收到，则自动确认收到，并将相应 message 从 RabbitMQ 的消息缓存中移除。但是在实际业务处理中，很可能消息接收到，业务处理出现异常，那么该消息就会丢失。

如果设置了手动确认方式，则需要在业务处理成功后，调用channel.basicAck()，手动签收，如果出现异常，则调用channel.basicNack()方法，让其自动重新发送消息。

#### （1）代码实现

1. 创建maven工程，消息的消费者工程，项目模块名称：rabbitmq-consumer-spring

2. 添加依赖

   ```xml
   <dependencies>
       <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-context</artifactId>
           <version>5.1.7.RELEASE</version>
       </dependency>
       <dependency>
           <groupId>org.springframework.amqp</groupId>
           <artifactId>spring-rabbit</artifactId>
           <version>2.1.8.RELEASE</version>
       </dependency>
       <dependency>
           <groupId>junit</groupId>
           <artifactId>junit</artifactId>
           <version>4.12</version>
       </dependency>
       <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-test</artifactId>
           <version>5.1.7.RELEASE</version>
       </dependency>
   </dependencies>
   
   <build>
       <plugins>
           <plugin>
               <groupId>org.apache.maven.plugins</groupId>
               <artifactId>maven-compiler-plugin</artifactId>
               <version>3.8.0</version>
               <configuration>
                   <source>1.8</source>
                   <target>1.8</target>
               </configuration>
           </plugin>
       </plugins>
   </build>
   ```

3. 在 resources 目录下创建 rabbitmq.properties 配置文件，添加链接RabbitMQ相关信息

   ```properties
   rabbitmq.host=localhost
   rabbitmq.port=5672
   rabbitmq.username=guest
   rabbitmq.password=guest
   rabbitmq.virtual-host=/
   ```

4. 在 resources 目录下创建 spring-rabbitmq-consumer.xml 配置文件，添加以下配置

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:rabbit="http://www.springframework.org/schema/rabbit"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
                http://www.springframework.org/schema/beans/spring-beans.xsd
                http://www.springframework.org/schema/context
                https://www.springframework.org/schema/context/spring-context.xsd
                http://www.springframework.org/schema/rabbit
                http://www.springframework.org/schema/rabbit/spring-rabbit.xsd">
       <!--加载配置文件-->
       <context:property-placeholder location="classpath:rabbitmq.properties"/>
   
       <!-- 定义rabbitmq connectionFactory -->
       <rabbit:connection-factory id="connectionFactory" host="${rabbitmq.host}"
                                  port="${rabbitmq.port}"
                                  username="${rabbitmq.username}"
                                  password="${rabbitmq.password}"
                                  virtual-host="${rabbitmq.virtual-host}"/>
   
   
       <context:component-scan base-package="com.ydlclass.listener" />
   
       <!--定义监听器容器  添加  acknowledge="manual" 手动-->
       <rabbit:listener-container connection-factory="connectionFactory" acknowledge="manual">
           <rabbit:listener ref="ackListener" queue-names="test_queue_confirm"/>
       </rabbit:listener-container>
   
   </beans>
   ```

5. 编写ackListener 监听类实现ChannelAwareMessageListener接口

   ```java
   package com.ydlclass.listener;
   
   import com.rabbitmq.client.Channel;
   import org.springframework.amqp.core.Message;
   import org.springframework.amqp.core.MessageListener;
   import org.springframework.amqp.rabbit.listener.api.ChannelAwareMessageListener;
   import org.springframework.stereotype.Component;
   
   import java.io.IOException;
   
   /**
    * Consumer ACK机制：
    *  1. 设置手动签收。acknowledge="manual"
    *  2. 让监听器类实现ChannelAwareMessageListener接口
    *  3. 如果消息成功处理，则调用channel的 basicAck()签收
    *  4. 如果消息处理失败，则调用channel的basicNack()拒绝签收，broker重新发送给consumer
    */
   @Component
   public class AckListener implements ChannelAwareMessageListener {
   
       @Override
       public void onMessage(Message message, Channel channel) throws Exception {
           long deliveryTag = message.getMessageProperties().getDeliveryTag();
   
           try {
               //1.接收转换消息
               System.out.println(new String(message.getBody()));
   
               //2. 处理业务逻辑
               System.out.println("处理业务逻辑...");
               int i = 3/0;//出现错误
               //3. 手动签收
               channel.basicAck(deliveryTag,true);
           } catch (Exception e) {
               //e.printStackTrace();
   
               //4.拒绝签收
               /*
               第三个参数：requeue：重回队列。如果设置为true，则消息重新回到queue，broker会重新发送该消息给消费端
                */
               channel.basicNack(deliveryTag,true,true);
               // 了解
               //channel.basicReject(deliveryTag,true);
           }
       }
   }
   ```

   

6. 编写测试类，启动容器监听MQ队列 com.ydlclass.rabbimq

   ```java
   @RunWith(SpringJUnit4ClassRunner.class)
   @ContextConfiguration(locations = "classpath:spring-rabbitmq-consumer.xml")
   public class ConsumerTest {
   
       @Test
       public void test(){
           while (true){
   
           }
       }
   }
   ```

![image-20220714135710130](https://www.ydlclass.com/doc21xnv/assets/image-20220714135710130.04d1062b.png)

#### （2）小结

- 在rabbit:listener-container标签中设置acknowledge属性，设置ack方式 none：自动确认，manual：手动确认
- 如果在消费端没有出现异常，则调用channel.basicAck(deliveryTag,false);方法确认签收消息
- 如果出现异常，则在catch中调用 basicNack或 basicReject，拒绝消息，让MQ重新发送消息。

> 如何保证消息的高可靠性传输？
>
> 1.持久化
>
> • exchange要持久化
>
> • queue要持久化
>
> • message要持久化
>
> 2.生产方确认Confirm
>
> 3.消费方确认Ack
>
> 4.Broker高可用

### 3、消费端限流

![1569164559749](https://www.ydlclass.com/doc21xnv/assets/1569164559749.a1e921d8.png)

如上图所示：如果在A系统中需要维护相关的业务功能，可能需要将A系统的服务停止，那么这个时候消息的生产者还是一直会向MQ中发送待处理的消息，消费者此时服务已经关闭，导致大量的消息都会在MQ中累积。如果当A系统成功启动后，默认情况下消息的消费者会一次性将MQ中累积的大量的消息全部拉取到自己的服务，导致服务在短时间内会处理大量的业务，可能会导致系统服务的崩溃。 所以消费端限流是非常有必要的。

可以通过MQ中的 listener-container 配置属性 perfetch = 1,表示消费端每次从mq拉去一条消息来消费，直到手动确认消费完毕后，才会继续拉去下一条消息。

#### （1）代码实现

1. 编写 QosListener 监听类，保证当前的监听类消息处理机制是 ACK 为手动方式

   ```java
   @Component
   public class QosListener implements ChannelAwareMessageListener {
   
       @Override
       public void onMessage(Message message, Channel channel) throws Exception {
   
           Thread.sleep(1000);
           //1.获取消息
           System.out.println(new String(message.getBody()));
           //2. 处理业务逻辑
           //3. 签收
           channel.basicAck(message.getMessageProperties().getDeliveryTag(),true);
       }
   }
   ```

2. 在配置文件的 listener-container 配置属性中添加配置

   ```xml
   <rabbit:listener-container connection-factory="connectionFactory" acknowledge="manual" prefetch="1" >
       
   <rabbit:listener ref="qosListener" queue-names="test_queue_confirm"/>
   ```

   > 配置说明：
   >
   > perfetch = 1,表示消费端每次从mq拉去一条消息来消费，直到手动确认消费完毕后，才会继续拉去下一条消息。

3.生产者，连发10条消息.

```text
    @Test
    public void testSend() {
        for (int i = 0; i < 10; i++) {
            //发送消息
            rabbitTemplate.convertAndSend("test_exchange_confirm", "confirm", "message confirm....");
        }
    }
```

#### （2）小结

- 在[rabbit:listener-container](rabbit:listener-container) 中配置 prefetch属性设置消费端一次拉取多少消息
- 消费端的确认模式一定为手动确认。acknowledge="manual"

### 4、TTL

设置队列参数、交换机参数、发消息都可以用页面。

也能用代码。

TTL 全称 Time To Live（存活时间/过期时间）。当消息到达存活时间后，还没有被消费，会被自动清除。

RabbitMQ可以对消息设置过期时间，也可以对整个队列（Queue）设置过期时间。

![1569166173852](https://www.ydlclass.com/doc21xnv/assets/1569166173852.55444692.png)

可以在RabbitMQ管理控制台设置过期时间，此处不做重点讲解。

**一起做一下**：

创建队列 test_queue_ttl 设置ttl为10秒

![image-20200321000505022](https://www.ydlclass.com/doc21xnv/assets/image-20200321000505022.3d21d748.png)

创建交换机test_exchange_ttl 绑定 队列

![image-20200321000710192](https://www.ydlclass.com/doc21xnv/assets/image-20200321000710192.0dad458c.png)

![image-20220714141856963](https://www.ydlclass.com/doc21xnv/assets/image-20220714141856963.34e53d20.png)

向交换机发一个消息

![image-20220714142043712](https://www.ydlclass.com/doc21xnv/assets/image-20220714142043712.98552c4c.png)

查看队列消息，过10秒删除

![image-20220714142021614](https://www.ydlclass.com/doc21xnv/assets/image-20220714142021614.3081f2f6.png)

#### （1）代码实现

##### a、设置队列的过期时间

1. 在消息的生产方中，在 spring-rabbitmq-producer.xml 配置文件中，添加如下配置：

   ```xml
   <!--ttl-->
   <rabbit:queue name="test_queue_ttl" id="test_queue_ttl">
       <!--设置queue的参数-->
       <rabbit:queue-arguments>
           <!--x-message-ttl指队列的过期时间-->
           <entry key="x-message-ttl" value="10000" value-type="java.lang.Integer"/>
       </rabbit:queue-arguments>
   </rabbit:queue>
   
   <rabbit:topic-exchange name="test_exchange_ttl" >
       <rabbit:bindings>
           <rabbit:binding pattern="ydl.#" queue="test_queue_ttl"></rabbit:binding>
       </rabbit:bindings>
   </rabbit:topic-exchange>
   ```

2. 编写发送消息测试方法

   ```java
       @Test
       public void ttlTest(){
           for (int i = 0; i < 10; i++) {
               rabbitTemplate.convertAndSend("test_exchange_ttl","ydl.itlils","hello ttl"+i);
           }
       }
   ```

   测试结果：当消息发送成功后，过10s后在RabbitMQ的管理控制台会看到消息会自动删除。

![image-20220714142521691](https://www.ydlclass.com/doc21xnv/assets/image-20220714142521691.9ddf683b.png)

##### b、设置单个消息的过期时间

编写代码测试，并且设置队列的过期时间为10s， 单个消息的过期时间为5s

```java
    @Test
    public void testTtl2() {

        // 消息后处理对象，设置一些消息的参数信息
        MessagePostProcessor messagePostProcessor = new MessagePostProcessor() {
            @Override
            public Message postProcessMessage(Message message) throws AmqpException {
                //1.设置message的信息
                message.getMessageProperties().setExpiration("5000");//消息的过期时间
                //2.返回该消息
                return message;
            }
        };

        //消息单独过期
        rabbitTemplate.convertAndSend("test_exchange_ttl",
                "ttl.hehe", "message ttl....",messagePostProcessor);

        for (int i = 0; i < 10; i++) {
            if(i == 5){
                //消息单独过期
                rabbitTemplate.convertAndSend("test_exchange_ttl", "ttl.hehe", "message ttl....",messagePostProcessor);
            }else{
                //不过期的消息
                rabbitTemplate.convertAndSend("test_exchange_ttl", "ttl.hehe", "message ttl....");

            }
        }
    }
```

> 如果设置了消息的过期时间，也设置了队列的过期时间，它以时间短的为准。
>
> - 队列过期后，会将队列所有消息全部移除。
> - 消息过期后，只有消息在队列顶端，才会判断其是否过期(移除掉)

![image-20220714143016960](https://www.ydlclass.com/doc21xnv/assets/image-20220714143016960.7b954414.png)

20s

![image-20220714143257013](https://www.ydlclass.com/doc21xnv/assets/image-20220714143257013.53b83dc2.png)

5 10 5 10

![image-20220714143611629](https://www.ydlclass.com/doc21xnv/assets/image-20220714143611629.d8ef4d4d.png)

#### （2）小结

- 设置队列过期时间使用参数：x-message-ttl，单位：ms(毫秒)，会对整个队列消息统一过期。
- 设置消息过期时间使用参数：expiration。单位：ms(毫秒)，当该消息在队列头部时（消费时），会单独判断这一消息是否过期。
- 如果两者都进行了设置，以时间短的为准。

### 5、死信队列

死信队列，英文缩写：DLX 。Dead Letter Exchange（死信交换机），当消息成为Dead message后，可以被重新发送到另一个交换机，这个交换机就是DLX。

![1569167524589](https://www.ydlclass.com/doc21xnv/assets/1569167524589.ebc81648.png)

**消息成为死信的三种情况：**

1. 队列消息长度到达限制；
2. 消费者拒接消费消息，basicNack/basicReject,并且不把消息重新放入原目标队列,requeue=false；
3. 原队列存在消息过期设置，消息到达超时时间未被消费；

**队列绑定死信交换机：**

给队列设置参数： x-dead-letter-exchange 和 x-dead-letter-routing-key

![1569167616750](https://www.ydlclass.com/doc21xnv/assets/1569167616750.b353ac0d.png)

#### （1）代码实现

1. 在消息的生产方中，在 spring-rabbitmq-producer.xml 配置文件中，添加如下配置：

   - 声明正常的队列(test_queue_dlx)和交换机(test_exchange_dlx)

     ```xml
     <rabbit:queue name="test_queue_dlx" id="test_queue_dlx">
     </rabbit:queue>
     <rabbit:topic-exchange name="test_exchange_dlx">
         <rabbit:bindings>
             <rabbit:binding pattern="test.dlx.#" queue="test_queue_dlx"></rabbit:binding>
         </rabbit:bindings>
     </rabbit:topic-exchange>
     ```

   - 声明死信队列(queue_dlx)和死信交换机(exchange_dlx)

     ```xml
     <rabbit:queue name="queue_dlx" id="queue_dlx"></rabbit:queue>
     <rabbit:topic-exchange name="exchange_dlx">
         <rabbit:bindings>
             <rabbit:binding pattern="dlx.#" queue="queue_dlx"></rabbit:binding>
         </rabbit:bindings>
     </rabbit:topic-exchange>
     ```

   - 正常队列绑定死信交换机，并设置相关参数信息

     ```xml
     <rabbit:queue name="test_queue_dlx" id="test_queue_dlx">
         <!--3. 正常队列绑定死信交换机-->
         <rabbit:queue-arguments>
             <!--3.1 x-dead-letter-exchange：死信交换机名称-->
             <entry key="x-dead-letter-exchange" value="exchange_dlx" />
     
             <!--3.2 x-dead-letter-routing-key：发送给死信交换机的routingkey-->
             <entry key="x-dead-letter-routing-key" value="dlx.hehe" />
     
             <!--4.1 设置队列的过期时间 ttl-->
             <entry key="x-message-ttl" value="10000" value-type="java.lang.Integer" />
             <!--4.2 设置队列的长度限制 max-length -->
             <entry key="x-max-length" value="10" value-type="java.lang.Integer" />
         </rabbit:queue-arguments>
     </rabbit:queue>
     <rabbit:topic-exchange name="test_exchange_dlx">
         <rabbit:bindings>
             <rabbit:binding pattern="test.dlx.#" queue="test_queue_dlx"></rabbit:binding>
         </rabbit:bindings>
     </rabbit:topic-exchange>
     ```

2. 编写测试方法

   ```java
       /**
        * 发送测试死信消息：
        *  1. 过期时间
        *  2. 长度限制
        *  3. 消息拒收
        */
       @Test
       public void testDlx(){
           //1. 测试过期时间，死信消息
           rabbitTemplate.convertAndSend("test_exchange_dlx",
                                         "test.dlx.haha","我是一条消息，我会死吗？");
   
           //2. 测试长度限制后，消息死信
          for (int i = 0; i < 20; i++) {
               rabbitTemplate.convertAndSend("test_exchange_dlx",
                                             "test.dlx.haha","我是一条消息，我会死吗？");
           }
   
           //3. 测试消息拒收
           rabbitTemplate.convertAndSend("test_exchange_dlx",
                                         "test.dlx.haha","我是一条消息，我会死吗？");
   
       }
   ```

3消息拒绝接受消费者增加监听

```java
package com.ydlclass.listener;

/**
 * creste by ydlclass.itcast
 */

import com.rabbitmq.client.Channel;
import org.springframework.amqp.core.Message;
import org.springframework.amqp.rabbit.listener.api.ChannelAwareMessageListener;
import org.springframework.stereotype.Component;

/**
 * Consumer ACK机制：
 *  1. 设置手动签收。acknowledge="manual"
 *  2. 让监听器类实现ChannelAwareMessageListener接口
 *  3. 如果消息成功处理，则调用channel的 basicAck()签收
 *  4. 如果消息处理失败，则调用channel的basicNack()拒绝签收，broker重新发送给consumer
 */
@Component
public class DlxListener implements ChannelAwareMessageListener {

    @Override
    public void onMessage(Message message, Channel channel) throws Exception {
        long deliveryTag = message.getMessageProperties().getDeliveryTag();

        try {
            //1.接收转换消息
            System.out.println(new String(message.getBody()));

            //2. 处理业务逻辑
            System.out.println("处理业务逻辑...");
            int i = 3/0;//出现错误
            //3. 手动签收
            channel.basicAck(deliveryTag,true);
        } catch (Exception e) {
            //e.printStackTrace();
            System.out.println("拒绝接受");
            //4.拒绝签收
            /*
            第三个参数：requeue：重回队列。如果设置为true，则消息重新回到queue，broker会重新发送该消息给消费端
             */
            channel.basicNack(deliveryTag,true,false);
            // 了解
            //channel.basicReject(deliveryTag,true);
        }
    }
}
```

配置文件

```xml
 <rabbit:listener ref="dlxListener" queue-names="test_queue_dlx"/>
```

#### （2）小结

1. 死信交换机和死信队列和普通的没有区别
2. 当消息成为死信后，如果该队列绑定了死信交换机，则消息会被死信交换机重新路由到死信队列
3. 消息成为死信的三种情况：
   - 队列消息长度到达限制；
   - 消费者拒接消费消息，并且不重回队列；
   - 原队列存在消息过期设置，消息到达超时时间未被消费；

### 6、延迟队列-重点

延迟队列，即消息进入队列后不会立即被消费，只有到达指定时间后，才会被消费。

提出需求：

1. 下单后，30分钟未支付，取消订单，回滚库存。
2. 新用户注册成功7天后，发送短信问候。

实现方式：

1. 定时器（不优雅！）
2. 延迟队列

![1569168202661](https://www.ydlclass.com/doc21xnv/assets/1569168202661.c342efd8.png)

注意：在RabbitMQ中并未提供延迟队列功能。

但是可以使用：**TTL+死信队列** 组合实现延迟队列的效果。

![1569168255196](https://www.ydlclass.com/doc21xnv/assets/1569168255196.be484518.png)

#### （1）代码实现

1. 在消息的生产方中，在 spring-rabbitmq-producer.xml 配置文件中，添加如下配置：

   ```xml
   <!-- 1. 定义正常交换机（order_exchange）和队列(order_queue)-->
   <rabbit:queue id="order_queue" name="order_queue">
       <!-- 3. 绑定，设置正常队列过期时间为30分钟-->
       <rabbit:queue-arguments>
           <entry key="x-dead-letter-exchange" value="order_exchange_dlx" />
           <entry key="x-dead-letter-routing-key" value="dlx.order.cancel" />
           <entry key="x-message-ttl" value="10000" value-type="java.lang.Integer" />
       </rabbit:queue-arguments>
   </rabbit:queue>
   <rabbit:topic-exchange name="order_exchange">
       <rabbit:bindings>
           <rabbit:binding pattern="order.#" queue="order_queue"></rabbit:binding>
       </rabbit:bindings>
   </rabbit:topic-exchange>
   
   <!--  2. 定义死信交换机（order_exchange_dlx）和队列(order_queue_dlx)-->
   <rabbit:queue id="order_queue_dlx" name="order_queue_dlx"></rabbit:queue>
   <rabbit:topic-exchange name="order_exchange_dlx">
       <rabbit:bindings>
           <rabbit:binding pattern="dlx.order.#" queue="order_queue_dlx"></rabbit:binding>
       </rabbit:bindings>
   </rabbit:topic-exchange>
   ```

2. 编写测试方法

   ```java
   @Test
   public  void testDelay() throws InterruptedException {
       //1.发送订单消息。 将来是在订单系统中，下单成功后，发送消息
       rabbitTemplate.convertAndSend("order_exchange",
                                     "order.msg","订单信息：id=1,time=2022年");
   
       //2.打印倒计时10秒
       for (int i = 10; i > 0 ; i--) {
           System.out.println(i+"...");
           Thread.sleep(1000);
       }
   }
   ```

3消费者

```java
package com.ydlclass.listener;

/**
 * creste by ydlclass.itcast
 */

import com.rabbitmq.client.Channel;
import org.springframework.amqp.core.Message;
import org.springframework.amqp.rabbit.listener.api.ChannelAwareMessageListener;
import org.springframework.stereotype.Component;

/**
 * Consumer ACK机制：
 *  1. 设置手动签收。acknowledge="manual"
 *  2. 让监听器类实现ChannelAwareMessageListener接口
 *  3. 如果消息成功处理，则调用channel的 basicAck()签收
 *  4. 如果消息处理失败，则调用channel的basicNack()拒绝签收，broker重新发送给consumer
 */
@Component
public class OrderListener implements ChannelAwareMessageListener {

    @Override
    public void onMessage(Message message, Channel channel) throws Exception {
        long deliveryTag = message.getMessageProperties().getDeliveryTag();

        try {
            //1.接收转换消息
            System.out.println(new String(message.getBody()));

            //2. 处理业务逻辑
            System.out.println("处理业务逻辑...");
            System.out.println("查询数据库。。。");
            System.out.println("查看支付状态。。。");

            //3. 手动签收
            channel.basicAck(deliveryTag,true);
        } catch (Exception e) {
            //e.printStackTrace();
            System.out.println("拒绝接受");
            //4.拒绝签收
            /*
            第三个参数：requeue：重回队列。如果设置为true，则消息重新回到queue，broker会重新发送该消息给消费端
             */
            channel.basicNack(deliveryTag,true,false);
            // 了解
            //channel.basicReject(deliveryTag,true);
        }
    }
}
```

配置文件

```text
<rabbit:listener ref="orderListener" queue-names="order_queue_dlx"/>
```

#### （2） 小结

1. 延迟队列 指消息进入队列后，可以被延迟一定时间，再进行消费。
2. RabbitMQ没有提供延迟队列功能，但是可以使用 ： **TTL + DLX** 来实现延迟队列效果。

### 7、日志与监控-了解

#### （1）RabbitMQ日志

RabbitMQ默认日志存放路径：

 Linux 下/var /log/rabbitmq/rabbit@xxx.log

 windows下C:\Users\Administrator\AppData\Roaming\RabbitMQ\log

RabbitMQ 日志所在的目录：

![1569168718549](https://www.ydlclass.com/doc21xnv/assets/1569168718549.49512116.png)

RabbitMQ日志详细信息：

日志包含了RabbitMQ的版本号、Erlang的版本号、RabbitMQ服务节点名称、cookie的hash值、RabbitMQ配置文件地址、内存限制、磁盘限制、默认账户guest的创建以及权限配置等等。

![image-20220714151931414](https://www.ydlclass.com/doc21xnv/assets/image-20220714151931414.b29a5e65.png)

#### （2）web管控台监控

**注意windows下**：进入sbin目录，命令后加“.bat”。如rabbitmqctl.bat

直接访问当前的IP:15672，输入用户名和密码（默认是 guest），就可以查看RabbitMQ的管理控制台。当然也可通过命令的形式来查看。如下：

- 查看队列：rabbitmqctl list_queues

  对应管理控制台的页面如下：

  ![image-20220714152127778](https://www.ydlclass.com/doc21xnv/assets/image-20220714152127778.fc84dbcc.png)

- 查看用户： rabbitmqctl list_users

- 查看连接：rabbitmqctl list_connections

> 其它相关命令（了解）：
>
> 查看exchanges：rabbitmqctl list_exchanges
>
> 查看消费者信息：rabbitmqctl list_consumers
>
> 查看环境变量：rabbitmqctl environment
>
> 查看未被确认的队列：rabbitmqctl list_queues name messages_unacknowledged
>
> 查看单个队列的内存使用：rabbitmqctl list_queues name memory
>
> 查看准备就绪的队列：rabbitmqctl list_queues name messages_ready

### 8、消息追踪-了解

在使用任何消息中间件的过程中，难免会出现某条消息异常丢失的情况。对于RabbitMQ而言，可能是因为生产者或消费者与RabbitMQ断开了连接，而它们与RabbitMQ又采用了不同的确认机制；也有可能是因为交换器与队列之间不同的转发策略；甚至是交换器并没有与任何队列进行绑定，生产者又不感知或者没有采取相应的措施；另外RabbitMQ本身的集群策略也可能导致消息的丢失。这个时候就需要有一个较好的机制跟踪记录消息的投递过程，以此协助开发和运维人员进行问题的定位。

在RabbitMQ中可以使用Firehose和rabbitmq_tracing插件功能来实现消息追踪。

#### （1）消息追踪-Firehose

firehose的机制是将生产者投递给rabbitmq的消息，rabbitmq投递给消费者的消息按照指定的格式发送到默认的exchange上。这个默认的exchange的名称为 **amq.rabbitmq.trace**，它是一个**topic**类型的**exchange**。发送到这个exchange上的消息的routing key为 publish.exchangename 和 deliver.queuename。其中exchangename和queuename为实际exchange和queue的名称，分别对应生产者投递到exchange的消息，和消费者从queue上获取的消息。

![1569201204959](https://www.ydlclass.com/doc21xnv/assets/1569201204959.dde46343.png)

**注意：打开 trace 会影响消息写入功能，适当打开后请关闭。**

rabbitmqctl trace_on：开启Firehose命令

**消息追踪验证：**

1. 创建一个队列 **test_trace**，并将当前的队列绑定到 **amq.rabbitmq.trace** 交换机上，设置RoutingKey为：**#**

   ![1569201672175](https://www.ydlclass.com/doc21xnv/assets/1569201672175.736f2200.png)

2. 未开启消息追踪之前，我们发送一个消息

   ![1569201776723](https://www.ydlclass.com/doc21xnv/assets/1569201776723.7acd3ef5.png)

   当前消息发送成功后，在控制台我们可以看到当前消息的具体信息

3. 设置**开启消息追踪**，在发送一条消息

   ![1569201317013](https://www.ydlclass.com/doc21xnv/assets/1569201317013.397a5f8c.png)

   完整的消息内容：

   ![1569202059745](https://www.ydlclass.com/doc21xnv/assets/1569202059745.3d93708c.png)

我们发现当前消息也正常存在，并且开启消息追踪后，会多出一条消息是 **amq.rabbitmq.trace** 交换机发给当前队列的消息，消息中的内容是比较完整的。

> 建议：在开发阶段我们可以开启消息追踪，在实际生产环境建议将其关闭
>
> rabbitmqctl trace_off：关闭Firehose命令

#### （2） 消息追踪-rabbitmq_tracing

rabbitmq_tracing和Firehose在实现上如出一辙，只不过rabbitmq_tracing的方式比Firehose多了一层GUI的包装，更容易使用和管理。

启用插件：rabbitmq-plugins enable rabbitmq_tracing

![1569202861087](https://www.ydlclass.com/doc21xnv/assets/1569202861087.f0450a8a.png)

发送消息成功后，我们点击日志文件，要求输入RabbitMQ的登录用户名和密码。

![1569203000215](https://www.ydlclass.com/doc21xnv/assets/1569203000215.8bca3152.png)

> 建议：在开发阶段我们可以开启消息追踪插件，在实际生产环境不建议建议开启，除非是非常特殊的业务场景，大家根据实际情况选择开启即可。

## 第二章 RabbitMQ应用问题-面试

### 1、消息可靠性保障

提出需求：如何能够保证消息的 100% 发送成功？

首先大家要明确任何一个系统都不能保证消息的 100% 投递成功，我们是可以保证消息以最高最可靠的发送给目标方。

在RabbitMQ中采用 **消息补充机制** 来保证消息的可靠性

![1569203830553](https://www.ydlclass.com/doc21xnv/assets/1569203830553.89c61c65.png)

消息勾兑

步骤分析：

参与部分：消息生产者、消息消费者、数据库、三个队列（Q1、Q2、Q3）、交换机、回调检查服务、定时检查服务

1. 消息的生产者将业务数据存到数据库中
2. 发送消息给 队列Q1
3. 消息的生产者等待一定的时间后，在发送一个延迟消息给队列 Q3
4. 消息的消费方监听 Q1 队列消息，成功接收后
5. 消息的消费方会 发送 一条确认消息给 队列Q2
6. 回调检查服务监听 队列Q2 发送的确认消息
7. 回调检查服务接收到确认消息后，将消息写入到 消息的数据库表中
8. 回调检查服务同时也会监听 队列Q3延迟消息， 如果接收到消息会和数据库比对消息的唯一标识
9. 如果发现没有接收到确认消息，那么回调检查服务就会远程调用 消息生产者，重新发送消息
10. 重新执行 2-7 步骤，保证消息的可靠性传输
11. 如果发送消息和延迟消息都出现异常，定时检查服务会监控 消息库中的消息数据，如果发现不一致的消息然后远程调用消息的生产者重新发送消息。

### 2、 消息幂等性处理

幂等性指一次和多次请求某一个资源，对于资源本身应该具有同样的结果。也就是说，其任意多次执行对资源本身所产生的影响均与一次执行的影响相同。

在MQ中指，消费多条相同的消息，得到与消费该消息一次相同的结果。

在本教程中使用 **乐观锁机制** 保证消息的幂等操作

![1569210134160](https://www.ydlclass.com/doc21xnv/assets/1569210134160.47cd8c5d.png)

### 3、RabbitMQ集群搭建-运维

摘要：实际生产应用中都会采用消息队列的集群方案，如果选择RabbitMQ那么有必要了解下它的集群方案原理

一般来说，如果只是为了学习RabbitMQ或者验证业务工程的正确性那么在本地环境或者测试环境上使用其单实例部署就可以了，但是出于MQ中间件本身的可靠性、并发性、吞吐量和消息堆积能力等问题的考虑，在生产环境上一般都会考虑使用RabbitMQ的集群方案。

#### （1） 集群方案的原理

RabbitMQ这款消息队列中间件产品本身是基于Erlang编写，Erlang语言天生具备分布式特性（通过同步Erlang集群各节点的magic cookie来实现）。因此，RabbitMQ天然支持Clustering。这使得RabbitMQ本身不需要像ActiveMQ、Kafka那样通过ZooKeeper分别来实现HA方案和保存集群的元数据。集群是保证可靠性的一种方式，同时可以通过水平扩展以达到增加消息吞吐量能力的目的。

![1565245219265](https://www.ydlclass.com/doc21xnv/assets/1566073768274.20ab1d25.png)

#### （2）单机多实例部署

由于某些因素的限制，有时候你不得不在一台机器上去搭建一个rabbitmq集群，这个有点类似zookeeper的单机版。真实生成环境还是要配成多机集群的。有关怎么配置多机集群的可以参考其他的资料，这里主要论述如何在单机中配置多个rabbitmq实例。

主要参考官方文档：https://www.rabbitmq.com/clustering.html

首先确保RabbitMQ运行没有问题

```bash
[root@super ~]# rabbitmqctl status
Status of node rabbit@super ...
[{pid,10232},
 {running_applications,
     [{rabbitmq_management,"RabbitMQ Management Console","3.6.5"},
      {rabbitmq_web_dispatch,"RabbitMQ Web Dispatcher","3.6.5"},
      {webmachine,"webmachine","1.10.3"},
      {mochiweb,"MochiMedia Web Server","2.13.1"},
      {rabbitmq_management_agent,"RabbitMQ Management Agent","3.6.5"},
      {rabbit,"RabbitMQ","3.6.5"},
      {os_mon,"CPO  CXC 138 46","2.4"},
      {syntax_tools,"Syntax tools","1.7"},
      {inets,"INETS  CXC 138 49","6.2"},
      {amqp_client,"RabbitMQ AMQP Client","3.6.5"},
      {rabbit_common,[],"3.6.5"},
      {ssl,"Erlang/OTP SSL application","7.3"},
      {public_key,"Public key infrastructure","1.1.1"},
      {asn1,"The Erlang ASN1 compiler version 4.0.2","4.0.2"},
      {ranch,"Socket acceptor pool for TCP protocols.","1.2.1"},
      {mnesia,"MNESIA  CXC 138 12","4.13.3"},
      {compiler,"ERTS  CXC 138 10","6.0.3"},
      {crypto,"CRYPTO","3.6.3"},
      {xmerl,"XML parser","1.3.10"},
      {sasl,"SASL  CXC 138 11","2.7"},
      {stdlib,"ERTS  CXC 138 10","2.8"},
      {kernel,"ERTS  CXC 138 10","4.2"}]},
 {os,{unix,linux}},
 {erlang_version,
     "Erlang/OTP 18 [erts-7.3] [source] [64-bit] [async-threads:64] [hipe] [kernel-poll:true]\n"},
 {memory,
     [{total,56066752},
      {connection_readers,0},
      {connection_writers,0},
      {connection_channels,0},
      {connection_other,2680},
      {queue_procs,268248},
      {queue_slave_procs,0},
      {plugins,1131936},
      {other_proc,18144280},
      {mnesia,125304},
      {mgmt_db,921312},
      {msg_index,69440},
      {other_ets,1413664},
      {binary,755736},
      {code,27824046},
      {atom,1000601},
      {other_system,4409505}]},
 {alarms,[]},
 {listeners,[{clustering,25672,"::"},{amqp,5672,"::"}]},
 {vm_memory_high_watermark,0.4},
 {vm_memory_limit,411294105},
 {disk_free_limit,50000000},
 {disk_free,13270233088},
 {file_descriptors,
     [{total_limit,924},{total_used,6},{sockets_limit,829},{sockets_used,0}]},
 {processes,[{limit,1048576},{used,262}]},
 {run_queue,0},
 {uptime,43651},
 {kernel,{net_ticktime,60}}]
```

停止rabbitmq服务

```bash
[root@super sbin]# service rabbitmq-server stop
Stopping rabbitmq-server: rabbitmq-server.
```

启动第一个节点：

```bash
[root@super sbin]# RABBITMQ_NODE_PORT=5673 RABBITMQ_NODENAME=rabbit1 rabbitmq-server start

              RabbitMQ 3.6.5. Copyright (C) 2007-2016 Pivotal Software, Inc.
  ##  ##      Licensed under the MPL.  See http://www.rabbitmq.com/
  ##  ##
  ##########  Logs: /var/log/rabbitmq/rabbit1.log
  ######  ##        /var/log/rabbitmq/rabbit1-sasl.log
  ##########
              Starting broker...
 completed with 6 plugins.
```

启动第二个节点：

> web管理插件端口占用,所以还要指定其web插件占用的端口号。

```bash
[root@super ~]# RABBITMQ_NODE_PORT=5674 RABBITMQ_SERVER_START_ARGS="-rabbitmq_management listener [{port,15674}]" RABBITMQ_NODENAME=rabbit2 rabbitmq-server start

              RabbitMQ 3.6.5. Copyright (C) 2007-2016 Pivotal Software, Inc.
  ##  ##      Licensed under the MPL.  See http://www.rabbitmq.com/
  ##  ##
  ##########  Logs: /var/log/rabbitmq/rabbit2.log
  ######  ##        /var/log/rabbitmq/rabbit2-sasl.log
  ##########
              Starting broker...
 completed with 6 plugins.
```

结束命令：

```bash
rabbitmqctl -n rabbit1 stop
rabbitmqctl -n rabbit2 stop
```

rabbit1操作作为主节点：

```bash
[root@super ~]# rabbitmqctl -n rabbit1 stop_app  
Stopping node rabbit1@super ...
[root@super ~]# rabbitmqctl -n rabbit1 reset	 
Resetting node rabbit1@super ...
[root@super ~]# rabbitmqctl -n rabbit1 start_app
Starting node rabbit1@super ...
[root@super ~]# 
```

rabbit2操作为从节点：

```bash
[root@super ~]# rabbitmqctl -n rabbit2 stop_app
Stopping node rabbit2@super ...
[root@super ~]# rabbitmqctl -n rabbit2 reset
Resetting node rabbit2@super ...
[root@super ~]# rabbitmqctl -n rabbit2 join_cluster rabbit1@'super' ###''内是主机名换成自己的
Clustering node rabbit2@super with rabbit1@super ...
[root@super ~]# rabbitmqctl -n rabbit2 start_app
Starting node rabbit2@super ...
```

查看集群状态：

```text
[root@super ~]# rabbitmqctl cluster_status -n rabbit1
Cluster status of node rabbit1@super ...
[{nodes,[{disc,[rabbit1@super,rabbit2@super]}]},
 {running_nodes,[rabbit2@super,rabbit1@super]},
 {cluster_name,<<"rabbit1@super">>},
 {partitions,[]},
 {alarms,[{rabbit2@super,[]},{rabbit1@super,[]}]}]
```

web监控：

![1566065096459](https://www.ydlclass.com/doc21xnv/assets/1566065096459.f6a3b079.png)

#### （3）集群管理

**rabbitmqctl join_cluster {cluster_node} [–ram]** 将节点加入指定集群中。在这个命令执行前需要停止RabbitMQ应用并重置节点。

**rabbitmqctl cluster_status** 显示集群的状态。

**rabbitmqctl change_cluster_node_type {disc|ram}** 修改集群节点的类型。在这个命令执行前需要停止RabbitMQ应用。

**rabbitmqctl forget_cluster_node [–offline]** 将节点从集群中删除，允许离线执行。

**rabbitmqctl update_cluster_nodes {clusternode}**

在集群中的节点应用启动前咨询clusternode节点的最新信息，并更新相应的集群信息。这个和join_cluster不同，它不加入集群。考虑这样一种情况，节点A和节点B都在集群中，当节点A离线了，节点C又和节点B组成了一个集群，然后节点B又离开了集群，当A醒来的时候，它会尝试联系节点B，但是这样会失败，因为节点B已经不在集群中了。

**rabbitmqctl cancel_sync_queue [-p vhost] {queue}** 取消队列queue同步镜像的操作。

**rabbitmqctl set_cluster_name {name}** 设置集群名称。集群名称在客户端连接时会通报给客户端。Federation和Shovel插件也会有用到集群名称的地方。集群名称默认是集群中第一个节点的名称，通过这个命令可以重新设置。

#### （4）RabbitMQ镜像集群配置

> 上面已经完成RabbitMQ默认集群模式，但并不保证队列的高可用性，尽管交换机、绑定这些可以复制到集群里的任何一个节点，但是队列内容不会复制。虽然该模式解决一项目组节点压力，但队列节点宕机直接导致该队列无法应用，只能等待重启，所以要想在队列节点宕机或故障也能正常应用，就要复制队列内容到集群里的每个节点，必须要创建镜像队列。
>
> 镜像队列是基于普通的集群模式的，然后再添加一些策略，所以你还是得先配置普通集群，然后才能设置镜像队列，我们就以上面的集群接着做。

**设置的镜像队列可以通过开启的网页的管理端Admin->Policies，也可以通过命令。**

> rabbitmqctl set_policy my_ha "^" '{"ha-mode":"all"}'

![1566072300852](https://www.ydlclass.com/doc21xnv/assets/1566072300852.f79b7823.png)

> - Name:策略名称
> - Pattern：匹配的规则，如果是匹配所有的队列，是^.
> - Definition:使用ha-mode模式中的all，也就是同步所有匹配的队列。问号链接帮助文档。

#### （5）负载均衡-HAProxy

HAProxy提供高可用性、负载均衡以及基于TCP和HTTP应用的代理，支持虚拟主机，它是免费、快速并且可靠的一种解决方案,包括Twitter，Reddit，StackOverflow，GitHub在内的多家知名互联网公司在使用。HAProxy实现了一种事件驱动、单一进程模型，此模型支持非常大的并发连接数。

##### a、 安装HAProxy

```bash
//下载依赖包
yum install gcc vim wget
//上传haproxy源码包
//解压
tar -zxvf haproxy-1.6.5.tar.gz -C /usr/local
//进入目录、进行编译、安装
cd /usr/local/haproxy-1.6.5
make TARGET=linux31 PREFIX=/usr/local/haproxy
make install PREFIX=/usr/local/haproxy
mkdir /etc/haproxy
//赋权
groupadd -r -g 149 haproxy
useradd -g haproxy -r -s /sbin/nologin -u 149 haproxy
//创建haproxy配置文件
mkdir /etc/haproxy
vim /etc/haproxy/haproxy.cfg
```

##### b、配置HAProxy

配置文件路径：/etc/haproxy/haproxy.cfg

```bash
#logging options
global
	log 127.0.0.1 local0 info
	maxconn 5120
	chroot /usr/local/haproxy
	uid 99
	gid 99
	daemon
	quiet
	nbproc 20
	pidfile /var/run/haproxy.pid

defaults
	log global
	
	mode tcp

	option tcplog
	option dontlognull
	retries 3
	option redispatch
	maxconn 2000
	contimeout 5s
   
     clitimeout 60s

     srvtimeout 15s	
#front-end IP for consumers and producters

listen rabbitmq_cluster
	bind 0.0.0.0:5672
	
	mode tcp
	#balance url_param userid
	#balance url_param session_id check_post 64
	#balance hdr(User-Agent)
	#balance hdr(host)
	#balance hdr(Host) use_domain_only
	#balance rdp-cookie
	#balance leastconn
	#balance source //ip
	
	balance roundrobin
	
        server node1 127.0.0.1:5673 check inter 5000 rise 2 fall 2
        server node2 127.0.0.1:5674 check inter 5000 rise 2 fall 2

listen stats
	bind 172.16.98.133:8100
	mode http
	option httplog
	stats enable
	stats uri /rabbitmq-stats
	stats refresh 5s
```

启动HAproxy负载

```bash
/usr/local/haproxy/sbin/haproxy -f /etc/haproxy/haproxy.cfg
//查看haproxy进程状态
ps -ef | grep haproxy

访问如下地址对mq节点进行监控
http://172.16.98.133:8100/rabbitmq-stats
```

代码中访问mq集群地址，则变为访问haproxy地址:5672