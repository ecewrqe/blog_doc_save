# sql training
## 初めに
Create DDL
```
create table Student(
	SId int not null auto-increment comment="学生番号",
	Sname varchar(16) not null comment="学生名前",
	Sage date comment="生年月日",
	Ssex char(2) comment="ジェンダー"
	constraint PK_SId primary key (`SId`),
	constraint `Ssex_chk` check `Ssex` in ("男", "女")
);
 
create table Course(
	CId int not null auto-increment comment="課程番号",
	Cname varchar(16) not null comment="課程",
	TId int comment="教師名前",
	constraint PK_CId primary key (`CId`),
	constraint FK_Teacher foreign key (`TId`) references Teacher(`TId`)
);
 
create table Teacher(
	TId int not null auto-increment comment="教師番号",
	Tname varchar(16) not null comment="教師名前"
);
 
create table SC(
	SId int not null comment="学生番号",
	CId int not null comment="課程番号",
	score
	int comment="点数"
	constraint `S_C_ID` primary key (`SId`, `CId`)
);
```

## 查询" 01 "课程比" 02 "课程成绩高的学生的信息及课程分数
课程01
`select * from SC where CId="01"`
课程02
`select * from SC where CId="02"`
t01表: 同一个学生的课程01和02 ，要同时存在: inner join, 

```
select
		a.SId as SId,
		a.CId as Course1,
		a.score as Course1_score,
		b.CId as Course2,
		b.score as Course2_score
	from
		(select * from SC where CId="01") a
	inner join
		(select * from SC where CId="02") b
		on a.SId = b.SId
```
和学生信息关联，并且查询course1_score > course2_score
```
select
	student.SId as SId, 
	student.Sname as Sname,
	DATE_FORMAT(student.Sage, "%Y-%m-%d") as Sage,
	Score.Course1 as CId1,
	Score.Course1_score as Score1,
	Score.Course2 as CId2,
	Score.Course2_score as Score2
from
	student
inner join
	(select
		a.SId as SId,
		a.CId as Course1,
		a.score as Course1_score,
		b.CId as Course2,
		b.score as Course2_score
	from
		(select * from SC where CId="01") a
	inner join
		(select * from SC where CId="02") b
		on a.SId = b.SId) Score
	on student.SId = Score.SId
where
	Score.Course1_score > Score.Course2_score;
```

### 1.1同时存在01课程和02课程的学生的信息
参照t01表
### 1.2存在01课程可能不存在02课程
01课程必须存在，02课程可以不存在, left join
```
select
		a.SId as SId,
		a.CId as Course1,
		a.score as Course1_score,
		b.CId as Course2,
		b.score as Course2_score
	from
		(select * from SC where CId="01") a
	left join
		(select * from SC where CId="02") b
		on a.SId = b.Sid
```
与学生信息联立，参照1
### 1.3 不存在01课程存在02课程
课程02必须有，left join，课程01会有部分变成null, 筛选课程01为null的所有学生信息
```
select
		a.SId as SId,
		a.CId as Course1,
		a.score as Course1_score,
		b.CId as Course2,
		b.score as Course2_score
	from
		(select * from SC where CId="02") a
	left join
		(select * from SC where CId="01") b
		on a.SId = b.Sid
```
与学生信息联立，参照1

## 2. 查询平均成绩大于等于60分的同学的学生编号和学生姓名和平均成绩
所有学生的平均成绩->筛选超过60分的，先联立学生信息和成绩，然后分组求平均值
```
select 
	SId, Sname,
	round(avg(score), 2) as avg_score
from
(select
	Student.SId as SId,
	Student.Sname as Sname,
	SC.score as score
from 
	Student 
	inner join 
	SC 
		on student.SId = SC.SId) student
group by SId
having avg_score > 60;
```

## 3.查询在SC表存在成绩的学生信息
存在成绩 的学生信息，先学生和成绩表内联，然后分组
```
select 
	SId, Sname
from
	(select
		Student.SId as SId,
		Student.Sname as Sname,
		SC.score as score
	from 
		Student 
		inner join 
		SC 
			on student.SId = SC.SId) student
group by SId;
```

