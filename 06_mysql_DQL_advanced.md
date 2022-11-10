DQL_advanced
==
* 子查询
* 连接查询
* mysql 标准连接语法
* 视图
----
子查询
--
子查询允许把一个查询嵌套在另一个查询当中.

子查询,又叫内部查询.相对于外部查询,包含内部查询的就称为外部查询.

- where 子查询
用一个查询的结果,作为另一个查询的过滤条件.
```
select id,name,salary
  from employees
  where salary=(select min(salary) from employees);

//符号后面为条件,单值多值取决 上一次查询数据结果,单行多行取决 上一次查询字段数目.
//单列单值子查询   = <> > <
//单列多值子查询
in     (是否在其中,在,则返回TRUE) 同=any
not in (同<>all )
all (所有的)   >all   大于所有值则返回TRUE
any (任意一个) >any   大于任意值则返回TRUE
//多列单值/多值子查询     (a,b) in ...
```  
- from 子查询(行内视图)
把内层查询的结果,供给外层再次查询.
```
//挂科两门以上的学生名字和挂科数目
select name,count(*) as gg from S
  where score<60
  group by name
  having gg>=2;
//挂科学生中,姓李的同学
select name
  from (select name,count(*) as gg from S where ....) as t   //必须取别名**
  where name like '李%';
```

- 字段列表 子查询
```
select a,b,(select ...from ...) from...;
```

- exits 子查询
把外层查询的结果,供给内层查询,是否成立
```
//学生对应的学号/姓名/分数
//内层查找 有成绩的学生(怎么知道id 001的学生又没成绩?外层传入id到内部查询,类"全局变量")
//外层查找 详细信息
select id,name,score from Students
  where exists(
    select * from Scores where Scores.id=Students.id;
    );
```
- 独立子查询和 相关子查询
子查询依赖外部查询的某些字段，这就导致子查询就依赖外部查询，就产生了相关性.

使用独立子查询，如果子查询部分对集合的最大遍历次数为n，外部查询的最大遍历次数为m时，我们可以记为：O(m+n)。而如果使用相关子查询，它的遍历次数可能会达到O(m+m*n)。可以看到，效率就会成倍的下降；所以，在使用子查询时，一定要考虑到子查询的相关性。

- 子查询总结：　　
　　1. where型子查询：把内层查询的结果作为外层查询的比较条件。

　　　 from型子查询：把内层的查询结果当成临时表，供外层sql再次查询。查询结果集可以当成表看待，临时表需要一个别名。

　　　 exists型子查询：把外层sql的结果，拿到内层sql去测试，如果内层的sql成立，则该行取出。内层sql是exists后的查询。

 　　　　

　　2. 子查询也可以嵌套在其它子查询中，嵌套程度可以很深。子查询必须要位于圆括号中。

 

　　3. 子查询的主要优势为：

　　　　　　子查询允许结构化的查询，这样就可以把一个语句的每个部分隔离开。

　　　　　　有些操作需要复杂的联合和关联。子查询提供了其它的方法来执行这些操作。

　　

　　4. ANY关键词必须后面接一个比较操作符。ANY关键词的意思是“对于在子查询返回的列中的任一数值，如果比较结果为TRUE的话，则返回TRUE”。 　　

　　　 词语 IN 是 ＝ANY 的别名，二者效果相同。

　　　 NOT IN不是  <> ANY  的别名，但是是  <> ALL  的别名。

 　　

　　5. 词语ALL必须接在一个比较操作符的后面。ALL的意思是“对于子查询返回的列中的所有值，如果比较结果为TRUE，则返回TRUE。”

 

　　6. 优化子查询

    　　①. 有些子句会影响在子查询中的行的数量和顺序，通过加一些限制条件来限制子查询查出来的条数。例如：

　　　　　　SELECT * FROM t1 WHERE t1.column1 IN (SELECT column1 FROM t2 ORDER BY column1); 

　　　　　　SELECT * FROM t1 WHERE t1.column1 IN (SELECT DISTINCT column1 FROM t2);

　　　　　　SELECT * FROM t1 WHERE EXISTS (SELECT * FROM t2 LIMIT 1);

 　　　 ②. 用子查询替换联合。例如：

　　　　　　SELECT DISTINCT column1 FROM t1 WHERE t1.column1 IN (SELECT column1 FROM t2);

　　　　　　代替这个：SELECT DISTINCT t1.column1 FROM t1, t2 WHERE t1.column1 = t2.column1;


----
连接查询
--
将多张表(>=2)进行记录的连接(按照某个指定的条件进行数据拼接).

连接查询的意义: 在用户查看数据的时候,需要显示的数据来自多张表.
- 交叉连接
```
select *
  from left_table cross join right_table;
或
select *
  from left_table,right_table;
//  
//最终结果:笛卡尔积  
```
- 内连接(见下)
```
select *
  from left_table [inner] join right_table on left_table.字段=right_table.字段;
```
- 外连接(见下)
```
select *
  from left_table left/right join right_table on left_table.字段=right_table.字段;
```
- 自然连接(略)

- 多表连接查询
用外键将多张表连接成一张大表
```
select a.xx,a.yy,a.zz, b.uu,b.vv
  from a,b
  where a.id=b.mid;
```
- 自连接查询
外键与本表中的主键连接; 将一张表看做是两张表
```
select a.xx,b.yy
  from emp a,emp b
  where a.id=b.id;
```

- 标准的sql语句 **连接语法**
```
//内连接: 等值连接
select ...
  from
    a inner join b on a.id=b.mid
      inner join c on c.id=a.mid
      [inner] join d on ...
      [inner] join e on ...

//外链接: 连接条件以外的数据,也显示出来
select ...
  from
  a left [outer] join b on ...
  a right [outer] join b on ...
```
![inner join](img_innerjoin.gif)

![left join](img_leftjoin.gif)

![right join](img_rightjoin.gif)

- 视图
将一个查询结果保存到数据库中;
```
//作用:
//简化查询
//安全: 创建视图给用户访问,让低权限用户只能查询,不能触碰真实数据表
//一般只从视图查询,不对视图做增删改操作
create [or replace] view view000                   -- 创建视图
  as
    select ... from ...;
show tables;                          -- 查看视图,同表
desc view000;
show create view view000;
drop view view000;                    -- 删除视图    
```

- 行内视图
从一个查询的结果中 再查询;行内视图后必须加一个别名;
```
select ... from (select ... from ...) 别名1 ;
```
