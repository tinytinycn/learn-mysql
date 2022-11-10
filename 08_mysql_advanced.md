advanced
==
* 会话变量
* 存储过程
* 函数
* 触发器
----
会话变量
--
- 一次会话过程中,使用的变量
- 可以自定义任意变量保存任意数据
- 可以访问数据库的系统变量
```
//@表示会话变量
//@@表示全局变量,指定是系统变量
set @var1='abc';              -- 自定义变量
select @var1;                 -- 查询变量
set @@tx_isolation='';        -- 修改系统变量;
```

存储过程
--
存储在数据库服务器上的一段程序代码

- 创建存储过程
```
delimiter //            -- 自定义代码的结束符
create procedure p1()
  begin
    select * from users;
  end;
  //                   -- 代码结束符
delimiter ;            -- 设置回;分号结束符   
```
- 调用存储过程
```
call p1;
```
- 删除存储过程
```
drop procedure [if exists] p1;  
```
- 参数:

存储过程的参数: in 输入参数 out 输出参数 inout 即可输入也可输出
```
delimiter //
create procedure p2(in a int, out b int)
  begin
  ...
  set b = a*2;
  end;
  //
delimiter ;   
call p2(255, @var1);
select @var1;  
```
- 流程控制
```
if .. then
  代码
end if;
-----------
if .. then
  代码
else
  代码
end if;
-----------
case
  when 条件 then ...;
  when 条件 then ...;
  else ...;
end case;
-----------
while 条件 do
  代码
end while;
-----------
loop
  代码
end loop;
-----------
repeat
  代码
until 条件 end repeat;
-----------
lp:loop -- 循环命名
leave   -- 跳出循环
iterate -- 跳到下次迭代  
```
- 局部变量
```
declare a int;
declare a int default 1;
局部变量在end 结束时,销毁
```
- 查看存储过程
```
show create procedure p1\G;
show procedure status\G;
show procedure status where db='tinydb'\G;
```

函数
--
- 和存储过程类似
- 函数有返回值
- 调用函数: select func()
```
create function f(arg) returns int
  begin
  ...
  return 0;
  end;
```

触发器
--
- 对一行数据进行增删改操作,可以触发一段代码执行
- 一张表最多能创建6个触发器:
- before insert
- before update
- before delete
- after insert
- after update
- after delete
- 包含两个隐含对象:
- new 新的数据行
- old 旧的数据行

```
//创建
create trigger 名称 before insert
  on tb for each row
  begin
    ...
  end;

//查看
use information_schmea;
select * from triggers\G;  

//范例
delimiter //
create trigger user_before_update_trigger
  before update on tb_user for each row
    begin
      set new.updated=now();
    end;
//
create trigger item_before_delete_trigger
  before delete on tb_user for each row
    begin
      delete from 不予许删除商品;  -- 暴力出错
    end;
//

```
