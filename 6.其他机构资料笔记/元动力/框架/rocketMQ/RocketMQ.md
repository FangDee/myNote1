## RocketMQ基础篇

### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_1-mq简介)1. MQ简介

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_1-1-项目工程弊端)1.1 项目工程弊端

![image-20210227005612570](https://ydlclass.com/doc21xnv/assets/image-20210227005612570.05496644.png)

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_1-2-mq简介)1.2 MQ简介

1. MQ（Message Queue）消息队列，是一种用来保存消息数据的队列.

   1. 队列：数据结构的一种，特征为 “先进先出”

      ![image-20210227005644041](https://ydlclass.com/doc21xnv/assets/image-20210227005644041.049e2350.png)

   2. 何为消息

      1. 服务器间的业务请求
         1. 原始架构：
            1. 服务器中的A功能需要调用B、C模块才能完成
         2. 微服务架构：
            1. 服务器A向服务器B发送要执行的操作（视为消息）
            2. 服务器A向服务器C发送要执行的操作（视为消息）

2. 小节:MQ概念

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_1-3-mq作用)1.3 MQ作用

优势：

- 应用解耦
- 异步提速
- 削峰填谷

劣势：

- 系统可用性降低
- 系统复杂度提高
- 一致性问题

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_1-4-mq基本工作模式)1.4 MQ基本工作模式

![image-20210227011631473](https://ydlclass.com/doc21xnv/assets/image-20210227011631473.cb178ec3.png)

应用解耦:（异步消息发送）

![image-20210227011703071](https://ydlclass.com/doc21xnv/assets/image-20210227011703071.844da7b4.png)

![image-20210227011721338](https://ydlclass.com/doc21xnv/assets/image-20210227011721338.f6d6ce2a.png)

流量削锋:（异步消息发送）

![image-20210227012322129](https://ydlclass.com/doc21xnv/assets/image-20210227012322129.823cdf25.png)

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_1-5-mq优缺点分析)1.5 MQ优缺点分析

优点（作用）:

1. 应用解耦
2. 快速应用变更维护
3. 流量削锋

缺点:

1. 系统可用性降低
2. 系统复杂度提高
3. 异步消息机制
   1. 消息顺序性
   2. 消息丢失
   3. 消息一致性
   4. 消息重复使用

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_1-6-mq产品介绍)1.6 MQ产品介绍

1. ActiveMQ

   java语言实现，万级数据吞吐量，处理速度ms级，主从架构，**成熟度高**

2. RabbitMQ

   **erlang语言实现**，万级数据吞吐量，**处理速度us级**，主从架构，

3. RocketMQ

   java语言实现，**十万级**数据吞吐量，处理速度ms级，分布式架构，功能强大，扩展性强

4. kafka

   scala语言实现，**十万级**数据吞吐量，处理速度ms级，分布式架构，功能较少，应用于大数据较多

**RocketMQ**

1. RocketMQ是阿里开源的一款非常优秀中间件产品，脱胎于阿里的另一款队列技术MetaQ，后捐赠给**Apache**基金会 作为一款孵化技术，仅仅经历了一年多的时间就成为Apache基金会的顶级项目。并且它现在已经在阿里内部被广泛 的应用，并且经受住了多次双十一的这种极致场景的压力（2017年的双十一，RocketMQ流转的消息量达到了万亿 级，峰值TPS达到5600万）
2. 解决所有缺点

### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_2-环境搭建)2. 环境搭建

![image-20201211151842484](https://ydlclass.com/doc21xnv/assets/image-20201211151842484.8d7fe268.png)

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_2-1-基础概念)2.1 基础概念

1. 生产者
2. 消费者
3. 消息服务器
4. 命名服务器
5. 消息
   1. 主题
   2. 标签
6. 心跳
7. 监听器
8. 拉取消费、推动消费
9. 注册

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_2-2-安装)2.2 安装

1. 命名服务器
2. 消息服务器

![image-20201211152332969](https://ydlclass.com/doc21xnv/assets/image-20201211152332969.5ba50bce.png)

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_2-3-下载)2.3 下载

⚫ https://www.apache.org/

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_2-4-linux安装过程)2.4 linux安装过程

1. 步骤1：安装JDK（1.8）

2. 步骤2：上传压缩包（zip）

   ```xml
   yum -y install lrzsz 
   ```

   ```text
   rz
   ```

3. 步骤3：解压缩

   ```text
   unzip rocketmq-all-4.5.2-bin-release.zip
   ```

4. 步骤4：修改目录名称

   ```text
   mv rocketmq-all-4.5.2-bin-release rocketmq
   ```

**启动服务器**

1. 步骤1：启动命名服务器（bin目录下）

```text
sh mqnamesrv
```

1. 步骤2：启动消息服务器（bin目录下）

   ```text
   sh mqbroker -n localhost:9876
   ```

   修改runbroker.sh文件中有关内存的配置（调整的与当前虚拟机内存匹配即可，推荐256m-128m）

**测试服务器环境**

1. 步骤1：配置命名服务器地址

   ```xml
   export NAMESRV_ADDR=localhost:9876
   ```

2. 步骤2：启动生产者程序客户端（bin目录下）

   ```text
   sh tools.sh org.apache.rocketmq.example.quickstart.Producer
   ```

   启动后产生大量日志信息（注意该信息是测试程序中自带的，不具有通用性，仅供学习查阅参考）

3. 步骤3：启动消费者程序客户端（bin目录下）

   ```text
   sh tools.sh org.apache.rocketmq.example.quickstart.Consumer
   ```

   启动后产生大量日志信息

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_2-5-windows安装)2.5 windows安装

##### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_2-5-1系统环境变量配置)2.5.1**系统环境变量配置**

 1、右键我的电脑-->属性

