### 一、Mysql库、表名、表别名大小写敏感问题：  
> > 阿里公约：  
> 【强制】表名、字段名必须使用小写字母或数字，禁止出现数字开头，禁止两个下划线中间只出现数字。数据库字段名的修改代价很大，因为无法进行预发布，所以字段名称需要慎重考虑。  
> 说明：MySQL在Windows下不区分大小写，但在Linux下默认是区分大小写。因此，数据库名、表名、字段名，都不允许出现任何大写字母，避免节外生枝。  
> 正例：aliyun_admin，rdc_config，level3_name 反例：AliyunAdmin，rdcConfig，level_3_name

- Windows下Mysql：   
```shell
mysql> show variables like 'lower%';
+------------------------+-------+
| Variable_name          | Value |
+------------------------+-------+
| lower_case_file_system | ON    |
| lower_case_table_names | 1     |
+------------------------+-------+
2 rows in set, 1 warning (0.00 sec)
```
- Linux下Mysql：  
```shell
mysql> show variables like 'lower%';
+------------------------+-------+
| Variable_name          | Value |
+------------------------+-------+
| lower_case_file_system | OFF    |
| lower_case_table_names | 1     |
+------------------------+-------+
2 rows in set, 1 warning (0.00 sec)
```
ps：我们的Linux上的测试Mysql已被对dba将lower_case_table_names从默认值0设置为1。  

关于这个两个参数：  
1. lower_case_file_system   
   > 此变量描述数据目录所在的文件系统上文件名的区分大小写情况。  
   >> 在Mysql中，数据库和表都是对应目录下的一个或多个文件。因此，操作系统的大小写敏感与否在决定了数据库库名、表名的敏感与否中占一席影响之地。  
   > ps：此变量是read-only的，因为它只是显示了Mysql当前所依托的文件系统的属性，设置它影响不了文件系统。   
   
   OFF：表示文件名区分大小写  
   ON：表示文件名不区分大小写
2. lower_case_table_names  
   ps：属静态，设置后需重启服务方才生效。  
   > 可设置为：0、1、2  
   > - 0（Unix、Linux默认）--大小写敏感。
   > 原理：创建库、表时，库、表名将原样保存保存到磁盘上。SQL也会原样解析。  
   > - 1（Windows默认）--大小写不敏感。  
   > 原理：创建库、表时，Mysql会将所有库表名转换成小写保存到磁盘上。SQL解析时也会将库、表名转换成小写。  
   > - 2（MacOS默认）--大小写不敏感。  
   > 原理：创建的库、表会原样保存在磁盘上，但SQL语句却会将库、表名转换成小写。  
   >> 总结一下：取值为0时，SQL解析时对库、表名做原样解析；取值非0时，SQL解析时将库、表名转换成小写。  
   
### 二、Mysql列名、列别名、字段内容大小写敏感问题  
> 1.列名与列的别名在所有的情况下均是忽略大小写的；  
> 2.字段内容默认情况下时大小写不敏感的。  

#### 2.1 令字段内容大小写敏感两种方案：  
1. 方案一：使用Mysql的 BINARY 关键字，使搜索时强制区分大小写。  
```shell
select * from sys_pay_record where binary serge = 'A24171188161716' limit 10;
```
ps：1.优点：这种方式相对简单，不用改动表结构，只需在需要区分查询的字段前加上 binary 关键字即可。  
   2.缺点：如果该需要区分的查询的字段上有索引，将造成索引失效。  
   > SELECT * FROM table where b_ = binary 'B'; 使用索引   
   > SELECT * FROM table where binary b_ = 'B';不使用索引
>> 因为，默认情况下（utf8_general_ci），对Mysql中的字段进行查询或者排序都是不区分大小写的。  
   
2. 方案二：在建表时进行限制（实质是设置校验规则为：COLLATE utf8_bin）  
第一种：只对目标字段加校验规则COLLATE utf8_bin
```shell
CREATE TABLE `tb_user1` (
    `id` BIGINT (20) UNSIGNED NOT NULL AUTO_INCREMENT COMMENT '用户id',
    `username` VARCHAR (50) BINARY NOT NULL COMMENT '用户名',
    PRIMARY KEY (`id`)
) ENGINE = INNODB DEFAULT CHARSET = utf8 COMMENT = '用户表';


mysql> show create table tb_user1;
tb_user1 | CREATE TABLE `tb_user1` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT '用户id',
  `username` varchar(50) CHARACTER SET utf8 COLLATE utf8_bin NOT NULL COMMENT '用户名',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='用户表'
1 row in set
```
或者，第二种：整表加校验规则COLLATE utf8_bin
```shell
CREATE TABLE `tb_user2` (
    `id` BIGINT (20) UNSIGNED NOT NULL AUTO_INCREMENT COMMENT '用户id',
    `username` VARCHAR (50) NOT NULL COMMENT '用户名',
    `info` VARCHAR (100) NOT NULL COMMENT '详情描述',
    PRIMARY KEY (`id`)
) ENGINE = INNODB DEFAULT CHARSET = utf8 COLLATE=utf8_bin COMMENT = '用户表';

mysql> show create table tb_user2;
tb_user2 | CREATE TABLE `tb_user2` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT '用户id',
  `username` varchar(50) COLLATE utf8_bin NOT NULL COMMENT '用户名',
  `info` varchar(100) COLLATE utf8_bin NOT NULL COMMENT '详情描述',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='用户表'
```

#### 2.2 关于Mysql的校验规则  
字段值的大小写由mysql的校对规则来控制。提到校对规则，就不得不说字符集。  
字符集是一套符号和编码，校对规则是在字符集内用于比较字符的一套规则。  
一般而言，校对规则以其相关的字符集名开始，通常包括一个语言名，并且以ci（大小写不敏感）、cs（大小写敏感）或_bin（二元）结束 。

比如 utf8mb4字符集，，如下表：
1）utf8mb4_bin：将字符串中的每一个字符用二进制数据存储，区分大小写。
2）utf8mb4_general_ci：不区分大小写，ci为case insensitive的缩写，即大小写不敏感。
3）utf8mb4_general_cs：区分大小写，cs为case sensitive的缩写，即大小写敏感。

- 查询校对规则参数设置：  
```shell
mysql> show variables like 'collation%';
+----------------------+--------------------+
| Variable_name        | Value              |
+----------------------+--------------------+
| collation_connection | utf8mb4_general_ci |
| collation_database   | utf8mb4_general_ci |
| collation_server     | utf8mb4_general_ci |
+----------------------+--------------------+
3 rows in set, 1 warning (0.00 sec)
```


### 三、参考阅读：  
> 关于Mysql库、表名、表别名大小写敏感问题，参考阅读：https://www.cnblogs.com/aflyun/p/11013604.html  
> 关于Mysql列名、列别名、字段内容大小写敏感问题，参考阅读：https://www.cnblogs.com/aflyun/p/11047737.html  