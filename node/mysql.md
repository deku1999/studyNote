# node操作mysql数据库

安装：`npm install mysql --save`

```javascript
var mysql = require('mysql')
//	创建连接
var connecttion = mysql.createConnection({
	host: 'localhost',
	user: 'me',	//用户名
	password:'root',	//密码
	database: 'dbname'	//数据库名
})
//	连接数据库
connection.connect()
//	执行数据操作
connection.query('..sql语句',function(err,result,files) {
	if(err) {throw err}
	console.log('the solution is',result[0].solution)
})
//	关闭连接
connection.end()
```

# mysql数据表基本数据类型

- 数字，tinyint，int，bigint；FLOAT，DOUBLE，decimal（这个存储字符一直是精准的）。
  - num decimal(10,5)，第一个参数表示整数部分加小数部分一共的位数，第二个参数表示小数点后的位数
- 字符串
  - char(10)，如果存储的字符串没有占满位数就会自动填充剩下的位置，最多255个字符。
  - varchar(10)，有几个就存几个不会自动填充剩下的，可以节省空间；不过查询速度不如char快。因为char(10)固定10位，程序可以自动跳过10位去查询别的列，因此char查询会更快。
  - 创建数据表时，把定长列往前放可以加快查询速度。意思就是将char列放在varchar前面，这样程序可以自动跳过固定的位数就会快点。
  - text，比char和varchar存的多，但是也是有限制的，当文件内容实在比较大时，我们一般是存url。文件交给固定的文件服务器。
- 时间类型
  - DATE（年月日），TIME（时分秒），YEAR（年），DATETIME（比较常用，从年月日到时分秒）
- 枚举类型（enum），枚举类型规定了只能填枚举出的几个类型
- 集合类型（set），集合类型规定了只能填集合类型中的任意组合

# mysql常用属性定义

## 主键与外键

- 主键，是为了实现数据的唯一标识，不能为空，不能重复。
  - 一个表只能由一个主键
  - 一个主键不只能对应一列，主键可以由多列组成，例如`PRIMARY KEY (nid,pid)`

- 外键

  - 当我们某一列的类型有限时，可以通过枚举来进行限制，但是枚举不能轻易更改类型数量，而且直接存储枚举变量比较占内存。而这时我们可以将这一列与另一张表建立映射关系，让这一列的值变为另一张表的id，而另一张表的id对应着不同的值。这时我们就称建立了该列的外键。
  - 外键定义命令示例，可以给多个列定义不同表的外键约束，只要在下面示例的外键定义后面逗号继续添加就行。
  - 注意，这里外键之所以有括号是为了适应有的表的主键由多列组成的情况。

  ```
  create table userinfo(
  uid bigint auot_increment primary key,
  name varchar(32),
  department_id int,
  //定义外键，fk_user..为外键名，注意外键名不能重复
  constraint fk_user_depar foreign key(department_id) references 表2(id)
  )engine=innodb default charset=utf8
  ```

- 外键变种之一对一
  
  - 当我们对某一列增加外键约束后，还想让该列本身不重复，可以再给他添加唯一索引，这就叫外键变种之一对一
- 外键变种之多对多
  - 就是说给多列都设置了外键约束,一般适用于用第三张表去映射另外两张表中存在的相互关系
  - 根据情况来确定是否使用联合唯一索引

## 索引

- 索引需要建立额外的文件保存特殊的数据结构，需要占用硬盘空间。

- 索引查询快，插入更新删除慢
- 还有两个不是真实的索引，即索引名词
  - 覆盖索引，在索引文件中直接获取数据
  - 索引合并，把多个单列索引合并使用

- 创建索引之后需要命中索引，即如果使用方法不对那么将无法调用索引查找。
  - 例如，like '%xx'，最好避免用这个，真正的项目开发中会有第三方工具
  - 避免调用函数，函数会将数据库所有的数据都操作一遍也比较慢
  - 与or结合可能也会无法命中索引
  - 传入的类型要一致，不然内部还有进行转换也会拖慢速度
  - 普通的索引使用`!=`时不会走索引，主键仍然会。类似的`>`，普通的索引也不会走，主键会。
  - 当根据索引排序的时候，选择的映射如果不是索引那么也不会走索引。当然主键也是例外的
  - 还有组合索引的最左前缀，下面组合索引中有介绍

