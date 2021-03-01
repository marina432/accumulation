### 1. @PostConstruct注解：  
> 参考阅读：https://www.cnblogs.com/lay2017/p/11735802.html

### 2. @PreDestroy注解：  
> 参考阅读：https://chenwei.blog.csdn.net/article/details/72008825?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.control&dist_request_id=066ecb86-2215-4a4f-abb2-3aa3c0deaca7&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.control


### 附：
> 总结：  
> 不管是通过实现 InitializingBean/DisposableBean 接口，
> 还是通过 <bean> 元素的init-method/destroy-method 属性进行配置，
> 都只能为 Bean 指定一个初始化 / 销毁的方法。
>> 但是使用 @PostConstruct 和 @PreDestroy 注释却可以指定多个初始化 / 销毁方法，那些被标注 @PostConstruct 或@PreDestroy 注释的方法都会在初始化 / 销毁时被执行。