![image-20210216112544747](https://ydlclass.com/doc21xnv/assets/image-20210216112544747.73835ac5.png)

 2、系统属性--环境变量

![image-20210216112631558](https://ydlclass.com/doc21xnv/assets/image-20210216112631558.7e29712b.png)

 3、系统变量中-->新建

![image-20210216112735780](https://ydlclass.com/doc21xnv/assets/image-20210216112735780.a6597055.png)

变量名：ROCKETMQ_HOME

变量值：MQ解压路径\MQ文件夹名

![image-20210216112826156](https://ydlclass.com/doc21xnv/assets/image-20210216112826156.50ae15d0.png)

##### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_2-5-2启动)2.5.2**启动**

 1、**启动NAMESERVER**

Cmd命令框执行进入至‘MQ文件夹\bin’下 端口9876

```text
start mqnamesrv.cmd
```

![image-20210216110141279](https://ydlclass.com/doc21xnv/assets/image-20210216110141279.99db0625.png)

 2、**启动BROKER**

```text
start mqbroker.cmd -n 127.0.0.1:9876 autoCreateTopicEnable=true
```

![image-20210216111405243](https://ydlclass.com/doc21xnv/assets/image-20210216111405243.60f804d4.png)

**注意**：闪退回命令行

 删除C:\Users\”当前系统用户名”\store下的所有文件。

##### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_2-5-3测试)2.5.3**测试**

 1、**新建环境变量**

 变量名：NAMESRV_ADDR

 变量值：localhost:9876

![image-20210216112924986](https://ydlclass.com/doc21xnv/assets/image-20210216112924986.d254a99f.png)

 2、测试生产者发送消息

 bin目录下

```text
tools.cmd  org.apache.rocketmq.example.quickstart.Producer
```

 3、测试消费者接收消息

 bin目录下

```text
tools.cmd org.apache.rocketmq.example.quickstart.Consumer
```

##### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_2-5-4控制台安装)2.5.4控制台安装

1下载源码

```text
git clone https://github.com/apache/rocketmq-externals.git
```

2进入rocketmq-externals\rocketmq-console 工程，编译源码

```text
mvn clean package -Dmaven.test.skip=true
```

3target 目录生成 jar包

4运行

```text
java -jar rocketmq-console-ng-2.0.0.jar
```

5访问 http://localhost:8080/#/

![image-20210216174310729](https://ydlclass.com/doc21xnv/assets/image-20210216174310729.6bcf3969.png)

### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_3-消息发送-重点)3. 消息发送（重点）

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_3-1-主要内容)3.1 主要内容

1. 基于Java环境构建消息发送与消息接收基础程序
   1. 单生产者单消费者
   2. 单生产者多消费者
   3. 多生产者多消费者
2. 发送不同类型的消息
   1. 同步消息
   2. 异步消息
   3. 单向消息
3. 特殊的消息发送
   1. 延时消息
   2. 批量消息
4. 特殊的消息接收
   1. 消息过滤
5. 消息发送与接收顺序控制
6. 事务消息

![image-20201211153150220](https://ydlclass.com/doc21xnv/assets/image-20201211153150220.212d1185.png)

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_3-2-消息发送与接收开发流程)3.2 消息发送与接收开发流程

1. 谁来发？
2. 发给谁？
3. 怎么发？
4. 发什么？
5. 发的结果是什么？
6. 打扫战场

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_3-3-单生产者单消费者消息发送-onetoone)3.3 单生产者单消费者消息发送（OneToOne）

1新建maven项目rocketmq

2导入RocketMQ客户端坐标

```xml
<dependency>
    <groupId>org.apache.rocketmq</groupId>
    <artifactId>rocketmq-client</artifactId>
    <version>4.8.0</version>
</dependency>
```

3生产者 com.ydl.base.Producer

```java
package com.ydl.base;

import org.apache.rocketmq.client.exception.MQClientException;
import org.apache.rocketmq.client.producer.DefaultMQProducer;
import org.apache.rocketmq.client.producer.SendResult;
import org.apache.rocketmq.common.message.Message;

public class Producer {
    public static void main(String[] args) throws Exception {
        /**
         1. 谁来发？
         2. 发给谁？
         3. 怎么发？
         4. 发什么？
         5. 发的结果是什么？
         6. 打扫战场
         **/

        //1.创建一个发送消息的对象Producer
        DefaultMQProducer producer = new DefaultMQProducer("group1");
        //2.设定发送的命名服务器地址
        producer.setNamesrvAddr("localhost:9876");
        //3.1启动发送的服务
        producer.start();
        //4.创建要发送的消息对象,指定topic，指定内容body
        Message msg = new Message("topic1", "hello rocketmq".getBytes("UTF-8"));
        //3.2发送消息
        SendResult result = producer.send(msg);
        System.out.println("返回结果：" + result);
        //5.关闭连接
        producer.shutdown();
    }
}
```

4消费者

```java
package com.ydl.base;

import org.apache.rocketmq.client.consumer.DefaultMQPushConsumer;
import org.apache.rocketmq.client.consumer.listener.ConsumeConcurrentlyContext;
import org.apache.rocketmq.client.consumer.listener.ConsumeConcurrentlyStatus;
import org.apache.rocketmq.client.consumer.listener.MessageListenerConcurrently;
import org.apache.rocketmq.client.exception.MQClientException;
import org.apache.rocketmq.common.message.MessageExt;

import java.util.List;

public class Consumer {
    public static void main(String[] args) throws Exception {
        /**
         1. 谁来发？
         2. 发给谁？
         3. 怎么发？
         4. 发什么？
         5. 发的结果是什么？
         6. 打扫战场
         **/

        //1.创建一个接收消息的对象Consumer
        DefaultMQPushConsumer consumer = new DefaultMQPushConsumer("group1");
        //2.设定接收的命名服务器地址
        consumer.setNamesrvAddr("localhost:9876");
        //3.设置接收消息对应的topic,对应的sub标签为任意
        consumer.subscribe("topic1","*");
        //3.开启监听，用于接收消息
        consumer.registerMessageListener(new MessageListenerConcurrently() {
            @Override
            public ConsumeConcurrentlyStatus consumeMessage(List<MessageExt> list, ConsumeConcurrentlyContext consumeConcurrentlyContext) {
                //遍历消息
                for (MessageExt msg : list) {
                    System.out.println("收到消息："+msg);
                }
                return ConsumeConcurrentlyStatus.CONSUME_SUCCESS;
            }
        });
        //4.启动接收消息的服务
        consumer.start();
        System.out.println("接受消息服务已经开启！");
        //5 不要关闭消费者！
    }
}
```

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_3-4-单生产者多消费者消息发送-onetomany)3.4 单生产者多消费者消息发送（OneToMany）

**1生产者** com.ydl.one2many.Producer

```java
		//1.创建一个发送消息的对象Producer
        DefaultMQProducer producer = new DefaultMQProducer("group1");
        //2.设定发送的命名服务器地址
        producer.setNamesrvAddr("localhost:9876");
        //3.1启动发送的服务
        producer.start();
        for (int i = 0; i < 10; i++) {
            //4.创建要发送的消息对象,指定topic，指定内容body
            Message msg = new Message("topic1", ("hello rocketmq"+i).getBytes("UTF-8"));
            //3.2发送消息
            SendResult result = producer.send(msg);
            System.out.println("返回结果：" + result);
        }
        //5.关闭连接
        producer.shutdown();
```

**2消费者**（负载均衡模式：默认模式）

开启多实例运行

![image-20210216122333256](https://ydlclass.com/doc21xnv/assets/image-20210216122333256.5ba41b96.png)

```java
        //1.创建一个接收消息的对象Consumer
        DefaultMQPushConsumer consumer = new DefaultMQPushConsumer("group1");
        //2.设定接收的命名服务器地址
        consumer.setNamesrvAddr("localhost:9876");
        //3.设置接收消息对应的topic,对应的sub标签为任意
        consumer.subscribe("topic1","*");
        //设置当前消费者的消费模式（默认模式：负载均衡）
        consumer.setMessageModel(MessageModel.CLUSTERING);
        //3.开启监听，用于接收消息
        consumer.registerMessageListener(new MessageListenerConcurrently() {
            @Override
            public ConsumeConcurrentlyStatus consumeMessage(List<MessageExt> list, ConsumeConcurrentlyContext consumeConcurrentlyContext) {
                //遍历消息
                for (MessageExt msg : list) {
                    System.out.println("收到消息："+msg);
                    System.out.println("消息是："+new String(msg.getBody()));
                }
                return ConsumeConcurrentlyStatus.CONSUME_SUCCESS;
            }
        });
        //4.启动接收消息的服务
        consumer.start();
        System.out.println("接受消息服务已经开启！");

        //5 不要关闭消费者！
```

注意：同一个消费者 多份。争抢topic数据。

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_3-5-单生产者多消费者消息发送-onetomany)3.5 单生产者多消费者消息发送（OneToMany）

消费者（广播模式）

```java
        //1.创建一个接收消息的对象Consumer
        DefaultMQPushConsumer consumer = new DefaultMQPushConsumer("group1");
        //2.设定接收的命名服务器地址
        consumer.setNamesrvAddr("localhost:9876");
        //3.设置接收消息对应的topic,对应的sub标签为任意
        consumer.subscribe("topic1","*");
        //设置当前消费者的消费模式（默认模式：负载均衡）
        //consumer.setMessageModel(MessageModel.CLUSTERING);
        //设置当前消费者的消费模式（广播模式）
        consumer.setMessageModel(MessageModel.BROADCASTING);
        //3.开启监听，用于接收消息
        consumer.registerMessageListener(new MessageListenerConcurrently() {
            @Override
            public ConsumeConcurrentlyStatus consumeMessage(List<MessageExt> list, ConsumeConcurrentlyContext consumeConcurrentlyContext) {
                //遍历消息
                for (MessageExt msg : list) {
                    System.out.println("收到消息："+msg);
                    System.out.println("消息是："+new String(msg.getBody()));
                }
                return ConsumeConcurrentlyStatus.CONSUME_SUCCESS;
            }
        });
        //4.启动接收消息的服务
        consumer.start();
        System.out.println("接受消息服务已经开启！");

        //5 不要关闭消费者！
```

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_3-6-多生产者多消费者消息发送-manytomany)3.6 多生产者多消费者消息发送（ManyToMany）

1. 多生产者产生的消息可以被同一个消费者消费，也可以被多个消费者消费

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_3-7-小节)3.7 小节

1. 消息发送
   1. One-To-One（基础发送与基础接收）
   2. One-To-Many（负载均衡模式与广播模式）
   3. Many-To-Many

### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_4-消息类别)4 消息类别

1. 同步消息
2. 异步消息
3. 单向消息

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_4-1-同步消息)4.1 同步消息

特征：即时性较强，重要的消息，且必须有回执的消息，例如短信，通知（转账成功）

![image-20201211154023471](https://ydlclass.com/doc21xnv/assets/image-20201211154023471.76f3f927.png)

代码实现

com.ydl.messageType拷贝producer

```java
SendResult result = producer.send(msg);
```

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_4-2-异步消息)4.2 异步消息

特征：即时性较弱，但需要有回执的消息，例如订单中的某些信息

![image-20201211154108518](https://ydlclass.com/doc21xnv/assets/image-20201211154108518.8c569153.png)

代码实现

```java
        //1.创建一个发送消息的对象Producer
        DefaultMQProducer producer = new DefaultMQProducer("group1");
        //2.设定发送的命名服务器地址
        producer.setNamesrvAddr("localhost:9876");
        //3.1启动发送的服务
        producer.start();
        for (int i = 0; i < 10; i++) {
            //4.创建要发送的消息对象,指定topic，指定内容body
            Message msg = new Message("topic1", ("hello rocketmq"+i).getBytes("UTF-8"));
            //3.2 同步消息
            //SendResult result = producer.send(msg);
            //System.out.println("返回结果：" + result);

            //异步消息
            producer.send(msg, new SendCallback() {
                //表示成功返回结果
                @Override
                public void onSuccess(SendResult sendResult) {
                    System.out.println(sendResult);
                }
                //表示发送消息失败
                @Override
                public void onException(Throwable throwable) {
                    System.out.println(throwable);
                }
            });
            
            System.out.println("消息"+i+"发完了，做业务逻辑去了！");
        }
        //休眠10秒
        TimeUnit.SECONDS.sleep(10);
        //5.关闭连接
        producer.shutdown();
```



#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_4-3-单向消息)4.3 单向消息

特征：不需要有回执的消息，例如日志类消息

![image-20201211154145199](https://ydlclass.com/doc21xnv/assets/image-20201211154145199.61ef2761.png)

代码实现

```java
producer.sendOneway(msg);
```

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_4-4-延时消息)4.4 延时消息

消息发送时并不直接发送到消息服务器，而是根据设定的等待时间到达，起到延时到达的缓冲作用

```java
Message msg = new Message("topic3",("延时消息：hello rocketmq "+i).getBytes("UTF-8"));
//设置延时等级3,这个消息将在10s之后发送(现在只支持固定的几个时间,详看delayTimeLevel)
msg.setDelayTimeLevel(3);
SendResult result = producer.send(msg);
System.out.println("返回结果："+result);
```

目前支持的消息时间

```java
private String messageDelayLevel = "1s 5s 10s 30s 1m 2m 3m 4m 5m 6m 7m 8m 9m 10m 20m 30m 1h 2h";
```

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_4-5-批量消息)4.5 批量消息

批量发送消息能显著提高传递小消息的性能.

发送批量消息:

```java
        List<Message> msgList = new ArrayList<Message>();
        Message msg1 = new Message("topic1", ("hello rocketmq1").getBytes("UTF-8"));
        Message msg2 = new Message("topic1", ("hello rocketmq2").getBytes("UTF-8"));
        Message msg3 = new Message("topic1", ("hello rocketmq3").getBytes("UTF-8"));

        msgList.add(msg1);
        msgList.add(msg2);
        msgList.add(msg3);


        SendResult result = producer.send(msgList);
```

**注意限制**：

1这些批量消息应该有相同的topic

2相同的waitStoreMsgOK

3不能是延时消息

4消息内容总长度不超过4M

 消息内容总长度包含如下：

-  topic（字符串字节数）
-  body （字节数组长度）
-  消息追加的属性（key与value对应字符串字节数）
-  日志（固定20字节）

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_4-6-消息过滤)4.6 消息过滤

##### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_4-6-1分类过滤)4.6.1分类过滤

按照tag过滤信息。

**生产者**

```java
Message msg = new Message("topic6","tag2",("消息过滤按照tag：hello rocketmq 2").getBytes("UTF-8"));
```

**消费者**

```java
//接收消息的时候，除了制定topic，还可以指定接收的tag,*代表任意tag
consumer.subscribe("topic6","tag1 || tag2");
```

##### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_4-6-2语法过滤-属性过滤-语法过滤-sql过滤)4.6.2语法过滤（属性过滤/语法过滤/SQL过滤）

基本语法

- 数值比较，比如：**>，>=，<，<=，BETWEEN，=；**
- 字符比较，比如：**=，<>，IN；**
- **IS NULL** 或者 **IS NOT NULL；**
- 逻辑符号 **AND，OR，NOT；**

常量支持类型为：

- 数值，比如：**123，3.1415；**
- 字符，比如：**'abc'，必须用单引号包裹起来；**
- **NULL**，特殊的常量
- 布尔值，**TRUE** 或 **FALSE**

**生产者**

```java
//为消息添加属性
msg.putUserProperty("vip","1");
msg.putUserProperty("age","20");
```

**消费者**

```java
//使用消息选择器来过滤对应的属性，语法格式为类SQL语法
consumer.subscribe("topic7", MessageSelector.bySql("age >= 18"));
consumer.subscribe("topic6", MessageSelector.bySql("name = 'litiedan'"));
```

**注意**：SQL过滤需要依赖服务器的功能支持，在broker.conf配置文件中添加对应的功能项，并开启对应功能

```properties
enablePropertyFilter=true
```

重启broker

```text
start mqbroker.cmd -n 127.0.0.1:9876 autoCreateTopicEnable=true
```

或者直接cmd中输入

```text
mqadmin.cmd updateBrokerConfig -blocalhost:10911 -kenablePropertyFilter -vtrue
```

页面查看开启与否

![image-20210216175129980](https://ydlclass.com/doc21xnv/assets/image-20210216175129980.81f41d00.png)

### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_5springboot整合)5springboot整合

新建 springboot项目

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_5-1导包)5.1导包

```text
 	<dependency>
        <groupId>org.apache.rocketmq</groupId>
        <artifactId>rocketmq-spring-boot-starter</artifactId>
        <version>2.0.3</version>
    </dependency>
```

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_5-2配置文件)5.2配置文件