## 4. 查询所有同学的学生编号，学生姓名，*选课总数course_count，所选课程总成绩
学生和成绩联立，选课总数， 总成绩，null的情况下需要用case判断，非null的数据进行count 和sum
```
select
	Student.SId as SId,
	Student.Sname as Sname,
	(
	case
		when SC.score is null then 0
		else
			count(Student.SId)
		end
	) as score_count,
	(
	case
		when SC.score is null then 0
		else
			sum(SC.score)
		end
	) as score_sum
from
	student
left join
	SC
	on student.SId = SC.SId
group by Student.SId;
```
### 4.1查询成绩的学生信息
```
select
	*
from 
	sc
left join
	student
	on sc.SId = student.SId;
```

## 5 查询li姓老师的数量
查询li老师授课的学生数量，li开头的老师: like ‘li%’
老师表->课程表->成绩表->学生表联立
```
select
	count(*)
from
	teacher
left join
	course
	on Course.TId = Teacher.TId
left join
	SC
	on SC.CId = Course.CId
left join
	Student
	on student.SId = SC.SId
where teacher.Tname like 'li%'
group by TId;
```

## 6.查询学过 张三 老师授课的学生的信息
老师表->课程表->成绩表->学生表联立
筛选 zhangsan 老师
```
select
	*
from
	teacher
left join
	course
	on Course.TId = Teacher.TId
left join
	SC
	on SC.CId = Course.CId
left join
	Student
	on student.SId = SC.SId
where
	teacher.Tname = 'zhangsan';
```

## 7.查询没有学全所有课程的同学的信息
根据学生找课程，没有学的将为null，分组计数，筛选小于课程总数的学生
```
select
	student.SId as SId,
	student.Sname as Sname,
	(
	case
		when SC.CId is null then 0
	else 
		count(SC.SId)
	end
	) as course_count
from
	student
left join
	SC
	on SC.SId = student.SId
group by student.SId
having course_count < (select count(*) from course);
```

## 8.查询至少有一门课与学号为 01 的同学所学相同的同学的信息
学号为01的同学的学生成绩表
```
select
	*
from
	SC
where
	SId = "01";
```

学号为非01的学生的成绩表
```
select
	*
from
	SC
where
	SId <> "01";
```

该两表以所学课程相同联立：inner join，并关联学生信息->分组
```
select
	student.SId as SId,
	student.Sname as Sname,
	date_format(student.Sage, "%Y-%m-%d") as Sage,
	SSex
from
	(select
		*
	from
		SC
	where
		SId = "01") student_1
inner join
	(select
		*
	from
		SC
	where
		SId <> "01") student_other
	on student_1.CId = student_other.CId
left join
	student
	on student_other.SId = student.SId
group by student.SId;
```

## 9.和01学生学过的完全相同课程的学生
01学生的所有课程的总数
`select count(*) from SC where SId = "01";`
left join联立后，所有其他学生的分组计数等于01学生的课程总数
```
select
	student.SId as SId,
	student.Sname as Sname,
	date_format(student.Sage, "%Y-%m-%d") as Sage,
	SSex,
	count(student.SId) as score_count
from
	(select
		*
	from
		SC
	where
		SId = "01") student_1
left join
	(select
		*
	from
		SC
	where
		SId <> "01") student_other
	on student_1.CId = student_other.CId
left join
	student
	on student_other.SId = student.SId
group by student.SId
having score_count = (select count(*) from SC where SId = "01");
```

## 10. 查询没有学过 zhangsan 老师讲授的任意一门课程的学生姓名
上过张三老师课程的学生，所有同学not in上过张三老师课程的学生
```
create view zhangsan_course_student as select
	student.Sid as Sid,
	student.Sname as Sname
from
	student
left join
	SC
	on student.SId = SC.SId
left join
	Course
	on SC.CId = Course.CId
left join
	teacher
	on teacher.TId = Course.TId
where
	teacher.Tname = "zhangsan";
```

排除
```
select
	sname
from
	student
where
	student.SId not in (select Sid from zhangsan_course_student);
```

## 11.查询两门及其以上不及格课程的同学的学号和姓名以及平均成绩
某个同学，他没有一门课不及格，不在需求范围内
某个同学，他有一门课不及格，不在需求范围内
某个同学，他有两门课不及格，计算它的及格计数，和平均成绩
某个同学，他有三门课不及格，计算它的及格计数，和所有的平均成绩

