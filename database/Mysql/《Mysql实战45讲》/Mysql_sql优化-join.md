问题：
1. 能不能使用join语句？  
> 结论：  
> 1. 如果可以使用"Index Nested-Loop Join"算法（也就是说：可以用上被驱动表上的索引），其实是没问题的。  
> 2. 如果使用"Block Nested-Loop join"算法，扫描行数就会过多。尤其是在大表上的join操作，这样可能要扫描被驱动表很多次，
> 会占用大量的系统资源。所以，这中join尽量不要用。  
>> ps：能否用上被驱动表的索引，1.决定了Mysql会使用哪种join算法，2.同时也对join语句的性能影响很大。  
2. 如果使用join，如何选择驱动表？  
> 结论：使用小表做驱动，  
> ps：怎么界定"小表"？--准确地说，应该是两个表按照各自的条件过滤，过滤完成之后，计算参与join
> 的各表的总数据量（=字段数*行数），数据量最小的那个就是小表。  


### "Index Nested-Loop Join"(NLJ)算法优化：  
1. Multi-Range Read优化--MRR
> MRR优化的目的是尽量使用顺序读盘。
> show variables like 'optimizer_switch';
read_rnd_buffer在回表时，用于缓存主键并排序。

2. Batched Key Access算法--BKA  
> 是"Index Nested-Loop Join"算法的优化。且依赖与MRR。  
> 开启方法：set optimizer_switch='mrr=on,mrr_cost_based=off,batched_key_access=on'  


### 总结：对于使用join业务的优化  
1. join语句的被驱动表建索引；  
2. 基于临时表（参考：[Mysql_临时表](Mysql_临时表.md)）优化。--被驱动表数据量虽大，但经过滤条件过滤后有效数据量不大，建立带索引但中间表存储有效数据，然后使用中间表做join的被驱动表。    
3. 业务上模拟"hash join"。--业务上做处理：使用过滤条件分别将两表有效数据读如内存，存入map中，让后做比较匹配，模仿Mysql中的join。--ps：性能比引入中间表做join稍好一些。  
> ps：前两种都是尽量将BNL算法转BKA算法。  

思考：-嵌套驱动问题（如何建索引）
select * from t1  
join t2 on(t1.a = t2.a)  
join t3 on(t2.b = t3.b)  
where t1.c > X and t2.c > X and t3.c > X;

思路：  
根据where过滤条件筛选出最小表；  
    如果最小表是t1，则t1是第一驱动表，则join驱动顺序是：t1-->t2-->t3，则t2索引建在a上，t3索引建在b上，并且在第一驱动表t1的a上建立索引。  
    如果最小表是t3，同理。  
    如果最小表是t2，则t2是第一驱动表，则需要在t1和t3中再筛选出一个小表，以t1是t1与t3中筛出的小表为例。则join驱动顺序：t2-->t1-->t3,
    则t1索引建在a上，t3索引建在b上，并且在第一驱动表t2的c上建立索引