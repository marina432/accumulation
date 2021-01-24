### 一、通用事务概述
#### 1.1 事务特性：ACID
- 原子性（Atomicity）  
- 一致性（Consistency）  
- 隔离性（Isolation）  
- 持久性（Durability）  

#### 1.2 多事务间存在的可见性问题  
- 脏读（dirty read）：读到其它事务未提交到数据  
- 不可重复读（non-repeatable read）：事务内，前后读取到记录内容不一致  
- 幻读（phantom read）：事务内，前后读取到记录数量不一致  

#### 1.3 SQL标准的事务隔离级别  
- 读未提交（read uncommitted）  
- 读已提交（read committed）--oracle默认隔离级别  
- 可重复读（repeatable read）--mysql默认隔离级别  
- 串行化（serializable）  
> 因oracle与mysql的默认隔离级别不同，所以，对于一些从oracle迁移到mysql到应用，为保证数据库隔离级别一致，务必要记得将
> mysql到隔离级别设置为'READ-COMMITTED'。  
> 设置方式：修改启动参数'transaction_isolation'的值为'READ-COMMITTED'。  
> 查看方式：show variables like 'transaction_isolation';  

> ps：任何sql都不能脱离事务单独执行。  
> 很多时候，客户端在没有使用begin...cimmit的情况下，输入一条sql就回车执行成功的现象，看似sql脱离了事务而独立执行。
> 其实不然，之所以有这种假象，是因为该连接线程的默认提交方式是自动提交autocommit=1，我们输入的这条sql的执行触发了一次事务，在该sql执行完毕后该事务又自动提交了，
> 属于隐式开启事务并自动提交，所以，仍该sql没有逃离事务。

### 二、MySQL事务启动与提交 
#### 2.1 事务的启动方式  
- 隐式开启事务：引擎在执行sql时，如发现没有开启事务，会先自动开启事务，再执行sql。  
- 显式开启事务：可以使用语句"begin;/start transaction;/start transaction with consistent snapshot;"来显示开启事务。  
特别要说明的一点是，显式开启的事务最终只能配套地使用显式地commit/rollback。  
> 其中，"begin;/start transaction;"只是声明即将开启事务，但尚未开启，这时用"select * from innodb_trx\G;"还查不到事务。
> 当声明的事务块内的第一条sql执行时才真正触发事务开启。  
> 与上面两种显式方式不同，"start transaction with consistent snapshot;"语句会立即触发事务开启。 
> 事务开启时，才会向Innodb引擎对事务系统申请事务id。  
> 另，实验证明："begin;/start transaction;/start transaction with consistent snapshot;"事务块声明命令会立即触发
> 当前线程尚未提交的事务隐式提交。  --即：显式提交命令会触发当前线程尚未提交事务的隐式提交（事务是跟线程一对一绑定的）

#### 2.2 事务的提交方式  
- 自动提交：当当前连接线程"set autocomit=1"时，显式声明事务块以外的sql执行完毕后就自动提交/回滚。  
- commit显式提交：当当前线程"set autocomit=0"时，事务不会自动提交，会一直持续到显示地commit或rollback。（ps：
  当然，事务块中可能会有某写命令会触发隐式提交的情况除外<eg. 1.alter ...; 2.begin/start transaction; ...>）  
  
#### 2.3 事务启动、提交方式建议
> 自动提交（set autocommit=1） + 显式开启  

在线程set autocommit=1（自动提交）的情况下，用begin显示开启的事务，如果执行commit则提交事务。
如果执行commit work and chain，则提交事务并启动下一个事务，这样可以省去再次执行begin语句的开销。  

#### 2.4 关于长事务  
##### 2.4.1 长事务的弊病  
- 相对与短事务会占用更多的存储空间（undo log）  
- 占用锁资源时间更长（可能导致其它事务lock timeout，默认50s）  

##### 2.4.2 如何避免长事务  

##### 2.4.3 如果查询长事务  
> select * from information_schema.innodb_trx where time_to_sec(timediff(now(), trx_started)) > 60;  



