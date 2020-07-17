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

# mysql常用名词定义

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

## 唯一索引

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

增，删，改一般就这几种，不过查比较多样

- 增

  - 插入单条记录，`insert into 表名(name,age)  values('alex',12)`
  - 插入多条记录，`insert into 表名(name,age)  values('alex',12),('hehe',15)`
  - 从a表中往b表中插入记录，`insert into b(name,age) select name,age from a`

- 删

  - 删除id小于6的行记录，`delete from t1 where id<6`

- 改

  - 将t1表中所有行的age列都变为18，`update t1 set age=18``
  - 同上，不过加上了age>18的限制条件，`update t1 set age=18 where age=17`

- 查

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
    ```

    

