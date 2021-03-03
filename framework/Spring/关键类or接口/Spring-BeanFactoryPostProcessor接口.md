### 动态注册的bean可使用aop等功能等实现原理：  
在Spring容器启动过程中，BeanFactory载入Bean的定义后，会立即执行BeanFactoryPostProcessor，
此时动态注册bean到Spring容器。
这种方式可以保证动态注册到bean被beanPostProcessor处理，并且可以保证优先于依赖它的那些其它bean完成自己的实例化及初始化工作。  

参考阅读：https://zhuanlan.zhihu.com/p/30070328