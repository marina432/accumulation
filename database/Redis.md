






### 跳跃表
Redis只在两个地方用到了跳表：  
- 一个是实现有序集合键  
- 另一个是在集群节点中用作内部数据结构  

### 跳表到实现  
1. zskiplist  
- header  
- tail  
- level 
- length  

2. zskiplistNode  
- level ***(实现跳表索引动态更新，新节点到level根据幂次定律随机生成)  
- backward  
- score  
- obj  





很好的参考阅读：https://cloud.tencent.com/developer/article/1353762
