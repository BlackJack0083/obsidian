### 数据类型
`numeric(a,b)` `a`表示总共的位数，`b`表示小数位
`char`用ascii编码，每个汉字需要两个宽度
`nchar`用的是unicode编码，一个宽度即可表示汉字

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

### 数据完整性
设置主键
```sql
CREATE TABLE Student1 (
Sno CHAR(8), 
Sname CHAR(20) UNIQUE, 
Ssex CHAR(6), 
Sbirthdate Date, 
Smajor VARCHAR(40), 
PRIMARY KEY(Sno) );
```
设计索引实际上是系统中设计B+树，通过索引进行查找
##### 参照完整性
设置外键
```sql
CREATE TABLE SC (  
Sno CHAR(8),  
Cno CHAR(5),  
Grade SMALLINT,  
Semester CHAR(5),  
Teachingclass CHAR(8),  
PRIMARY KEY (Sno, Cno),  
FOREIGN KEY (Sno) REFERENCES Student(Sno),  
FOREIGN KEY (Cno) REFERENCES Course(Cno)  
);
```

设置违约处理
```sql
CREATE TABLE SC (  
Sno CHAR(8),  
Cno CHAR(5),  
Grade SMALLINT,  
Semester CHAR(5),  
Teachingclass CHAR(8),  
PRIMARY KEY (Sno, Cno),  
FOREIGN KEY(Sno) REFERENCES Student(Sno)  
ON DELETE CASCADE  
ON UPDATE CASCADE,  
FOREIGN KEY (Cno) REFERENCES Course (Cno)  
ON DELETE NO ACTION 
ON UPDATE CASCADE 
);
```

属性约束
- not null
```sql
CREATE TABLE SC (  
Sno CHAR(8) NOT NULL,  
Cno CHAR(5) NOT NULL,  
Grade SMALLINT NOT NULL,  
Semester CHAR(5),  
Teachingclass CHAR(8),  
PRIMARY KEY (Sno,Cno)  
);
```
- unique
```sql
CREATE TABLE School (  
Shno CHAR(8) PRIMARY KEY,  
SHname VARCHAR(40) UNIQUE,  
SHfounddate Date );
```
- check
```sql
CREATE TABLE Student (
Sno CHAR(8) PRIMARY KEY,  
Sname CHAR(20) NOT NULL,  
Ssex CHAR(6) CHECK (Ssex IN('男','女'))，  
Sbirthdate Date,
Smajor VARCHAR(40) );
```
多列约束
```sql
CREATE TABLE Student (  
Sno CHAR(8),  
Sname CHAR(20) NOT NULL,  
Ssex CHAR(6),  
Sbirthdate Date,  
Smajor VARCHAR(40),  
PRIMARY KEY(Sno),  
CHECK (Ssex='女' OR Sname NOT LIKE 'Ms.%') ); -- %表示0或多个字符
```

`constraint` 完整性约束语句
```sql
CREATE TABLE Student (  
Sno CHAR(8)  
CONSTRAINT C1 CHECK (Sno BETWEEN '10000000' AND '29999999'),  
Sname CHAR(20)  
CONSTRAINT C2 NOT NULL,  
Sbirthdate Date  
CONSTRAINT C3 CHECK (Sbirthdate > '1980-1-1'),  
Ssex CHAR(6)  
CONSTRAINT C4 CHECK (Ssex IN ('男', '女')),  
Smajor VARCHAR(40),  
CONSTRAINT StudentKey PRIMARY KEY (Sno) );
```

修改约束
对于没有命名的约束，一般直接在server中删除，但如果设置了变量名，可以用`alter table 表名 drop 约束`删除后，再重新增加约束
```sql
ALTER TABLE Student  
DROP CONSTRAINT C1;

ALTER TABLE Student  
ADD CONSTRAINT C1 CHECK (Sno BETWEEN '900000' AND '999999');
```

- 设置默认值
`default`
```sql
CREATE TABLE table8 (
c1 int, 
c2 int DEFAULT 2*5, 
c3 datetime DEFAULT getdate() );

INSERT table8 (c1) VALUES (1)  -- 只输入第一个值，其余默认
SELECT * FROM table8
```

```sql
CREATE TABLE table8b (  
c1 int,  
c2 int,  
c3 datetime );

ALTER TABLE table8b ADD CONSTRAINT con1 DEFAULT 2*5 FOR c2 -- 把约束的值绑定到c2 
ALTER TABLE table8b ADD CONSTRAINT con2 DEFAULT getdate() FOR c3
INSERT table8b(c1) VALUES (1)
SELECT * FROM table8b
```

### 索引
```sql
create unique index idxCname on course(cname) -- 创建索引
drop index course.idxcname  -- 删除索引
```

### 视图
```sql
CREATE VIEW CS_Student AS 
SELECT sno, sname, sex, age, dept 
FROM student 
WHERE dept='CS' 
WITH CHECK OPTION; 
```

- 视图查询
```sql
SELECT sno, sname 
FROM studentBirth 
WHERE Birthyear<=2003; 
```