```text
rocketmq.name-server=localhost:9876
rocketmq.producer.group=demo_producer
```

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_5-3实体类)5.3实体类

```text
public class user implements Serializable {
    String userName;
    String userId;

    public user(){

    }

    public user(String userName, String userId) {
        this.userName = userName;
        this.userId = userId;
    }

    @Override
    public String toString() {
        return "demoEntity{" +
                "userName='" + userName + '\'' +
                ", userId='" + userId + '\'' +
                '}';
    }
```

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_5-4生产者)5.4生产者

```text
@RestController
public class DemoProducers {
    @Autowired
    private RocketMQTemplate template;

    @RequestMapping("/producer")
    public String producersMessage() {
        User user = new User("sharfine", "123456789");
        template.convertAndSend("demo-topic", user);
        return JSON.toJSONString(user);
    }
}
```

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_5-5消费者)5.5消费者

```text
@Service
@RocketMQMessageListener(topic = "demo-topic", consumerGroup = "demo_consumer")
public class DemoConsumers1 implements RocketMQListener<user> {
    @Override
    public void onMessage(user user) {
        System.out.println("Consumers1接收消息:" + demoEntity.toString());
    }
}
```

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_5-6其他消息)5.6其他消息

**异步发送**

```text
       rocketMQTemplate.asyncSend("topic9", user, new SendCallback() {
            @Override
            public void onSuccess(SendResult sendResult) {
                System.out.println(sendResult);
            }

            @Override
            public void onException(Throwable throwable) {
                System.out.println(throwable);
            }
        });
```

