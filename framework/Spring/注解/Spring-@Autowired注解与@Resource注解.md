### 一、@Autowired注解实现依赖注入
@Autowired注解实现自动注入，构造器注入，setter注入：
- 当@Autowired注解在类属性上时，则实现自动注入；  
- 当@Autowired注解在构造方法上时，则实现构造器注入；
- 当@Autowired注解在setter方法上时，则实现setter注入。  

> @Autowired实现注入规则：
> 先按byType在容器中查找该类型当唯一组件：1.如果没找到，则按byName查找目标同名组件；2.如果找到多个，则会报错，这时可使用@Qulifier注解指定目标组件。  


### 二、@Resource注解实现依赖注入
注入规则同Autowired相同，只是在按byType查找如果找到多个同类型报错后，则可以使用@Resourec(name="XXX")实现指定（而无需借助其它注解）  

参考阅读：https://blog.csdn.net/weixin_42117272/article/details/81223276
