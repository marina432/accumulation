

### 1. Spring框架下IOC容器中对象的作用域有哪些？  
1. singleton（默认作用域）：单例，每次从IOC容器中获取，都返回该单例对象  
2. prototype：多例，每次从IOC容器中获取，都返回一个该类型的新对象  
3. request（只针对Web环境中使用）：每次http请求都会创建一个新对象，当请求结束时会自动销毁这个对象  
4. session（只针对Web环境中使用）：同一个http session共享一个对象，不同都http session使用不同都对象，当session结束时销毁这个对象  
5. globalSession（只针对Web环境中使用，类似session作用域，且只有对portlet才有意义）  
> 附：1.默认情况下，当使用ApplicationContext接口启动IOC容器时，会自动实例化所有singleton作用域的对象。--这一点与BeanFactory不同（BeanFactory实例化对象过程可参考：https://www.cnblogs.com/leskang/p/6411011.html）  
> 虽然这么做，在IOC启动时会很耗时，但优点是初始化后的对象会被保存在IOC容器缓存中，这样，省去了使用对象时的初始化对象耗时，从而提高程序运行效率。
> 同时，有利于我们早点二发现问题，如果我们配置的对象有问题，则会直接在容器启动阶段抛出异常（而不用等到程序的运行阶段才暴露问题）。  
> 2.使用lazy-init属性可以延迟对象初始化。当其设置为true时，通过ApplicationContext接口启动IOC容器，singleton作用域对象的初始化时机会延迟到运行时触发。  

ps：参考阅读：https://cloud.tencent.com/developer/article/1487077?from=article.detail.1487079

### 2. Spring BeanFactory实例化Bean的详细过程？
1. 调用Bean的默认构造方法（或者指定其它构造方法），生成bean实例；  
2. （检查setter注入）检查bean配置文件中是否注入Bean的属性值。如果有则进行属性注入；  
3. 检查Bean是否实现InitializingBean接口。如果实现了此接口，则调用afterPropertiesSet()方法对bean进行相应对操作；  
4. 检查Bean配置文件中是否指定了init-method属性，如果已指定，则调用该属性指定的方法对bean进行相应对操作。  

ps：参考阅读：https://www.cnblogs.com/leskang/p/6411011.html
   
### 3. 什么是IOC和DI？并且简要说明以下DI是如何实现的？  
IoC叫控制反转，是Inversion of Control的缩写，DI（Dependency Injection）叫依赖注入，是对IoC更简单的诠释。
控制反转是把传统上由程序代码直接操控的对象的调用权交给容器，通过容器来实现对象组件的装配和管理。
所谓的"控制反转"就是对组件对象控制权的转移，从程序代码本身转移到了外部容器，由容器来创建对象并管理对象之间的依赖关系。
IoC体现了好莱坞原则 - "Don’t call me, we will call you"。
依赖注入的基本原则是应用组件不应该负责查找资源或者其他依赖的协作对象。
配置对象的工作应该由容器负责，查找资源的逻辑应该从应用组件的代码中抽取出来，交给容器来完成。
DI是对IoC更准确的描述，即组件之间的依赖关系由容器在运行期决定，形象的来说，即由容器动态的将某种依赖关系注入到组件之中。

一个类A需要用到接口B中的方法，那么就需要为类A和接口B建立关联或依赖关系，最原始的方法是在类A中创建一个接口B的实现类C的实例，
但这种方法需要开发人员自行维护二者的依赖关系，也就是说当依赖关系发生变动的时候需要修改代码并重新构建整个系统。
如果通过一个容器来管理这些对象以及对象的依赖关系，则只需要在类A中定义好用于关联接口B的方法（构造器或setter方法），将类A和接口B的实现类C放入容器中，通过对容器的配置来实现二者的关联。
依赖注入可以通过setter方法注入（设值注入）、构造器注入和接口注入三种方式来实现，
Spring支持setter注入和构造器注入，通常使用构造器注入来注入必须的依赖关系，对于可选的依赖关系，则setter注入是更好的选择，
setter注入需要类提供无参构造器或者无参的静态工厂方法来创建对象。
> ps:关于Spring为何不支持接口注入可参考：https://blog.csdn.net/xuebing1995/article/details/75389143  
> 总结来说亮点：1.接口注入必须要定义接口，且实现该实现接口；2.setter注入完全可以实现接口注入的效果，而无需引入接口。  

