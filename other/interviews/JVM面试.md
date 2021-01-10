







### ClassNotFoundException 与 NoClassDefFoundError 的区别？
解：ClassNotFoundException是checked异常，发生在类加载过程（加载-连接-初始化-使用-卸载）的加载阶段，当JVM虚拟机试图通过类的全限定名来获取类的二进制字节流、但是在外存中却没有找到时，会抛出此异常。  
NoClassDefFoundError是Error，发生在类加载过程的连接阶段，此时(经过加载阶段的)内存中应该已经生成了该类的各种数据访问入口的class对象，但却从内存中找不到该类的定义，会抛出此错误。
















参考阅读：https://blog.csdn.net/zhaocuit/article/details/93038538