学生和成绩联立，排除null，我只需要计数考过的，判断不及格为1 放到列isnot_passed
```
select
	student.Sid as SId,
	student.Sname as Sname,
	SC.CId as CId,
	SC.score as score,
	(
	case
		when SC.score < 60 then 1
	else
		0
	end
	) as isnot_passed
from 
	student
inner join
	SC
	on student.SId = SC.SId;
```
以学生和通过情况进行分组，计数不通过情况到not_passed_count，count的时候加条件，并计算平均成绩，然后筛选isnot_passed为1
```
select
	*,
	count(
		case 
			when isnot_passed = '1' then 1 else null end
	) as not_passed_count,
	round(avg(score), 2) as score_avg
from
	(
	select
		student.Sid as SId,
		student.Sname as Sname,
		SC.CId as CId,
		SC.score as score,
		(
		case
			when SC.score < 60 then 1
		else
			0
		end
		) as isnot_passed
	from 
		student
	inner join
		SC
		on student.SId = SC.SId) student
group by SId
having isnot_passed='1';
```

## 12.检索01课程分数小于60，按分数降序排列学生信息

课程和学生联立，筛选出小于60的，降序
```
select
	*
from
	SC
left join
	student
	on SC.SId = student.SId
where
	SC.score < 60
order by score desc;
```

## 13.按平均成绩从高到低显示所有学生的所有课程的成绩以及平均成绩
显示所有学生的所有课程的成绩, 求出分组平均值
```
select
	student.SId as SId,
	round(avg(SC.Score))as score_avg
from
	student
left join
	SC
	on SC.SId = student.SId
group by student.SId;
```

再次联立

```
select
	*
from
	student
left join
	SC
	on SC.SId = student.SId
left join
	(select
		student.SId as SId,
		round(avg(SC.Score), 2)as score_avg
	from
		student
	left join
		SC
		on SC.SId = student.SId
	group by student.SId) average
	on student.SId = average.SId;
```

按平均成绩从高到低显示
```
order by score_avg desc;
```

## 14.查询各科成绩最高分、最低分和平均分
以如下形式显示：课程id（course Id），课程名(course name)，最高值(best score)，最低值(worst score)，平均值(average score)，通过率(the passed rate)，中等率(the middle rate)，良好率(the good rate)，优秀率(the excellent rate)

the passed rate >= 60, the middle rate is between 70 and 80, the good is between 80-90, the excellent rate is great than 90

要求输出课程号course id和选修人数proceeded count
结果按人数降序排列， 人数相同按课程号升序排列

课程->成绩联立
```
select 
	*
from
	Course
left join
	SC
	on SC.CId = Course.CId;
```

结果
```
select
	Course.CId as 'course id',
	count(SC.SId) as 'proceeded count',
	max(SC.score) as 'the best score',
	min(SC.score) as 'the worst score',
	round(avg(SC.score), 2) as 'the average score',
	round(count(
	case
		when SC.score >= 60 then 1 else NUll end
	)/count(SC.SId), 2) as 'the passed rate',
	round(count(
	case
		when SC.score >= 70 and SC.score < 80 then 1 else NUll end
	)/count(SC.SId), 2) as 'the middle rate',
	round(count(case
		when SC.score >= 80 and SC.score < 90 then 1 else NUll end
	)/count(SC.SId), 2) as 'the good rate',
	round(count(case
		when SC.score >= 90 then 1 else NUll end
	)/count(SC.SId), 2) as 'the excellent rate'
	
from
	Course
left join
	SC
	on SC.CId = Course.CId
group by Course.CId
order by count(SC.SId) desc, Course.CId asc;
```

## 15.按各科成绩进行排序，并显示排名， Score 重复时保留名次空缺

学生->成绩联立
增加排序列
```
select
	*,
	rank() over(PARTITION BY SC.CId ORDER BY SC.score DESC) as score_rank
from
	course
left join
	SC
	on Course.CId=SC.CId
left join
	student
	on SC.SId = student.SId
order by SC.CId asc, SC.score desc;
```

## 16.查询学生的总成绩，并进行排名，总分重复时保留名次空缺
```
select
	*,
	sum(SC.score) as 'the total score',
	rank() over(ORDER BY sum(SC.score) DESC) as score_rank
from
	student
left join
	SC
	on SC.SId = student.SId
left join
	Course
	on Course.CId=SC.CId
group by student.SId
order by sum(SC.score) desc;
```

## 17统计各科成绩各分数段人数：课程编号，课程名称，[100-85]，[85-70]，[70-60]，[60-0] 及所占百分比
[60-0] lost rate, [70-60] middle rate, [85-70] good rate, [100-85]excellent rate