### 三、MySQL事务隔离的实现
#### 3.1 Mysql里的两个视图  
- view视图：它是一个用查询语句定义的虚拟表，在被调用的时候，执行查询语句并生成结果，
  这些生成结果就是这张虚拟视图（表）的数据。

- consistent read view一致性读视图：是innodb在实现mvcc时用到的一致性读视图。
  用于支持rr及rc隔离级别的实现。（没有物理结构，作用是：用来定义事务执行期间我能看到什么数据）  
  
#### 3.2 MVCC实现Mysql事务隔离  
> Innodb利用"所有数据都有多个版本"的特性，实现了"秒级创建快照"的能力。  
> ps：Innodb每个事务都有一个唯一是事务ID（transaction id），它是在事务真正开启时想Innodb的事务系统申请的，
是按申请顺序严格递增的。每次事务中更新数据都会生成一个新的数据版本，并将该transaction id赋值给row trx_id。  

##### 3.2.1 read-view（快照，基于整库的）--用于保证MVCC的一致性读  
Innodb在接到创建read view快照的命令时，会构造一个数组，用于保存当前数据库中正在"活跃"（启动了，但尚未提交）的所有事务ID。  
- 低水位：数组中但最小事务id  
- 高水位：数组中但最大事务ID+1  
> 事务的可见性就由这个数组和高水位组成的一致性视图read-view来保证：  
> 1. 若行记录版本中row trx_id字段小于当前快照中低水位值，则该数据版本对当前事务可见。  
> 2. 若行记录版本中row trx_id字段大于等于高水位值，则该数据版本对当强事务不可见。  
> 3. 若行记录版本中row trx_id字段值介于高、低水位值之间时：
> - 若row trx_id在数组中，则该数据版本对当前事务不可见。
> - 若row trx_id不在数组中，则该数据版本对当前事务可见。  

通过控制一致性读视图的创建时机，来控制Innodb的隔离级别：  
- 读未提交--直接"当前读"就可以，无需创建一致性读视图。
- 读已提交--事务内，每个语句执行前都会创建一致性读视图，然后该sql基于新创建的一致性读视图获取可见数据。    
- 可重复读--事务开启时（第一条sql），为整个事务一次性创建一个一致性读视图，然后事务内所有的普通读取数据sql都基于该视图获取可见数据。    
- 串行化--通过加锁实现，无需创建一致性读视图。    

#### 3.2.2 一致性读与当前读  
- 一致性读：从当前内存中获取记录行（可能是脏页，也可能是干净页，如果内存中没有则需从磁盘读入内存）开始，借助事务的read-view快照，沿着回滚链，
逐步回放undo log生成数据，直到找到第一个对当前事务可见的数据版本为止。  
  
- 当前读：直接获取内存中的记录行（可能是脏页，也可能是干净页，如果内存中没有则需从磁盘读入内存）  

> rr可重复读（隔离级别）的核心是一致性读；  
> 而事务更新数据的时候，只能用当前读。（更新数据都是先读后写的，如果这个读是基于一致性读的，会造成其它事务丢失更新，所以
> 更新数据里的先"读"一定是"当前读"）。
>> ps：除了update必须是当前读外，select在加锁的情况下也可以实现当前读：  
>> - select ... lock in share mode;     //读锁（S锁，共享锁）  
>> - select ... for update;     //写锁（X锁，排它锁）  











### 附：
1. 事务的概念是什么？  
2. 事务的隔离级别有哪些？怎么理解？（mysql的默认隔离级别）  
3. rc、rr是怎么通过视图构建实现的？  
4. 可重复读使用场景举例？  
5. 事务隔离是怎么通过read-view（读视图）实现的？  
6. mvcc的概念是什么？是如何实现的？  
7. 使用长事务的弊病？（为什么可能拖垮整个库？）如何避免长事务？  
8. 如何查询长事务？  
9. 事务启动方式有哪几种？  

