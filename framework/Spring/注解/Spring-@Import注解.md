@Import只能用在类上，需要搭配 @Configuration（用于使容器发现被该@Import注解的类），
它可以快速实例化多个给定的Bean到Spring的IOC容器中，可以是实现第三方包（@ComponentScan(basePackages="xxx,yyy,...")中未指定扫描的包目录）导入。  
ps：@Import及 @Bean都可以实现第三方包导入，但@Autowired不支持。  

> 参考阅读：  
> https://www.cnblogs.com/yichunguo/p/12122598.html  
> https://blog.csdn.net/mamamalululu00000000/article/details/86711079

