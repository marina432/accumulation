






InnoDB引擎有两块非常重要都日志  
- undo log--保证事务都原子性，以及InnoDB的MVCC  
- redo log--保证事务的持久性  
> WAL, 事务提交前写入内存log buffer，commit时将logger buffer写入redo log（及binlog），等系统空闲或
> 刷内存脏页时数据落盘。  
> 
