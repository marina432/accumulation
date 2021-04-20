全限定名：org.springframework.beans.factory.DisposableBean  

这个扩展点也只有一个方法：destroy()，
- 触发时机：当此对象销毁时，会自动执行这个方法。
  比如说运行applicationContext.registerShutdownHook时，就会触发这个方法。

- 扩展方式：
```java
public class NormalBeanA implements DisposableBean {
    @Override
    public void destroy() throws Exception {
        System.out.println("[DisposableBean] NormalBeanA");
    }
}
```


> ps：@PreDestroy注解与指功效相同，可参看：[J2EE-@PostConstruct与@PreDestroy.md](注解/J2EE-@PostConstruct与@PreDestroy.md) 