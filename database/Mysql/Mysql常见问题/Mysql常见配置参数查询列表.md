1. 校对规则查询：
```shell
SHOW VARIABLES LIKE 'collation%';
```
> 参考阅读：http://c.biancheng.net/view/7543.html

2. 查询大小写敏感与否参数设置：  
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
3. 