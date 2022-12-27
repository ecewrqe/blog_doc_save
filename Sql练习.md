#Sql练习
# 准备
Create DDL
```
create table Student(
	SId int not null auto-increment comment="学生编号",
	Sname varchar(16) not null comment="学生姓名",
	Sage date comment="出生年月",
	Ssex char(2) comment="学生性别"
	constraint PK_SId primary key (`SId`),
	constraint `Ssex_chk` check `Ssex` in ("男", "女")
);
 
create table Course(
	CId int not null auto-increment comment="课程编号",
	Cname varchar(16) not null comment="课程名称",
	TId int comment="教师编号",
	constraint PK_CId primary key (`CId`),
	constraint FK_Teacher foreign key (`TId`) references Teacher(`TId`)
);
 
create table Teacher(
	TId int not null auto-increment comment="教师编号",
	Tname varchar(16) not null comment="教师姓名"
);
 
create table SC(
	SId int not null comment="学生编号",
	CId int not null comment="课程编号",
	score
	int comment="{分数"
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