- 我们应该对频繁查找的列建立索引，而不是一味的建立索引

```
//普通索引
create index 索引名称 on 表名(列名)		
drop index 索引名称 on 表名		

//唯一索引
create unique index 索引名称 on 表名(列名)		
drop unique index 索引名称 on 表名

//联合索引
create unique index 索引名称 on 表名(列名，列名。。)		
drop unique index 索引名称 on 表名
```

### 普通索引

- 加速查找

### 主键索引

- 加速查找，不能为空，不能重复

### 唯一索引

- 约束(不能重复)，加速查找。唯一索引跟主键不同，唯一索引可以为空因为这样也不算重复，主键不行

```
//为num创建唯一索引，uql为约束名
create table t1(
id int ...,
num int,
xx int,
unique uq1 (num) )
//联合唯一索引，就是说他们两个的组合不能重复
create table t1(
id int ...,
num int,
xx int,
unique uq1 (num,xx) )
```

### 联合索引（多列）

- 联合主键索引，联合唯一索引，联合普通索引
- 联合索引遵循最左前缀匹配，即定义了组合索引后进行查找时，开头的列必须是定义的时候的最左列

```
create index ix_name on userinfo(name,email)	//创建联合索引
select * from userinfo where name='alex' 	//√
select * from userinfo where name='alex' and email='xx@qq.com' 		//√
select * from userinfo where email="xx@qq.com"		//×
```

### 其他注意事项

- 避免使用select *
- count(1)或count(列)代替count(*)
- 创建表时尽量使用char代替varchar，而且varchar要尽量放在列的后面
- 索引散列值（重复少）不适合建索引，例如性别就不合适
- 表的字段顺序固定长度的字段优先
- 组合索引代替多个单列索引（经常使用多个条件查询时）
- 尽量使用短索引
- 使用连接`join`来代替子查询
- 连表时注意条件类型需一致

## 自增列

- 自增列

  - 指定下一个自增值，`alter table 表名 auto_increment=数值`，这样可以指定自增列的下一个值。

  - 指定步长，需要在登录状态后会话级别（cmd窗口）指定步长然后进行插入记录时步长就是该值。在别的窗口进行操作不会影响，退出重登后也会恢复默认步长。也有全局级别的，不过基本不会用到。

    ```
    基于会话级别：
    show session variables like 'auto_inc%'		//查看全局变量
    set session auto_increment_increment=2		//设置会话步长
    set session auto_increment_offset = 10		//设置步长的起始值
    ```

## 分组

- 对于某列的重复值进行计数，例如计算专业为计算机的人一共有多少

- 分组有几个聚合函数，max：取合并的最大值；min：取合并的最小值；count：计数；还有两个sum（求和，avg（求平均值）

```
//用聚合函数进行part_id重复的计数
select count(id),part_id from tb group by part_id	
//如果对于聚合函数进行二次筛选，必须使用having，而不能使用where
select count(id),part_id from tb group by part_id having count(id)>1	/✔
select count(id),part_id from tb where id>5 group by part_id having count(id)>1	/✔
```

## 连表操作

- 连表分为左右连表和上下连表

- 就是将几张表中的信息联合查询然后进行展示，不过要记得外键关联，下例为左右连表

```
//用where连
select * from userinfo,department where userinfo.part_id=department.id

//通过left join 与on，推荐用这个，left join表示左边的表全部显示，如果是right join就是右边的全部显示。如果有数据没有与之对应的结果，那么就显示为null。如果是inner join就表示将null的那一行进行隐藏
select * from userinfo left join department on userinfo.part_id=department.id
//可以同时连接多张表，只要继续在后面写left join即可
```

- 上下连表，在查询语句中间加入`union`即可。默认的重复条数会自动去重，可以通过`union al`l来取消自动去重。

# mysql常用命令

## 操作数据库

- 连接数据库（在开启了mysql服务并配置了环境变量的前提下），`mysql -u root -p`，这里用root账户演示，不同的用户只要替换相应的用户名即可。`-u`代表用户名的意思，还有`-h`代表主机名。

- 展示所有的数据库，`show databases `
- 创建数据库
  - `create database 数据库名`，直接创建数据库
  - `create database  数据库名  default charset utf8	`，创建数据库并设置字符编码，默认的是gbk
