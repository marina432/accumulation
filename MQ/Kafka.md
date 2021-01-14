

### Kafka Producer  
Producer发送消息的过程:  
Producer消息--》拦截器--》序列化器--》分区器--》累加器--》（封装Request）--》broker
