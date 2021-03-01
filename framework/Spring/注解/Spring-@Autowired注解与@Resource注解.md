### 一、@Autowired注解实现依赖注入（是Spring的注解）
@Autowired注解实现自动注入，构造器注入，setter注入：
- 当@Autowired注解在类属性上时，则实现自动注入；  
- 当@Autowired注解在构造方法上时，则实现构造器注入；
- 当@Autowired注解在setter方法上时，则实现setter注入。  

> @Autowired实现注入规则(经验证：最大限度匹配)：  
> 先按byType在容器中查找该类型当唯一组件：  
> 1.如果找到唯一一个类型匹配到对象，则注入成功。  
> 2.否则，如果没找到或找到多个，则接着按byName查找，如果还找不到或找到多个，则报错。（@Autowired默认required为true，要求必须找到唯一一个，否则报错）；
>> @Autowired + @Qualifier("xxx")，匹配注入的对象必须满足：1.byType，属性类型匹配；2.并且byName，对象名为"xxx"。否则报错。  


### 二、@Resource注解实现依赖注入（是J2EE的注解）
> @Resource实现注入规则:  
> @Resource默认按照ByName自动注入，由J2EE提供，需要导入包javax.annotation.Resource。
> @Resource有两个重要的属性：name和type，而Spring将@Resource注解的name属性解析为bean的名字，而type属性则解析为bean的类型。
> 所以:  
> 1.如果指定name属性，则使用byName的自动注入策略，如果找到且符合被@Resource注解的属性的类型，则注入成功；否则报错。    
> 2.如果指定type属性，则使用byType自动注入策略。如果找不到则报错；如果找到多个再用byName筛选一下，如果能筛出唯一一个，则注入成功。否则其它情况都报错。    
> 3.如果既不指定name也不指定type属性，这时按照byName策略从容器中查找：  
> 1)如果找到，再看找到的这个对象的类型是否与被@Resource注解的属性的类型匹配，如果匹配，则注入成功；如果不匹配，则报错，注入失败；  
> 2)如果没找到，则按byType策略从容器中查找，如果还无法找到或找到多个，都报错，注入失败；只有找到唯一一个，才注入成功。  
 


### 综上总结：
1. @Autowired先用byType从容器中筛选出一个集合，然后在这个集合中通过"byName-默认属性名"或者@Quafier来筛选出唯一可以注入的对象。  
ps：这里Quafier的优先级高于"byName-默认属性名"。

2. 有一点要明确，在Spring容器中byName可以唯一确定一个对象！！！  

