全限定名：org.springframework.beans.factory.config.BeanFactoryPostProcessor  
- 调用时机：在spring在读取完beanDefinition信息之后，实例化bean之前。  
- 扩展作用：这个接口是beanFactory的扩展接口，在这个时机，用户可以通过实现这个扩展接口来自行处理一些东西，比如修改已经注册的beanDefinition的元信息。  
- 扩展方式为：
```java
public class TestBeanFactoryPostProcessor implements BeanFactoryPostProcessor {
    @Override
    public void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) throws BeansException {
        System.out.println("[BeanFactoryPostProcessor]");
    }
}
```
