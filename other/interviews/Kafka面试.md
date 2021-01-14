### 什么是分布式消息中间件？  









### Kafka  
#### 简单讲下Kafka的架构？  
> Producer, Consumer, Consumer Group, Topic, Partition  

#### Kafka是推模式还是拉模式？  
> Producer向Broker发送消息使用的是push模式。  
> Consumer消费采用的是pull模式。pull模式使得Broker server保持无状态性，让consumer自己管理offset，可以提高读取性能。  

#### Kafka如何广播消息？  
> Consumer Group：一个消费者组可以包含一个或多个消费者。通过多分区+多消费者方式可以极大提高下游的数据消费速度。
> 一个分区只能被该消费组中的一个消费者所消费，因此，同一消费者组中的消费者不会重复消费同一消息。
> 另，因为消费状态offset由consumer自己维护，所有，当不同消费组消费同一topic时，各组消费消息互不影响。  

#### Kafka的消息是否是有序的？  
> - Topic级无序：Kafka中的消息以topic为单位进行划分，生产者将消息发送到特定topic，而消费者负责订阅topic的消息并进行消费。
> 而topic是个逻辑概念，在Kafka中，同一个topic会被分成多个分区（且每个分区仅属于这一个topic）并将其分布在多个broker上，这多个分区可以并发读写。
> 因此，生产者在向同一个topic发送消息时，会负载均衡地将消息合理地发送到这些分布式的broker的该topic下属分区上。所以，Kafka在Topic层级是无序的。  
> - Partition级有序：Kafka用offset来保证消息在分区上的顺序性，是消息在分区上的唯一标识（不跨越分区）。  
> ps：阅读拓展：https://blog.csdn.net/weixin_43975220/article/details/93190906  

#### 如何让kafka的消息有序？
> Kafka在Topic级别本身是无序的，只有在Partition上才有序。
> 所以，为了保证处理顺序，可以自定义分区器，将需要顺序处理的数据全部发送到一个partition上来实现。  

#### Kafka是否支持读写分离？  
不支持，只有Leader对外提供读写服务  
> Kafka的同一个Partition的数据可以在多个broker存在多个副本，这是Kafka保持高可用的方式。
> 但通常只有该分区但Leader对外提供读写服务。只有在Leader所在但broker崩溃或者放上网络异常时，kafka会在controller但管理下重新选择新的Leader对外提供读写服务。  
> ps：Kafka controller的作用参考阅读：https://blog.csdn.net/yanerhao/article/details/106481252  

#### Kafka如何保证数据高可用？  
> - partition多副本存储：一个topic分为多个partition，每个partition以多副本多方式存储在多个broker上，整体上，保证了在Leader副本宕机的情况下，选举任意一个follower副本为leader数据大体上不丢失。
> - ACK+HW：只有Leader副本对外提供读写服务，当leader有消息写入后（leader都LEO会增大，HW不变），所有follower副本都会从leader来pull消息，leader在收到来所有ISR中副本都ack后，才认为这条消息被commit了，
> leader都HW才会相应后移，而消费者只能消费leader中HW前都消息。而producer收不到kafka到消息确认，可以选择重发消息。Kafka以这一整套机制来保证消息不丢失。  
> 参考阅读：https://blog.csdn.net/gongzhiyao3739124/article/details/79688813?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromBaidu-1.not_use_machine_learn_pai&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromBaidu-1.not_use_machine_learn_pai  
> 要点：（多）副本机制，producer ack，重试，自动leader选举，consumer自平衡（rebalance）

#### Kafka中zookeeper的作用？  
用于集群管理与元数据管理。包括：
1.broker注册；2.topic注册；3.生产者负载均衡；4.消费者负载均衡；5记录消费者与topic分区的对应关系，以及消费进度offset；6.Controller选举等等。  

#### Kafka是否支持事务？  
Kafka从0.11版本开始支持事务，可以实现"exactly once"  

#### Kafka分区是否可以减少？  
目前Kafka只支持增加分区数而不支持减少分区数。  

#### Kafka Porducer的执行过程？  
Producer消息--》拦截器--》序列化器--》分区器--》累加器--》（封装Request）--》broker  

#### Kafka如何保证数据发送不丢失？  
ack机制和重试机制  

#### 如果同一group下的consumer数量大于消费topic的partition数量，kafka如何处理？  
Kafka的分区只能被group中的一个consumer消费，则多余的consumer将处于无用状态，不消费数据，资源浪费。  

#### Kafka consumer是否线程安全的？  
不安全，单线程消费，多线程处理。  
（为了保证线程安全，并提上消费性能，可以在Consumer端采用类似Reactor线程模型来消费数据）

#### 讲一讲使用Kafka consumer消费消息时的线程模型，为何如此设计？  
拉取和处理分离  

#### Kafka producer的常见配置？  
borker，ack配置，网络和发送参数，压缩参数，ack参数

#### Kafka consumer的常见配置？  
broker，网络和拉取参数，心跳参数  

#### consumer什么时候会被踢出集群？  
奔溃，网络异常  
处理时间过长提交移位超时  

#### 什么是rebalance？何时会rebalance？  
> - consumer group成员变更（新增consumer，已有consumer主动离开组，已有consumer奔溃）  
> - 订阅的topic数发生变更（topic数量及topic的分区数） 

#### Kafka的交付语义？  
> 1. at least once(通过producer端配置ack)  
> 2. at most once(通过producer端配置ack)  
> 3. exactly once  

#### Replic的作用？  
实现数据高可用  

#### 什么是AR？ISR？  
> - AR：Assigned Replicas。AR是topic被创建后，分区创建时被分配副本的集合，副本个数由副本因子决定。  
> - ISR：In-Sync Replicas。Kafka中特别重要的概念，指代的是AR中那些与Leader（基本）保持同步的副本集合。leader天然在ISR中。与Leader延迟太多的follower副本在OSR中。AR=ISR+OSR。  

#### 如何判断follower副本是否应该属于ISR？  

#### Kafka中的HW代表什么？  
高水位值（High watermark）。这是控制消费者可读消息范围的重要字段，
consumer能够消费的partition上的消息的范围是Log start Offset到HW之前的所有消息，
HW以上的消息是对consumer尚不可见的。  

#### Kafka为保证优越的性能做了哪些处理？  
1. partition并发（集群优势+多磁盘优势）
    - 一方面，由于不同的partition可位于不同的机器上，因此可以充分利用集群优势，实现多机器间并行处理  
    - 另一方面，由于partition在物理上对应一个文件夹，即使多个partition位于同一节点上，也可以通过配置，让多个partition位于
    不同的disk driver上，从而实现磁盘间的并行处理，充分发挥多磁盘优势。  
      
2. 顺序读写磁盘  
    - Kafka每一个partition目录下的文件都被切割成大小相等的数据文件（默认一个文件是500M，可以手动设置），
    每一个文件都被成为一个段（segment file），每个segment都采用append的方式追加（日志log）数据。  
      
3. page cache：按页读取磁盘，也因此实现预读（broker将要消费的消息提前读入内存）  
4. 高性能序列化（二进制）  
5. 内存映射  
6. 无锁offset管理（consumer自行维护）：提高consumer group间并发能力  
7. Java NIO模型（consumer消费消息）  
8. 批量：批量读写  
9. 压缩：消息压缩、存储压缩，减小网络和IO开销  
