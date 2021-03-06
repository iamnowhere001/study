iOS开发 数据缓存－数据库(1)
字数2004 阅读3618 评论0 喜欢52
iOS中数据存储方式
Plist(NSArray\NSDictionary)
Preference(偏好设置\NSUserDefaults)
NSCoding (NSKeyedArchiver\NSkeyedUnarchiver)
SQlite3
Core Date

Plist、Preference、NSCoding的存储方式
详见 [iOS开发 文件存储方式](http://www.jianshu.com/p/1a9f84392691)

数据库的存储方式
Core Date：Core Data是iOS5之后才出现的一个框架，它提供了对象-关系映射(ORM)的功能，即能够将OC对象转化成数据，保存在SQLite数据库文件中，也能够将保存在数据库中的数据还原成OC对象。在此数据操作期间，我们不需要编写任何SQL语句。由于功能太过强大，所以带来的性能损耗也比较大，在此先不介绍Core Data的处理方式，有关Core Data与SQLite 的选择，详见[谈谈用 SQLite 和 FMDB 而不用 Core Data](http://ios.jobbole.com/81943/)。

SQLite：

什么是SQLite
SQLite是一款轻量级的嵌入式数据库
它占用资源非常的低，在嵌入式设备中，可能只需要几百K的内存就够了
它的处理速度比MySQL、PostgreSQL这两款著名的数据库还快

什么是数据库
数据库（Database）是按照数据结构来组织、存储和管理数据的仓库　
数据库可以分为两大类:关系型数据库（主流） 和 对象型数据库

常用关系数据库
SQLite
MySQL
Oracle

数据库是如何存储数据的 : 数据库的存储结构和excel很像，以表（table）为单位.
数据库存储数据的步骤
新建一张表 （table）
添加多个字段（column、列、属性）
添加多行记录 (row、每行存放多个字段对应的值)

SQL 语句简介
1.SQL语句的特点：

不区分大小写
每条语句必须以分号结尾

2.SQL中的常用关键词

select、insert、update、delete、from、create、where、desc、order、by、group、table、alter、view、index等等

3.数据库中不可以使用关键字来命名表、字段

SQL语句的种类
数据定义语句 （DDL：Data Definition Language）
包括create和drop等操作
在数据库中创建新表或删除表（create table 或 drop table）

数据操作语句（DML：Data Manipulation Language）
包括insert、update、delete等操作
上面的3种操作分别用于添加、修改、删除表中的数据

数据查询语句（DQL：Data Query Language）
可以用于查询获得表中的数据
关键字select是DQL（也是所有SQL）用得最多的操作
其他DQL常用的关键字有where，order by，group by 和 having

DDL 数据定义
创表
格式(一般表名以t_作为前缀)
create table 表名 (字段名1 字段类型1, 字段名2 字段类型2, …) ;
create table if not exists 表名 (字段名1 字段类型1, 字段名2 字段类型2, …) ;

字段类型
integer : 整型值
real : 浮点值
text : 文本字符串
blob : 二进制数据（比如文件）

示例create table t_student (id integer, name text, age inetger, score real) ;

删表
格式
drop table 表名 ;
drop table if exists 表名 ;

示例
drop table t_student ;

DML 数据操作
插入数据（insert）
格式 : 数据库中的字符串内容应该用单引号 ’ 括住
insert into 表名 (字段1, 字段2, …) values (字段1的值, 字段2的值, …) ; 

示例insert into t_student (name, age) values ('kingsly', 20) ;

更新数据（update）
格式
update 表名 set 字段1 = 字段1的值, 字段2 = 字段2的值, … ; 

示例update t_student set name = ‘Roger’, age = 34 ;
结果
将t_student表中的所有记录的name改为Roger，age都改为34

删除数据 (delete)
格式
delete from 表名;

示例delete from t_student;
结果
将t_student表表中的所有字段的所有记录都删除

DQL 数据查询
格式
select 字段1,字段2,...from 表名;
select * from 表名; // 查询所有字段

示例select name,age frome t_student;select * frome t_student;
重命名字段名
格式
select 字段1 别名 , 字段2 别名 , … from 表名 别名 ; 
select 字段1 别名, 字段2 as 别名, … from 表名 as 别名 ;
select 字段1 别名, 字段2 as 别名, … from 表名 as 别名 ;

示例
给name起个叫做myname的别名，给age起个叫做myage的别名select name myname, age myage from t_student ;
给t_student表起个别名叫做s，利用s来引用表中的字段select s.name, s.age from t_student s ;

统计
格式
select count [字段] frome 表名;
select count [*] frome 表名

示例
select count (age) from t_student ;

SQL 语句中的条件限制
如果只想更新或者删除某些固定的记录，那就必须在DML语句后加上一些条件
条件语句常见的格式
where 字段 = 某个值 ; // 不能用两个 =
where 字段 != 某个值 ; 
where 字段 > 某个值 ; 
where 字段1 = 某个值 and 字段2 > 某个值 ; // and相当于C语言中的 &&
where 字段1 = 某个值 or 字段2 = 某个值 ; // or 相当于C语言中的 ||

示例
将t_student表中年龄大于10 并且 姓名不等于jack的记录，年龄都改为 5update t_student set age = 5 where age > 10 and name != ‘jack’ ;

排序
select * from t_student order by 字段 ;
查询出来的结果可以用order by进行排序

select * from t_student order by age ;

默认是按照升序排序(由小到大),也可以变为降序（由大到小）select * from t_student order by age desc ; //降序select * from t_student order by age desc ; //降序
也可以用多个字段进行排序先按照年龄排序（升序），年龄相等就按照身高排序（降序）select * from t_student order by age asc, height desc ;

limit 查询数量
使用limit可以精确地控制查询结果的数量
格式
select * from 表名 limit 数值1, 数值2 ;

示例
在第5条数据后，然后取10条记录

select * from t_student limit 5, 10 ;

建表约束
建表时可以给特定的字段设置一些约束条件，常见的约束有
not null ：规定字段的值不能为null
unique ：规定字段的值必须唯一
default ：指定字段的默认值

示例
name字段不能为null，并且唯一,age字段不能为null，并且默认为1create table t_student (id integer, name text not null unique, age integer not null default 1) ;

主键
Primary Key ,用来唯一地标识某一条记录
t_student 表中，若没有设置一个字段为主键，则难免会出现几个记录中name和age完全相等的情况，因此需要一个标识来区分，比如人的身份证id，来区分同名和相同年龄的人
主键可以是一个或者多个字段
主键应当不影响用户记录的数据，最好是由电脑自动生成主键

主键的声明
在创表的时候用primary key声明一个主键，eg：用一个integer类型的id字段作为t_student表的主键create table t_student (id integer primary key, name text, age integer) ;
主键字段默认就包含了not null 和 unique 两个约束
让integer类型的主键自动增长，需要添加 autoincrement,一般情况下让主键自动增加便于管理create table t_student (id integer primary key autoincrement, name text, age integer) ;

外键约束
利用外键约束可以用来建立表与表之间的联系 : 一个表中的一个字段来自另外一张表里面的一个字段。
学生表的班级字段，来自班级表中的id字段create table t_student (id integer primary key autoincrement, name text, age integer, class_id integer, constraint fk_t_student_class_id_t_class_id foreign key (class_id) (id)) ; references t_class

表连接查询
如果需要联合多张表才能查到想要的数据，一般需要使用表连接
表连接的类型
内连接：inner join 或者 join （显示的是左右表都有完整字段值的记录）
左外连接：left outer join （保证左表数据的完整性）

示例：查询网络工程2班的所有学生select s.name,s.age from t_student s, t_class c where s.class_id = c.id and c.name = '网络工程2班';

---

iOS开发 数据缓存－FMDB的使用(2)
字数1044 阅读2697 评论5 喜欢13
1.SQLite3的使用
要使用SQLite3，先要添加库文件 libsqlite3.dylib 。

QQ20150823-1.png
导入头文件import<sqlite3.h>那么到这里FMDB的准备工作已经就绪了，接下来我们看一下FMDB的使用方法 

2.FMDB的介绍
FMDB是iOS平台的SQLite数据库框架，以OC的方式封装了SQLite的C语言API，使用起来比单纯调用SQLite语句方便
FMDB使用起来更加面向对象，省去了很多麻烦、冗余的C语言代码
对比苹果自带的Core Data框架，更加轻量级和灵活
提供了多线程安全的数据库操作方法，有效地防止数据混乱
下载地址：[FMDB的github](https://github.com/ccgus/fmdb)import "FMDB.h"

3.核心三大类
FMDatabase(创建、删除)
一个FMDatabase对象就代表一个单独的SQLite数据库
用来执行SQL语句

FMResultSet(用来查询)
使用FMDatabase执行查询后的结果集

FMDatabaseQueue
用于在多线程中执行多个查询或更新，它是线程安全的

4.使用步骤
(1) 打开数据库
通过指定SQLite数据库文件路径来创建FMDatabase对象FMDatabase *db = [FMDatabase databaseWithPath:path];if ([db open]) {// 数据库打开成功}
示例// 1.获得数据库文件的路径NSString *doc =[NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) lastObject];// 2.拼接文件名NSString *filename = [doc stringByAppendingPathComponent:@"status.sqlite"];// 3.得到数据库_db = [FMDatabase databaseWithPath:filename];// 4.打开数据库if ([_db open]) {// 5.创表BOOL result = [_db executeUpdate:@"CREATE TABLE IF NOT EXISTS t_home_status (id integer PRIMARY KEY AUTOINCREMENT, access_token text NOT NULL, status_idstr text NOT NULL, status_dict blob NOT NULL);"];if (result) {NSLog(@"成功创表");} else {NSLog(@"创表失败");}3.文件路径的三种情况(1)具体文件路径

- 如果不存在会自动创建

(2)空字符串@""（不推荐）

- 会在临时目录创建一个空的数据库

- 当FMDatabase连接关闭时，数据库文件也被删除

(3)nil

- 会创建一个内存中的临时文件，当FMDatabase连接关闭时，数据库也会被销毁
(2)执行更新
CREATE,DROP,INSERT,UPDATE,DELETE等除了查询以外的操作，都称为更新操作
使用executeUpdate:方法做更新操作
-(BOOL)executeUpdate:(NSString*)sql, ...
-(BOOL)executeUpdateWithFormat:(NSString*)format, ...
-(BOOL)executeUpdate:(NSString)sql withArgumentsInArray:(NSArray )arguments
在SQL字符串中写SQL语句

示例[db executeUpdate:@"UPDATE t_student_classtwo SET age = ? WHERE name = ?;", @"21", @"Kingsly"]

(3) 执行查询
执行数据库查询的方法:(查询使用FMResultSet对象)
-(FMResultSet )executeQuery:(NSString )sql, ...
-(FMResultSet )executeQueryWithFormat:(NSString)format, ...

-(FMResultSet )executeQuery:(NSString )sql withArgumentsInArray:(NSArray *)arguments
用 FMResultSet 对象的 next 方法来遍历查询到的数据

示例// 查询数据FMResultSet *rs = [db executeQuery:@"SELECT * FROM t_student_classtwo"];// 遍历结果集while ([rs next]) {NSString *name = [rs stringForColumn:@"name"];int age = [rs intForColumn:@"age"];double score = [rs doubleForColumn:@"score"];}

FMDatabaseQueue的使用
为什么使用FMDatabaseQueue
FMDatabase这个类是线程不安全的，如果在多个线程中同时使用一个FMDatabase实例，会造成数据混乱等问题。

![](http://upload-images.jianshu.io/upload_images/1336268-995fd7d86640744b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
图中对象A和对象B同时修改银行数据库中Money的余额，A在原来的基础上加200，B在原来的基础上加500，这样会发生冲突，会把相互修改的覆盖，这样无缘无故少了几百块钱，银行规定多出来的钱才是跟银行有关、钱少了是不负责任的。所以这样是线程不安全的，会出问题。
为了保证线程安全，FMDB提供方便快捷的FMDatabaseQueue类
示例FMDatabaseQueue *queue = [FMDatabaseQueue databaseQueueWithPath:path];
简单使用[queue inDatabase:^(FMDatabase *db) {[db executeUpdate:@"INSERT INTO t_student(name) VALUES (?)", @"Jack"];[db executeUpdate:@"INSERT INTO t_student(name) VALUES (?)", @"Rose"];[db executeUpdate:@"INSERT INTO t_student(name) VALUES (?)", @"Jim"];FMResultSet *rs = [db executeQuery:@"select * from t_student"];while ([rs next]) {// …}}];
开启事务:事务对于数据库的作用是对数据的一系列操作，要么全部成功，要么全部失败，防止中间状态的出现，以确保数据库中的数据始终处于正确及和谐状态。[self.queue inDatabase:^(FMDatabase *db) {// 开启事务[db beginTransaction];BOOL result = [db executeUpdate:@"update t_student set age = 20 where id >= 20; "];if (result) {NSLog(@"修改成功");}else{NSLog(@"修改失败");}// 提交事务[db commit];}];