**单向发送**

```text
rocketMQTemplate.sendOneWay("topic9",user);
```

**延时消息**

```text
rocketMQTemplate.syncSend("topic9", MessageBuilder.withPayload("test delay").build(),2000,2);
```

**批量**

```text
List<Message> msgList = new ArrayList<>();
msgList.add(new Message("topic6", "tag1", "msg1".getBytes()));
msgList.add(new Message("topic6", "tag1", "msg2".getBytes()));
msgList.add(new Message("topic6", "tag1", "msg3".getBytes()));        rocketMQTemplate.syncSend("topic8",msgList,1000);
```

**Tag过滤**

消费者

```text
@RocketMQMessageListener(topic = "topic9",consumerGroup = "group1",selectorExpression = "tag1")
```

Sql过滤

```text
@RocketMQMessageListener(topic = "topic9",consumerGroup = "group1",selectorExpression = "age>18"
        ,selectorType= SelectorType.SQL92)
```

改消息模式

```text
@RocketMQMessageListener(topic = "topic9",consumerGroup = "group1",messageModel = MessageModel.BROADCASTING)
```

## [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#rocketmq-进阶篇)RocketMQ 进阶篇

### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_1消息的特殊处理)1消息的特殊处理

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_1-1-错乱的消息顺序)1.1 错乱的消息顺序

原因

消息有序指的是可以按照消息的发送顺序来消费(FIFO)。RocketMQ可以严格的保证消息有序，可以分为分区有序或者全局有序。

顺序消费的原理解析，在默认的情况下消息发送会采取Round Robin轮询方式把消息发送到不同的queue(分区队列)；而消费消息的时候从多个queue上拉取消息，这种情况发送和消费是不能保证顺序。但是如果控制发送的顺序消息只依次发送到同一个queue中，消费的时候只从这个queue上依次拉取，则就保证了顺序。当发送和消费参与的queue只有一个，则是全局有序；如果多个queue参与，则为分区有序，即相对每个queue，消息都是有序的。

下面用订单进行分区有序的示例。一个订单的顺序流程是：创建、付款、推送、完成。订单号相同的消息会被先后发送到同一个队列中，消费时，同一个OrderId获取到的肯定是同一个队列。

![image-20210227193832410](https://ydlclass.com/doc21xnv/assets/image-20210227193832410.dd83739c.png)

先读到第一个订单的创建和完成消息

![image-20210227193914094](https://ydlclass.com/doc21xnv/assets/image-20210227193914094.155711c3.png)

想要的效果

![image-20210227194435616](https://ydlclass.com/doc21xnv/assets/image-20210227194435616.4d9e4f37.png)

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_1-2-顺序消息)1.2 顺序消息

1.2.1订单步骤实体类

```text
package com.ydl.order.domain;

/**
 * 订单的步骤
 */
public class OrderStep {
    private long orderId;
    private String desc;

    public long getOrderId() {
        return orderId;
    }

    public void setOrderId(long orderId) {
        this.orderId = orderId;
    }

    public String getDesc() {
        return desc;
    }

    public void setDesc(String desc) {
        this.desc = desc;
    }

    @Override
    public String toString() {
        return "OrderStep{" +
                "orderId=" + orderId +
                ", desc='" + desc + '\'' +
                '}';
    }
}
```



1.2.1发送消息

```java
package com.ydl.order;

import com.ydl.order.domain.OrderStep;
import org.apache.rocketmq.client.producer.DefaultMQProducer;
import org.apache.rocketmq.client.producer.MessageQueueSelector;
import org.apache.rocketmq.client.producer.SendResult;
import org.apache.rocketmq.common.message.Message;
import org.apache.rocketmq.common.message.MessageQueue;

import java.text.SimpleDateFormat;
import java.util.ArrayList;
import java.util.Date;
import java.util.List;

public class Producer {
    public static void main(String[] args) throws Exception {
        DefaultMQProducer producer = new DefaultMQProducer("group1");
        producer.setNamesrvAddr("localhost:9876");
        producer.start();

        List<OrderStep> orderList = new Producer().buildOrders();

        //设置消息进入到指定的消息队列中
        for (final OrderStep order : orderList) {
            Message msg = new Message("topic1", order.toString().getBytes());
            //发送时要指定对应的消息队列选择器
            SendResult result = producer.send(msg, new MessageQueueSelector() {
                //设置当前消息发送时使用哪一个消息队列
                public MessageQueue select(List<MessageQueue> list, Message message, Object o) {
                    //根据发送的信息不同，选择不同的消息队列
                    //根据id来选择一个消息队列的对象，并返回->id得到int值
                    long orderId = order.getOrderId();
                    long mqIndex = orderId % list.size();
                    return list.get((int) mqIndex);
                }
            }, null);
            System.out.println(result);
        }

        producer.shutdown();


        //for (int i = 0; i < 10; i++) {
        //    Message msg = new Message("topic1", ("hello rocketmq"+i).getBytes("UTF-8"));
        //    SendResult result = producer.send(msg);
        //    System.out.println("返回结果：" + result);
        //}
    }

    /**
     * 生成模拟订单数据
     */
    private List<OrderStep> buildOrders() {
        List<OrderStep> orderList = new ArrayList<OrderStep>();

        OrderStep orderDemo = new OrderStep();
        orderDemo.setOrderId(1L);
        orderDemo.setDesc("创建");
        orderList.add(orderDemo);

        orderDemo = new OrderStep();
        orderDemo.setOrderId(2L);
        orderDemo.setDesc("创建");
        orderList.add(orderDemo);

        orderDemo = new OrderStep();
        orderDemo.setOrderId(1L);
        orderDemo.setDesc("付款");
        orderList.add(orderDemo);

        orderDemo = new OrderStep();
        orderDemo.setOrderId(3L);
        orderDemo.setDesc("创建");
        orderList.add(orderDemo);

        orderDemo = new OrderStep();
        orderDemo.setOrderId(2L);
        orderDemo.setDesc("付款");
        orderList.add(orderDemo);

        orderDemo = new OrderStep();
        orderDemo.setOrderId(3L);
        orderDemo.setDesc("付款");
        orderList.add(orderDemo);

        orderDemo = new OrderStep();
        orderDemo.setOrderId(2L);
        orderDemo.setDesc("完成");
        orderList.add(orderDemo);

        orderDemo = new OrderStep();
        orderDemo.setOrderId(1L);
        orderDemo.setDesc("推送");
        orderList.add(orderDemo);

        orderDemo = new OrderStep();
        orderDemo.setOrderId(3L);
        orderDemo.setDesc("完成");
        orderList.add(orderDemo);

        orderDemo = new OrderStep();
        orderDemo.setOrderId(1L);
        orderDemo.setDesc("完成");
        orderList.add(orderDemo);

        return orderList;
    }
}
```



1.2.2接收消息

```java
         //使用单线程的模式从消息队列中取数据，一个线程绑定一个消息队列
		consumer.registerMessageListener(new MessageListenerOrderly() {
            //使用MessageListenerOrderly接口后，对消息队列的处理由一个消息队列多个线程服务，转化为一个消息队列一个线程服务
            public ConsumeOrderlyStatus consumeMessage(List<MessageExt> list, ConsumeOrderlyContext consumeOrderlyContext) {
                for (MessageExt msg : list) {
                    System.out.println(Thread.currentThread().getName()+"。消息：" + new String(msg.getBody())+"。queueId:"+msg.getQueueId());
                }
                return ConsumeOrderlyStatus.SUCCESS;
            }
        });
```

1
2
3
4
5
6
7
8
9
10

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_1-3-事务消息)1.3 事务消息

1. 正常事务过程
2. 事务补偿过程

![image-20201211160454763](https://ydlclass.com/doc21xnv/assets/image-20201211160454763.88ac51a3.png)

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_1-4-事务消息状态)1.4 事务消息状态

1. 提交状态：允许进入队列，此消息与非事务消息无区别
2. 回滚状态：不允许进入队列，此消息等同于未发送过
3. 中间状态：完成了half消息的发送，未对MQ进行二次状态确认
4. 注意：事务消息仅与生产者有关，与消费者无关

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_1-5-事务消息)1.5 事务消息

