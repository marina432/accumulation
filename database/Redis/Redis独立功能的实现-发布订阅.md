## 发布与订阅  
### 频道的订阅与退订  
实现：Redis服务器将所有频道的订阅关系都保存在服务器状态的pubsub_channels字典里。该字典的键是某个被订阅的频道，而键的值则是一个链表，链表里存储的是所有订阅该频道的客户端。  
- 订阅频道命令：SUBSCRIBE "channel1" "channel2" ...  
- 退订频道命令：UNSUBSCRIBE "channel1" "channel2" ...  

### 模式的订阅与退订  
实现：Redis服务器将所有模式的订阅关系都保存在服务器状态pubsub_patterns属性里。pubsub_patterns属性是一个链表，链表节点是pubsubPatterns结构体，该结构体里有两个属性，其中pattern属性记录里被订阅的模式，client属性则记录里订阅模式的客户端。  
- 订阅模式命令：PSUBSCRIBE "pattern1" "pattern2" ...  
- 退订模式命令：PUNSUBSCRIBE "pattern1" "pattern2" ...  

### 发送消息  
- 发送消息命令：PUBLISH <channel> <message>  
原理：当服务器接收到客户端的PUBLISH发送消息命令后，需执行以下两个动作：  
    - 将message发送给channel频道的所有订阅者。--查看pubsub_channels字典  
    - 将message发送给与channel相匹配的pattern订阅者。--查看pubsub_patterns链表  
    
### 查看订阅信息（Redis2.8新增）  
ps：客户端可以通过该命令来查看频道或模式的相关信息。  
1. PUBSUB CHANNELS [pattern]  
该子命令是通过遍历pubsub_channels字典的所有键，然后记录并返回所有符合条件的频道来实现的。
   
2. PUBSUB NUMSUB [channel-1 channel-2 ... channel-n]  
该子命令通过在pubsub_channels字典中找到指定频道的订阅者链表，然后返回订阅者链表的长度来实现的。  
   
3. PUBSUB NUMPAT  
用于返回服务器当前被订阅模式的数量。该自命令是通过返回pubsub_patterns链表的长度实现的。