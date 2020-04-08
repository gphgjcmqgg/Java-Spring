# Java-Spring

## Spring是什么

Spring是另一个主流的 Java Web 开发框架，是一个开源的轻量级的 Java 开发框架， 具有控制反转（IoC）和面向切面（AOP）两大核心。Java Spring 框架通过声明式方式灵活地进行事务的管理，提高开发效率和质量。

在实际开发中，通常服务器端采用三层体系架构，分别为表现层（web）、业务逻辑层（service）、持久层（dao）。

## Spring优点

Spring 框架的主要优点具体如下:

1. 方便解耦，简化开发
2. 方便集成各种优秀框架
3. 降低 Java EE API 的使用难度
4. 方便程序的测试
5. AOP 编程的支持
6. 声明式事务的支持

## Spring体系结构

Spring 框架采用分层架构，根据不同的功能被划分成了多个模块，这些模块大体可分为 Data Access/Integration、Web、AOP、Aspects、Messaging、Instrumentation、Core Container 和 Test

1. Data Access/Integration（数据访问／集成）
2. Web 模块
3. Core Container（核心容器）
4. 其他模块

## Spring目录结构和基础JAR包

https://repo.spring.io/simple/libs-release-local/org/springframework/spring/

 Spring 框架压缩包解压文件的目录结构

1. docs	    包含 Spring 的 API 文档和开发规范
2. libs	    包含开发需要的 JAR 包和源码包
3. schema	  包含开发所需要的 schema 文件，在这些文件中定义了 Spring 

在 libs 目录中，包含了 Spring 框架提供的所有 JAR 文件，其中有四个 JAR 文件是 Spring 框架的基础包，分别对应 Spring 容器的四个模块

1. spring-core-3.2.13.RELEASE.jar
  包含 Spring 框架基本的核心工具类，Spring 其他组件都要用到这个包中的类，是其他组件的基本核心。
2. spring-beans-3.2.13.RELEASE.jar
  所有应用都要用到的，它包含访问配置文件、创建和管理 bean 以及进行 Inversion of Control（IoC）或者 Dependency Injection（DI）操作相关的所有类。
3. spring-context-3.2.13.RELEASE.jar
  Spring 提供在基础 IoC 功能上的扩展服务，此外还提供许多企业级服务的支持，如邮件服务、任务调度、JNDI 定位、EJB 集成、远程访问、缓存以及各种视图层框架的封装等
4. spring-expression-3.2.13.RELEASE.jar
  定义了 Spring 的表达式语言。需要注意的是，在使用 Spring 开发时，除了 Spring 自带的 JAR 包以外，还需要一个第三方 JAR 包 commons.logging 处理日志信息

http://commons.apache.org/proper/commons-logging/download_logging.cgi

使用 Spring 框架时，只需将 Spring 的四个基础包以及 commons-logging-1.2.jar 包复制到项目的 lib 目录，并发布到类路径中即可。

## Spring IoC容器

IoC 是指在程序开发中，实例的创建不再由调用者管理，而是由 Spring 容器创建。Spring 容器会负责控制程序之间的关系，而不是由程序代码直接控制，因此，控制权由程序代码转移到了 Spring 容器中，控制权发生了反转，这就是 Spring 的 IoC 思想。

Spring 提供了两种 IoC 容器，分别为 BeanFactory 和 ApplicationContext

### BeanFactory

BeanFactory 是基础类型的 IoC 容器，它由 org.springframework.beans.facytory.BeanFactory 接口定义，并提供了完整的 IoC 服务支持。简单来说，BeanFactory 就是一个管理 Bean 的工厂，它主要负责初始化各种 Bean，并调用它们的生命周期方法。

BeanFactory 接口有多个实现类，最常见的是 org.springframework.beans.factory.xml.XmlBeanFactory，它是根据 XML 配置文件中的定义装配 Bean 的。
创建 BeanFactory 实例时，需要提供 Spring 所管理容器的详细配置信息，这些信息通常采用 XML 文件形式管理。
BeanFactory beanFactory = new XmlBeanFactory(new FileSystemResource("D://applicationContext.xml"));

### ApplicationContext

ApplicationContext 是 BeanFactory 的子接口，也被称为应用上下文。该接口的全路径为 org.springframework.context.ApplicationContext，它不仅提供了 BeanFactory 的所有功能，还添加了对 i18n（国际化）、资源访问、事件传播等方面的良好支持。

ApplicationContext 接口有两个常用的实现类，具体如下。
1）ClassPathXmlApplicationContext
该类从类路径 ClassPath 中寻找指定的 XML 配置文件，找到并装载完成 ApplicationContext 的实例化工作，具体如下所示。
ApplicationContext applicationContext = new ClassPathXmlApplicationContext(String configLocation);

在上述代码中，configLocation 参数用于指定 Spring 配置文件的名称和位置，如 applicationContext.xml。

2）FileSystemXmlApplicationContext
该类从指定的文件系统路径中寻找指定的 XML 配置文件，找到并装载完成 ApplicationContext 的实例化工作，具体如下所示。
ApplicationContext applicationContext = new FileSystemXmlApplicationContext(String configLocation);

它与 ClassPathXmlApplicationContext 的区别是：在读取 Spring 的配置文件时，FileSystemXmlApplicationContext 不再从类路径中读取配置文件，而是通过参数指定配置文件的位置，它可以获取类路径之外的资源，如“F：/workspaces/applicationContext.xml”。

在使用 Spring 框架时，可以通过实例化其中任何一个类创建 Spring 的 ApplicationContext 容器。

需要注意的是，BeanFactory 和 ApplicationContext 都是通过 XML 配置文件加载 Bean 的。

二者的主要区别在于，如果 Bean 的某一个属性没有注入，则使用 BeanFacotry 加载后，在第一次调用 getBean() 方法时会抛出异常，而 ApplicationContext 则在初始化时自检，这样有利于检查所依赖的属性是否注入。
因此，在实际开发中，通常都选择使用 ApplicationContext，而只有在系统资源较少时，才考虑使用 BeanFactory
