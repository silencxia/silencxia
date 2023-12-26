# Spring

##              1.请你说说Spring的核心是什么?

​         Spring包括Core、Testing、Data Access、Web Servlet等模块，其中Core模式是Spring的核心模块，Core模块提供Ioc容器、AOP功能、数据绑定、类型转换等一系列的基础功能,这些功能以及其他模块的功能都是建立在IoC和AOP之上的,所以IoC和AOP是Spring框架的核心

​        IoC:控制反转,是一种面向对象编程的设计思想,可以帮我们维护对象与对象之间的依赖关系,降低对象之间的耦合度，不采用这种关系,我们需要自己维护对象与对象之间的依赖关系,很容易造成对象之间耦合度过高,不利于代码维护。

​        DI:依赖注入,IoC的实现方式IoC是通过DI来实现的,IoC比较抽象而DI比较直观,习惯将IoC和DI画上等号,实现依赖注入的关键是IoC容器,本质是一个工厂

​         AOP:面向切面编程思想,是多对OOP的补充,在OOP的基础上进一步提高编程的效率,可以统一解决一批组件的共性需求。在AOP思想下,我们可以将解决共性需求的代码独立出来,然后通过配置的方式,声明这些代码在什么地方,什么时机调用。当满足调用条件时,AOP会将该业务代码织入到我们指定的位置,从而统一解决了问题,又不需要修改这一批组件的代码。



## 2.说一说你对Spring容器的了解

主要提供了两种类型的容器:BeanFactory和ApplicationContext

BeanFactory:

​       基础类型的IoC容器,提供完整的IoC服务支持。默认采用延迟初始化策略。只有当客户端对象需要访问容器中的某个受管对象的时候，才对该受管对象进行初始化以及依赖注入操作。
​       相对来说,容器启动初期速度较快,所需要的资源有限,适用于资源有限,并且功能要求不是很严格的场景。
ApplicationContext:

​       是在BeanFactory的基础上构建的,是相对比较高级的容器实现,拥有BeanFactory的所有支持,还提供了其他高级特性,如事件发布、国际化信息支持等。ApplicationContext所管理的对象，在该类型容器启动之后,默认全部初始化并绑定完成。
​        相对来说，要求更多的系统资源，启动时间也会长一些，适用于系统资源充足，并且要求更多功能的场景中。



## 3.说一说你对BeanFactory的了解

​        BeanFactory是一个类工厂,与传统工厂相比,是类的通用工厂,可以创建并管理各种类的对象。Spring称这些可被创建和管理的Java对象为Bean。
​        Spring中的Bean比JavaBean更为宽泛,所有可以被Spring容器实例化并管理的Java类都可以成为Bean

​         BeanFactory是Spring容器的顶层接口,Spring为BeanFactory提供了多种实现,XMLBeanFactory在Spring3.2已被废弃,建议使用XMLBeanDefinitionReader,DefaultListableBeanFactory替代.BeanFactory最主要的方法是getBean(String beanName),该方法从容器中返回特定名称的Bean



## 4.说一说你对Spring IOC的理解

同1中的IoC和DI
具体实现中，主要有三种注入方式:
     1.构造方法注入
	      被注入对象可以在它的构造方法中声明依赖对象的参数列表,让外部知道它需要哪些依赖对象。
	      构造方法注入方式比较直观,对象被构建完成后，即进入就绪状态,可以马上使用。
     2.setter方法注入
          通过setter方法，更改相应的对象属性。
          当前对象只要为其依赖对象所对应的属性添加setter方法,就可以通过setter方法将对应的。
          依赖对象设置到被注入对象中。相对构造方法注入更宽松一些。
     3.接口注入
   	   被注入对象如果想要IoC Service Provider为其注入依赖对象,就必须实现某个接口。
          IoC Service Provider最终通过这些接口来了解应该为被注入对象注入什么依赖对象。
          相对于前两种，比较死板和烦琐。

结:构造方法注入和setter方法注入  

​     侵入性较弱,易于理解和使用,是现在使用最多的注入方式

​     接口注入         侵入性较强,不流行



## 5.Spring是如何管理Bean的?

​        通过IoC容器来管理Bean,通过XML配置或者注解配置,注解配置比XML配置方便许多,所以大多时候用注解配置的方式

常用注解:
        @ComponentScan 声明扫描策略,通过声明,容器知道扫描哪些包下带有声明的类,也可以知道哪些特定的类是被排除在外的

​         @Component、@Repository、@Service、@Controller用于声明Bean
​         @Component 声明通用的Bean
​         @Repository 声明DAO层的Bean
​         @Service 声明业务层的Bean
​         @Controller 声明视图层的控制器Bean

