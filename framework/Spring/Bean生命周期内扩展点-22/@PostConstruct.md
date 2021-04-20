注解：javax.annotation.PostConstruct  
这个并不算一个扩展点，其实就是一个标注。  
- 作用：是在bean的初始化阶段，如果对一个方法标注了@PostConstruct，会先调用这个方法。  
- 这个触发点是在postProcessBeforeInitialization之后，InitializingBean.afterPropertiesSet之前。
- 使用场景：用户可以对某一方法进行标注，来进行初始化某一个属性

- 扩展方式：
```java
public class NormalBeanA {
    public NormalBeanA() {
        System.out.println("NormalBean constructor");
    }

    @PostConstruct
    public void init(){
        System.out.println("[PostConstruct] NormalBeanA");
    }
}
```
