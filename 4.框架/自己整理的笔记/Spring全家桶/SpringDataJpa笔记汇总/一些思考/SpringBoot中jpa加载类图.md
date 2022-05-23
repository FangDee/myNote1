SpringBoot中jpa加载类图 ![img](http://www.jackzhang.cn/spring-data-jpa-guide/images/hibernate_bootstrap/1.png)

##### 来看一下如何加载Hibernate jpa:

![img](http://www.jackzhang.cn/spring-data-jpa-guide/images/hibernate_bootstrap/2.png) ![img](http://www.jackzhang.cn/spring-data-jpa-guide/images/hibernate_bootstrap/3.png) ![img](http://www.jackzhang.cn/spring-data-jpa-guide/images/hibernate_bootstrap/4.png)

##### 直接看出来有哪些配置

![img](http://www.jackzhang.cn/spring-data-jpa-guide/images/hibernate_bootstrap/5.png) ![img](http://www.jackzhang.cn/spring-data-jpa-guide/images/hibernate_bootstrap/6.png)

##### 加载Hibernate的过程：

![img](http://www.jackzhang.cn/spring-data-jpa-guide/images/hibernate_bootstrap/7.png) ![img](http://www.jackzhang.cn/spring-data-jpa-guide/images/hibernate_bootstrap/8.png) ![img](http://www.jackzhang.cn/spring-data-jpa-guide/images/hibernate_bootstrap/9.png) ![img](http://www.jackzhang.cn/spring-data-jpa-guide/images/hibernate_bootstrap/10.png) ![img](http://www.jackzhang.cn/spring-data-jpa-guide/images/hibernate_bootstrap/11.png) ![img](http://www.jackzhang.cn/spring-data-jpa-guide/images/hibernate_bootstrap/12.png) ![img](http://www.jackzhang.cn/spring-data-jpa-guide/images/hibernate_bootstrap/13.png) ![img](http://www.jackzhang.cn/spring-data-jpa-guide/images/hibernate_bootstrap/14.png) ![img](http://www.jackzhang.cn/spring-data-jpa-guide/images/hibernate_bootstrap/15.png)

##### 启动成功之后通过@PersistenceUnit和@@PersistenceContext可以获得想要的EntityManager

![img](http://www.jackzhang.cn/spring-data-jpa-guide/images/hibernate_bootstrap/16.png) ![img](http://www.jackzhang.cn/spring-data-jpa-guide/images/hibernate_bootstrap/17.png)