提交状态

```java
//事务消息使用的生产者是TransactionMQProducer
TransactionMQProducer producer = new TransactionMQProducer("group1");
producer.setNamesrvAddr("localhost:9876");
//添加本地事务对应的监听
producer.setTransactionListener(new TransactionListener() {
//正常事务过程
public LocalTransactionState executeLocalTransaction(Message message, Object o) {
return LocalTransactionState.COMMIT_MESSAGE;
}
//事务补偿过程
public LocalTransactionState checkLocalTransaction(MessageExt messageExt) {
return null;
}
});
producer.start();
Message msg = new Message("topic8",("事务消息：hello rocketmq ").getBytes("UTF-8"));
SendResult result = producer.sendMessageInTransaction(msg,null);
System.out.println("返回结果："+result);
producer.shutdown();
```

回滚状态

```java
            producer.setTransactionListener(new TransactionListener() {
            //正常事务
            @Override
            public LocalTransactionState executeLocalTransaction(Message msg, Object arg) {
                return LocalTransactionState.ROLLBACK_MESSAGE;
            }
            //事务补偿
            @Override
            public LocalTransactionState checkLocalTransaction(MessageExt msg) {
                return null;
            }
        });
```

中间状态

```java
public static void main(String[] args) throws Exception {
        TransactionMQProducer producer=new TransactionMQProducer("group1");
        producer.setNamesrvAddr("localhost:9876");
        producer.setTransactionListener(new TransactionListener() {
            //正常事务
            @Override
            public LocalTransactionState executeLocalTransaction(Message msg, Object arg) {
                return LocalTransactionState.UNKNOW;
            }
            //事务补偿 正常执行UNKNOW才会触发
            @Override
            public LocalTransactionState checkLocalTransaction(MessageExt msg) {
                System.out.println("事务补偿");
                return LocalTransactionState.COMMIT_MESSAGE;
            }
        });
        producer.start();
        Message msg = new Message("topic13", "hello rocketmq".getBytes("UTF-8"));
        SendResult result = producer.sendMessageInTransaction(msg, null);
        System.out.println("返回结果：" + result);

        //事务补偿生产者一定要一直启动着
        //producer.shutdown();
    }
```

### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_2-集群搭建)2. 集群搭建

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_2-1-rocketmq集群分类)2.1 RocketMQ集群分类

1. 单机
   1. 一个broker提供服务（宕机后服务瘫痪）
2. 集群
   1. 多个broker提供服务（单机宕机后消息无法及时被消费）
   2. 多个master多个slave
      1. master到slave消息同步方式为同步（较异步方式性能略低，消息无延迟）
      2. master到slave消息同步方式为异步（较同步方式性能略高，数据略有延迟）

![image-20201211160840336](https://ydlclass.com/doc21xnv/assets/image-20201211160840336.f8e211bc.png)

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_2-2-rocketmq集群特征)2.2 RocketMQ集群特征