​         @Autowired、@Qualifier用于注入Bean,即告诉容器应该为当前属性注入哪个Bean
​         @Autowired按照Bean的类型进行匹配的, 如果这个属性的类型具有多个Bean,就可以通过@Qualifier指定Bean的名称,以消除歧义
​         @Scope用于声明Bean的作用域,默认情况下Bean是单例的,即在整个容器中这个类型只有一个实例,可以通过Scope注解指定prototype值将其声明为多例的,也可以将Bean声明为session级作用域、request级作用域。最常用的还是默认的单例模式。
​         @PostConstruct、@PreDestroy声明Bean的生命周期,被@PostConstruct修饰的方法将在Bean实例化后被调用
​         @PreDestroy修饰的方法将在容器销毁前被调用



## 6.介绍Bean的作用域

 默认情况下,Bean在Spring容器中是单例的,通过@Scope属性修改作用域
 singleton:在Spring容器中仅存在一个实例,即Bean以单例的形式存在
 prototype:每次调用getBean()时,都会执行new操作,返回一个新的实例
 request:每次HTTP请求都会创建一个新的Bean
 session:同一个HTTP Session共享一个Bean,不同的HTTP Session使用不同的Bean
 globalSession:同一个全局的Session共享一个Bean,一般用于Protlet环境



## 7.说一说Bean的生命周期

 1.初始化
 2.依赖注入
 3.setBeanName方法(接口BeanNameAware)
 4.setBeanFactory方法(接口BeanFactoryAware)
 5.setApplicationContext方法(接口ApplicationContextAware)
 6.postProcessBeforeInitialization方法(BeanPostProcessor的预初始化方法,针对全部Bean生效)
 7.自定义初始化方法(@PostConstruct标注方法)
 8.afterPropertiesSet方法(接口InitializingBean)
 9.postProcessAfterInitialization方法(BeanPostProcessor的后初始化方法,针对全部Bean生效)
 10.生存期
 11.自定义销毁方法(@PreDestroy标注方法)
 12.destroy方法(接口DisposableBean)

 过程由Spring容器自动管理,有两个环节我们可以进行干预
     1.自定义初始化方法,并在该方法前增加@PostConstruct注解,会在调用setBeanFactory方法之后调用该方法
     2.自定义销毁方法,并在该方法前增加@PreDestroy注解,在Spring容器销毁前调用该方法



## 8.Spring是如何解决循环依赖的?

 Spring对循环依赖的处理有三种情况:
      1.构造器的循环依赖:这种依赖是处理不了的,直接抛出BeanCurrentlyInCreationException异常
      2.单例模式下的setter循环依赖:通过“三级缓存"处理循环依赖
      3.非单例循环依赖:无法处理

 Spring单例对象的初始化大略分为三步:
      1.createBeanInstance:实例化,即调用对象的构造方法实例化对象
      2.populateBean:填充属性,这一步主要是多bean的依赖属性进行填充
      3.initializeBean:调用spring xml中的init方法,Spring解决循环依赖的诀窍就在于singletonFactories这个cache, 这个cache的类型是ObjectFactory



## 9.@Autowired和@Resource注解有什么区别?

1.@Autowired是Spring提供的注解,@Resource是JDK提供的注解
2.@Autowired是只能按类型注入,@Resource默认按名称注入,也支持按类型注入u
3.@Autowired按类型装配依赖对象,默认情况下依赖对象必须存在,允许为null值时,设置required属性为false,如果想要按名称装配,结合@Qualifier注解一起使用

@Resource:name和type
    name属性指定byName.如果没有指定name属性,当注解标注在字段上。
    默认取属性名作为bean名称寻找依赖对象,当注解标注在属性的setter方法上,即默认取属性名作为bean名称寻找依赖对象bean名称寻找依赖对象



## 10.Spring中默认提供的单例是线程安全的吗?

​        不是,Spring容器本身并没有提供Bean的线程安全策略.
​        如果单例的Bean是一个无状态的Bean,即线程中的操作不会对Bean的成员执行查询以外的操作,那么这个单例的Bean是线程安全的。如果单例的Bean是一个有状态的Bean,则可以采用ThreadLocal对状态数据做线程隔离,来保证线程安全。



## 11.说一说你对Spring AOP的理解

