# OracleProject

# SQL Developer

## SQL 修改表语句

### 使用 SQL 语句创建表 E

```sql
create table E (
    -- 列名  存放数据的数据类型  空/非空
	EID varchar2(3) null/not null,
    ENAME varchar2(30)
);
```

### 查看表 E

```sql
select * from E ；
```

### 使用 ALTER 修改表 D

```sql
alter table D	-- 添加字段(2个)/添加列
		-- 列名  存放数据类型  默认值'12345'
	add(location varchar(20) default '12345'，
        phone varchar2(20) null;	-- 第二字段
        );
```

### 修改字段长度

```sql
alter table D	-- 修改表中已有 location 字段的长度为10
	modify(location varchar(10));
```

### 删除字段

```sql
alter table D	-- 删除 D 表中的 note 字段
	drop column note;	-- 删除列
```

### 删除表

```sql
truncate table D	-- 删除整个表中的数据，不可撤销
delete E	-- 删除表中数据，通过加 WHERE 语句来实现删除表中部分数据
drop table	-- 删除整表
```

### 复制表

```sql
create table myemp(编号, 姓名, 年薪)
as select * from DEP
```

### 重命名列名

```sql
select 原列名, 新列名
	   原列明 as 新列名
```



## 约束

### 约束分类

> `not null` 非空
>
> `unique key` 唯一键
>
> `primary key` 主键
>
> `check` 检查
>
> `foreign key` 外键



数据完整性 = 正确性 + 一致性

> 1. 域名完整性（列）约束方法：限制数据类型，检查约束，外键约束，默认值非空约束
>
> 2. 实体完整性（行）：保证每一行不重复
>
> 3. 引用完整性（参照）：主从关系，从 -> 主，外键约束实现
>
>    （数据完整性都是有不同的约束类型来保障的）



### 主键约束

关键字：`primary key`

功能：非空且唯一（一个表中只允许有一个主键）

```sql
create table S(	-- 创建表时添加主键约束
	S# CHAR(9) constraint_S# primary key	-- 约束的名称：constraint_S#
);
```

```sql
alter table S	-- 修改表时添加主键约束
add constraint S_SID_DK primary key(SID);	-- 约束名称：S_SID_DK；列名：SID
```

```sql
select * from user_constraints where table_name = "T"	-- 查看为 T 表创建的约束
```



### 非空约束

关键字：`not null`

```sql
alter table S
	modify name varchar(20) not null;	-- 修改表过程中添加非空约束
```

### 外键约束

关键字：`foreign key`

1. 用于确保相关的两个字段之间的参照关系
2. 子表外键中的值必须在主表范围内，或 null
3. 外键参照的必须是主表的主键或唯一键，当被子表参照时主表相应记录，不允许被删除

```sql
create table SC(	-- 创建 SC 表，同时为主表已有的列 S# 和 C# 字段添加外键，分别引用已有主表的 S 和 C 列
	S# char(9),
    C# char(7),
    constraint SC_S#_S1 foreign key(S#) references S(S#),
    constraint SC_C#_S2 foreign key(C#) references C(C#),
)
```

>`SC_C#_S2` 创建的外键约束的名字（子表名\_列名\_子表名）
>
>foreign key(`C#`) 已有主表主键约束的列名
>
>references `C`(`C#`) 已有主表名，已有主表列名（含主键）



```sql
alter table S	-- 在修改表时为行字段添加外键约束（已有主表S）
	add constraint SC_S#_S1 foreign key(S#)
	references SC(S#)
```

>`SC_S#_S1` 约束名称
>
>foreign key(`S#`) 已有主表主键约束的列名
>
>`SC`(`S#`) 建立新的子表 SC，新子表的列名 S#



### 删除表的主键约束

```sql
alter table S
	drop constraints SS_S#_S1; [cascade]
```

> `SS_S#_S1` 约束名
>
> `[cascade]` 用于删除外键使用



### 删除表的外键约束

```sql
alter table SS
	drop constraint SS_S1
```

> `SS` 外键表名
>
> `SS_S1` 外键约束名



### 检查约束（check）

用于定义数据字段必须要满足的条件（取值范围，文本内容格式）



### 创建表时添加检查约束

在创建表语句（create table）后添加：constraint 约束名 check (检查约束条件，多个条件使用 and 链接)



### 修改表时添加检查约束

```sql
alter table S add check(SSEX='男' OR SSEX='女');	--使用系统自动提供的约束名
```

`S` 需要修改的表名

`SSEX='男' OR SSEX='女'` 约束条件



```sql
alter table S add constraint S_SO check('');
	(sage between 0 and 120)	--表级检查约束
	或(sage >= 0 and sage <= 120)
```

`S_SO` 设定的约束名



### 移除检查约束

```sql
alter table S
drop constraint S_S0;
```

`S` 表名

`S_S0` 检查约束名



### 唯一约束（unique）

用于确保约束的字段不出现**重复的值**，允许有空值，允许定义多个唯一性约束



### 创建表时添加唯一约束

```sql
create table C(
	CNAME char(7) not null,
    unique(CNAME)	-- 表级定义
);
或
constrant C_0 unique(CNAME)
```

`C_0` 约束名

`CNAME` 列名



### 移除唯一约束（修改表）

```sql
alter table C
drop constaint C_0;
```

`C` 表名

`C_0` 唯一约束名



### 非空约束（not null）

```sql
alter table C
modify column not null
```



### 添加数据（insert）

```sql
insert into C(C#, CNAME, CLASSH) values('C1', '00', '72');
```

`C` 表名

`C#, CNAME, CLASSH` 指定数据列名

`'C1', '00', '72'` 要插入的数据



### 修改表中数据（update）

```sql
update SC
set GRADE = 61	-- 修改表中数据，不加 where 修改一列全部数据
(可省略) where C# = 'C401001' and GRADE <= 80;	-- 修改符合条件对应的列数据
```

`SC` 表名

set `GRADE` 要修改数据的列名



### 删除数据（delete）

```sql
delete S
(可省略) where PLAEOFB = '上海';
```

`S` 表名

where `PLAEOFB` = `'上海'`; 列名PLAEOFB，查找符合条件的数据'上海'



### 清空数据（truncate）

```sql
truncate table SS;	-- 清空 SS 表中的所有数据
```


### 合并表（merge）

`commit` 指令结束语句，提交标识符以上的所有命令，提交完成后继续执行下方语句

`savepoint PI；` 保存节点，用于指定标记位置后期回滚操作

`rollback to PI` 回滚到 PI 处


## 基本查询

数据操纵语言（DML）
* SELECT
* INSERT
* DELETE
* UPDATE
* MERGE

数据定义语言（DDL）
* CREATE
* ALTER
* DROP
* RENAME
* TRUNCATE

数据控制语言（DCL）
* GRANT
* REVOKT

事务控制语句
* COMMIT
* ROLLBACK

会话控制语句
* ALTER
* SESSION
* SET ROLE

> 字符串连接运算符 `||` 可以把知乎或其他表达式连接在一起，得到一个新的字符串，实现 “合成” 列的功能

> `"` 双引号为转义字符


## 查询

```sql
select sal, comm, sal + NVL(comm, 0) from EMP;  --(comm, 0): 如果 comm 列的值为空，则赋值为 0
```
```sql
select DISTINCT deptno from EMP;  --DISTINCT deptno: 去重(合并重复项)
```
```sql
select ename || 'IS A' AS 姓名, job AS JOB FROM EMP;  --ename: 列名; IS A: 接入名; 姓名: 列首名; ,: 分隔列，SELECT 至逗号前为第一列，逗号后到 FROM 前为第二列
```