课程->成绩
```
select
	*,
	count(SC.SId) as 'persons',
	round(count(
	case
		when SC.score < 60 then 1 else null end
	)/count(SC.SId), 2) as 'lost rate',
	
	round(count(
	case
		when SC.score >= 60 and SC.score < 70 then 1 else null end
	)/count(SC.SId), 2) as 'middle rate',
	
	round(count(
	case
		when SC.score >= 70 and SC.score < 85 then 1 else null end
	)/count(SC.SId), 2) as 'good rate',
	round(count(
	case
		when SC.score >= 85 then 1 else null end
	)/count(SC.SId), 2) as 'excellent rate'
from
	course
left join
	SC
	on course.CId = SC.CId
group by course.CId;
```

## 18查询各科成绩前三名的记录
成绩->学生，成绩排名，分组，limit
```
select 
	*,
	substring_index(GROUP_CONCAT(SC.score ORDER BY SC.score DESC), ',', 3)
	grouped_score
from
	SC
left join
	Course
	on SC.CId=Course.CId
left join
	student
	on SC.SId=student.SId
group by SC.CId;
```

正解
```
select
	*
from 
(
select 
	Course.CId as CId,
	Course.Cname as Cname,
	SC.SId as SId,
	SC.score as score,
	student.Sname as Sname,
	row_number()over(partition by SC.CId order by SC.score desc) student_rank
from
	SC
left join
	Course
	on SC.CId=Course.CId
left join
	student
	on SC.SId=student.SId
order by SC.CId asc, SC.score desc) student
where student_rank <= 3;
```

## 20.查询出只选修两门课程的学生学号和姓名
```
select
	student.*,
	count(student.SId) as scount
from
	student
left join
	SC
	on student.SId = SC.SId
group by student.SId
having scount = 2;
```

## 21. 查询男生、女生人数
```
select 
	Ssex,
	count(Ssex) as Ssex_count
from
	student
group by Ssex;
```

## 22. 查询名字中含有「feng」字的学生信息
one
```
select 
	Ssex,
	count(Ssex) as Ssex_count
from
	student
group by Ssex;
```
two
```
select
	*
from 
	student
where
	Sname like '%feng%';
```
or
```
select
	*
from 
	student
where
	Sname regexp '.*feng.*';
```

## 23.查询同名同性学生名单，并统计同名人数
计数名字重复的学生，超过2的
```
select
	SId, Sname, count(SId) as SId_count
from
	student
group by sname
having SId_count >= 2;
```

## 24. 查询 1990 年出生的学生名单
```
select
	*
from
	student
where
	Sage between '1990-01-01' and '1991-01-01';
```

## 26. 查询平均成绩大于等于 85 的所有学生的学号、姓名和平均成绩
```
select
	*,
	round(avg(SC.score), 2) as score_avg
from 
	student
left join 
	SC
	on SC.SId=student.SId
group by student.SId
having avg(SC.score)>85;
```

## 27.查询课程名称为「数学」，且分数低于 60 的学生姓名和分数
```
select 
	*
from
	student
left join
	SC
	on SC.SId=student.SId
left join
	Course
	on Course.CId=SC.CId
where
	Course.Cname = 'math' and SC.score < 60;
```

## 28.查询所有学生的课程及分数情况
```
select 
	*
from
	student
left join
	SC
	on SC.SId=student.Sid
```

## 29.查询任何一门课程成绩在 70 分以上的姓名、课程名称和分数
```
select 
	*,
	count(if(SC.score>70, 1, null)) as score_gt_70
from
	student
left join
	SC
	on SC.SId=student.SId
group by student.SId
having count(if(SC.score>70, 1, null)) > 0;
```

## 30. 查询不及格的课程
```
select
	*
from
	student
left join
	SC
	on SC.SId=student.SId
where
	SC.score < 60;
```

## 31.查询课程编号为 01 且课程成绩在 80 分以上的学生的学号和姓名
```
select
	*
from
	student
left join
	SC
	on SC.SId=student.SId
where
	SC.score >= 80;
```

