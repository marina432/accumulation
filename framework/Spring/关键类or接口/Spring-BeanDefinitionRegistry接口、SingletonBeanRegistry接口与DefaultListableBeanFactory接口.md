### BeanDefinitionRegistry接口与SingletonBeanRegistry接口  
- Spring中的Bean定义都保存在BeanDefinitionRegistry接口中；
- Spring中都单例对象bean都保存在SingletonBeanRegistry接口中。  

因此，Spring提供两种api提供动态注册bean到容器到方式：
1. 使用BeanDefinitionRegistry接口的void registerBeanDefinition(String beanName, BeanDefinition beanDefinition) throws BeanDefinitionStoreException 方法.  
2. 使用SingletonBeanRegistry接口的void registerSingleton(String beanName, Object singletonObject) 方法.  
> 两者区别在于使用前者时，Spring容器会根据BeanDefinition实例化bean实例，而使用后者时，bean实例就是传递给registerSingleton方法的对象。  

### DefaultListableBeanFactory  
DefaultListableBeanFactory接口同时实现了上述这两个接口（并具体实现类registerBeanDefinition方法 & registerSingleton方法），在实践中通常会使用这个接口。  

### 向Spring容器动态注册bean的时机  
有了Spring提供的上述api接口，就可以在任意获取到上述接口实例对象到地方动态向Spring容器中随意注册对象（不过这里要注意，如果此时动态注册的对象id与容器中已有的相同，可能会有问题，
具体看怎么实现和怎么设置了<可参看DefaultListableBeanFactory源码>）。  
不过，动态注册对象的时机（或者说在何处注册）会影响该bean能否应用aop、Bean Balidation等功能。  
所以，根据注册的bean后期能否应用aop等，可分为以下两种动态注册bean的方式：  
1. 在普通类中调用上述api。--则无aop  
2. 在BeanFactoryPostProcessor（实现类的postProcessBeanFactory方法）中调用上述api完成动态注册。
   （参看：[Spring-BeanFactoryPostProcessor接口.md](./关键类or接口/Spring-BeanFactoryPostProcessor接口.md)）  


> 参考阅读：https://zhuanlan.zhihu.com/p/30070328  