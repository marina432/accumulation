### 1. Queue中的poll()与remove()有何区别？ 
```text
Queue  
  ｜---Deque  
  ｜     ｜---LinkedList  
  ｜---AbstractQueue  
            ｜---PriorityQueue  
```
- offer(E e)与add(E e)：都是向队列中添加一个元素。区别：在队满情况下，调用add(E e)会抛出IllegalStateException异常，
  而调用offer(E e)则返回false。
- peek()与element()：都是获取队列首元素且不弹出首元素。区别：在队空队情况下，调用element()会抛出NoSuchElementException异常，
  而调用peek()则返回null。
- poll()与remove()：都是获取队列首元素且将其弹出。区别：在队空队情况下，调用remove()会抛出NoSuchElementException异常，
  而调用poll()则返回null。  

### 2. I/O流分几种？  
```text
                         |--BufferedReader
              |--Reader--|
              |          |--InputStreamReader--FileReader
    |--字符流--|
    |         |          |--BufferedWriter
    |         |--Writer--|
    |                    |--OutputStreamWriter--FileWriter
流--｜
    |                         |--FileInputStream
    |         |--InputStream--|
    |         |               |--FilterInputStream--BufferedInputStream
    |--字节流--|
              |                |--FileOutputStream
              |--OutputStream--|
                               |--FilterOutputStream--BufferedOutputStream    
```
java中I/O流是将不同的输入、输出源通过流的形式连接起来，通过流的输入、输出操作进行数据交换。  
I/O流的分类方式：  
- 按数据流向分：输入流 + 输出流  
- 按流数据格式分：字符流 + 字节流  
- 按流数据的包装过程分：节点流（低级流）+ 处理流（高级流）  

### 3. 单例的几种实现方式   
- 饿汉式  
- 静态内部类实现懒加载（普通写法懒汉式实现单例存在问题）  
- 双重检测  
- 枚举  

### jdk动态代理与cglib动态代理？  
jdk动态代理是Java JDK利用反射原理实现的。  
cglib是spring利用asm字节码生成技术实现的。  

> 具体参考阅读：https://blog.csdn.net/flyfeifei66/article/details/81481222  
> 



















