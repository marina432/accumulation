1. procedure analyse();--分析表字段和表中实际数据，并给出可参考的有用建议：  
```shell
select * from sys_pay_record procedure analyse();
```
2. 将ip转 UNSIGNED INT 与 将 UNSIGNED INT 转ip 函数：  
```shell
mysql> select INET_ATON('10.4.104.37'), INET_NTOA(168060965);
+--------------------------+----------------------+
| INET_ATON('10.4.104.37') | INET_NTOA(168060965) |
+--------------------------+----------------------+
|                168060965 | 10.4.104.37          |
+--------------------------+----------------------+
1 row in set (0.00 sec)
```
3. 