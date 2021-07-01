

### JVM 堆、栈可能会抛出的异常  
1. 堆会抛出的异常：  
    - OutOfMemoryError  
    
2. 栈会抛出的异常：  
    - StackOverflowError--单个线程中方法调用链过深，导致超出-Xss（设置的每个线程的栈空间大小）（默认1M）  
    - OutOfMemoryError--eg.创建线程过多，导致再创建线程时内存空间不够造成  
   

   
Thread.UncaughtExceptionHandler-uncaughtException