- 切换到该数据库下，`use 数据库名`
- 删除数据库，`drop database 数据库名`

## 操作表

- 展示该数据库下所有表，`show tables`

- 展示详细的表的信息，包括类型等，`desc 表名`

- 展示表是如何被创建的，`show create table 表名`

- 创建表，以下设置可以进行组合使用，还有些设置没有列举，例如无符号设置

  - 最简单的表

    `create table 表名(id int，name char(10))`

  - 加上是否可以为空限制，默认不能为空

    ``create table 表名(id int null，name char(10) not null)``

  - 加上编码设置

    `create table 表名(id int,name char(10))  default charset=utf8`

  - 加上引擎设置，是为了使得在进行事务型操作出错时直接回溯到默认状态，这里引擎还有个叫myisam，在不涉及事务型操作时更有优势。

    `create table 表名(id int,name char(10))  engine=innodb default charset=utf8`

  - 设置自增列，auto_increment代表该列自增需要与primary key搭配使用，默认从1开始自增。primary key：表示约束（不能重复也不能为空）；加速查找。一个表只能有一个自增列和一个主键。`create table 表名(id int null,name char(10),列名 类型 auto_increment primary key
    )`

- 清空表内容
  - `delete from 表名`，这样清空表再进行插入数据如有有自增列是从上次的记录id号开始
  - `truncate table 表名`，自增列就从1开始，且速度比delete要快很多
- 删除表，`drop table 表名`

## 操作行

增，删，改一般就这几种，不过查比较多样。

- 增

  - 插入单条记录，`insert into 表名(name,age)  values('alex',12)`
  - 插入多条记录，`insert into 表名(name,age)  values('alex',12),('hehe',15)`
  - 从a表中往b表中插入记录，`insert into b(name,age) select name,age from a`

- 删

  - 删除id小于6的行记录，`delete from t1 where id<6`

- 改

  - 将t1表中所有行的age列都变为18，`update t1 set age=18``
  - 同上，不过加上了age>18的限制条件，`update t1 set age=18 where age=17`

- 查，查询出来的结果可以作为一张临时表以让别的条件继续在它基础上进行查询。

  - 查找所有列的内容，`select * from 表名`，查看特定列只需将*换成列名即可

  - 或条件下查，`select name,age from t1 where age>18 or name='xxx'`

  - 将查出来的列设置别名进行查找，`select id,name as cname from tb where id>10`

  - 增加列进行查找，这样查出来的结果增加了11该列，值都是11，`select name,age,11 from tb`

  - 其他条件

    ```
    select * from tb where id in/(not in) (1,5,12)	//id等于1，5，12的
    select * from tb where id between 5 and 12  //头，尾id都取
    select * from tb where id in (select id from tb11)	//将另一个表的查询列结果作为该次查询的条件
    ```

  - 通配符，与like结合使用，%，%不限制字符个数，例如`like a%`，以a开头的任意个数的字符；_，只能有一个，例如`like _a`，以a结尾的2个字符的词。

    ```
    select * from tb where name like "%a"
    select * from tb where name like "a_"
    ```

  - 限制个数与区间，limit

    ```
    select * from tb where limit 10		//取出前10条数据
    select * from tb limit 10,20	//取第10条后的20条数据，不包括第10条本身
    select * from tb limit 10 offset 20		//也是取出第10条后的20条数据
    ```

  - 排序

    ```
    select * from tb	//默认是从前到后排
    select * from tb order by id desc   //按照id从大到小排
    select * from tb order by id asc	//按照id从小到大排
    select * from tb order by id asc，age desc	//可以传多个条件进行组合排序来处理重复情况
    ```
## 查询扩展

- 例如，最小值大于20的显示出来，小于的显示0，可以这样写

```
case when min(num) <20 then 0 else min(num) end  
```

- mysql中的三元运算，例如`avg(if(isnull(score.num),0,score.num))`

## 备份sql文件

- 除了使用navicat这种图形化界面进行转存外，也可以用命令行来进行存储。

```

//导出现有数据库数据
备份：数据表结构+数据
mysqldump -u root db1>db1.sql -p
备份：数据表结构
mysqldump -u root -d db1>db1.sql -p

