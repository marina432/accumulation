





### MQ的四大作用  
- 异步通信
  - 异步通信，减少线程等待，特别是处理批量业务等大事务、耗时的操作。
- 系统解耦  
  - 系统间不直接调用，降低依赖，特别是服务不在线也能保证通信最终完成。  
- 削峰填谷  
  - 压力大时，缓冲部分请求消息，类似于背压处理  
    ps：[关于backpressure与背压处理](https://www.zhihu.com/question/49618581/answer/237078934)  
- 可靠通信  
  - 提供多种消息模式、服务质量、顺序保障等。  
    
### MQ（选型）的几个思考项  
- 关于性能：吞吐量与延迟  
- 关于选型：场景与指标  
- 关于并发：抽象与封装  
- 关于维护：稳定性与高可用  
> 建议：复杂情况下，做好选型的PoC（Proof of Concept）测试。将业务中使用到其的场景简化成代码，实现完成后去压测几种备选的MQ，
> 最终，根据压测数据以及个个MQ在该特定场景中的表现作出选择。  


### 消息模式与消息协议  
#### 消息处理模式：  
- 点对点（P2P）：Point-To-Point，对应与Queue  
- 发布订阅（PubSub）：Public-Subscribe，对应与Topic  

#### 消息处理的保障：  
1. 三种QoS（Quality of Service，服务质量）（注意：这是消息语义（层面的保障），而不是业务语义层面的）
- At most once，至多一次。  --消息可能丢失，但不会重复发送。
- At least once，至少一次。 --消息不会丢失，但可能重复发送。  
- Exactly once，精确一次。  --每条消息肯定会被传输一次，且仅传输一次。  
> ps：实际业务系统中，都使用"At least once"，针对它存在都"消息重复发送"问题，有以下两种解决方式：
> 1 业务做幂等处理。  
> 2 在数据库之外，业务系统中做去重处理。（推荐：RoaringBitmap）  

2. 消息处理的事务性  
- 通过确认机制实现事务性  
- 可以被Spring事务管理器管理，甚至可以支持（分布式）XA协议。  

#### 消息的有序性  
同一个Queue或Topic的消息，在没做其它操作处理时，理论上是保障按顺序投递的。  
但，如果做了消息分区、或批量预取之类的操作，可能就没有顺序了。  


#### 消息协议  
- STOMP  --所有交互信息为文本的  
- JMS *  --J2EE子协议规范，只是客户端的协议，（并不完整）只给出一组接口，需由对应的MQ的实现方自己来实现。对服务端有一点要求：要求服务端在接收到客户端的请求后，必须要先将其持久化到本地。  
- AMQP *  --（完整协议，具有一致性、通用性。可移植）RabbitMQ  
- MQTT *  --（完整协议，具有一致性、通用性。可移植）用在物联网方面比较有优势  
- XMPP  
- Open Messaging  --阿里巴巴的
> ps：其中*标注的为最为常用的协议   
> 另，所谓的完整协议，指把对服务端所有的命令、格式、交互、异常处理等等都做了完备的描述，同时描述了客户端与服务器端的行为。  

#### JMS(Java Message Service)协议  
关注于应用层的API协议（类似JDBC）  

#### 消息队列通用结构  
#### MQ的封装层级  
- 客户端应用层：发送和接收消息的API接口  
- 消息模型层：消息、连接、会话、事务等  
- 消息处理层：消息交互逻辑定义、持久化  
- 网路传输层：序列化协议、传输协议、可靠机制  
- 安全层：加密协议、用户权限、身份认证  
- 管理层：如何管理MQ  


## 三代开源消息中间件/消息队列
- No.1代：ActiveMQ/RabbitMQ---基于JMS、AMQP等协议  
  > 第一代特点：1.基于内存，不支持堆积，因此这一代内存是最大问题。  
  > 支持P2P、PubSub两种消息处理模式  
- No.2代：Kafka
          /RocketMQ--衍生自Kafka  
  > 第二代特点：基于磁盘和内存，支持堆积，默认消息是不会删除代。使用WAL技术，将消息追加性地不断写入。--消息存储在borker上  
  > 支持PubSub消息处理模式  
- No.3代：Apache Pulsar  
  > 第三代特点：计算（对外服务）节点与存储节点分离。实现计算节点、存储节点各自独立扩展。
  > 支持P2P、PubSub两种消息处理模式
  

### ActiveMQ  
1. 使用场景  
- 所有需要使用消息队列的地方；  
- 订单处理、消息通知、服务降级等等；  
- 特别地，它是纯Java语言实现，支持嵌入到应用系统。  











附：  
#### 1. 死信队列（Dead Letter Exchange）
> Messages from a queue can be "dead-lettered"; that is, republished to an exchange when any of the following events occur:  
> 1. The message is negatively acknowledged by a consumer using basic.reject or  basic.nack with requeue parameter set to false.  
> 2. The message expires due to per-message TTL; or  
> 3. The message is dropped because its queue exceeded a length limit  





cammel可实现MQ中消息迁移  
《Enterprise integration patterns》  
