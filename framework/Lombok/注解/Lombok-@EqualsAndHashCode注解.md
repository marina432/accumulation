lombok官方文档提到：
1. 此注解会生成equals(Object other) 和 hashCode()方法。  
2. 它默认使用非静态，非瞬态的属性  
3. 可通过参数exclude排除一些属性  
4. 可通过参数of指定仅使用哪些属性  
5. 它默认仅使用该类中定义的属性且不调用父类的方法  
6. 可通过callSuper=true解决上一点问题。让其生成的方法中调用父类的方法。  



另：@Data相当于@Getter @Setter @RequiredArgsConstructor @ToString @EqualsAndHashCode这5个注解的合集。  
通过官方文档，可以得知，当使用@Data注解时，则有了@EqualsAndHashCode注解，那么就会在此类中存在equals(Object other) 和 hashCode()方法，且不会使用父类的属性，这就导致了可能的问题。  
比如，有多个类有相同的部分属性，把它们定义到父类中，恰好id（数据库主键）也在父类中，那么就会存在部分对象在比较时，它们并不相等，却因为lombok自动生成的equals(Object other) 和 hashCode()方法判定为相等，从而导致出错。  



修复此问题的方法很简单：  
1. 使用@Getter @Setter @ToString代替@Data并且自定义equals(Object other) 和 hashCode()方法，比如有些类只需要判断主键id是否相等即足矣。  
2. 或者使用在使用@Data时同时加上@EqualsAndHashCode(callSuper=true)注解。  


参考阅读：https://blog.csdn.net/c851204293/article/details/96989512  
