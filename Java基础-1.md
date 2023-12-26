# Java基础-1

##          1.JDK和JRE有什么区别?

​        JDK:Java Development Kit,Java开发包工具,提供Java开发环境和运行环境

​        JRE:Java Runtime Environment,Java运行环境,为Java运行提供所需环境

​        具体:JDK包含了JRE,包含了编译Java源码的编译器Javac,包含了很多Java程序调试和分析的工具

​        简单:运行Java程序,只需安装JRE,编写Java程序,需要安装JDK

   

##           2. ==和equals的区别？

 ==:

​      基本类型:比较的是值是否相同

​      引用类型:比较的是引用是否相同

equals本质上就是==

equals:

​      默认情况下是引用比较,很多类重新写了equals方法,如String、Integer等把它变成了值比较

​      一般情况下 equals比较的是值是否相等

​                  

##           3.两个对象的hashCode()相同,则equals()也一定为true,对吗?

​       在散列表中,hashCode()相等即两个键值对的哈希值相等,然而哈希值相等,不一定能得出键值对相等.

​                   

##           4.final在Java中有什么作用?

​       final修饰的类叫最终类,该类不能被继承

​       final修饰的方法不能被重写

​       final修饰的变量叫常量,常量必须初始化,初始化之后值就不能被修改

​                    

##           5.Java中的Math.round(-1.5)等于多少? 

​       -1,在数轴上取值时,中间值(0.5)向右取整,所以正0.5是往上取整,负0.5是直接舍弃.



## 6.String属于基础的数据类型吗?

​       String属于对象,基础类型有8种:byte,boolean,char,short,int,float,long,double



##           7.Java中操作字符串都有哪些类?它们之间有什么区别?

​       操作字符串的类有:String,StringBuffer,StringBuilder

​       String声明的是不可变的对象,每次操作都会生成新的String对象,然后将指针指向新的String对象,而StringBuffer、StringBuilder可以在原有基础上进行操作,在经常改变字符串内容的情况下最好不要使用String。

StringBuffer和StringBuilder的区别:

​       StringBuffer是线程安全的,StringBuilder是非线程安全的,但StringBuilder的性能却高StringBuffer,所以在单线程环境下推荐使用StringaBuilder,多线程环境下推荐使用StringBuffer。

​                   

##            8.String str="i"与String str=new String("i")一样吗?

​       不一样,内存的分配方式不一样。String str="i"的方式,Java虚拟机会将其分配到常量池中;而String str = new String("i")则会被分到堆内存中。

​                    

##            9.如何将字符串反转?

​        StringBuffer或者StringBuilder的reverse()方法。



##            10.String类的常用方法都有哪些?

​         indexOf():返回指定字符的索引

​         charAt():返回指定索引处的字符
​         replace():字符串替换

​         trim():取出字符串两端空白

​         split():分割字符串,返回一个分割后的字符串数组

​         getBytes():返回字符串的byte类型数组

​         length():返回字符串长度
​         toLowerCase():将字符串转成小写字母

​         toUpperCase():将字符串转成大写字母

​         substring():截取字符串

​         equals():字符串比较



##             11.抽象类必须要有抽象方法吗

抽象类不一定要有抽象方法.

如:

public static void sayHi(){

​              System.out.println("hi~~~");

​              System.out.println("hi");

 }

上面代码,抽象类并没有抽象方法但完全可以正常运行。

​                        

##               12.普通类和抽象类有哪些区别？

​          普通类不能包含抽象方法,抽象类可以包含抽象方法

​          抽象类不能直接实例化,普通类可以直接实例化

​        

##               13.抽象类能使用final修饰吗?

​         不能,定义抽象类就是让其他类继承的,如果定义为final,该类就不能被继承,这样彼此会产生矛盾。

​                           

##                14.接口和抽象类有什么区别?

​          实现:抽象类的子类使用extends来继承,接口必须使用implements来实现接口.

​          构造函数:抽象类可以有构造方法,接口不能有

​           实现数量:类可以实现很多个接口,但是只能继承一个抽象类

​           访问修饰符:接口中的方法默认使用public修饰;抽象类中的方法可以是任意访问修饰符。

​                                

##                  15.Java中IO流分为几种?

​           功能:输入流(input)、输出流(output)

​           类型:字节流和字符流

​           字节流和字符流的区别:字节流 8位以字节为单位传输

​                                                   字符流16位以字符为单位传输

​                                 

##                   16.BIO、NIO、AIO有什么区别?

​         BIO:Block IO同步阻塞式IO,平常使用的传统IO,

​         特点:模式简单使用方便,并发处理能力低

​         NIO:Non IO同步非阻塞IO,传统IO的升级

​         特点:客户端和服务器通过Channel(通道)通讯,实现了多路复用

​         AIO:Asynchronous IO是NIO的升级,也叫NIO2

​         特点:实现了异步非阻塞IO,异步IO的操作基于时间和回调机制



##                    17.Files的常用方法都有哪些?

​       Files.exists():检测文件路径是否存在

​       Files.createFile():创建文件

​       Files.createDirectory():创建文件夹

​       Files.delete():删除一个文件或目录
​       Files.copy():复制文件

​       Files.move():移动文件
​       Files.size():查看文件个数

​       Files.read():读取文件

​       Files.write():写入文件

​                

​                    

​       



  