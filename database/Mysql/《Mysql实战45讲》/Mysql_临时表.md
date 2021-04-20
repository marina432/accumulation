### 1. 概念区分  
- 临时表：可以是各种引擎类型的表。
  如果使用Innodb或MyISAM引擎的话，数据要持久化到磁盘。  
- 内存表：仅指的是使用Memory引擎的表，建表语句："create table XX ... engine=memory;"。
  这表的数据都保存在内存里，系统重启的时候数据会被清空，唯独表结构还在。  

### 2. Mysql临时表特点  
- 建表语句：create temporary table XX ...;  
- 作用域及自动清理：仅限在创建它的session内。session结束时，自动删除相关临时表。  
- 可以与session作用域内的普通表同名。发生session内同名时，show create语句、CRUD语句只能访问到同名的临时表。  
- show tables命令不显示临时表。  



### 3. Mysql中自动使用临时表的语句
- UNION语句。--（因为涉及到去重，所以要使用临时表进行暂存、比较。ps：UNION ALL因为不涉及去重问题，查询结果直接输出，所以不会使用临时表）  
- group by语句。--不过可以优化掉临时表（参见：[Mysql_sql优化-group by](Mysql_sql优化-group_by.md)）
> ps：当数据量超出内存临时表设置当阈值（tmp_table_size）后，就会把内存临时表转成磁盘临时表。磁盘临时表默认使用InnoDB引擎。（而内存临时表默认使用MyISAM引擎）  


### 4. 总结：Mysql何时会使用内部临时表？  
