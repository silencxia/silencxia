# MyBatis

## 1.谈谈MyBatis和JPA的区别

ORM映射不同:
​       MyBatis是半自动的ORM框架,提供数据库与结果集的映射
​        JPA是全自动的ORM框架,提供对象与数据库的映射
​可移植性不同:
​       JPA大大降低了对象与数据库的耦合性
​       MyBatis需要写入SQL,数据库的耦合性直接取决于SQL的写法

日志系统的完整性不同:
​       JPA日志系统非常健全,涉及广泛,包括:SQL记录、关系异常、优化警告、缓存提示、脏数据警告等
​       MyBatis除了基本的记录功能外,日志功能薄弱很多。

SQL优化上的区别:
​       MyBatis的SQL写在XML里,优化SQL比Hibernate方便很多
​       Hibernate的SQL是自动生成的,无法直接维护SQL,虽支持原生SQL,但开发模式上却与ORM不同,需要转换思维,使用上不是非常方便



## 2.MyBatis输入输出支持的类型有哪些?

parameterType:
​     1.简单的类型,如整数、小数、字符串
​     2.集合类型
​     3.自定义的JavaBean

简单的类型,其数值直接映射到参数上
Map或JavaBean则将其属性按照名称映射到参数上



## 3.MyBatis里如何实现一对多关联查询?

​       一对多映射有两种配置方式,都是使用collection标签实现的。
​       在此之前，为了能够存储一对多的数据，需要在主表对应的实体类中增加集合属性，用于封装子表对应的实体类

嵌套查询:
​       1.通过select标签定义查询主表的SQL，返回结果通过reusltMap进行映射。
​       2.在在resultMap中，除了映射主表属性，还要通过collection标签映射子表属性，该标签需包含如下内容:
​           通过property属性指定子表属性名
​           通过javaType属性指定封装子表属性的集合类型
​           通过ofType属性指定子表的实体类型
​           通过select属性指定查询子表所依赖的SQL,这个SQL需单独定义,内部包含查询子表的语句

嵌套结果:
​       1.通过select标签定义关联查询主表和子表的SQL,返回结果通过resultMap进行映射。
​       2.在resultMap中,除了映射主表属性,还要通过collection标签映射子表属性,该标签需包含如下内容:
​            通过property属性指定子表属性名
​            通过ofType属性指定子表的实体类型
​            通过result子标签定义子表字段和属性的映射关系



## 4.MyBatis中的$和#有什么区别?

​       使用#设置参数时,MyBatis会创建预编译的SQL语句,在执行SQL时MyBatis会为预编译SQL中的占位符(?)赋值预编译的SQL语句执行效率高,并且可以防止注入攻击。
​        使用$设置参数时,MyBatis只是创建普通的SQL语句,在执行SQL语句时MyBatis将参数直接拼入到SQL里。效率、安全性上都不如前者。



## 5.既然$不安全,为什么还需要$,什么时候会用到它?

​        可以解决一些特殊情况下的问题。如一些动态表格(根据不同的条件产生不同的动态列)中,我们要传递SQL的列名,根据某些列进行排序,或者传递列名给SQL都是比较常见的场景,这就无法使用预编译的方式了。 



## 6.MyBatis的xml文件和Mapper接口是怎么绑定的?

​        通过xml文件中,<mapper>根标签的namespace属性进行绑定的,即namespace属性的值需要配置成接口的全限定名称,MyBatis内部就会通过这个值将这个接口与这个xml关联起来。



## 7.MyBatis分页和自己写的分页哪个效率高?

​       自己写的分页效率高
​       分页插件的原理:拦截SQL,在这个SQL基础上自动为其添加limit分页条件。它会大大提高开发的效率,无法对分页语句做出有针对性的优化,自己写的分页SQL里确实可以灵活实现的。



## 8.了解MyBatis缓存机制吗?

​       MyBatis的缓存分为一级缓存和二级缓存

一级缓存:
​        本地缓存,默认会启用,并且不能关闭。存在于SqlSession的生命周期中,是SqlSession级别的缓存。在同一个SqlSession中查询时,MyBatis 会把执行的方法和参数通过算法生成缓存的键值,将键值和查询结果存入一个Map对象中。如果同一个SqlSession 中执行的方法和参数完全一致,那么通过算法会生成相同的键值,当Map缓存对象中己经存在该键值时,则会返回缓存中的对象。

二级缓存:
​        存存在于SqlSessionFactory 的生命周期中,是SqlSessionFactory级别的缓存

若要使用,需要在如下两处进行配置
​        在MyBatis的全局配置settings中有一个参数cacheEnabled,这个参数是二级缓存的全局开关,默认值是true,初始状态为启用状态。
​         MyBatis的二级缓存是和命名空间绑定的,即二级缓存需要配置在Mapper.xml映射文件中。

二级缓存具有如下效果:
​        映射语句文件中的所有SELECT语句将会被缓存
​        映射语句文件中的所有时INSERT 、UPDATE 、DELETE 语句会刷新缓存
​        缓存会使用LRU算法来收回
​        根据时间表,缓存不会以任何时间顺序来刷新
​        缓存会存储集合或对象(无论查询方法返回什么类型的值)的1024 个引用
​        缓存会被视为read/write的，意味着对象检索不是共享的，而且可以安全地被调用者修改,而不干扰其他调用者或线程所做的潜在修改