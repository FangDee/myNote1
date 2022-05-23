## 初识Spring Cloud与微服务

 2018-04-02 |  Visit count 644943

在传统的软件架构中，我们通常采用的是单体应用来构建一个系统，一个单体应用糅合了各种业务模块。起初在业务规模不是很大的情况下，对于单体应用的开发维护也相对容易。但随着企业的发展，业务规模与日递增，单体应用变得愈发臃肿。由于单体应用将各种业务模块聚合在一起，并且部署在一个进程内，所以通常我们对其中一个业务模块的修改也必须将整个应用重新打包上线。为了解决单体应用变得庞大脯肿之后产生的难以维护的问题，微服务架构便出现在了大家的视线里。



## 什么是微服务

微服务 (Microservices) 是一种软件架构风格，起源于Peter Rodgers博士于 2005 年度云端运算博览会提出的微 Web 服务 (Micro-Web-Service) 。微服务主旨是将一个原本独立的系统 拆分成多个小型服务，这些小型服务都在各自独立的进程中运行，服务之间通过基于HTTP的RESTful API进行通信协作。下图展示了单体应用和微服务之间的区别：

![micro-deployment.png](https://mrbird.cc/img/micro-deployment.png)

在微服务的架构下，单体应用的各个业务模块被拆分为一个个单独的服务并部署在单独的进程里，每个服务都可以单独的部署和升级。这种去中心化的模式使得后期维护和开发变得更加灵活和方便。由于各个服务单独部署，所以可以使用不同的语句来开发各个业务服务模块。

## 什么是Spring Cloud

[Spring Cloud](https://projects.spring.io/spring-cloud/)是一个基于Spring Boot实现的微服务架构开发工具。它为微服务架构中涉及的配置管理、服务治理、断路器、智能路由、微代理、控制总线、全局锁、决策竞选、分布式会话和集群状态管理等操作提供了一种简单的开发方式。Spring Cloud的诞生并不是为了解决微服务中的某一个问题，而是提供了一套解决微服务架构实施的综合性解决方案。

Spring Cloud是一个由各个独立项目组成的综合项目，每个独立项目有着不同的发布节奏，为了管理每个版本的子项目清单，避免Spring Cloud的版本号与其子项目的版本号相混淆，没有采用版本号的方式，而是通过命名的方式。这些版本的名字采用了伦敦地铁站的名字，根据字母表的顺序来对应版本时间顺序。比如”Angel”是Spring Cloud的第一个发行版名称, “Brixton”是Spring Cloud的第二个发行版名称。当一个版本的Spring Cloud项目的发布内容积累到临界点或者一个严重bug解决可用后，就会发布一个”service releases”版本，简称SRX版本，其中X是一个递增的数字，所以Brixton.SR5就是Brixton的第5个Release版本。

截至2018年4月02日，Spring Cloud已经发布了代号为Finchley的快照版本，采用的Spring Boot版本为2.0.1.RELEASE。Spring Cloud的版本和Spring Boot的版本关系可以查看官网给的例子。

以下是Spring Cloud版本与各个独立项目版本对应关系表：

| Component                 | Edgware.SR3    | Finchley.RC1     | Finchley.BUILD-SNAPSHOT |
| :------------------------ | :------------- | :--------------- | :---------------------- |
| spring-cloud-aws          | 1.2.2.RELEASE  | 2.0.0.RC1        | 2.0.0.BUILD-SNAPSHOT    |
| spring-cloud-bus          | 1.3.2.RELEASE  | 2.0.0.RC1        | 2.0.0.BUILD-SNAPSHOT    |
| spring-cloud-cli          | 1.4.1.RELEASE  | 2.0.0.RC1        | 2.0.0.BUILD-SNAPSHOT    |
| spring-cloud-commons      | 1.3.3.RELEASE  | 2.0.0.RC1        | 2.0.0.BUILD-SNAPSHOT    |
| spring-cloud-contract     | 1.2.4.RELEASE  | 2.0.0.RC1        | 2.0.0.BUILD-SNAPSHOT    |
| spring-cloud-config       | 1.4.3.RELEASE  | 2.0.0.RC1        | 2.0.0.BUILD-SNAPSHOT    |
| spring-cloud-netflix      | 1.4.4.RELEASE  | 2.0.0.RC1        | 2.0.0.BUILD-SNAPSHOT    |
| spring-cloud-security     | 1.2.2.RELEASE  | 2.0.0.RC1        | 2.0.0.BUILD-SNAPSHOT    |
| spring-cloud-cloudfoundry | 1.1.1.RELEASE  | 2.0.0.RC1        | 2.0.0.BUILD-SNAPSHOT    |
| spring-cloud-consul       | 1.3.3.RELEASE  | 2.0.0.RC1        | 2.0.0.BUILD-SNAPSHOT    |
| spring-cloud-sleuth       | 1.3.3.RELEASE  | 2.0.0.RC1        | 2.0.0.BUILD-SNAPSHOT    |
| spring-cloud-stream       | Ditmars.SR3    | Elmhurst.RELEASE | Elmhurst.BUILD-SNAPSHOT |
| spring-cloud-zookeeper    | 1.2.1.RELEASE  | 2.0.0.RC1        | 2.0.0.BUILD-SNAPSHOT    |
| spring-boot               | 1.5.10.RELEASE | 2.0.1.RELEASE    | 2.0.0.BUILD-SNAPSHOT    |
| spring-cloud-task         | 1.2.2.RELEASE  | 2.0.0.RC1        | 2.0.0.RELEASE           |
| spring-cloud-vault        | 1.1.0.RELEASE  | 2.0.0.RC1        | 2.0.0.BUILD-SNAPSHOT    |
| spring-cloud-gateway      | 1.0.1.RELEASE  | 2.0.0.RC1        | 2.0.0.BUILD-SNAPSHOT    |
| spring-cloud-openfeign    |                | 2.0.0.RC1        | 2.0.0.BUILD-SNAPSHOT    |

Finchley使用Spring Boot 2.0.x构建，不建议与Spring Boot 1.5.x一起使用。

Dalston和Edgware发行版建立在Spring Boot 1.5.x之上，不建议与Spring Boot 2.0.x一起使用。

之后博文的例子将采用[Spring Cloud Edgware SR3](http://cloud.spring.io/spring-cloud-static/Edgware.SR3/multi/multi_spring-cloud.html)版本，对应的Spring Boot版本为1.5.13.RELEASE。