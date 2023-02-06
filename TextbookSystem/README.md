# 1 前言

随着科学技术的发展，社会进步，计算机迅速的发展,教材征订管理的方法也日新月异,以前全是由人工管理的方法现存在很多的缺点：管理效率低，劳动强度大，信息处理速度低而且准确率也不够令人满意。为了提高教材征订管理效率，减轻劳动强度，提高信息处理速度和准确性；为管理员提供更方便、科学的服务项目；更先进、科学的服务系统。于是我们便选择了由计算机来设计一个教材征订管理系统的方案。让管理员使用计算机对教材征订进行自动管理，管理员可以直接在计算机上实现教材征订的信息管理，并能够在一定程度上实现自动化。我们在现行系统初步调查的基础上提出了新系统目标，即新系统建立后所要求达到的运行指标，这是系统开发和评价的依据。

编写一个程序来处理一个书库与学校之间的教材选取、订购、修改、查询的功能，通过创建表等一系列操作记录来实现存入书库的具体记录信息（书库编号，书库名称，库存量，书库地址，教材编号，取出时间，入库时间）然后根据设计的E-R图把数据信息存放到指定表中。最后实现可以调查学校里每个系订购图数量和书库里的库存量等，并显示在屏幕上。

# 2 需求分析
## 2.1功能要求

（1）使用SQL Developer实现教材征订的信息管理；

（2）利用表连接进行相关信息处理；

（3）绘制E-R图

## 2.2任务目标

（1）实现程序处理书库与学校之间的教材选取、订购、修改、查询的功能

（2）实现存入书库的具体记录信息（书库编号，书库名称，库存量，书库地址，教材编号，取出时间，入库时间）

（3）绘制E-R图

（4）编写代码；

（5）PL/SQL代码分析与调试。


## 2.3运行环境

Oracle数据库系统

## 2.4开发工具

SQL Plus

SQL Developr

# 3.数据库设计

## 3.1创建概念数据模型

### 3.1.1 确定实体
	
系统包含教材选取、查询、修改、订购子系统，系统所需的教务信息和教材库存信息来自网上原有数据库。其中，教研室通过B/S模式可以进行教材选取、查询、修改等工作。该系统主要包括系统功能输入模块、基本资料维护模块、综合查询功能模块。

系统要实现基本信息录入、修改、查询等功能：

1.	信息的输入：缺书信息、教材入库信息、库存信息、货价信息等。

2.	信息的修改、删除。

3.	根据要求，查询统计符合条件的各类信息。

依据实际需要，对重要新信息进行统计
	
### 3.1.2 确定关系并绘制E-R图
	
#### 3.1.2.1 确定关系：

- 教材隶属于关系

- 院系使用教材

- 书库管理员属于书库

- 操作记录与书库管理员，书库，教材，院系之间的记录


#### 3.1.2.2 绘制E-R图：

