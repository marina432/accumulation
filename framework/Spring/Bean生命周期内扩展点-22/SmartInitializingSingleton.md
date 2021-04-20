全限定名：org.springframework.beans.factory.SmartInitializingSingleton
- 作用：在spring容器管理的所有单例对象（非懒加载对象）初始化完成之后调用的回调接口。  
- 触发时机：在postProcessAfterInitialization之后。
- 使用场景：用户可以扩展此接口在对所有单例对象初始化完毕后，做一些后置的业务处理。

- 扩展方式：
```java
public class TestSmartInitializingSingleton implements SmartInitializingSingleton {
    @Override
    public void afterSingletonsInstantiated() {
        System.out.println("[TestSmartInitializingSingleton]");
    }
}
```