//导入数据库数据，需要先自行创建数据库
create database db5
mysqldump -u root -d db5<db1.sql -p
```

# mysql更高级点用法

## 视图

- 有时我们在使用过程中，一些临时表会频繁被使用，这时我们可以给它起个别名，又称给它建立个视图
- 视图可以修改，需要改sql语句即可。

```
create view 视图名 as sql语句	//创建视图
drop view 视图名称		//删除视图
```

## 触发器

- 触发器就是在我们执行某次操作时，它可以给我们在执行前，执行后分别再自动执行其余的操作。
- 触发器没有查询类型的，只有增，删，改。

```
delimiter //
create trigger t1 before insert on student for each row
begin
	//new代指新数据，old代指老数据，当删除的时候可以取出
	insert inot teacher(tname) values('王老师')/values(new.sname);	
end	//
delimiter ;
```

## 函数

mysql中有不少内置函数来辅助我们对数据进行处理。

- 内置函数
  - 例如，sum，DATE_FORMAT等
- 自定义函数

```
create function f1(
i1 int,
i2 int)
returns int
begin
	//函数体
	declare num int;
	set num = i1+i2;
	return (num)
end
delimiter
//调用
select f1(1,100);
```

## 存储过程

### 普通使用

- 将mysql上一堆sql语句定义一个别名，对它进行调用，这一般就叫存储过程
- 存储过程一般是没有返回值的，有一种变相的返回值用法就是用out，一般是用out来判断执行的过程是否成功。

```
//简单定义
create procedure p1()
begin
	select * from student
	insert into teacher(tname) values('ct')
end;
call p1()	//调用存储过程

//传参定义，参数定义有in，out，inout三种类型
create procedure p1(
in n1 int,
in n2 int
)
begin
	select * from student where sid>n1
end;
call p1(12,2)

//修改全局变量,out类型在存储过程拿不到传入的值，仅能对它进行修改。inout就是既有in功能又有out功能
create procedure p1(
in n1 int,
out n2 int
)
begin
	set n2 = 123123
	select * from student where sid>n1
end;
set @v1 = 0;	//创建了一个session级别的变量
call p1(12,@v1);
select @v1;
```

### 支持事务操作

- 就是处理正常的一系列sql操作时，如果出现异常要对它进行一定的处理

```
create procedure p1(
out status_code tinyint
)
begin
	declare exit handler for sqlexception	//声明sql异常
	begin
		set status_code = 1
		rollback	//出错回滚
	end
start transaction	//开始事务
	delete from tb1
	insert into tb2(name) values('seven')
commit
set status_code = 2
end;
set @v1 = 0;	//创建了一个session级别的变量
call p1(@v1);
select @v1;
```

### 存储过程与游标结合

- 游标一般就是指mysql里的循环操作

```
create procedure p2()
begin
	declare row_id int
	declare row_number int
	declare done int default false
	declare temp int
	declare my_cursor cursor for select id,num from tbA
	declare continue handler for not found set done =  true
	open my_cursor
		circulation:loop
			fetch my_cursor inot row_id,row_num
			if done then
				leave circulation
			end if
			set temp = row_id + row_number
			insert into B(number) values(temp)
		end loop circulaton
	close my_cursor
end;
```

## 慢日志查询

- 为了观察哪些sql查询不太合格的日志文件

```
基于内存
show varibles like '%query%'
set global 变量名 = 值

基于配置文件
mysqld --defaults-file='D:\my.conf'
my.conf内容：
	slow_query_log = on
	slow_query_log_file = D:/...
注意：修改配置文件之后，需要重启服务
```

# mysql分页性能相关方案

- 不让看，不是好的方案

- 索引表中扫：`select * from  userinfo where id in (select id from userinfo limit 20000,10)`  

- 理想方案，记录当前页最大或最小id，然后通过`where id>或者<`加上limit来取出数据。不能通过between and来获取，因为id不一定是连续的，即不要想着直接通过页码来取得id范围。 

  - 页面只有上一页，下一页

  ```
#maxid
  #minid
  下一页：
  	select * from userinfo where id > maxid limit 10
  上一页：
  	select * from userinfo where id < maxid order by id desc limit 10
  ```
  
  - 上一页，192 193 ... 197这种，假如直接从192跳到了197页
  
  ```
  select * from userinfo where id in (
  select id from (select id from userinfo where id>max limit 30) as N order by N.id
  desc limit 10
  )
  ```