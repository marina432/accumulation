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
> @Autowired实现注入规则:  
> 先按byName+byType在容器中查找唯一组件(因为即使按byName找到了，但与属性类型不匹配也注入不成功)：  
> 1. 如果找不到或找到多个，都报错。  
>> 使用@Resource(name="xxx"/type=X.class)，优先级高与默认。
 


### 综上总结：
1. @Autowired先用byType从容器中筛选出一个集合，然后在这个集合中通过"byName-默认属性名"或者@Quafier来筛选出唯一可以注入的对象。  
ps：这里Quafier的优先级高于"byName-默认属性名"。
   
2. @Resource先用"byName-默认属性名"+"byType-默认属性类型"从容器中筛选出唯一一个对象注入，找不到就报错。  

3. 有一点要明确，在Spring容器中byName可以唯一确定一个对象！！！  