![image-20201211160927121](https://ydlclass.com/doc21xnv/assets/image-20201211160927121.1d9c9bbc.png)

RocketMQ集群工作流程

1. 步骤1：NameServer启动，开启监听，等待broker、producer与consumer连接
2. 步骤2：broker启动，根据配置信息，连接所有的NameServer，并保持长连接
3. 步骤2补充：如果broker中有现存数据， NameServer将保存topic与broker关系
4. 步骤3：producer发信息，连接某个NameServer，并建立长连接
5. 步骤4：producer发消息
   1. 步骤4.1若果topic存在，由NameServer直接分配
   2. 步骤4.2如果topic不存在，由NameServer创建topic与broker关系，并分配
6. 步骤5：producer在broker的topic选择一个消息队列（从列表中选择）
7. 步骤6：producer与broker建立长连接，用于发送消息
8. 步骤7：producer发送消息

**comsumer工作流程同producer**

![image-20210227230840419](https://ydlclass.com/doc21xnv/assets/image-20210227230840419.9726b7cd.png)

双主双从集群搭建:

![image-20210228145023406](https://ydlclass.com/doc21xnv/assets/image-20210228145023406.326553b2.png)

操作步骤：**注意两台机器同时操作**

1. 配置服务器环境:

   ```properties
   vim /etc/hosts
   ```

   1

   ```properties
   # nameserver
   192.168.200.129 rocketmq-nameserver1
   192.168.200.130 rocketmq-nameserver2
   # broker
   192.168.200.129 rocketmq-master1
   192.168.200.129 rocketmq-slave2
   192.168.200.130 rocketmq-master2
   192.168.200.130 rocketmq-slave1
   ```

2. 配置完毕后重启网卡，应用配置

```properties
systemctl restart network
```

1. 关闭防火墙或者开发指定端口对外提供服务

   ```properties
   # 关闭防火墙
   systemctl stop firewalld.service
   # 查看防火墙的状态
   firewall-cmd --state
   # 禁止firewall开机启动
   systemctl disable firewalld.service
   ```

2. 配置服务器环境

   ```properties
   vim /etc/profile
   ```

   ```properties
   #set rocketmq
   ROCKETMQ_HOME=/rocketmq
   PATH=$PATH:$ROCKETMQ_HOME/bin
   export ROCKETMQ_HOME PATH
   ```

3. 配置完毕后重启网卡，应用配置

   ```properties
   source /etc/profile
   ```

4. 将rocketmq解压到/rocketmq

5. 创建集群服务器的数据存储目录

   ```properties
   #master 数据存储目录
   mkdir /rocketmq/store
   mkdir /rocketmq/store/commitlog
   mkdir /rocketmq/store/consumequeue
   mkdir /rocketmq/store/index
   
   #slave 数据存储目录
   mkdir /rocketmq/store-slave
   mkdir /rocketmq/store-slave/commitlog
   mkdir /rocketmq/store-slave/consumequeue
   mkdir /rocketmq/store-slave/index
   ```

6. 注意master与slave如果在同一个虚拟机中部署，需要将存储目录区分开

7. **第一台129机器上**

   ```text
   cd /rocketmq/conf/2m-2s-sync
   ```

   ```text
   vim broker-a.properties
   ```

   ```properties
   #所属集群名字
   brokerClusterName=rocketmq-cluster
   #broker名字，注意此处不同的配置文件填写的不一样
   brokerName=broker-a
   #0 表示 Master，>0 表示 Slave
   brokerId=0
   #nameServer地址，分号分割
   namesrvAddr=rocketmq-nameserver1:9876;rocketmq-nameserver2:9876
   #在发送消息时，自动创建服务器不存在的topic，默认创建的队列数
   defaultTopicQueueNums=4
   #是否允许 Broker 自动创建Topic，建议线下开启，线上关闭
   autoCreateTopicEnable=true
   #是否允许 Broker 自动创建订阅组，建议线下开启，线上关闭
   autoCreateSubscriptionGroup=true
   #Broker 对外服务的监听端口
   listenPort=10911
   #删除文件时间点，默认凌晨 4点
   deleteWhen=04
   #文件保留时间，默认 48 小时
   fileReservedTime=120
   #commitLog每个文件的大小默认1G
   mapedFileSizeCommitLog=1073741824
   #ConsumeQueue每个文件默认存30W条，根据业务情况调整
   mapedFileSizeConsumeQueue=300000
   #destroyMapedFileIntervalForcibly=120000
   #redeleteHangedFileInterval=120000
   #检测物理文件磁盘空间
   diskMaxUsedSpaceRatio=88
   #存储路径
   storePathRootDir=/rocketmq/store
   #commitLog 存储路径
   storePathCommitLog=/rocketmq/store/commitlog
   #消费队列存储路径存储路径
   storePathConsumeQueue=/rocketmq/store/consumequeue
   #消息索引存储路径
   storePathIndex=/rocketmq/store/index
   #checkpoint 文件存储路径
   storeCheckpoint=/rocketmq/store/checkpoint
   #abort 文件存储路径
   abortFile=/rocketmq/store/abort
   #限制的消息大小
   maxMessageSize=65536
   #flushCommitLogLeastPages=4
   #flushConsumeQueueLeastPages=2
   #flushCommitLogThoroughInterval=10000
   #flushConsumeQueueThoroughInterval=60000
   #Broker 的角色
   #- ASYNC_MASTER 异步复制Master
   #- SYNC_MASTER 同步双写Master
   #- SLAVE
   brokerRole=SYNC_MASTER
   #刷盘方式
   #- ASYNC_FLUSH 异步刷盘
   #- SYNC_FLUSH 同步刷盘
   flushDiskType=SYNC_FLUSH
   #checkTransactionMessageEnable=false
   #发消息线程池数量
   #sendMessageThreadPoolNums=128
   #拉消息线程池数量
   #pullMessageThreadPoolNums=128
   ```

   ```text
   vim broker-b-s.properties
   ```

   ```properties
   #所属集群名字
   brokerClusterName=rocketmq-cluster
   #broker名字，注意此处不同的配置文件填写的不一样
   brokerName=broker-b
   #0 表示 Master，>0 表示 Slave
   brokerId=1
   #nameServer地址，分号分割
   namesrvAddr=rocketmq-nameserver1:9876;rocketmq-nameserver2:9876
   #在发送消息时，自动创建服务器不存在的topic，默认创建的队列数
   defaultTopicQueueNums=4
   #是否允许 Broker 自动创建Topic，建议线下开启，线上关闭
   autoCreateTopicEnable=true
   #是否允许 Broker 自动创建订阅组，建议线下开启，线上关闭
   autoCreateSubscriptionGroup=true
   #Broker 对外服务的监听端口
   listenPort=11011
   #删除文件时间点，默认凌晨 4点
   deleteWhen=04
   #文件保留时间，默认 48 小时
   fileReservedTime=120
   #commitLog每个文件的大小默认1G
   mapedFileSizeCommitLog=1073741824
   #ConsumeQueue每个文件默认存30W条，根据业务情况调整
   mapedFileSizeConsumeQueue=300000
   #destroyMapedFileIntervalForcibly=120000
   #redeleteHangedFileInterval=120000
   #检测物理文件磁盘空间
   diskMaxUsedSpaceRatio=88
   #存储路径
   storePathRootDir=/rocketmq/store-slave
   #commitLog 存储路径
   storePathCommitLog=/rocketmq/store-slave/commitlog
   #消费队列存储路径存储路径
   storePathConsumeQueue=/rocketmq/store-slave/consumequeue
   #消息索引存储路径
   storePathIndex=/rocketmq/store-slave/index
   #checkpoint 文件存储路径
   storeCheckpoint=/rocketmq/store-slave/checkpoint
   #abort 文件存储路径
   abortFile=/rocketmq/store-slave/abort
   #限制的消息大小
   maxMessageSize=65536
   #flushCommitLogLeastPages=4
   #flushConsumeQueueLeastPages=2
   #flushCommitLogThoroughInterval=10000
   #flushConsumeQueueThoroughInterval=60000
   #Broker 的角色
   #- ASYNC_MASTER 异步复制Master
   #- SYNC_MASTER 同步双写Master
   #- SLAVE
   brokerRole=SLAVE
   #刷盘方式
   #- ASYNC_FLUSH 异步刷盘
   #- SYNC_FLUSH 同步刷盘
   flushDiskType=ASYNC_FLUSH
   #checkTransactionMessageEnable=false
   #发消息线程池数量
   #sendMessageTreadPoolNums=128
   #拉消息线程池数量
   #pullMessageThreadPoolNums=128
   ```

   ```text
   rm -rf broker-a-s.properties
   rm -rf broker-b.properties
   ```

   **第二台130机器上**

   ```text
   cd /rocketmq/conf/2m-2s-sync
   ```

   ```text
   vim broker-b.properties
   ```

   ```properties
   #所属集群名字
   brokerClusterName=rocketmq-cluster
   #broker名字，注意此处不同的配置文件填写的不一样
   brokerName=broker-b
   #0 表示 Master，>0 表示 Slave
   brokerId=0
   #nameServer地址，分号分割
   namesrvAddr=rocketmq-nameserver1:9876;rocketmq-nameserver2:9876
   #在发送消息时，自动创建服务器不存在的topic，默认创建的队列数
   defaultTopicQueueNums=4
   #是否允许 Broker 自动创建Topic，建议线下开启，线上关闭
   autoCreateTopicEnable=true
   #是否允许 Broker 自动创建订阅组，建议线下开启，线上关闭
   autoCreateSubscriptionGroup=true
   #Broker 对外服务的监听端口
   listenPort=10911
   #删除文件时间点，默认凌晨 4点
   deleteWhen=04
   #文件保留时间，默认 48 小时
   fileReservedTime=120
   #commitLog每个文件的大小默认1G
   mapedFileSizeCommitLog=1073741824
   #ConsumeQueue每个文件默认存30W条，根据业务情况调整
   mapedFileSizeConsumeQueue=300000
   #destroyMapedFileIntervalForcibly=120000
   #redeleteHangedFileInterval=120000
   #检测物理文件磁盘空间
   diskMaxUsedSpaceRatio=88
   #存储路径
   storePathRootDir=/rocketmq/store
   #commitLog 存储路径
   storePathCommitLog=/rocketmq/store/commitlog
   #消费队列存储路径存储路径
   storePathConsumeQueue=/rocketmq/store/consumequeue
   #消息索引存储路径
   storePathIndex=/rocketmq/store/index
   #checkpoint 文件存储路径
   storeCheckpoint=/rocketmq/store/checkpoint
   #abort 文件存储路径
   abortFile=/rocketmq/store/abort
   #限制的消息大小
   maxMessageSize=65536
   #flushCommitLogLeastPages=4
   #flushConsumeQueueLeastPages=2
   #flushCommitLogThoroughInterval=10000
   #flushConsumeQueueThoroughInterval=60000
   #Broker 的角色
   #- ASYNC_MASTER 异步复制Master
   #- SYNC_MASTER 同步双写Master
   #- SLAVE
   brokerRole=SYNC_MASTER
   #刷盘方式
   #- ASYNC_FLUSH 异步刷盘
   #- SYNC_FLUSH 同步刷盘
   flushDiskType=SYNC_FLUSH
   #checkTransactionMessageEnable=false
   #发消息线程池数量
   #sendMessageTreadPoolNums=128
   #拉消息线程池数量
   #pullMessageThreadPoolNums=128
   ```

   ```text
   vim broker-a-s.properties
   ```

   ```properties
   #所属集群名字
   brokerClusterName=rocketmq-cluster
   #broker名字，注意此处不同的配置文件填写的不一样
   brokerName=broker-a
   #0 表示 Master，>0 表示 Slave
   brokerId=1
   #nameServer地址，分号分割
   namesrvAddr=rocketmq-nameserver1:9876;rocketmq-nameserver2:9876
   #在发送消息时，自动创建服务器不存在的topic，默认创建的队列数
   defaultTopicQueueNums=4
   #是否允许 Broker 自动创建Topic，建议线下开启，线上关闭
   autoCreateTopicEnable=true
   #是否允许 Broker 自动创建订阅组，建议线下开启，线上关闭
   autoCreateSubscriptionGroup=true
   #Broker 对外服务的监听端口
   listenPort=11011
   #删除文件时间点，默认凌晨 4点
   deleteWhen=04
   #文件保留时间，默认 48 小时
   fileReservedTime=120
   #commitLog每个文件的大小默认1G
   mapedFileSizeCommitLog=1073741824
   #ConsumeQueue每个文件默认存30W条，根据业务情况调整
   mapedFileSizeConsumeQueue=300000
   #destroyMapedFileIntervalForcibly=120000
   #redeleteHangedFileInterval=120000
   #检测物理文件磁盘空间
   diskMaxUsedSpaceRatio=88
   #存储路径
   storePathRootDir=/rocketmq/store-slave
   #commitLog 存储路径
   storePathCommitLog=/rocketmq/store-slave/commitlog
   #消费队列存储路径存储路径
   storePathConsumeQueue=/rocketmq/store-slave/consumequeue
   #消息索引存储路径
   storePathIndex=/rocketmq/store-slave/index
   #checkpoint 文件存储路径
   storeCheckpoint=/rocketmq/store-slave/checkpoint
   #abort 文件存储路径
   abortFile=/rocketmq/store-slave/abort
   #限制的消息大小
   maxMessageSize=65536
   #flushCommitLogLeastPages=4
   #flushConsumeQueueLeastPages=2
   #flushCommitLogThoroughInterval=10000
   #flushConsumeQueueThoroughInterval=60000
   #Broker 的角色
   #- ASYNC_MASTER 异步复制Master
   #- SYNC_MASTER 同步双写Master
   #- SLAVE
   brokerRole=SLAVE
   #刷盘方式
   #- ASYNC_FLUSH 异步刷盘
   #- SYNC_FLUSH 同步刷盘
   flushDiskType=ASYNC_FLUSH
   #checkTransactionMessageEnable=false
   #发消息线程池数量
   #sendMessageThreadPoolNums=128
   #拉消息线程池数量
   #pullMessageThreadPoolNums=128
   ```

   ```text
   rm -rf broker-a.properties
   rm -rf broker-b-s.properties
   ```

8. 检查启动内存

   ```properties
   vim /rocketmq/bin/runbroker.sh
   ```

   ```properties
   # 开发环境配置 JVM Configuration
   JAVA_OPT="${JAVA_OPT} -server -Xms256m -Xmx256m -Xmn128m"
   ```

9. 启动服务器（在bin目录下依次启动）

   129上

   ```properties
   nohup sh mqnamesrv &
   ```

   ```properties
   nohup sh mqbroker -c ../conf/2m-2s-sync/broker-a.properties &
   ```

   ```properties
   nohup sh mqbroker -c ../conf/2m-2s-sync/broker-b-s.properties &
   ```

​         130上

```properties
nohup sh mqnamesrv &
```

```properties
nohup sh mqbroker -c ../conf/2m-2s-sync/broker-a-s.properties &
```

```properties
nohup sh mqbroker -c ../conf/2m-2s-sync/broker-b.properties & 
```

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_4-3-rocketmq-console集群监控平台搭建)4.3 rocketmq-console集群监控平台搭建

1. incubator-rocketmq-externals是一个基于rocketmq的基础之上扩展开发的开源项目
2. 获取地址：https://github.com/apache/rocketmq-externals
3. rocketmq-console是一款基于java环境开发的（springboot）的管理控制台工具

### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_3-高级特性-重点)3. 高级特性（重点）

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_3-1-消息的存储)3.1 消息的存储

1. 消息生成者发送消息到MQ
2. MQ返回ACK给生产者
3. MQ push 消息给对应的消费者
4. 消息消费者返回ACK给MQ

说明：ACK（Acknowledge character）

![image-20201211162105593](https://ydlclass.com/doc21xnv/assets/image-20201211162105593.da7ab600.png)

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_3-2-消息的存储)3.2 消息的存储

1. 消息生成者发送消息到MQ
2. MQ收到消息，将消息进行持久化，存储该消息
3. MQ返回ACK给生产者
4. MQ push 消息给对应的消费者
5. 消息消费者返回ACK给MQ
6. MQ删除消息

注意：

1. 第⑤步MQ在指定时间内接到消息消费者返回ACK，MQ认定消息消费成功，执行⑥
2. 第⑤步MQ在指定时间内未接到消息消费者返回ACK，MQ认定消息消费失败，重新执行④⑤⑥

![image-20201211162231313](https://ydlclass.com/doc21xnv/assets/image-20201211162231313.9285f9b2.png)

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_3-3-消息的存储介质)3.3 消息的存储介质

1. 数据库
   1. ActiveMQ
   2. 缺点：数据库瓶颈将成为MQ瓶颈
2. 文件系统
   1. RocketMQ/Kafka/RabbitMQ
   2. 解决方案：采用消息刷盘机制进行数据存储
   3. 缺点：硬盘损坏的问题无法避免

![image-20201211162334322](https://ydlclass.com/doc21xnv/assets/image-20201211162334322.221493d8.png)

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_3-4-高效的消息存储与读写方式)3.4 高效的消息存储与读写方式

1. SSD（Solid State Disk）

   ![image-20201211162416020](https://ydlclass.com/doc21xnv/assets/image-20201211162416020.37016a21.png)

   1. 随机写（100KB/s）

      ![image-20201211162448564](https://ydlclass.com/doc21xnv/assets/image-20201211162448564.28e735f2.png)

   2. 顺序写 （600MB/s）1秒1部电影

![image-20201211162541076](https://ydlclass.com/doc21xnv/assets/image-20201211162541076.35fabc45.png)

![image-20201211162638525](https://ydlclass.com/doc21xnv/assets/image-20201211162638525.b15728cc.png)

1. Linux系统发送数据的方式

2. “零拷贝”技术

   1. 数据传输由传统的4次复制简化成3次复制，减少1次复制过程
   2. Java语言中使用MappedByteBuffer类实现了该技术
   3. 要求：预留存储空间，用于保存数据（1G存储空间起步）

   ![image-20201211162729614](https://ydlclass.com/doc21xnv/assets/image-20201211162729614.fd985403.png)

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_3-5-消息存储结构)3.5 消息存储结构

1. MQ数据存储区域包含如下内容
   1. 消息数据存储区域
      1. topic
      2. queueId
      3. message
   2. 消费逻辑队列
      1. minOffset
      2. maxOffset
      3. consumerOffset
   3. 索引
      1. key索引
      2. 创建时间索引

![image-20201211162857722](https://ydlclass.com/doc21xnv/assets/image-20201211162857722.2ff7e84e.png)

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_3-6-刷盘机制)3.6 刷盘机制

1. 同步刷盘

   1. 生产者发送消息到MQ，MQ接到消息数据

   2. MQ挂起生产者发送消息的线程

   3. MQ将消息数据写入内存

   4. 内存数据写入硬盘

   5. 磁盘存储后返回SUCCESS

   6. MQ恢复挂起的生产者线程

   7. 发送ACK到生产者

      ![image-20201211163159680](https://ydlclass.com/doc21xnv/assets/image-20201211163159680.93998f9b.png)

2. 异步刷盘

   1. 生产者发送消息到MQ，MQ接到消息数据
   2. 
   3. MQ将消息数据写入内存
   4. 
   5. 
   6. 
   7. 发送ACK到生产者

![image-20201211163238738](https://ydlclass.com/doc21xnv/assets/image-20201211163238738.eb3bcd53.png)

1. 同步刷盘：安全性高，效率低，速度慢（适用于对数据安全要求较高的业务）
2. 异步刷盘：安全性低，效率高，速度快（适用于对数据处理速度要求较高的业务）

**配置方式**

```properties
#刷盘方式
#- ASYNC_FLUSH 异步刷盘
#- SYNC_FLUSH 同步刷盘
flushDiskType=SYNC_FLUSH
```

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_5-7-高可用性)5.7 高可用性

1. nameserver
   1. 无状态+全服务器注册
2. 消息服务器
   1. 主从架构（2M-2S）
3. 消息生产
   1. 生产者将相同的topic绑定到多个group组，保障master挂掉后，其他master仍可正常进行消 息接收
4. 消息消费
   1. RocketMQ自身会根据master的压力确认是否由master承担消息读取的功能，当master繁忙 时候，自动切换由slave承担数据读取的工作

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_5-8-主从数据复制)5.8 主从数据复制

1. 同步复制

   1. master接到消息后，先复制到slave，然后反馈给生产者写操作成功
   2. 优点：数据安全，不丢数据，出现故障容易恢复
   3. 缺点：影响数据吞吐量，整体性能低

2. 异步复制

   1. master接到消息后，立即返回给生产者写操作成功，当消息达到一定量后再异步复制到slave
   2. 优点：数据吞吐量大，操作延迟低，性能高
   3. 缺点：数据不安全，会出现数据丢失的现象，一旦master出现故障，从上次数据同步到故障时间的数据将丢失

3. 配置方式

   ```properties
   #Broker 的角色
   #- ASYNC_MASTER 异步复制Master
   #- SYNC_MASTER 同步双写Master
   #- SLAVE
   brokerRole=SYNC_MASTER
   ```

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_5-9-负载均衡)5.9 负载均衡

1. Producer负载均衡
   1. 内部实现了不同broker集群中对同一topic对应消息队列的负载均衡
2. Consumer负载均衡
   1. 平均分配
   2. 循环平均分配
3. 广播模式（不参与负载均衡）

![image-20201211163654002](https://ydlclass.com/doc21xnv/assets/image-20201211163654002.33a93491.png)

![image-20201211163706995](https://ydlclass.com/doc21xnv/assets/image-20201211163706995.3eb09b97.png)

![image-20201211163736770](https://ydlclass.com/doc21xnv/assets/image-20201211163736770.58dc53f1.png)

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_5-10-消息重试)5.10 消息重试

1. 当消息消费后未正常返回消费成功的信息将启动消息重试机制
2. 消息重试机制
   1. 顺序消息
   2. 无序消息

##### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_5-10-1-顺序消息重试)5.10.1 顺序消息重试

![image-20201211163848104](https://ydlclass.com/doc21xnv/assets/image-20201211163848104.704cd4c8.png)

![image-20201211163854405](https://ydlclass.com/doc21xnv/assets/image-20201211163854405.4a9e5f3f.png)

1. 当消费者消费消息失败后，RocketMQ会自动进行消息重试（每次间隔时间为 1 秒）
2. 注意：应用会出现消息消费被阻塞的情况，因此，要对顺序消息的消费情况进行监控，避免阻塞现象的发生

![image-20201211163910587](https://ydlclass.com/doc21xnv/assets/image-20201211163910587.d0277d4a.png)

![image-20201211163928369](https://ydlclass.com/doc21xnv/assets/image-20201211163928369.22d707c7.png)

##### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_5-10-2-无序消息重试)5.10.2 无序消息重试

1. 无序消息包括普通消息、定时消息、延时消息、事务消息
2. 无序消息重试仅适用于负载均衡（集群）模型下的消息消费，不适用于广播模式下的消息消费
3. 为保障无序消息的消费，MQ设定了合理的消息重试间隔时长

![image-20201211164005887](https://ydlclass.com/doc21xnv/assets/image-20201211164005887.fdcb00ba.png)

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_5-11-死信队列)5.11 死信队列

1. 当消息消费重试到达了指定次数（默认16次）后，MQ将无法被正常消费的消息称为死信消息（Dead-Letter Message）
2. 死信消息不会被直接抛弃，而是保存到了一个全新的队列中，该队列称为死信队列（Dead-Letter Queue）\
3. 死信队列特征
   1. 归属某一个组（Gourp Id），而不归属Topic，也不归属消费者
   2. 一个死信队列中可以包含同一个组下的多个Topic中的死信消息
   3. 死信队列不会进行默认初始化，当第一个死信出现后，此队列首次初始化
4. 死信队列中消息特征
   1. 不会被再次重复消费
   2. 死信队列中的消息有效期为3天，达到时限后将被清除

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_5-12-死信处理)5.12 死信处理

1. 在监控平台中，通过查找死信，获取死信的messageId，然后通过id对死信进行精准消费

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_5-13-消息重复消费)5.13 消息重复消费

1. 消息重复消费原因
   1. 生产者发送了重复的消息
      1. 网络闪断
      2. 生产者宕机
   2. 消息服务器投递了重复的消息
      1. 网络闪断
   3. 动态的负载均衡过程
      1. 网络闪断/抖动
      2. broker重启
      3. 订阅方应用重启（消费者）
      4. 客户端扩容
      5. 客户端缩容

![image-20201211164244534](https://ydlclass.com/doc21xnv/assets/image-20201211164244534.0bb9f42c.png)

#### [#](https://ydlclass.com/doc21xnv/distribute/rocketmq/#_5-14-消息幂等)5.14 消息幂等

1. 对同一条消息，无论消费多少次，结果保持一致，称为消息幂等性

2. 解决方案

   1. 使用业务id作为消息的key
   2. 在消费消息时，客户端对key做判定，未使用过放行，使用过抛弃

3. 注意：messageId由RocketMQ产生，messageId并不具有唯一性，不能作用幂等判定条件

4. 常见的幂等方法示例

   •新增:不幂等 insert into order values (……)

   •查询:幂等

   •删除:幂等 delete from 表 where id =1

   •修改:不幂等 update account set balance = balance+100 where no=1

   •修改:幂等 update account set balance =100 where no=1

计算机考研：

1计算机组成原理 ：

2网络：3次握手 4次挥手

3操作系统：linux

4数据结构-算法：tree b+ 链表

执行力