@Inherited注解是JDK1.5开始出现的元注解，
表示带有@Inherited标签的注解在类的继承关系链上具有沿链向下传递的特性。  

### 实验验证结果如下：
```java
    public static void main(String[] args) {

        System.out.println("======>Father注解：");
        final Annotation[] annotations = Father.class.getAnnotations();
        Arrays.stream(annotations).forEach(annotation -> System.out.println(annotation.annotationType().getName()));
        final Annotation[] annotations1 = SonExtendsFather.class.getAnnotations();
        System.out.println("======>Son注解：");
        Arrays.stream(annotations1).forEach(annotation -> System.out.println(annotation.annotationType().getName()));
        System.out.println("======>总结：说明在类与类的继承关系中，父类上的打Inherited标签的注解会被子类继承过来！");

        System.out.println("======>FatherInterface注解：");
        final Annotation[] annotations2 = Fatherinterface.class.getAnnotations();
        Arrays.stream(annotations2).forEach(annotation -> System.out.println(annotation.annotationType().getName()));
        System.out.println("======>SonInterface注解：");
        final Annotation[] annotations3 = SonInterface.class.getAnnotations();
        Arrays.stream(annotations3).forEach(annotation -> System.out.println(annotation.annotationType().getName()));
        System.out.println("======>总结：说明在接口与接口的继承关系中，父接口上的打Inherted标签的注解不会被子接口继承过来！");

        System.out.println("======>SonImplements:");
        final Annotation[] annotations4 = SonImplementsFatherInterface.class.getAnnotations();
        Arrays.stream(annotations4).forEach(annotation -> System.out.println(annotation.annotationType().getName()));
        System.out.println("======>说明：在类实现接口的关系中，接口上的打Inherted标签的注解不会被实现类继承过来！");
    }
```
terminal输出：
```text
======>Father注解：
org.example.commonTest.testInherited.DoInheritedAnnotation
org.example.commonTest.testInherited.NotInheritedAnnotation
======>Son注解：
org.example.commonTest.testInherited.DoInheritedAnnotation
======>总结：说明在类与类的继承关系中，父类上的打Inherited标签的注解会被子类继承过来！
======>FatherInterface注解：
org.example.commonTest.testInherited.DoInheritedAnnotation
org.example.commonTest.testInherited.NotInheritedAnnotation
======>SonInterface注解：
======>总结：说明在接口与接口的继承关系中，父接口上的打Inherted标签的注解不会被子接口继承过来！
======>SonImplements:
======>说明：在类实现接口的关系中，接口上的打Inherted标签的注解不会被实现类继承过来！
```
