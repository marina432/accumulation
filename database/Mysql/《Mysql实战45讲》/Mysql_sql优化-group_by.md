### group by原理  
> 通过构建一个带唯一索引带临时表。

### group by执行流程  


### group by优化  
1. 基于索引。  
2. 直接排序。-SQL_BIG_RESULT。
> - 初始化sort_buffer。 
> - 扫描表，依次将group by的表字段值放入sort_buffer中。  
> - 扫描完成后，对sort_buffer中对数据做排序（如果sort_buffer内存不够，就会利用磁盘临时文件辅助排序）  
> - 排序完成后就得到一个有序数组。
> - 最后顺序遍历数组，计算统计值。