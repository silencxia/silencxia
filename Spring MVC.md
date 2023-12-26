# Spring MVC

## 1.什么是MVC?

​        MVC是一种设计模式,即Model(模型),View(试图),Controller(控制器),Model代表数据,View代表用户界面,Controller代表数据的处理逻辑,是Model和View之间的桥梁。将软件进行分层,可以降低对象间的耦合,便于代码的维护



## 2.DAO层是做什么的？

​       DAO数据访问对象,专门用于访问数据库,实现技术有很多,常用的有Spring JDBC,Hibernate,JPA,MyBatis等。在Spring框架下访问数据库,编程模式是统一的。



## 3.介绍一下Spring MVC的执行流程

1.客户端发出一个HTTP请求,Web应用服务器接收到这个请求,如果匹配DispatcherServlet的请求映射路径,则Web容器将请求转发给DispatcherServlet处理。
​2.DispatcherServlet接收到这个请求后,将根据请求的信息及HandlerMapping的配置找到处理请求的处理器(handler)。

3.当DispatcherServlet根据HandlerMapping得到对应当前请求的Handler后,通过HandlerAdapter对Handler进行封装,再以统一的适配器接口调用Handler。HandlerAdapter是Spring MVC框架级接口.是一个适配器，它用统一的接口对各种Handler方法进行调用。
​4.处理器完成业务逻辑的处理后,将返回一个ModelAndView给DispatcherServlet,ModelAndView包含了视图逻辑名和模型数据信息。
​5.DispatcherServlet借由ViewResolver完成逻辑视图名到真实视图对象的解析工作。
​6.当得到真实的视图对象View后，DispatcherServlet就使用这个View对象对ModelAndView中的模型数据进行视图渲染。
​7.最终客户端得到的响应消息。



## 4.说一说你知道的Spring MVC注解

@RequestMapping:

​       用来处理请求地址映射的,即将其中的处理器方法映射到url路径上。
​属性:
​       method:指定请求的method类型,如get、post
​       value:请求的实际地址,多个地址用{}
​       produces:指定返回的内容类型
​       consumes:指定处理请求的提交内容类型
​       headers:指定request中必须包含哪些的header值时,使用该方法处理请求
​       params:指定request中一定要有的参数值,使用该方法处理请求

@RequestParam:
​       将请求参数绑定到你的控制器的方法参数上,是Spring MVC中接收普通参数的注解
​属性:
​       value是请求参数中的名称
​       required是请求参数是否必须提供参数,默认是true,意思是表示必须提供

@RequestBody:
​       作用在方法上,表示该方法的返回结果是直接按写入的Http responsebody中
​属性:

​       required,是否必须有请求体.默认值为true,使用注解时,为true时get的请求方式是报错的。为false时,get的请求为null

@PathVariable:
​       用于绑定url中的占位符,是Spring MVC支持rest风格url的一个重要标志



## 5.介绍一下Spring MVC的拦截器

​	   拦截器对处理器进行拦截,通过拦截器增强处理器功能。
​	   Spring MVC中所有拦截器都实现HandlerInterceptor接口,该接口包括preHandle()、postHandle()、afterCompletion()方法

拦截器执行流程:
​	    1.执行preHandle方法,它会返回一个布尔值。如果为false,则结束所有流程,如果为true,则执行下一步
​        2.执行处理器逻辑,它包含控制器的功能
​        3.执行postHandle方法
​        4.执行视图解析和视图渲染
​        5/执行afterCompletion方法

Spring MVC拦截器的开发步骤如下:
​          开发拦截器:实现handlerInterceptor接口,从三个方法中选择合适的方法,实现拦截时要执行的具体业务逻辑。
​          注册拦截器:定义配置类,并让它实现WebMvcConfigurer接口,在接口的addInterceptors方法中,注册拦截器,并定义该拦截器匹配哪些请求路径。



##    6.怎么去做请求拦截?

​       对Controller记性拦截,则可以使用Spring MVC的拦截器
​       对所有的请求进行来接,则可以使用Filter
​       对除了Controller之外的其他Bean的请求进行拦截,则可以使用Spring AOP