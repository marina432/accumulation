### 1. Mybatis的#{}与${} 
解：#{}是预编译处理，${}是字符串替换。  
    Mybatis 在处理#{}时，会将 sql 中的#{}替换为?号，调用 PreparedStatement 的 set 方法来赋值;  
    Mybatis 在处理${}时，就是把${}替换成变量的值。 使用#{}可以有效的防止 SQL 注入，提高系统安全性。  
参考阅读：https://www.yuque.com/mona432/xmh6iw/yty3kx  

### 2. Mybatis的一级缓存与二级缓存  

参考阅读：https://www.cnblogs.com/happyflyingpig/p/7739749.html  

### 3. 