
Java面试题
=================

      * [Java面试题](#java面试题)
            * [<em>1.请简述SpringMVC的工作流程?</em>](#1请简述springmvc的工作流程)
            * [<em>2.请简述一下mybatis中，Collection和Association标签的区别？</em>](#2请简述一下mybatis中collection和association标签的区别)
            * [<em>3.请简述下Bean注入属性有几种方式，分别是什么？</em>](#3请简述下bean注入属性有几种方式分别是什么)
            * [<em>4.请简述一下SpringMVC中，@RequestBody和@ResponseBody的区别？</em>](#4请简述一下springmvc中requestbody和responsebody的区别)
            * [<em>5.请说一下，mybatis中#和@的区别？</em>](#5请说一下mybatis中和的区别)
            * [<em>6.请说一下事务的管理机制是什么？</em>](#6请说一下事务的管理机制是什么)
            * [<em>7.请简述下SpringMVC主要使用了哪些设计模式？</em>](#7请简述下springmvc主要使用了哪些设计模式)
            * [<em>8.Spring如何保证Controller并发的安全？</em>](#8spring如何保证controller并发的安全)
            * [<em>9.Spring Bean的生命周期是如何管理的？</em>](#9spring-bean的生命周期是如何管理的)
            * [<em>10.“a==b”和“a.equals(b)”有什么区别？</em>](#10ab和aequalsb有什么区别)
            * [<em>11.HashMap是线程安全的吗？为什么不是线程安全？</em>](#11hashmap是线程安全的吗为什么不是线程安全)
            * [<em>12.HashMap,ConcurrentHashMap与LinkedHashMap</em>](#12hashmapconcurrenthashmap与linkedhashmap)
            * [<em>13.如何创建一个线程，有哪几种方式，他们的区别是什么？</em>](#13如何创建一个线程有哪几种方式他们的区别是什么)
            * [<em>14.wait和sleep的区别</em>](#14wait和sleep的区别)
            * [<em>15.请描述一下HashMap的扩容过程</em>](#15请描述一下hashmap的扩容过程)
            * [<em>16.请说一下强引用、软引用、弱引用、虚引用分别是什么？</em>](#16请说一下强引用软引用弱引用虚引用分别是什么)
            * [<em>17.synchronize在静态方法和普通方法的区别？</em>](#17synchronize在静态方法和普通方法的区别)
            * [<em>18.如何处理HashMap的并发问题？</em>](#18如何处理hashmap的并发问题)
            * [<em>19.线程池的种类有哪些，他们的区别和使用场景？</em>](#19线程池的种类有哪些他们的区别和使用场景)

Created by [gh-md-toc](https://github.com/ekalinin/github-markdown-toc)



#### *1.请简述SpringMVC的工作流程?*
*   客户端发送url请求
*   核心控制器dispatcher servlet接受到url请求，通过系统或者自定义的配置找到handler处理器映射器，并由url获得对应的controller
*   通过核心控制器找到相对应的处理适配器
*   使用该适配器调用相对应的接口，获取数据模型和视图对象返回给核心控制器
*   核心控制器将其数据和视图结合的对象返回给视图解析器，解析器解析后响应给核心控制器
*   核心控制器将结果返回给客户端**
***

#### *2.请简述一下mybatis中，Collection和Association标签的区别？*
* assocation表示一对一和多对一的情况，比如一个Person类包含一个Card类，因为一个人对应一个身份证，这里是一对一的情况
* Collection表示的是一对多的情况。
***
    一对一：
    <resultMap type="com.glj.poji.Person" id="personMapper">
    <id property="id" column="id"/>
    <result property="name" column="name"/>
    <result property="sex" column="sex"/>
    <result property="age" column="age"/>
    <association property="card" column="card_id"
    select="com.glj.mapper.CardMapper.selectCardById"
    javaType="com.glj.poji.Card">
    </association>
    </resultMap>
***
***
    一对多：
    <resultMap type="com.glj.pojo.Clazz" id="clazzResultMap">
    <id property="id" column="id"/>
    <result property="code" column="code"/>
    <result property="name" column="name"/>
    <!-- property: 指的是集合属性的值, ofType：指的是集合中元素的类型 -->
    <collection property="students" ofType="com.glj.pojo.Student"
    column="id" javaType="ArrayList"
    fetchType="lazy" select="com.glj.mapper.StudentMapper.selectStudentByClazzId">
    <id property="id" column="id"/>
    <result property="name" column="name"/>
    <result property="sex" column="sex"/>
    <result property="age" column="age"/>
    </collection>
    </resultMap>
***
***
    多对一：
    <resultMap type="com.glj.pojo.Student" id="studentResultMap">
    <id property="id" column="id"/>
    <result property="name" column="name"/>
    <result property="sex" column="sex"/>
    <result property="age" column="age"/>
    <association property="clazz" javaType="com.glj.pojo.Clazz">
    <id property="id" column="id"/>
    <result property="code" column="code"/>
    <result property="name" column="name"/>
    </association>
    </resultMap>
***

#### *3.请简述下Bean注入属性有几种方式，分别是什么？*
Spring容器中支持的依赖注入方式主要有属性注入、构造函数注入、工厂方法注入。
* 属性注入：即通过setXXX( )方法注入bean的属性值或依赖对象。由于属性注入方式具有可选择性和灵活性高的特点，因此它也是实际开发中最常用的注入方式。对于属性注入方式来说，只能人为的在配置文件中提供保证，而无法在语法级别提供保证。
* 构造函数注入：除属性注入之外的另一种常用的注入方式，它可以保证一些必要的属性在bean实例化时就得到了设置，并在实例化后就可以使用。构造函数注入的前提是： bean必须提供带参的构造函数。
* Spring工厂注入的方法可以分为 静态 和 非静态 两种。
```
构造函数注入选择理由：
构造函数保证重要属性预先设置；
无需提供每个属性的setter方法，减少类的方法个数；
可以更好地封装类变量，避免外部错误调用。

属性注入选择理由：
属性过多时，构造函数变的臃肿；
构造函数注入灵活性不强，有时需要为属性注入null值；
多个构造函数时，配置上产生歧义，复杂度升高；
构造函数不利于类的继承和扩展；
构造函数注入会引起循环依赖的问题。 

其实Spring为我们注入参数提供了这么多方法，那么这些方法必然有他们存在的道理，每个方法在某一问题上会有独特的优势，我们只需要按照我们具体的使用需求选择适合的方法来使用就好了，但一般不太推荐工厂方法注入。
```
***

#### *4.请简述一下SpringMVC中，@RequestBody和@ResponseBody的区别？*
* @RequestBody：将前台的key，value数据data：name=“1”&id=1 或者json数据data ：{name：“1”，id：1 } 转换成java对象入形参。
* @ResponseBody：将返回到前台的java对象 转换成json串输出。
```
两个注解都是用于数据格式和pojo对象之间的转换
```
***

#### *5.请说一下，mybatis中#和@的区别？*
* #{}可以防止sql注入，它是一种预编译的语句，即在使用jdbc时的prepareStatement，sql如果存在参数则会使用?占位符，使用#{}形成的sql语句，是带有引号的。比如select * from table1 where id = #{id}中实际上的select * from table1 where id = '1'，另外，使用xml配置时只能使用#{}语法，而且，当parameterType是int时，必须使用该语法。
* 使用${}形成的sql语句没有被引号包裹，但是在以下sql语句时最好使用该语句：select 'table1' order by 'id'表示按哪个表和按某个列排序时，
***

#### *6.请说一下事务的管理机制是什么？*
    fuck
***

#### *7.请简述下SpringMVC主要使用了哪些设计模式？*
* 简单工厂模式，例如BeanFactory
* 工厂方法模式
* 适配器模式
* 包装器模式
* 代理模式
* 观察者模式
* 策略模式
* 模板方法模式
***

#### *8.Spring如何保证Controller并发的安全？*
    Spring中controller默认是单例模式，如果加注解@Scope(prototype)则为多例模式，Spring多线程访问的都是一个controller，如果controller中含有类变量，那所有的请求都会共享这个变量，可能出现于预期的值不一样。可以用多例模式解决，但是使用多例模式会产生时间的开销。最好的解决方案是：使用ThreadLocal来保存类变量，将类变量保存在线程的变量域中，让不同的请求隔离开来。ThreadLocal会为每一个线程提供一个独立的变量副本
***

#### *9.Spring Bean的生命周期是如何管理的？*
* 实例化bean对象(通过构造方法或者工厂方法)
* 设置对象属性(setter等)（依赖注入）
* 如果Bean实现了BeanNameAware接口，工厂调用Bean的setBeanName()方法传递Bean的ID。（和下面的一条均属于检查Aware接口）
* 如果Bean实现了BeanFactoryAware接口，工厂调用setBeanFactory()方法传入工厂自身
* 将Bean实例传递给Bean的前置处理器的postProcessBeforeInitialization(Object bean, String beanname)方法
* 调用Bean的初始化方法
* 将Bean实例传递给Bean的后置处理器的postProcessAfterInitialization(Object bean, String beanname)方法
* 使用Bean
* 容器关闭之前，调用Bean的销毁方法
***

#### *10.“a==b”和“a.equals(b)”有什么区别？*
* "a==b"指的是对象的引用，只有当两个引用都指向堆中的同一个对象时才返回true
* "a.equals(b)"指的是内容的比较，当内容相等时候返回true，这里指的是逻辑一致性的比较。例如，String类需要重写equals方法，可以用于两个不同的对象，但是内容相等的比较
***

#### *11.HashMap是线程安全的吗？为什么不是线程安全？*
    HashMap不是线程安全的。
    HashMap底层是一个entry数组，当发生哈希碰撞时，HashMap会采用链表的方式解决，在对应的数组位置上存放链表的头结点，然后新加入的节点会从改头结点加入，而此过程是非同步的。如果多个线程同时访问一个HashMap，在访问同一个hash映射的同时对映射关系进行结构上的修改，会造成不同步的现象。
***

#### *12.HashMap,ConcurrentHashMap与LinkedHashMap*
* HashMap线程不安全，无序
* ConcurrentHashMap采用分段锁方式进行数据同步，但是因为进行两次哈希算法（第一次确定该元素位于哪一个段即segment，第二次确定元素位置），因此效率相比于HashMap要低，但是在多线程的情况下满足一定的线程安全场景（牺牲性能换取数据安全）
* LinkedHashMap维护一个双链表，可以按数据的写入顺序读出
```
注意：这里ConcurrentHashMap指的是在一个段(segment)内线程安全，分配的锁作用于段上。应用场景是高并发，但是不能保证完全的线程安全
```
***

#### *13.如何创建一个线程，有哪几种方式，他们的区别是什么？*
* 自定义Thread的子类，并重写父类的run方法即执行体，然后创建Thread的实例，调用实例的start方法
* 自定义类实现Runnable接口，并重写run方法，然后使用new Thread()并注入自定义类对象，接着低啊用start方法
* 通过Callable和Future创建线程
```
使用runnable和Callable接口的优势是可以多实现，还可以继承其他类
使用Thread子类继承的方式的优势是无需使用Thread.currentThread访问当前的线程，而只需要使用this即可
```
***

#### *14.wait和sleep的区别*
* wait是Object对象上的方法，在同步代码块或者同步方法中使用，会释放当前的同步锁，让出cpu，进入等待状态，可以用notify和notifyAll方法唤醒
* sleep是Thread的静态方法，可以在任何地方使用，必须捕获异常，不会释放锁，可以使用interrupt方法唤醒
***

#### *15.请描述一下HashMap的扩容过程*
    当HashMap填满了0.75负载因子的桶（bucket）时才会发生扩容: HashMap.size()> Capacity(容量) * loadFactor(负载因子)
    1.扩容: 创建一个新的entry空数组，长度是原数组的两倍
    2.Rehash: 因为长度不一样，哈希计算的值也不一定，遍历原来的entry数组，把所有的entry数组重新hash到扩容的数组上
***

#### *16.请说一下强引用、软引用、弱引用、虚引用分别是什么？*
* 强引用：一般是new出来的对象，垃圾回收器是不会回收强引用类型的对象，即使当程序抛出OutMemoryError错误也不会回收具有强引用的对象。如果想让强引用对象回收，可以认为设置对象为null让垃圾回收器GC回收
* 软引用：当内存充足时不会进行垃圾回收，当内存不足时则会进行垃圾回收。当内存不足时，JVM优先对软引用设置为null，再进行垃圾回收
* 弱引用：不管内存是否充足，都会回收软引用对象的内存。不过由于软引用是一个优先级很低的引用，所以不一定很快就会发现软引用
* 虚引用：虚引用顾名思义，就是形同虚设。与其他几种引用都不同，虚引用并不会决定对象的生命周期。如果一个对象仅持有虚引用，那么它就和没有任何引用一样，在任何时候都可能被垃圾回收器回收
***

#### *17.synchronize在静态方法和普通方法的区别？*
* 修饰一个方法，被修饰的方法称为同步方法，其作用的范围是整个方法，作用的对象是调用这个方法的对象
* 修改一个静态的方法，其作用的范围是整个静态方法，作用的对象是这个类的所有对象
***

#### *18.如何处理HashMap的并发问题？*
    使用ConcurrentHashMap
***

#### *19.线程池的种类有哪些，他们的区别和使用场景？*
    newCachedThreadPool：
    底层：返回ThreadPoolExecutor实例，corePoolSize为0；maximumPoolSize为Integer.MAX_VALUE；keepAliveTime为60L；unit为TimeUnit.SECONDS；workQueue为SynchronousQueue(同步队列)
    通俗：当有新任务到来，则插入到SynchronousQueue中，由于SynchronousQueue是同步队列，因此会在池中寻找可用线程来执行，若有可以线程则执行，若没有可用线程则创建一个线程来执行该任务；若池中线程空闲时间超过指定大小，则该线程会被销毁。
    适用：执行很多短期异步的小程序或者负载较轻的服务器
    
    newFixedThreadPool：
    底层：返回ThreadPoolExecutor实例，接收参数为所设定线程数量nThread，corePoolSize为nThread，maximumPoolSize为nThread；keepAliveTime为0L(不限时)；unit为：TimeUnit.MILLISECONDS；WorkQueue为：new LinkedBlockingQueue<Runnable>() 无解阻塞队列
    通俗：创建可容纳固定数量线程的池子，每隔线程的存活时间是无限的，当池子满了就不在添加线程了；如果池中的所有线程均在繁忙状态，对于新任务会进入阻塞队列中(无界的阻塞队列)
    适用：执行长期的任务，性能好很多
    
    newSingleThreadExecutor:
    底层：FinalizableDelegatedExecutorService包装的ThreadPoolExecutor实例，corePoolSize为1；maximumPoolSize为1；keepAliveTime为0L；unit为：TimeUnit.MILLISECONDS；workQueue为：new LinkedBlockingQueue<Runnable>() 无解阻塞队列
    通俗：创建只有一个线程的线程池，且线程的存活时间是无限的；当该线程正繁忙时，对于新任务会进入阻塞队列中(无界的阻塞队列)
    适用：一个任务一个任务执行的场景
    
    NewScheduledThreadPool:
    底层：创建ScheduledThreadPoolExecutor实例，corePoolSize为传递来的参数，maximumPoolSize为Integer.MAX_VALUE；keepAliveTime为0；unit为：TimeUnit.NANOSECONDS；workQueue为：new DelayedWorkQueue() 一个按超时时间升序排序的队列
    通俗：创建一个固定大小的线程池，线程池内线程存活时间无限制，线程池可以支持定时及周期性任务执行，如果所有线程均处于繁忙状态，对于新任务会进入DelayedWorkQueue队列中，这是一种按照超时时间排序的队列结构

***