### 4. 简要说一下依赖注入的方式有哪几种？  
构造器注入、setter注入、接口注入。
ps：Spring只持前两种。  

### 5.请说明一下Spring中BeanFactory和ApplicationContext的区别是什么？
概念：  
BeanFactory：
BeanFactory是spring中比较原始，比较古老的Factory。因为比较古老，所以BeanFactory无法支持spring插件，例如：AOP、Web应用等功能。

ApplicationContext:
ApplicationContext是BeanFactory的子类，因为古老的BeanFactory无法满足不断更新的spring的需求，于是ApplicationContext就基本上代替了BeanFactory的工作，以一种更面向框架的工作方式以及对上下文进行分层和实现继承，并在这个基础上对功能进行扩展：
<1>MessageSource, 提供国际化的消息访问
<2>资源访问（如URL和文件）
<3>事件传递
<4>Bean的自动装配
<5>各种不同应用层的Context实现

二者区别：  
<1>如果使用ApplicationContext，如果配置的bean是singleton，那么不管你有没有或想不想用它，它都会被实例化。好处是可以预先加载，坏处是浪费内存。
<2>BeanFactory，当使用BeanFactory实例化对象时，配置的bean不会马上被实例化，而是等到你使用该bean的时候（getBean）才会被实例化。好处是节约内存，坏处是速度比较慢。多用于移动设备的开发。
<3>没有特殊要求的情况下，应该使用ApplicationContext完成。因为BeanFactory能完成的事情，ApplicationContext都能完成，并且提供了更多接近现在开发的功能。

### 6. 请说明一下springIOC原理是什么？如果你要实现IOC需要怎么做？请简单描述一下实现步骤？  
- IOC原理：  
  IoC（Inversion of Control，控制倒转）。这是spring的核心，贯穿始终。  
  所谓IoC，对于Spring框架来说，就是由spring来负责控制对象的生命周期和对象间的关系。  
  IoC的一个重点是在系统运行中，动态的向某个对象提供它所需要的其他对象。这一点是通过DI（Dependency Injection，依赖注入）来实现的。
  比如对象A需要操作数据库，以前我们总是要在A中自己编写代码来获得一个Connection对象，
  有了spring我们就只需要告诉spring，A中需要一个Connection，至于这个Connection怎么构造，何时构造，A不需要知道。
  在系统运行时，spring会在适当的时候制造一个Connection，然后像打针一样，注射到A当中，这样就完成了对各个对象之间关系的控制。
  A需要依赖Connection才能正常运行，而这个Connection是由spring注入到A中的，依赖注入的名字就这么来的。
  那么DI是如何实现的呢？ Java 1.3之后一个重要特征是反射（reflection），它允许程序在运行的时候动态的生成对象、执行对象的方法、改变对象的属性，spring就是通过反射来实现注入的。  
  
- 实现IOC的步骤:  
  1) 定义用来描述bean的配置的Java类;  
  2) 解析bean的配置，將bean的配置信息转换为BeanDefinition对象保存在内存中，spring中采用HashMap进行对象存储，其中会用到一些xml解析技术;  
  3) 遍历存放BeanDefinition的HashMap对象，逐条取出BeanDefinition对象，获取bean的配置信息，利用Java的反射机制实例化对象，將实例化后的对象保存在另外一个Map中即可。

### 7. 说一下Bean的生命周期  
参考阅读：https://www.cnblogs.com/zrtqsk/p/3735273.html  

### 8. 请说一下@Controller与@RestController的区别？  
@RestController相当与@Controller + @ResponseBody两个注解的组合，该被@RestController注解的类下接口，
只能返回数据，不能返回jsp、html页面，视图解析器无法解析jsp、html页面。
> @ResponseBody注解表示方法的返回结果直接写入HTTP response body中。
> 方法在使用@RequestMapping后，返回值通常会被解析为跳转路径，但在同时使用@ResponseBody注解后，返回值就不会被解析为跳转路径，
> 而是直接写入HTTP response body中。  

### 9. 请问在以前的学习中有使用过Spring里面的注解吗？如果有请谈一下autowired 和resource区别是什么？  
[Spring-@Autowired注解与@Resource注解.md](./注解/Spring-@Autowired注解与@Resource注解.md)

### 

































