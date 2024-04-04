`select` 查询
`from` 连接
`as` 起别名
```sql
-- 注意，这个查询是在当前数据库的
select cname from student
select * from student --查询所有学生记录
select *,sname, age from student  -- 可以人为改变列的显示次序
select sno, 2024-age as birthday from student -- 起别名 as 
select *, lower(dept) department from student -- 按小写字母显示
```

`where`匹配
```sql
select * from sc where cno='c2'  -- 查询选了c2课程的学生，注意char要用''
select * from student where age >= 20
select * from student where age <> 20  -- 查询年龄不为20的学生
select sno, grade from sc where cno='C2' and grade >= 85 -- 多个查询条件
select sno, grade from sc where (cno='C2' or cno ='C1') and grade >= 70 
-- 多个查询注意优先级
select sno, grade from sc where grade >= 65 and grade <= 75
select sno, grade from sc where grade between 60 and 75   -- 两种写法等价
```

`distinct`去重
`all` 所有(默认)
```sql
select distinct sno from sc where cno='C1' or cno='C2'
select all sno from sc where cno='C1' or cno='C2'
```
`in` 查询类中元素
`not in` 
```sql
select all sno from sc where cno in ('C1', 'C2', 'C3')
select * from student where dept not in('CS', 'MA')
```

`like`模糊查询
```sql
select * from student where dept like 'CS'  -- 相当于 = 
select * from course where cname like 'P%' -- 查询以P开头的课程，这里不能用=
select * from course where cname like '_o%' -- 查询第二个字母为o的课程
select * from student where sname like '赵%' -- 查询姓赵的学生

```
`%` 0-多个通配符
`_` 1个通配符

`is null` 查询空值
```sql
select * from course where len(trim(cname))=0
select * from course where cname is null
```

`desc` 降序排列
默认升序排列，可省略
```sql
select * from sc where cno='C1' order by grade desc -- 成绩降序排列
select * from student where sno in ('S1', 'S3', 'S5') order by age desc -- 年龄降序排列
select * from student order by dept, age desc  -- 先按系升序(省略)，再按年龄降序排列
```

`sum，avg，count，max，min` 聚合函数
```sql
select avg(age) as Average_age from student where dept='CS' 
select avg(grade) as Average_grade, sum(grade) as Sum_grade from sc  where sno='S3' 
select count(distinct dept) as Sum_dept from student -- 统计有多少系

select max(grade), min(grade), max(grade)-min(grade) as diff from sc where cno = 'C3' -- 最大最小
```

`group` 分组统计
```sql
select cno, count(*) as 人数 from sc group by cno -- 统计每门课有多少人学
select sno, count(*) as 门数 from sc group by sno -- 统计每人学习门数
```

`having`拥有属性(在group by 用于条件查询)
```sql
select sno, count(*) as 门数 from sc group by sno having count(*) >= 4 -- 统计至少选修了4门课程的学号和门数

-- 统计选了4门以上学生的平均成绩
select sno, avg(grade) as 平均成绩 from sc 
where grade >= 60 
group by sno
having count(*) >= 4
order by 平均成绩 desc 
```

`from` 直接用是笛卡尔积
```sql
-- 笛卡尔积
select * from student, sc
-- 自然连接
select student.sno, sname, cno, grade
from student, sc where student.sno = sc.sno
```

复合连接
```sql
select cno, grade from student, sc
where student.sno = sc.sno and sname = '张三'

-- 连表查询
-- 每个人的课程与对应成绩
select sname, cname, grade from sc, student, course
where student.sno = sc.sno and sc.cno =course.cno

-- 自连接，找到和吴二同龄的人的名字
select a.sname from student a, student b
where b.sname = '吴二' 
and a.age = b.age
and a.sname <> '吴二'
```

嵌套子查询
```sql
select sname from student where age = 
(select age from student where sname = '吴二')
-- 找到选了数学的人的课程号和名字
select sno from sc where cno = (select cno from course where cname='MATHS')
-- 加上名字
select sname from student where sno in (select sno from sc where cno = (select cno from course where cname='MATHS'))
-- 每门课程成绩>80
select sname from student where sno not in (select sno from sc where grade < 80)
```

