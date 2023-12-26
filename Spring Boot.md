# Spring Boot

##                1. 谈谈你对SpringBoot的理解

​        本质:Spring Boot就是Spring,做了那些没有它你自己也会去做的Spring Bean配置

​        理念"习惯优于配置" 让你的项目快速运行 使用Spring Boot可以不用或者只需要很少的Spring配置
​简言之:Spring Boot本身并不提供Spring的核心功能,而是作为Spring的脚手架框架,以达到快速构建项目、预置三方配置、开箱即用的目的
​优点:1.快速构建项目
​         2.对主流开发框架的无配置集成
​	     3.项目可独立运行,无需外部依赖Servlet容器
​	     4.提供运行时的应用监控
​	     5.可以极大地提高开发、部署效率
​	     6.与云计算天然集成

​                            

##                  2.Spring Boot Starter有什么用?

​      提供众多起步依赖降低项目依赖的复杂度。

​      本质是一个Maven项目对象模型(Project Object Model),定义了对其他库的传递依赖,这些东西加在一起即支持某项功能,很多起步依赖的命名都暗示了它们提供的某种或某种功能



##                  3. 介绍Spring Boot的启动流程

1.SpringBoot项目创建创建完成会默认生成一个名为*Application的入口类,我们是通过该类的main方法启动Spring Boot项目的

2.在main方法中.通过SpringApplication的静态方法,即run方法进行SpringApplication类的实例化操作

3.再针对实例化对象调用另一个run方法来完成整个项目的初始化和启动

SpringApplication调用run方法的大致流

​     1.获取SpringApplicationListener监听器
​     2.启动所获取到的所有监听器
​     3.初始化ConfigurableEnvironment
​     4.打印Banner图标
​     5.创建ConfigurableApplicationContext
​     6.准备容器ConfigurableApplicationContext
​     7.初始化容器ConfigurableApplicationContext
​     8.监听器通知容器启动完成
​     9.监听器通知容器正在运行

SpringApplication在run方法中重点做了以下操作:
​     获取监听器和参数配置
​     打印Banner信息
​     创建并初始化容器
​     监听器发送通知

run方法运行过程中还涉及启动时长统计、异常报告、启动日志、异常处理等辅助操作。



##                   4.SpringBoot是如何导入包的?

​      通过Spring Boot Starter导入包
​      其余同2



##                   5.请描述Spring Boot自动装配的过程

​     1.通过@EnableAutoConfiguration注解开启自动配置,加载spring.factories中注册的各类AutoConfiguration类

​     2.当某个AutoConfiguration类满足其注解@Conditional指定的生效条件时,实例化该AutoConfiguration类中定义的Bean(组件等),并注入Spring容器,就可以完成依赖框架的自动配置

​                                   

##                    6.说说你对SpringBoot注解的了解

@SpringBootApplication:

​     入口类中,唯一的一个注解,Spring Boot项目的核心注解,用于开启自动配置,准确说是通过该注解内组合的@EnableAutoConfiguration开启了自动配置

@EnableAutoConfiguration:

​        主要功能是启动Spring应用程序上下文时进行自动配置,它会尝试猜测并配置项目可能需要的Bean。自动配置通常是基于项目classpath中引入的类和已定义的Bean来实现的。

@Import:

​       @EnableAutoConfiguration的关键功能是通过@Import注解导入的ImportSelector来完成的@Conditional:

​       Spring4.0版本引入的新特性,可根据是否满足指定的条件来决定是否进行Bean的实例化及配置。即根据一些特定条件来控制Bean实例化的行为

​     @Conditional衍生注解:

​     @ConditionalOnBean:在容器中有指定Bean的条件下

​     @ConditionalOnClass:在classpath类路径下有指定类的条件下

​     @ConditionalOnCloudPlatform:在指定的云平台处于active状态时

​     @ConditionalOnExpression:基于SpEL表达式的条件判断

​     @ConditionalOnJava:基于JVM版本作为判断条件
​     @ConditionalOnJndi:在JNDI存在的条件下查找指定的位置

​     @ConditionalOnMissingBean:当容器里没有指定Bean的条件时

​     @ConditionalOnMissingClass:当类路径下没有指定类的条件时

​     @ConditionalOnNotWebApplication:在项目不是一个Web项目的条件下

​     @ConditionalOnProperty:在指定的属性有指定值的条件下

​     @ConditionalOnResource:类路径是否有指定的值

​     @ConditionalOnSingleCandidate:当指定的Bean在容器中只有一个或者有多个但是制定了首选的Bean时

​     @ConditionalOnWebApplication:在项目是一个Web项目的条件下