### Redis的Java客户端
1. Jedis  
   官方客户端，类似与JDBC，支持的Redis命令比较全，可以看作是对reids命令的包装。  
   缺点：基于BIO，性能不高；线程不安全；需要配置连接池管理连接。

2. Lettuce  
   目前主流推荐的驱动，各框架支持较好，基于Netty NIO，API线程安全。

3. Redission  
   基于Netty NIO，API线程安全。
   亮点：大量丰富的高可用分布式功能特性，比如JUC的线程安全集合与工具的分布式版本，分布式的基本数据类型和锁等。 
   
### Redis与Spring的整合  
核心是RedisTemplate（可以配置基于Jedis，Lettuce，Redisson）  
使用方式类似于MongoDBTemplate，JDBCTemplate或JPA  

### Spring Boot与Redis集成  
引入spring-boot-starter-data-redis，配置spring redis即可

### Spring Cache本地缓存框架与Redis集成  
默认使用全局的cacheManager自动集成  
Spring Cache支持使用ConcurrentHashMap或ehcache集成，这时，不需要考虑序列化问题。  
Spring Cache也可以与Redis集成，这时需要考虑以下内容：  
- 默认使用java的对象序列化，对象需要实现Serializable接口  
- 自定义配置，可以修改为其它序列化方式  

### 

