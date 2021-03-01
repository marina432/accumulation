### @ConfigurationProperties注解：
ps：@ConfigurationProperties需要借助其它注解（eg. @Component、@Bean、@EnableConfigurationProperties 等）才能实现将被其注释的Bean实例化到Spring容器中，
即：@ConfigurationProperties并无实例化对象到Spring容器到能力，它只完成了Bean属性与配置文件中配置参数的映射绑定而已。  
> 参考阅读：https://www.cnblogs.com/jimoer/p/11374229.html  



### @EnableConfigurationProperties注解：

> 参考阅读：https://zhuanlan.zhihu.com/p/201363487  