- 通过视图可以反向更新表
```sql
update cs_student set sname='王老五'
where sname = '王五'
```
但是对于非基本表(如运算结果等)则无法更新
```sql
update studentGradeAVG set savg = 88 
where sno = 'S2'

/*对视图或函数 'studentGradeAVG' 的更新或插入失败，因其包含派生域或常量域。*/
```

- 视图删除某行
```sql
delete from cs_student where sno = 'S11'
```

- 视图定义更改
```sql
ALTER VIEW studentDegree  
AS  
SELECT sname AS 姓名, cname AS 课程, grade AS 成绩  
FROM student, course, sc  
WHERE student.sno=sc.sno  
AND course.cno=sc.cno
```

- 视图/表重命名
```sql
exec sp_rename studentDegree, studentGrade
```

- 查看视图信息
```sql
exec sp_helptext view_1  

Text
--------------------------------------------------------------------------
CREATE VIEW studentDegree 
AS 
SELECT sname AS 姓名, cname AS 课程, grade AS 成绩 
FROM student, course, sc 
WHERE student.sno=sc.sno 
AND course.cno=sc.cno
```

```sql
exec sp_helpdb 查询数据库
```

- 删除视图
```sql
DROP VIEW studentBirth;
```

### 存储过程
- 创建存储过程
```sql
create procedure maxgrade
as 
select max(grade) 最高分 from sc
go
```

- 建立好了就可以直接执行
```sql
exec studDegree
```

- 在查询过程中传递参数
```sql
CREATE PROCEDURE 存储过程名(参数列表) AS SQL语句
```

两种方式：
1. 传递的参数和定义时的参数顺序一致
2. EXEC 存储过程名 实参列表
3. 采用“参数=值”的形式，参数顺序可任意
4. EXEC 存储过程名 参数1=值1, 参数2=值2, …

```sql
CREATE PROCEDURE detail (@no char(10)) AS  
SELECT student.sno, sname, cname, grade  
FROM student, course, sc  
WHERE student.sno = @no  
AND student.sno = sc.sno  
AND course.cno = sc.cno
-- 两种传参方式
EXEC detail 'S1'

EXEC detail @no='S1'
```

- 添加默认值
```sql
CREATE PROCEDURE detail2 (@no char(10)='S1') AS  
SELECT student.sno, sname, cname, grade  
FROM student, course, sc  
WHERE student.sno=@no  
AND student.sno=sc.sno  
AND course.cno=sc.cno
```

- 存储过程的返回参数
```sql
CREATE PROCEDURE average (  
@st_no char(10),  
@st_name char(8) OUTPUT,  /*返回参数*/  
@st_avg float OUTPUT  /*返回参数*/  
) AS  
SELECT @st_name=sname, @st_avg=AVG(grade)  
FROM student, sc  
WHERE student.sno=sc.sno  
GROUP BY student.sno, sname  
HAVING student.sno=@st_no  
/*存储过程average返回两个参数@st_name和＠st_avg*/

DECLARE @st_name char(10)

DECLARE @st_avg float

EXEC average 'S1', @st_name OUTPUT, @st_avg OUTPUT

SELECT @st_name as 姓名, @st_avg as 平均分
```

- 管理存储过程
(1) `sp_help [name]`：显示存储过程的参数及其数据类型
(2) `sp_helptext [name]`：显示存储过程的源代码
(3) `sp_depends [name]`：显示和存储过程相关的数据库对象
(4) `sp_stored_procedures`：返回当前数据库中的存储过程列表

### 登录名和数据库用户的区别
在SQL Server中，`zhang3`和`zhang3db1`通常代表不同的概念：

1. **zhang3**：这通常指的是一个**登录名**（Login），也就是一个用于连接到SQL Server实例的凭据。登录名可以是SQL Server验证的账户，也可以是Windows验证的账户。登录名拥有连接到SQL Server实例的权限，并且可以访问实例级别的资源。

2. **zhang3db1**：这通常指的是一个**数据库用户**，是在特定数据库（如`db1`）中创建的。数据库用户与登录名相关联，但它们存在于数据库的上下文中，并且它们的权限是针对特定数据库的。数据库用户继承自它们所属的角色，包括`public`角色，并且可以被授予或拒绝数据库内对象的特定权限。

**区别**：

- **范围**：登录名是实例级别的，而数据库用户是特定于数据库的。
- **权限**：登录名的权限影响其在SQL Server实例中的所有数据库，除非在特定数据库中明确更改。数据库用户的权限仅限于它们被创建的数据库。
- **关联性**：一个登录名可以映射到一个或多个数据库用户，而一个数据库用户只能与一个登录名关联。
- **安全性**：管理登录名的权限涉及控制谁可以登录到SQL Server实例，而管理数据库用户的权限涉及控制用户在特定数据库中的操作。

在实际使用中，当一个用户使用登录名`zhang3`成功登录到SQL Server实例后，如果存在与该登录名相关联的数据库用户（如`zhang3db1`），则该用户在`db1`数据库中将以`zhang3db1`用户的身份执行操作，并且只能行使该用户在`db1`数据库中被授予的权限。如果没有创建特定的数据库用户，登录名也可以直接在数据库中作为用户，但这种情况下权限管理可能会变得不够灵活和安全。