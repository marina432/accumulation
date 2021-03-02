@ContextConfiguration注解是org.springframework.spring-test包中的注解，
它的作用概括起来说就是：作用在单元测试类上，为单元测试类指定xml或Java方式的类收集器。  

> 指定xml方式类收集器：@ContextConfiguration(locations = {"classpath*:/*.xml"})  
> 指定java方式类收集器：@ContextConfiguration(classes=CDPlayConfig.class)  
> 具体参考阅读：https://www.cnblogs.com/bihanghang/p/10023759.html  
