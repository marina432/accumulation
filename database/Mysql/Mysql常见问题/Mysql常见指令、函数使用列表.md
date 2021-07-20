1. procedure analyse();  
   --分析表字段和表中实际数据，并给出可参考的有用建议：  
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
3. Mysql 的 character_length(xx) 与 length(xx)：  
- character_length()  
  --单位为字符  
  --不管是汉字、数字、还是字母都算一个字符  
- length()  
  --单位为字节  
  --utf8 编码下，一个汉字字符占三个字节空间，一个数字、字母字符占一个字节空间  
    gbk 编码下，一个汉字字符占两个字节空间，一个数字、字母字符占一个字节空间  
> ps：字符是逻辑概念，字节则是具体到底层的实际物理存储了。  
>     length()<>char_length()可以用来检验是否含有中文字符  

4. 