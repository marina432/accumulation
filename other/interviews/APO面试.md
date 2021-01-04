### 1. Spring AOP 和 Aspect AOP 存在哪些区别？

- AppectJ是AOP对完整实现，Spring AOP则只是部分实现， 
- Spring AOP比AspectJ使用简单，因为Spring AOP仅整合了AspcetJ的注解，移除了一些比较复杂的语法。 
- Spring AOP只是整合了AspectJ注解与Spring Ioc容器，使得Spring中的bean很容易被AspectJ作为代理使用。
- Spring AOP仅支持代理模式的AOP  
- Spring AOP仅支持方法级别的Pointcuts，不支持字段级别的Pointcuts
- ps：从上述来看，Spring AOP做了两方面努力：一方面是Spring自己对AOP实现上所做对努力（部分实现AOP）；另一方面是Spring AOP在注解方面对AspectJ进行整合所做对努力。其中，第二方面努力打通两AspcetJ与Ioc容器的联系，使得容器中的bean很容易被AspectJ作为代理所使用。

### 


























