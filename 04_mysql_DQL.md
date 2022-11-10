查询数据 select
==
语法
--
```
select 字段 from ...;
select 字段1,字段2 from ...;
select 字段 as 字段别名 from ...;
select ... from ... where ...;
select ... from ... where ... order by ...;
```

- distinct 去除重复数据
```
select distinct a from ...  //查询并 去除重复数据
select distinct a,b from ...//查询并 去除a,b组合 重复数据
```

- as 字段别名
```
select name as n, age as a, gender as g
from ...                  //不能在where子句中, 使用字段别名!!
select name n, age a, gender g
from ...                  //可以省略as
select employee_id as id, concat(first_name,'',last_name) as name
from ...                  //连接字符串显示, 取别名
```

- where 子句 过滤条件
```
=          //等值过滤
<>         //不等过滤
> >= < <=  //比较过滤
between .. and ..  //大于等于/小于等于,必须从小到大查询
in         //从指定的一组值中取值
like       //字符串模糊查询
  %        //通配符,匹配0到多个任意字符
  _        //通配符,匹配单一任意字符
  注意: 若要匹配 '_'下划线,则需转义 '\_'
  比如: 若要匹配 'abc_ddd',则需'abc\_%'.
and        //与
or         //或
not        //非
  not between .. and ..
  not in
  is not null
is null    //空过滤,必须使用它判断空值.
```

- group by 子句 分组
求多行函数时,使用group by 进行分组计算
```
select department_id,count(*)
  from employees;
```

- having 子句 分组过滤条件
```
多行函数分组计算;对多行函数结果进行过滤;
不能用 where.
where: 过滤普通条件
having: 过滤多行函数结果
```
- order by 子句 排序
```
order by a,b           //默认升序
order by a desc, b asc //先按a 排序,相同a 按b排序
```

- 分页查询
没有标准的sql语句,不同数据库提供不同的分页查询方式;
mysql提供limit 语法;
```
limit 5     //前5条
limit 0,5   //第1行开始,前5条
limit 10,5  //第11行开始,前5条
```
