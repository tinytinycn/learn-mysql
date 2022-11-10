1. 多行函数和字段 一般不能一起使用;
2. where和字段别名 一般不能一起使用;
3. group by分组查询结果 只能使用having过滤;
4. 编写 存储过程/函数/触发器 中代码时,记得改变结束符delimiter // (记得改回;结束符)
5. distinct 与group by 去重的结果相同:
- group by 对数据进行分组;而 distinct 对字段进行分组.
- group by 与 distinct加order by 结果相同.
- distinct 常常与聚合函数一起使用, count(distinct xxx)
6. 