## 33.成绩不重复，查询选修「张三」老师所授课程的学生中，成绩最高的学生信息及其成绩
```
select
	*
from
	student
left join
	SC
	on SC.SId=student.SId
left join
	Course
	on Course.CId = SC.CId
left join
	Teacher
	on Teacher.TId = Course.TId
where
	Teacher.Tname = 'zhangsan'
order by SC.score desc
limit 1;
```
有重复，排序后，选1
```
select
	*,
	rank()over(order by SC.score desc) as score_rank
from
	student
left join
	SC
	on SC.SId=student.SId
left join
	Course
	on Course.CId = SC.CId
left join
	Teacher
	on Teacher.TId = Course.TId
where
	Teacher.Tname = 'zhangsan'
order by SC.score desc;
```

## 35.查询不同课程成绩相同的学生的学生编号、课程编号、学生成绩
以学生，成绩分组, 计数超过2的学生
```
select
	student.SId as SId,
	student.Sname as Sname,
	SC.Score as Score,
	Course.CId as CId,
	Course.Cname as Cname,
	count(SC.Score) as score_count
from
	student
inner join
	SC
	on SC.SId=student.SId
inner join
	Course
	on Course.CId = SC.CId
group by student.SId, SC.Score
having count(SC.Score) >= 2;
```

## 38.检索至少选修两门课程的学生学号
```
select
	student.SId as SId,
	student.Sname as Sname
from
	student
inner join
	SC
	on SC.SId=student.SId
group by student.SId
having count(student.SId) >= 2;
```

## 39.查询选修了全部课程的学生信息
```
select
	student.SId as SId,
	student.Sname as Sname
from
	student
inner join
	SC
	on SC.SId=student.SId
group by student.SId
having count(student.SId) = (select count(*) from Course);
```

## 40．查询各学生的年龄，只按年份来算
```
select
	*,
	datediff(curdate(), Sage) / 365.0 as age
from
	student;
```

## 41. 查询本周过生日的学生
```
select
	*
from
	student;
```
Birth: 1990-02-03, today: 2005-02-01, 
Birth day of current year: Concat_ws(“-”, today.year, birth.month, birth.day)
Str_to_date(current birth day, “%Y-%m-%d”)

Dayofweek(today),  from: subdate(today, Dayofweek), to: adddate(today, 7-dayofweek)

Birth day of current year is in the day of the start week and the day  of the end week

本周开始日期
`select subdate(curdate(), dayofweek(curdate()));`
本周结束日期
`select adddate(curdate(), 7-dayofweek(curdate()));`
今年的生日日期
```
select
	*,
	str_to_date(
		concat_ws('-', year(curdate()), month(sage), day(sage)),
		'%Y-%m-%d'
	) as cur_birth_day
from student;
```
合并
```
select
	*
from 
	student
where 
	(str_to_date(
		concat_ws('-', year(curdate()), month(sage), day(sage)),
		'%Y-%m-%d'
	)) between 
		(subdate(curdate(), dayofweek(curdate()))) 
		and 
			(adddate(curdate(), 7-dayofweek(curdate())));
```

## 42. 查询下周过生日的学生
参见41
## 43. 查询本月过生日的学生
本月开始日期
`select subdate(curdate(), dayofmonth(curdate())-1);`
本月结束日期
`select last_day(curdate());`
今年的生日日期
参见41
合并
```
select
	*
from 
	student
where 
	(str_to_date(
		concat_ws('-', year(curdate()), month(sage), day(sage)),
		'%Y-%m-%d'
	)) between 
		(subdate(curdate(), dayofmonth(curdate())-1))
		and
		(last_day(curdate()));
```

## 44. 查询下月过生日的学生
参见43

## Extra
```
create index
	index_name
on
	tbl_name(key_part)
```
事件schedule
```
create event event_name
	on schedule
		schedules
	comment '',
	do
		anysql
```

结构化sql
```
set @a=, @b=
create function sp_name (in a, out b)
begin
	
	return 
end
 
create procedure sp_name (in a, out b)
begin
	
	return 
end
 
call sp_name(a, b)

-- create trigger
create trigger trigger_name
	trigger_time trigger_event
	on tbl_name for each row
	trigger_body
	
-- create view
create view view_name
	as (everysql)
```

constraint
```
constraint symbol primary key index_type(key1, key2)
constraint symbol unique index_type(key1, key2)
constraint symbol index index_type(key1, key2)
constraint symbol key index_type(key1, key2)
constraint symbol check expr
constraint symbol foreign key index_type(key1, key2) 
	references tbl_name（key1, key2） 
	on delete 
		no action|set default|cascade|restrict
	on update
```