AOP 面相切面编程,对OOP的一种补充
​面相对象编程将程序抽象成各个层次的对象,面向切面编程是将程序抽象成各个切面。
​切面:相当于应用对象间的横切点,我们可以将其单独抽象成单独的模块。
​AOP术语:
​        连接点(join point):对应的是具体被拦截的对象,因为因为Spring只能支持方法,所以被拦截的对象往往就是指特定的方法,AOP将通过动态代理技术把它织入对应的流程中。  
​        切点(point cut):切面可能应用于多个类的不同方法，这时,可以通过正则式和指示器的规则去定义,从而适配连接点。
​        通知(advice):就是按照约定的流程下的方法,分为前置通知、后置通知、环绕通知、事后返回通知和异常通知,它会根据约定织入流程中。
​        目标对象(target):即被代理对象。
​        引入(introduction):是指引入新的类和其方法,增强现有Bean的功能。
​        织入(weaving):通过动态代理技术,为原有服务对象生成代理对象,然后将与切点定义匹配的连接点拦截,并按约定将各类通知织入约定流程的过程。
​        切面(aspect):是一个可以定义切点、各类通知和引入的内容。

Spring AOP:
​Spring AOP支持如下两种实现方式:
​       JDK动态代理:默认方式,Java提供的的动态代理技术,可以在运行时创建接口的代理实例
​       CGLib动态代理:采用底层的字节码技术,在运行时创建子类代理的实例。当目标对象不存在接口时,采用这种方式。



## 12.请你说说AOP的应用场景

​    1.应用可以直接使用AOP的功能,设计应用的横切关注点,把跨越应用程序多个模块的功能抽象出来,并通过简单的AOP的使用,灵活地编制到模块中
​     2.一些支持模块通过Spring AOP来实现,如事务处理



## 13.Spring AOP不能对哪些类进行增强?

   1.不受容器管理的对象(只能对IoC容器中的Bean进行增强)
   2.不能对final修饰的类进行代理(采用动态创建子类的方式生成代理对象)



## 14.JDK动态代理和CGLib有什么区别?

​     同11Spring AOP



## 15.既然没有接口都可以使用CGLib,为什么Spring还要使用JDK动态代理?

​	 性能:CGLib创建的代理对象比JDK动态代理创建的对象高很多
​	 花费时间:CGLIB比JDK多
​	 适用对象:CGLib动态代理,单例的对象(无需频繁创建代理对象)
​	                  JDK动态代理,多例的对象(需要频繁地创建代理对象)



## 16.Spring如何管理事务?

​    Spring为事务管理提供了一致的编程模板,在高层次上建立了同意的事务抽象, 让用户以同意的编程模型进行事务管理

​    Spring支持两种事务编程模型:
​       1).编程式事务
​           利用TransactionTemplate模板,通过编程的方式实现事务管理,而无需关注资源获取、复用、释放、事务同步及异常处理等操作。
​          优缺点:相对麻烦,更灵活,范围可以控制得更精确
​        2).声明式事务
​          Spring事务管理的亮点,通过声明的方式,在IoC配置中指定事务的边界和事务属性,
​          Spring会自动在指定的事务边界上应用事务属性。
​          优缺点:方便



## 17.Spring的事务传播方式有哪些?

​    PROPAGATION_REQUIRED:如果没有这个事务,则新建一个事务;如果已存在一个事务,则加入到这个事务中,这是最常见的选择。
​    PROPAGATION_SUPPORTS:支持当前事务,如果没有这个事务,则以非事务方式执行。
​    PROPAGATION_MANDATORY:使用当前的事务,如果当前没有事务,则抛出异常。
​    PROPAGATION_REQUIRES_NEW:新建事务,如果当前存在事务,则把当前事务挂起。   
​    PROPAGATION_NOT_SUPPORTED:以非事务方式执行操作,如果当前存在事务,则把当前事务挂起。
​    PROPAGATION_NEVER:以非事务方式执行操作,如果当前存在事务,则抛出异常。
​    PROPAGATION_NESTED:当前存在事务,则在嵌套事务内执行;如果当前没有事务,则执行与PROPAGATION_REQUIRED类似的操作。



## 18.Spring的事务如何配置,常用注解有哪些?

​     事务的打开、回滚和提交是由事务管理器来完成的,我们使用不同的数据库访问框架,就要使用与之对应的事务管理器.
​     Spring Boot中,添加起步依赖后,自动实例化正确的事务管理器。
​     对于声明式事务,使用@Transactional进行标注,这个标注可以标注在类或者方法上。
​               标注在类上,代表这个类所有公共非静态方法都将启用事务功能。
​               标注在方法上,代表这个方法将启用事务功能。
​     使用isolation属性声明事务的隔离级别,使用propagation属性声明事务的传播机制



## 19.说一说你对声明式事务的理解

​      同16第2点	

