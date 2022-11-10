事务
==
- 事务时数据操作的最小单元.
- 多个数据的增删改操作,完成的一项业务处理
- 事务成功,则每一项操作都生效;事务失败,则每一项操作都失败;
- 事务只对当前连接可见
- 事务满足一下条件:ACID (原子性Atomic/一致性Consistency/隔离性Isolation/持久性Durancy)

事务操作
--
```
//开始事务
start transaction;/begin
set autocommit=0;  -- 改变自动提交模式
//提交事务
commit;
//回滚事务
rollback;
```

事务隔离级别
--
```
set tx_isolation='READ-UNCOMMITTED';  //会导致 幻读,不可重复读,脏读,一般不使用
set tx_isolation='read-committed';    //会导致 幻读,不可重复读
set tx_isolation='repeatable-read';   //会导致 幻读
set tx_isolation='serializable';      //事务排队依次执行,不能同时执行

show variables like 'tx%'; -- 查看隔离级别
```

数据访问冲突问题
--
- 脏读: 读取到其他事务未提交的数据
- 虚读(不可重复读): 一个事务添加或删除数据并未提交,再次查询的数据与第一次查询的数据不一致
- 幻读: 一个事务添加或删除数据并提交,另一个事务查询不到新数据
- 可重复读: mysql默认,repeatabe-read.事务前后读取的数据一致.
