全限定名：org.springframework.beans.factory.BeanNameAware
```text
Aware接口
    ｜--BeanNameAware接口
```

- 触发点：在bean的初始化之前，也就是postProcessBeforeInitialization之前，这个类的触发点方法只有一个：setBeanName
- 使用场景为：用户可以扩展这个点，在初始化bean之前拿到spring容器中注册的的beanName，来自行修改这个beanName的值。

- 扩展方式：
```java
public class NormalBeanA implements BeanNameAware{
    public NormalBeanA() {
        System.out.println("NormalBean constructor");
    }

    @Override
    public void setBeanName(String name) {
        System.out.println("[BeanNameAware] " + name);
    }
}
```