`all`所有
```sql
-- C1成绩最高的学号
select sno from sc 
where grade >= all(select grade from sc where cno = 'C1') and cno = 'C1'
-- 查询没有选修C6课程的学生姓名
SELECT sname FROM Student WHERE sno NOT IN (SELECT sno FROM Sc WHERE cno='C6')
SELECT sname FROM Student WHERE sno != ALL (SELECT sno FROM Sc WHERE cno='C6')
-- 检索平均成绩最高的学生学号
select sno from sc group by sno having avg(grade) >= all(select avg(grade) from sc group by sno)
```

### 相关子查询
- 里面的结果与外面的相关
- `EXISTS` 是一个条件谓词，用于检查子查询是否返回任何行
- 如果子查询返回任何行，`EXISTS` 将返回 TRUE，否则返回 FALSE。

```sql
--- 选修了C6的同学名字
select sname from student where exists
(select * from sc where cno = 'C6' and sc.sno=student.sno)

-- 查询所有学生都选修的课程名称
SELECT cname FROM Course  
WHERE NOT EXISTS  
(SELECT * FROM Student  
WHERE NOT EXISTS  
(SELECT * FROM Sc  
WHERE Student.sno=Sc.sno  
AND Sc.cno=Course.cno))
```

### 并交差集合查询
#### 集合查询概述
- 标准SQL中有集合并(UNION)的操作，
- 但没有直接提供集合的差(MINUS或EXCEPT)和交(INTERSECT)操作，
- 差和交可以用其他方法实现
```sql
-- 查询选修了C2或C3课程的学生
select sno from sc where cno = 'C2' or cno = 'C3'
-- 等价
select sno from sc where cno = 'C2' union select sno from sc where cno = 'C3'

-- 至少选修了C2和C3的学生(交)
select * from sc where cno = 'C2' and sno in(select sno from sc where cno = 'C3')

-- 将Student和Student1两表合并
SELECT sno, sname, sex FROM Student UNION SELECT sno, sname, sex FROM Student1

-- 查询至少选修了C2和C3的学生（交）
SELECT sno FROM Sc WHERE cno='C2' AND sno IN (SELECT sno FROM Sc WHERE cno='C3')

-- 查询选修了C2但没有选修C3的学生（差）
SELECT sno FROM Sc WHERE cno='C2' AND sno NOT IN (SELECT sno FROM Sc WHERE cno='C3')
```

### 数据定义语言
- 创建数据库
```sql
create database 数据库名称
```
- 使用数据库
```sql
use 数据库名称
```
- 删除数据库
```sql
drop database 数据库名称
```

```sql
create database StudentManage1

use StudentManage1
create table course1(cno Char(4), cname nChar(20))
create table student(sno Char(12), sname Char(10), age int, sex nchar(1), dept nChar(10))

-- 更改类型
alter table student add Birthday nchar(10)
-- 修改表的某列
alter table student alter column Birthday date
-- 添加属性
alter table student add x nchar(11)
-- 删除属性
alter table student drop column x
-- 删除表
drop table s
```

### 数据操控
- 插入
`insert`
```sql
INSERT [INTO] 表或视图名称 [(列名表)] VALUES (数据值)

insert into student(sno, sname, sex, age, dept, birthday)
values('202211079261', '张三', 'M', 19, 'CS', '2004-3-7')

insert into student values('20022201223', 'Li4',20, 'M', '2012-9-18')

-- 也可以只插入某几项
insert into student(sno, sname, age)
values('20123212322', 'wang5', 7)
```

- 更新
`update`
```sql
UPDATE 表或视图名称
SET 列名1=数据值1[,…n]
[WHERE 条件]

update student set sex='M', dept = 'MA' where sname = '张三'
```

- 删除
`delete`
```sql
DELETE FROM 表或视图名称 [WHERE 条件]

delete from student
DELETE FROM Sc WHERE sno IN (SELECT sno FROM Student WHERE sname='lin')
```

