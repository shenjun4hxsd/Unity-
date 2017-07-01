##数据库

数据库（Database）是按照数据结构来组织、存储和管理数据的仓库，每个数据库都有一个或多个不同的API用于创建，访问，管理，搜索和复制所保存的数据。

我们也可以将数据存储在文件中，但是在文件中读写数据速度相对较慢。
所以，现在我们使用关系型数据库管理系统（RDBMS）来存储和管理的大数据量。所谓的关系型数据库，是建立在关系模型基础上的数据库，借助于集合代数等数学概念和方法来处理数据库中的数据。

RDBMS即关系数据库管理系统(Relational Database Management System)的特点：

        1.数据以表格的形式出现
        2.每行为各种记录名称
        3.每列为记录名称所对应的数据域
        4.许多的行和列组成一张表单
        5.若干的表单组成database


####RDBMS 术语
在我们开始学习MySQL 数据库前，让我们先了解下RDBMS的一些术语：

```
• 数据库: 数据库是一些关联表的集合。
• 数据表: 表是数据的矩阵。在一个数据库中的表看起来像一个简单的电子表格。
• 列: 一列(数据元素) 包含了相同的数据, 例如邮政编码的数据。
• 行：一行（=元组，或记录）是一组相关的数据，例如一条用户订阅的数据。
• 冗余：存储两倍数据，冗余降低了性能，但提高了数据的安全性。
• 主键：主键是唯一的。一个数据表中只能包含一个主键。你可以使用主键来查询数据。
• 外键：外键用于关联两个表。
• 复合键：复合键（组合键）将多个列作为一个索引键，一般用于复合索引。
• 索引：使用索引可快速访问数据库表中的特定信息。索引是对数据库表中一列或多列的值进行排序的一种结构。类似于书籍的目录。
• 参照完整性: 参照的完整性要求关系中不允许引用不存在的实体。与实体完整性是关系模型必须满足的完整性约束条件，目的是保证数据的一致性。
```

####Mysql数据库
MySQL是一个关系型数据库管理系统，由瑞典MySQL AB公司开发，目前属于Oracle公司。MySQL是一种关联数据库管理系统，关联数据库将数据保存在不同的表中，而不是将所有数据放在一个大仓库内，这样就增加了速度并提高了灵活性。

```
• Mysql是开源的，所以你不需要支付额外的费用。
• Mysql支持大型的数据库。可以处理拥有上千万条记录的大型数据库。
• MySQL使用标准的SQL数据语言形式。
• Mysql可以允许于多个系统上，并且支持多种语言。这些编程语言包括C、C++、Python、Java、Perl、PHP、Eiffel、Ruby和Tcl等。
• Mysql对PHP有很好的支持，PHP是目前最流行的Web开发语言。
• MySQL支持大型数据库，支持5000万条记录的数据仓库，32位系统表文件最大可支持4GB，64位系统支持最大的表文件为8TB。
• Mysql是可以定制的，采用了GPL协议，你可以修改源码来开发自己的Mysql系统。
```



####SQL语句：

```
mysql -h localhost -u 用户名 -p密码
use 库名；	// 切换默认库

DDL：对数据库和表进行操作
	create database 库名	// create database if not exists 库名
	create table [苦命.]表名   //use 库名

	drop database 库名
	drop table 表名

DML：对数据库表进行增删改
	insert into users(id,name) values('1','zhangsan');
	update users set name='lili',age='18' where id='1';   // 更新
	delete from name where id='1';

DQL:对数据库进行查询操作
	select * from name

DCL:查看所有库、表结构等
	show databases	看所有库
	show tables	看所有表
	decs 表名		看表结构
	show variables	配置文件变量

	\s	看状态

帮助的使用
	？ contents	看帮助内容
	? int
	? show
	? create table

-- 创建数据库
create database game_data;

-- 切换默认表
use game_data;

-- 创建表
create table tbl_enemy (id int(3), name varchar(20), movespeed int(4), atk int(4)) character set = utf8;

-- 删除表
drop table tbl_enemy;

-- 查看表
select * from tbl_enemy;

-- 添加数据
insert into tbl_enemy values(1, '近战兵', 10, 10);
insert into tbl_enemy values(2, '弓箭手', 10, 10);
insert into tbl_enemy values(1, '法师', 10, 10, 1, 100);

-- 修改字段值
update tbl_enemy set id=3 where name='法师';
update tbl_enemy set move_speed=8, atk_interval=1 where id=1;
update tbl_enemy set move_speed = 12 where id=2;
update tbl_enemy set atk=20, atk_interval=1 where id=3;
update tbl_enemy set atk = 8, atk_interval=1 where id=2;

update tbl_enemy set atk_interval=2;

update tbl_enemy set atk_interval=1 where id=1;

update tbl_enemy set hp=100;

-- 增加字段
alter table tbl_enemy add atkinterval int(4);
alter table tbl_enemy add hp int(4) not null;
alter table tbl_enemy add email varchar(20);

-- 删除字段
alter table tbl_enemy drop email;

-- 删除行
delete from tbl_enemy where hp=100;

-- 字段改名(改数据类型)
alter table tbl_enemy change movespeed move_speed int(4);
alter table tbl_enemy change atkinterval atk_interval int(4);

alter table tbl_enemy rename table_enemy;
alter table table_enemy rename tbl_enemy;

```





