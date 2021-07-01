

### JVM 堆、栈可能会抛出的异常  
1. 堆会抛出的异常：  
    - OutOfMemoryError  
    
2. 栈会抛出的异常：  
    - StackOverflowError--线程中方法调用链过深，导致超出-Xss设置的每个线程的私有栈空间大小（默认1M）  
    - OutOfMemoryError--  
   