![E-R图](https://github.com/lsjsss/OracleProject/blob/main/TextbookSystem/E-RImage/E-RImage.png)

### 3.1.3 将多对多关系更改为实体
	
书库管理员与书库、教材、院系之间以及教材与院系和书库之间使用操作记录做实体。
	
### 3.1.4 确定属性
	
实体属性：

- 书库（书库编号，书库名称，库存量，书库地址，教材编号，取出时间，入库时间）

- 教材（教材编号，教材名称，教材数量，教材种类）

- 出版社（出版社编号，出版社电话，出版社名称，出版社地址，教材编号，出版时间，出版数量）

- 院系（学校名称，院系编号，院系负责人姓名，学校地址，教材编号，需求量）

- 书库管理员（管理员工号，管理员姓名，管理员电话，管理教材类型，管理员密码）

- 操作记录（操作记录编号，管理员工号，书库编号，教材编号，教材数量，院系编号，操作时间，操作类型）











### 3.1.5 规范化数据
	
符合第三范式要求
	
## 3.2 生成物理数据模型

### 3.2.1 将实体转换为表

#### 3.2.1.1 book--教材表

| 字段名称 | 含义 | 数据类型 | 是否可null | 约束条件 |
| -- | -- | -- | -- | -- |
| book_id | 教材编号 | varchar2(8) | not null | primary key |
| book_name | 教材名称 | varchar2(30) | not null |  |
| book_number | 教材数量 | number(4,0) | not null |  |
| book_kind | 教材种类 | varchar2(10) |  |  |		


#### 3.3.1.2 library --书库表

| 字段名称 | 含义 | 数据类型 | 是否可null | 约束条件 | 
| -- | -- | -- | -- | -- |
| library_id | 书库编号 | varchar2(10) | not null | primary key | 
| library_name | 书库名称 | varchar2(10) | not null |  | 
| inventory	库存量 | number(5,0) | not null |  | 
| library_add | 书库地址 | varchar2(10) |  |  | 
| book_id | 教材编号 | varchar2(8) | not null | foreign key | 
| takeout_date | 取出时间 | date | 	 |  | 
| storage_date | 入库时间 | date | 	 |  | 

#### 3.2.1.3 press--出版社表

| 字段名称 | 含义 | 数据类型 | 是否可null | 约束条件 | 
| -- | -- | -- | -- | -- |
| press_id | 出版社编号 | varchar2(10) | not null | primary key | 
| press_phone_number | 出版社电话 | number(8) |  |  | 
| press_name | 出版社名称 | varchar(30) | not null |  | 
| press_address | 出版社地址 | varchar(10)	 |  |  | 
| book_id | 教材编号 | varchar2(8) | not null | foreign key | 
| publish_date | 出版时间 | date |  |  | 	
| publish_number | 出版数量 | number(4,0) | not null | 

#### 3.2.1.4 faculty--院系表

| 字段名称 | 含义 | 数据类型 | 是否可null | 约束条件 | 
| -- | -- | -- | -- | -- |
| faculty_name | 院系名称 | varchar2(20) | not null | 
| faculty_number | 院系编号 | varchar(8) | not null | primary key | 
| department_name | 院系负责人姓名 | varchar(8) |  |  | 	
| book_id | 教材编号 | varchar(8) | not null | foreign key | 
| demand | 需求量 | number(4,0)	 |  |  | 

#### 3.2.1.5 manager--管理员表

| 字段名称 | 含义 | 数据类型 | 是否可null | 约束条件 | 
| -- | -- | -- | -- | -- |
| management_number | 管理员工号 | number(11,0) | not null | primary key | 
| management_name | 管理员姓名 | varchar2(8) | not null |  | 
| management_phone_number | 管理员电话 | number(11)	 |  |  | 
| book_kind | 管理教材种类 | varchar2(10) | not null | foreign key | 
| management_password | 管理员密码 | varchar(20) | not null |  | 

#### 3.2.1.6 records--操作记录表

| 字段名称 | 含义 | 数据类型 | 是否可null | 约束条件 | 
| -- | -- | -- | -- | -- |
| records_number | 操作记录编号 | number(6,0) | not null | primary key | 
| management_number | 管理员工号 | varchar2(10) | not null | foreign key | 
| library_id | 书库编号 | varchar2(10) | not null | foreign key | 
| book_id | 教材编号 | varchar2(8) | not null | foreign key | 
| book_number | 教材数量 | number(4,0) | not null | foreign key | 
| faculty_number | 院系编号 | varchar2(8) | not null | foreign key | 
| operation_time | 操作时间 | date | not null |  | 
| action_type | 操作类型 | varchar2(8) | not null | check('取出' or '入库')
	
	
### 3.2.2 解析关系

| 出版社——教材 | 多对多 |
| -- | -- |
| 教材——院系 | 多对多 |
| 教材——书库 | 多对多 |
| 书库——书库管理员 | 多对多 |
| 操作记录——书库管理员 | 多对一 |
| 操作记录——书库 | 多对一 |
| 操作记录——教材 | 多对一 |
| 操作记录——院系 | 多对一 |
	
## 3.3 数据实施与维护
### 3.3.1 数据表的创建
	
#### 3.3.1 A 创建first用户

```sql
create user first identified by first;
```

#### 3.3.1 B 修改first用户密码

```sql
alter user first identified by one;
```

#### 3.3.1 C 授予用户权限

```sql
grant DBA, connect, resource to first;
```

#### 3.3.1 D 删除first用户

```sql
drop user first cascade;
```

#### 3.3.1.1 创建表

```sql
create table book( --教材
  book_id varchar2(8) not null primary key, --教材编号
  book_name varchar2(30) not null, --教材名称
  book_number number(4,0) not null, --教材数量
  book_kind varchar2(10) --教材种类
);

create table library ( --书库
  library_id varchar2(10) not null primary key, --书库编号
  library_name varchar2(10), --书库名称
  inventory number(5,0) not null, --库存量
  library_add varchar2(10), --书库地址
  book_id varchar2(8) not null, --教材编号
  takeout_date date, --取出时间
  storage_date date, --入库时间
  constraint li_b foreign key(book_id) references book(book_id)
);

create table press( --出版社
  press_id varchar2(10) primary key, --出版社编号
  press_phone_number number(8), --出版社电话
  press_name varchar(30) not null, --出版社名称
  press_address varchar(10), --出版社地址
  book_id varchar2(8) not null, --教材编号
  publish_date date, --出版时间
  publish_number number(4,0) not null, --出版数量
  constraint pr_b foreign key(book_id) references book(book_id)
);

create table faculty( --院系
  faculty_name varchar2(20) not null, --院系名称
  faculty_number varchar(8) not null primary key, --院系编号
  department_name varchar(8), --院系负责人姓名
  school_address varchar(8), --学校地址
  book_id varchar(8) not null, --教材编号
  demand number(4,0), --需求量
  constraint fa_b foreign key(book_id) references book(book_id)
);

create table manager( --管理员
  management_number varchar2(10) not null primary key, --管理员工号
  management_name varchar2(8) not null, --管理员姓名
  management_phone_number number(11), --管理员电话
  book_kind varchar2(10) not null, --管理教材种类
  management_password varchar(20) not null --管理员密码
);

create table records( --操作记录
  records_number number(5,0) not null primary key, --操作记录编号
  management_number varchar2(10) not null, --管理员工号
  library_id varchar2(10) not null, --书库编号
  book_id varchar(8) not null, --教材编号
  book_number number(4,0) not null, --教材数量
  faculty_number varchar(8) not null, --院系编号
  operation_time date not null, --操作时间
  action_type varchar2(4) not null check(action_type='取出' or action_type='入库'), --操作类型
  constraint re_man foreign key(management_number) references manager(management_number),
  constraint re_lii foreign key(library_id) references library(library_id),
  constraint re_boi foreign key(book_id) references book(book_id)
);
```

#### 3.3.1.2 插入数据

##### 3.3.1.2.1 插入教材（教材编号，教材名称，教材数量， 教材种类）

```sql
insert into book values ('B1011','高等数学(上)',4975,'数学');
insert into book values ('B1012','高等数学(下)',5113,'数学');
insert into book values ('B1013','线性代数',2785,'数学');
insert into book values ('B1014','离散数学',7584,'数学');
insert into book values ('B2021','大学物理',958,'物理');
insert into book values ('B3001','计算机应用基础',9256,'计算机');
insert into book values ('B3005','计算机网络基础',1213,'计算机');
insert into book values ('B3013','高级程序语言设计(C)',2371,'计算机');
insert into book values ('B3014','高级程序语言设计(C++)',1682,'计算机');
insert into book values ('B3021','java编程基础',1563,'计算机');
insert into book values ('B3022','java编程进阶',960,'计算机');
insert into book values ('B3031','python程序设计',1150,'计算机');
insert into book values ('B3052','前端开发技术',2291,'计算机');
insert into book values ('B3055','多媒体技术及应用',3958,'计算机');
insert into book values ('B3062','数据库原理及应用',5990,'计算机');
insert into book values ('B5011','形势与政策',9257,'政治');
insert into book values ('B5023','思想道德修养与法律基础',9893,'政治');
insert into book values ('B5027','马克思主义基本原理',8960,'政治');
insert into book values ('B5030','中国近现代史纲要',9765,'政治');
insert into book values ('B7001','大学英语1',9968,'英语');
insert into book values ('B7002','大学英语2',8950,'英语');
insert into book values ('B7003','大学英语3',9700,'英语');
insert into book values ('B7004','大学英语4',9562,'英语');
```

##### 3.3.1.2.2 插入书库（书库编号， 书库名称， 库存量， 书库地址， 教材编号， 取出时间， 入库时间）

```sql
insert into library values ('L6010','朱雀',5000,'北京市','B1011','13-8月-2019','1-8月-2016');
insert into library values ('L6021','腾蛇',7000,'上海市','B1013','23-1月-2018','1-8月-2016');
insert into library values ('L6022','青牛',9000,'天津市','B2021','14-3月-2019','1-8月-2016');
insert into library values ('L6023','青玄',9000,'重庆市','B3001','27-9月-2017','1-8月-2016');
insert into library values ('L6852','当康',8000,'香港','B3013','2-11月-2016','1-8月-2016');
insert into library values ('L6853','玄机',7000,'澳门','B3014','17-5月-2017','1-8月-2016');
insert into library values ('L6020','丹雀',9000,'广州市','B3021','7-12月-2016','1-8月-2016');
insert into library values ('L6028','苍穹',6000,'成都市','B3022','20-3月-2019','1-8月-2016');
insert into library values ('L6025','鸿鹄',9000,'南京市','B3031','19-1月-2017','1-8月-2016');
insert into library values ('L6027','腾霄',8000,'武汉市','B3062','30-6月-2019','1-8月-2016');
insert into library values ('L6029','角虎',5000,'西安市','B5011','16-6月-2017','1-8月-2016');
insert into library values ('L6024','紫龙',7000,'沈阳市','B7001','15-2月-2019','1-8月-2016');
```

#### 3.3.1.2.3 插入出版社（出版社编号，出版社电话，出版社名称，出版社地址，教材编号，出版时间，出版数量）

```sql
insert into press values ('P0001',86830665,'人民出版社','北京市','B5011','1-8月-2019',9999);
insert into press values ('P0302',62770175,'清华大学出版社','北京市','B1014','15-4月-2015',9000);
insert into press values ('P0111',67391106,'机械工业出版社','沈阳市','B3062','10-9月-2013',7000);
insert into press values ('P0300',62511675,'中国人民大学出版社','北京市','B5030','25-7月-2014',7000);
insert into press values ('P0308',88925953,'浙江大学出版社','杭州市','B1014','12-5月-2016',5000);
insert into press values ('P5605',82667874,'西安交通大学出版社','西安市','B1013','20-6月-2012',3000);
insert into press values ('P0193',67883580,'中国工商联合出版社','天津市','B7001','2-3月-2017',8000);
insert into press values ('P0301',62752864,'北京大学出版社','北京市','B3013','8-7月-2015',6000);
insert into press values ('P6380',65976483,'首都经济贸易大学出版社','北京市','B5027','9-1月-2016',5000);
```

##### 3.3.1.2.4 插入院系（学校名称，院系编号，院系负责人姓名，学校地址，教材编号，需求量）

```sql
insert into faculty values ('北京大学',01000101,'张华','北京','B3022',2000);
insert into faculty values ('中国人民大学',01000205,'李建平','北京','B1014',2000);
insert into faculty values ('清华大学',01000303,'王丽丽','北京','B3055',1000);
insert into faculty values ('北京航空航天大学',01000406,'杨秋红','北京','B7002',1000);
insert into faculty values ('北京理工大学',01000507,'吴志伟','北京','B3052',2000);
insert into faculty values ('中国农业大学',01000602,'李涛','北京','B3062',2000);
insert into faculty values ('北京师范大学',01000701,'赵晓燕','北京','B3021',1000);
insert into faculty values ('中央民族大学',01000403,'许子文','北京','B1011',1000);
insert into faculty values ('南开大学',02200111,'孔健','天津','B7004',2000);
insert into faculty values ('天津大学',02200152,'张来有','天津','B3001',3000);
insert into faculty values ('大连理工大学',02200531,'张庆','天津','B3013',4000);
insert into faculty values ('东北大学',02400499,'葛坤','辽宁','B1012',1000);
insert into faculty values ('吉林大学',02400320,'李松岭','吉林','B7004',2000);
insert into faculty values ('哈尔滨工业大学',03100122,'符小龙','黑龙江','B3005',1000);
insert into faculty values ('复旦大学',02100101,'沈明义','上海','B5011',3000);
insert into faculty values ('同济大学',02100305,'杨文彬','上海','B5030',4000);
insert into faculty values ('上海交通大学',02100402,'卢伟兴','上海','B7002',1000);
insert into faculty values ('华东师范大学',02100309,'怀仁','上海','B3013',1000);
insert into faculty values ('南京大学',02300506,'沈志','江苏','B1013',2000);
insert into faculty values ('东南大学',02300409,'李勇','江苏','B3013',1000);
insert into faculty values ('中国矿业大学',02300109,'毛子涵','江苏','B3062',1000);
insert into faculty values ('浙江大学',04100303,'马玉','浙江','B3005',3000);
insert into faculty values ('中国科学技术大学',05500901,'马俊','安徽','B7001',2000);
insert into faculty values ('厦门大学',07200503,'文海东','福建','B5023',1000);
insert into faculty values ('山东大学',04600102,'马翔宇','山东','B3014',2000);
insert into faculty values ('中国海洋大学',04600302,'石千一','山东','B2021',3000);
insert into faculty values ('武汉大学',05900406,'林如月','湖北','B3062',5000);
insert into faculty values ('湖南大学',08200701,'黄天成','湖南','B5030',3000);
insert into faculty values ('中山大学',09300502,'燕竹林','广东','B5027',1000);
insert into faculty values ('华南理工大学',09300907,'祝茂','广东','B1014',3000);
insert into faculty values ('重庆大学',02300315,'刘学明','重庆','B7002',2000);
insert into faculty values ('电子科技大学',05800417,'高俊','四川','B3055',1000);
insert into faculty values ('西安交通大学',04500601,'孔聪','陕西','B3014',1000);
```

##### 3.3.1.2.5 插入书库管理员（管理员工号，管理员姓名，管理员电话，管理教材类型，管理员密码）

```sql
insert into manager values ('F0001','李侠',15105247391,'B5011',746374);
insert into manager values ('F0002','张浩',18905247391,'B3062',103928);
insert into manager values ('F0003','高钧剑',18746981574,'B3055',335432);
insert into manager values ('F0004','杨卓群',18705247391,'B3052',541876);
insert into manager values ('F0005','杨泽',13358141242,'B3031',348123);
insert into manager values ('F0006','陈镇洪',18257969999,'B3022',431873);
insert into manager values ('F0007','白承龙',15167864444,'B3021',752815);
insert into manager values ('F0008','赵叶红',15072082710,'B1014',375193);
insert into manager values ('F0009','王平',13929378888,'B1013',023719);
insert into manager values ('F0010','周浩哲',13393557863,'B1012',046138);
insert into manager values ('F0011','金龙',13467098696,'B1011',401263);
insert into manager values ('F0012','李宝龙',13467098693,'B7004',152630);
insert into manager values ('F0013','方正容',15133039999,'B7003',735284);
insert into manager values ('F0014','宋国庆',13099305179,'B7002',092365);
insert into manager values ('F0015','张志英',13881374992,'B7001',342654);
insert into manager values ('F0016','张云鹏',13076435674,'B5030',341953);
insert into manager values ('F0017','杨子军',13269699999,'B5027',327134);
insert into manager values ('F0018','许东瑞',15760339682,'B5023',561532);
insert into manager values ('F0019','张雷',15171828888,'B5011',342816);
insert into manager values ('F0020','张云鹏',18794495366,'B3062',132042);
insert into manager values ('F0021','陈乐军',13117605465,'B3055',146358);
insert into manager values ('F0022','张志英',13170301194,'B3052',923843);
insert into manager values ('F0023','张子萱',18244473499,'B3031',036537);
insert into manager values ('F0024','李昌',15152685313,'B3022',356732);
insert into manager values ('F0025','尤利',15896065780,'B3021',835246);
insert into manager values ('F0026','乐善',13383559994,'B3014',186432);
insert into manager values ('F0027','马向前',13140139851,'B3013',256854);
insert into manager values ('F0028','王自',13806352269,'B3005',225376);
insert into manager values ('F0029','夏帆',15023456976,'B3001',356853);
insert into manager values ('F0030','孟杰',13605548277,'B2021',567732);
```

##### 3.3.1.2.6 A 插入操作记录（操作记录编号，管理员工号，书库编号，教材编号，操作教材数量，院系编号，操作时间，操作类型）

```sql
insert into records values ( 19001,'F0001','L6010','B1011', 430,01000101,'3-1月-2019','取出');
insert into records values ( 19002,'F0002','L6010','B1012', 430,01000205,'9-1月-2019','入库');
insert into records values ( 19003,'F0003','L6010','B1013', 430,01000303,'13-3月-2019','入库');
insert into records values ( 19004,'F0004','L6021','B1014', 430,01000406,'10-2月-2019','取出');
insert into records values ( 19005,'F0005','L6021','B2021', 430,01000507,'15-9月-2019','入库');
insert into records values ( 19006,'F0006','L6021','B3001', 430,01000602,'20-7月-2019','取出');
insert into records values ( 19007,'F0007','L6022','B3005', 430,01000111,'12-11月-2019','取出');
insert into records values ( 19008,'F0008','L6022','B3013', 430,01000403,'7-4月-2019','取出');
insert into records values ( 19009,'F0009','L6022','B3014', 430,02200111,'9-6月-2019','入库');
insert into records values ( 19010,'F0010','L6023','B3021', 430,02200152,'11-5月-2019','入库');
insert into records values ( 19011,'F0011','L6023','B3022', 430,02200531,'19-9月-2019','取出');
insert into records values ( 19012,'F0012','L6023','B3031', 430,02400320,'15-6月-2019','取出');
insert into records values ( 19013,'F0013','L6852','B3052', 430,03100122,'13-5月-2019','入库');
insert into records values ( 19014,'F0014','L6852','B3055', 430,02100101,'25-7月-2019','取出');
insert into records values ( 19015,'F0015','L6852','B3062', 430,02100305,'16-8月-2019','取出');
insert into records values ( 19016,'F0016','L6853','B5011', 430,02100402,'31-1月-2019','取出');
insert into records values ( 19017,'F0017','L6853','B5023', 430,02100309,'18-5月-2019','入库');
insert into records values ( 19018,'F0018','L6853','B5027', 430,02300506,'22-7月-2019','取出');
insert into records values ( 19019,'F0019','L6020','B5030', 430,02300409,'15-6月-2019','取出');
insert into records values ( 19020,'F0020','L6020','B7001', 430,02300109,'29-1月-2019','入库');
insert into records values ( 19021,'F0021','L6024','B7002', 430,04200303,'16-10月-2019','取出'); 
insert into records values ( 19022,'F0022','L6024','B7003', 430,05500901,'30-5月-2019','取出');
insert into records values ( 19023,'F0023','L6028','B7004', 430,07200503,'19-7月-2019','入库');
insert into records values ( 19024,'F0024','L6028','B5030', 430,04600102,'7-6月-2019','取出');
insert into records values ( 19025,'F0025','L6025','B3014', 430,05900406,'9-4月-2019','入库');
insert into records values ( 19026,'F0026','L6025','B3013', 430,08200701,'5-11月-2019','入库');
insert into records values ( 19027,'F0027','L6027','B1012', 430,09300502,'1-6月-2019','入库');
insert into records values ( 19028,'F0028','L6027','B1011', 430,09300907,'22-7月-2019','取出');
insert into records values ( 19029,'F0029','L6029','B3062', 430,02300315,'12-5月-2019','入库');
insert into records values ( 19030,'F0030','L6029','B5011', 430,04500601,'27-9月-2019','取出');	
```

##### 3.3.1.2.6 B1 创建序列

```sql
create sequence records_num start with 19001 increment by 1 minvalue 19001;
```

##### 3.3.1.2.6 B2 使用序列插入带有序列值的操作记录（操作记录编号，管理员工号，书库编号，教材编号，操作教材数量，院系编号，操作时间，操作类型）

```sql
insert into records values ( records_num.nextval,'F0001','L6010','B1011', 430,01000101,sysdate,'取出');
insert into records values ( records_num.nextval,'F0002','L6010','B1012', 430,01000205, sysdate,'入库');
insert into records values ( records_num.nextval,'F0003','L6010','B1013', 430,01000303, sysdate,'入库');
insert into records values ( records_num.nextval,'F0004','L6021','B1014', 430,01000406, sysdate,'取出');
insert into records values ( records_num.nextval,'F0005','L6021','B2021', 430,01000507, sysdate,'入库');
insert into records values ( records_num.nextval,'F0006','L6021','B3001', 430,01000602, sysdate,'取出');
insert into records values ( records_num.nextval,'F0007','L6022','B3005', 430,01000111, sysdate,'取出');
insert into records values ( records_num.nextval,'F0008','L6022','B3013', 430,01000403, sysdate,'取出');
insert into records values ( records_num.nextval,'F0009','L6022','B3014', 430,02200111, sysdate,'入库');
insert into records values ( records_num.nextval,'F0010','L6023','B3021', 430,02200152, sysdate,'入库');
insert into records values ( records_num.nextval,'F0011','L6023','B3022', 430,02200531, sysdate,'取出');
insert into records values ( records_num.nextval,'F0012','L6023','B3031', 430,02400320, sysdate,'取出');
insert into records values ( records_num.nextval,'F0013','L6852','B3052', 430,03100122, sysdate,'入库');
insert into records values ( records_num.nextval,'F0014','L6852','B3055', 430,02100101, sysdate,'取出');
insert into records values ( records_num.nextval,'F0015','L6852','B3062', 430,02100305, sysdate,'取出');
insert into records values ( records_num.nextval,'F0016','L6853','B5011', 430,02100402, sysdate,'取出');
insert into records values ( records_num.nextval,'F0017','L6853','B5023', 430,02100309, sysdate,'入库');
insert into records values ( records_num.nextval,'F0018','L6853','B5027', 430,02300506, sysdate,'取出');
insert into records values ( records_num.nextval,'F0019','L6020','B5030', 430,02300409, sysdate,'取出');
insert into records values ( records_num.nextval,'F0020','L6020','B7001', 430,02300109, sysdate,'入库');
insert into records values ( records_num.nextval,'F0021','L6024','B7002', 430,04200303, sysdate,'取出'); 
insert into records values ( records_num.nextval,'F0022','L6024','B7003', 430,05500901, sysdate,'取出');
insert into records values ( records_num.nextval,'F0023','L6028','B7004', 430,07200503, sysdate,'入库');
insert into records values ( records_num.nextval,'F0024','L6028','B5030', 430,04600102, sysdate,'取出');
insert into records values ( records_num.nextval,'F0025','L6025','B3014', 430,05900406, sysdate,'入库');
insert into records values ( records_num.nextval,'F0026','L6025','B3013', 430,08200701, sysdate,'入库');
insert into records values ( records_num.nextval,'F0027','L6027','B1012', 430,09300502, sysdate,'入库');
insert into records values ( records_num.nextval,'F0028','L6027','B1011', 430,09300907, sysdate,'取出');
insert into records values ( records_num.nextval,'F0029','L6029','B3062', 430,02300315, sysdate,'入库');
insert into records values ( records_num.nextval,'F0030','L6029','B5011', 430,04500601, sysdate,'取出');	
```

### 3.3.2 视图

#### 3.3.2.1 创建视图

##### 3.3.2.1.1 教材——出版社 视图

```sql
create or replace view book_press as 
select b.book_id 教材编号, b.book_name 教材名称, b.book_kind 教材种类, p.press_id 出版社编号, p.press_name 出版社名称， p.publish_number 出版数量 
from book b, press p
where b.book_id = p.book_id;
```

##### 3.3.2.1.2 书库——教材 视图

```sql
create or replace view library_book as 
select l.library_id 书库编号, l.library_name 书库名称, l.library_add 书库地址, b.book_id 教材编号, b.book_name 教材名称，b.book_number 教材数量
from library l, book b
where l.book_id = b.book_id;
```

##### 3.3.2.1.3 院系——教材 视图

```sql
create or replace view faculty_book as 
select f.faculty_number 院系编号, f.faculty_name 院系名称, b.book_id 教材编号, b.book_name 教材名称，f.demand 需求量
from faculty f, book b
where f.book_id = b.book_id;
```

##### 3.3.2.1.4 操作记录清单 视图

```sql
create or replace view records_ as 
select r.records_number 操作记录编号, m.management_name 管理员姓名, 
        m.management_phone_number 管理员电话, l.library_add 书库地址, 
        l.library_name 书库名称, f.school_address 学校地址, 
        f.faculty_name 院系名称, f.department_name 院系负责人姓名, 
        b.book_name 教材名称, b.book_number 已有数量, 
        f.demand 变更数量, to_char(operation_time, 'yyyy-mm-dd') 操作时间, 
        action_type 操作类型
from records r, manager m, library l, book b, faculty f
where r.management_number = m.management_number and
      r.library_id = l.library_id and
      r.book_id = b.book_id and
      r.faculty_number = f.faculty_number;
```

#### 3.3.2.2 创建索引

```sql
create index bo_bn on book(book_name);
create index li_ln on library(library_name);
create index pr_pn on press(press_name);
create index fa_fn on faculty(faculty_name);
create index ma_mn on manager(management_name);
create index re_ti on records(operation_time);
```

### 3.3.3 存储过程

#### 3.3.3.1存储过程

##### 3.3.3.1.1 A 创建通过查询教材编号显示教材相关信息存储过程

```sql
create or replace procedure search_book_id(book_id_p varchar2) as
  se_bo_id_ book%rowtype;
  se_pr_id_ press%rowtype;
  se_bo_error exception;
  se_pr_error exception;
  cursor cur_se_bo_id is
    select *
      from book
      where book_id= book_id_p;
  cursor cur_se_pr_id is
    select *
      from press
      where book_id= book_id_p;
begin
  open cur_se_bo_id;
  open cur_se_pr_id;
  fetch cur_se_bo_id into se_bo_id_;
  fetch cur_se_pr_id into se_pr_id_;
  if cur_se_bo_id%found then 
    dbms_output.put_line('所查询到教材编号 '||se_bo_id_.book_id||
                          ' 的教材名称是: '||se_bo_id_.book_name||
                          ', 教材数量:'||se_bo_id_.book_number);
    if cur_se_pr_id%found then
      dbms_output.put_line('其所属出版社编号为: '||se_pr_id_.press_id||
                            ' 出版社名称为: '||se_pr_id_.press_name||
                            ' 出版社地址: '||se_pr_id_.press_address||
                            ' 出版日期: '||se_pr_id_.publish_date);
    else
      raise se_pr_error;
    end if;
  else
    raise se_bo_error;
  end if;
  close cur_se_pr_id;
  close cur_se_bo_id;
  exception when se_bo_error then
    dbms_output.put_line('无法查到相应的教材编号。');
  when se_pr_error then
    dbms_output.put_line('该教材的出版社暂未录入。');
end;
```

##### 3.3.3.1.1 B 测试search_book_id(book_id_p varchar)存储过程

```sql
set serveroutput on;
execute search_book_id('B1011');

set serveroutput on;
execute search_book_id('&输入教材编号，查询教材的相关信息');
```

##### 3.3.3.1.2 A 创建通过查询书库地址显示该地址所存在的书库、出版社和院系的存储过程

```sql
create or replace procedure search_add(add_name varchar2) as
  library_add_ library%rowtype;
  press_add_ press%rowtype;
  faculty_add_ faculty%rowtype;
  
  library_add_error exception;
  press_add_error exception;
  faculty_add_error exception;
  
  cursor cur_li_id is
    select *
      from library
      where library_add= add_name;
      
  cursor cur_pr_id is
    select *
      from press
      where press_address= add_name;
      
  cursor fac_pr_id is
    select *
      from faculty
      where school_address= add_name;
      
begin
  open cur_li_id;
  open cur_pr_id;
  open fac_pr_id;
  
  fetch cur_li_id into library_add_;
  fetch cur_pr_id into press_add_;
  fetch fac_pr_id into faculty_add_;
  
  if cur_li_id%found then 
    dbms_output.put_line('所查询地址 '|| library_add_.library_add||
                          ' 的书库编号是: '|| library_add_.library_id||
                          ', 书库名称:'||library_add_.library_name||
                          ', 所存入的教材编号为:'||library_add_.book_id);
  else
    raise library_add_error;
  end if;
  
  if cur_pr_id%found then 
    dbms_output.put_line('所查询地址 '|| press_add_.press_address||
                          ' 的出版社编号是: '|| press_add_.press_id||
                          ', 出版社电话:'|| press_add_.press_phone_number||
                          ', 出版社名称:'|| press_add_.press_name||
                          ', 所出版的教材编号为:'||press_add_.book_id);
  else
    raise press_add_error;
  end if;
  
  if fac_pr_id%found then 
    dbms_output.put_line('所查询地址 '|| faculty_add_.faculty_name||
                          ' 的出版社编号是: '|| faculty_add_.faculty_number);
  else
    raise faculty_add_error;
  end if;
  
  close cur_pr_id;
  close cur_li_id;
  close fac_pr_id;
  
  exception 
    when library_add_error then
      dbms_output.put_line('未查到相应的书库地址。');
    when press_add_error then
      dbms_output.put_line('未查到相应的出版社地址。');
    when faculty_add_error then
      dbms_output.put_line('未查到相应的院系地址。');
    when others then
      dbms_output.put_line('其他错误。');
end;
```

##### 3.3.3.1.2 B 测试search_add(book_id_p varchar)存储过程

```sql
set serveroutput on;
execute search_add('北京市');

set serveroutput on;
execute search_add('&输入地区，查询该地址所存在的书库、出版社和院系');
```

##### 3.3.3.1.3 A 创建通过查询管理员工号显示管理员的信息的存储过程

```sql
create or replace procedure search_manager_(manager_number varchar2) as
  manager_num_ manager%rowtype;
  manager_number_error exception;
  cursor cur_man_num is
    select *
      from manager
      where management_number= manager_number;
begin
  open cur_man_num;
  fetch cur_man_num into manager_num_;
  if cur_man_num%found then 
    dbms_output.put_line('所查询管理员工号 '|| manager_num_.management_number||
                          ' 的姓名: '|| manager_num_.management_name||
                          ', 电话:'|| manager_num_.management_phone_number||
                          ', 所管理的教材种类:'|| manager_num_.book_kind||
                          ', 管理员密码:'|| manager_num_.management_password);
  else
    raise manager_number_error;
  end if;
  close cur_man_num;
  exception 
    when manager_number_error then
      dbms_output.put_line('未查到相应的管理员工号。');
end;
```

##### 3.3.3.1.3 B 测试search_manager_(manager_number varchar2)存储过程

```sql
set serveroutput on;
execute search_manager_('F0001');

set serveroutput on;
execute search_manager_('&输入管理员工号，显示管理员的相关信息');
```

##### 3.3.3.1.4 A 创建通过查询管理员姓名显示管理员操作记录的存储过程

```sql
create or replace procedure search_manager_records(manager_name varchar2) as
  manager_records records%rowtype;
  manager_records_error exception;
  cursor cur_man_rec is
    select *
      from records
      where management_number= (
        select management_number
        from manager
        where management_name= manager_name);
begin
  open cur_man_rec;
  if cur_man_rec%isopen then
    loop
      fetch cur_man_rec into manager_records;
      exit when cur_man_rec%notfound;
      dbms_output.put_line('所查询管理员 '|| manager_name||
                            ' 的工号 '|| manager_records.management_number||
                            ' 在 '|| to_char(manager_records.operation_time,'yyyy-mm-dd')||
                            ' 对书库 '|| manager_records.library_id||
                            ' 中的教材 '|| manager_records.book_id||
                            ' 进行 '|| manager_records.action_type||
                            ' 操作, 涉及院系:'|| manager_records.faculty_number);
    end loop;
  else
    raise manager_records_error;
  end if;
  close cur_man_rec;
  exception 
    when manager_records_error then
      dbms_output.put_line('未查到相应的管理员姓名。');
end;
```

##### 3.3.3.1.4 B 测试search_manager_records(manager_name varchar2)存储过程

```sql
set serveroutput on;
execute search_manager_records('王平');

set serveroutput on;
execute search_manager_records('&输入管理员姓名，查询管理员的操作记录');
```

#### 3.3.3.2 ?函数(不可修改表中数据)

##### 3.3.3.2.1 A 通过查询书库编号检查书库库存量是否需要调整

```sql
create or replace function library_f(library_id_f in varchar2)
return number is
  inventory_ number;
  inventory_average number;
begin
  select inventory
    into inventory_
    from library
    where library_id = library_id_f;
  select avg(inventory)
    into inventory_average
    from library;
  if inventory_ < inventory_average then
    return (inventory_ + 1000);
  else
    return (inventory_ - 1000);
  end if;
end library_f;
```

##### 3.3.3.2.1 B 测试library_f(library_id_f in varchar2)函数

```sql
select library_f('&请输入书库编号，查询是否需要扩容。')
from dual;
```

### 3.3.4 触发器

#### 3.3.4.1 A 通过book_press视图插入数据

```sql
create or replace trigger insert_book_press
  instead of insert on book_press
  for each row
declare 
  book_ book%rowtype;
  press_ press%rowtype;
begin
  insert into book(book_id, book_name, book_kind, book_number) 
    values(:new.教材编号, :new.教材名称, :new.教材种类, :new.出版数量);
  insert into press(press_id, press_name, book_id, publish_number)
    values(:new.出版社编号, :new.出版社名称, :new.教材编号, :new.出版数量);
end insert_press;
```

#### 3.3.4.1 B 测试触发器insert_book_press数据

```sql
insert into book_press values('B7005','商务英语','英语','P6390','山东大学出版社',2000);
insert into book_press values('&教材编号','&教材名称','&教材种类','&出版社编号','&出版社名称',&出版数量);
```

#### 3.3.4.2 A 在非工作时间不允许对操作记录表进行操作

```sql
create or replace trigger time_records 
  before insert or update or delete on records
begin
  if (to_char(sysdate, 'dy') in ('星期一','星期六','星期日'))
    or (to_char(sysdate, 'hh24') not between '08:00' and '18:00')
  then
    raise_application_error(-20999, '只允许在工作时间内修改表');
  end if;
end;
```

#### 3.3.4.2 B 测试触发器time_records数据

```sql
insert into records values ( 1931,'F0029','L6023','B5011', 430,09300502, sysdate, '入库');
insert into records values ( records_num.nextval,'&管理员编号','&书库编号','&教材编号', &教材数量, &院系编号, sysdate, '&操作');

update records set faculty_number=&为相应的操作记录修改电话号码，首先输入修改后的电话号码 where records_number=&相应的操作记录编号;

delete from records where records_number=&要删除操作记录的操作记录编号;
```

### 3.3.5 删除用户

#### 3.3.1 D 删除first用户

```sql
drop user first cascade;
```

# 4 课程设计总结

总结

我们的这个学校书库管理系统具有丰富的数据，以及详细的信息资料，还有很多类型教材，达到了覆盖范围广，样式多，每个院系可以更好的了解自己需要的图书教材，也有管理员功能可以进行及时的弥补错误。最后达到书库的流畅运行。通过这次课程设计真正让哦我们一个团队意识到了团结的力量，每个人或多或少收获到了知识，收到了友情，在设计中每次与同学、老师的沟通也一次次的告诉自己要时刻保持积极的态度，努力学习。我们这次的设计也存在很多不足，不光是知识上面的，也有这个设计本身的不足，管理效率低，劳动强度大，信息处理速度低而且准确率也不够令人满意.我们希望我们可以在后面的学习利用所学到的知识加强这个系统的运行速率，保证正确性的同时，可以加一些新鲜的想法，比如院系或者老师可以自己操作一些书库的功能。减少劳动强度，让更多人提意见，加入到这个系统设计进来。争取达到更先进、更科学的服务系统。最后我觉得以我们现在的知识能设计出来现在的这个书库管理系统，已经是一种欣慰，自己对自己的一种鼓励。最后我希望这个系统真的可以有实际价值帮助到一些人。
