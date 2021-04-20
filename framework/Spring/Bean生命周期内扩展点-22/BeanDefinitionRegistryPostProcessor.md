全限定名：org.springframework.beans.factory.support.BeanDefinitionRegistryPostProcessor  
```text
BeanFactoryPostProcessor接口
            ｜--BeanDefinitionRegistryPostProcessor接口
```
- 调用时机：在Spring读取完项目中的beanDefinition之后
- 扩展作用：这个接口在读取项目中的beanDefinition之后执行，提供一个补充的扩展点  
- 使用场景：你可以在这里动态注册自己的beanDefinition，可以加载classpath之外的bean

- 扩展方式:
```java
public class TestBeanDefinitionRegistryPostProcessor implements BeanDefinitionRegistryPostProcessor {

    //1.在Spring读取完项目中的beanDefinition之后先执行
    @Override
    public void postProcessBeanDefinitionRegistry(BeanDefinitionRegistry registry) throws BeansException {
        System.out.println("[BeanDefinitionRegistryPostProcessor] postProcessBeanDefinitionRegistry");
    }

    //2.在上面postProcessBeanDefinitionRegistry()后执行
    @Override
    public void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) throws BeansException {
        System.out.println("[BeanDefinitionRegistryPostProcessor] postProcessBeanFactory");
    }
}
```
