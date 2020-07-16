# 基本介绍

## 关系型数据库和非关系型数据库

- 表就是关系，或者说表与表之间存在关系
  - 所有的关系型数据库都需要通过`sql`语言来操作
  - 所有的关系型数据库在操作之前都需要设计表结构
  - 而且数据表还支持约束，例如唯一的，主键，默认值，非空
- 非关系型数据库非常的灵活，有的非关系型数据库就是key-value对儿。
- 但是mongoDB是长的最像关系数据库的非关系型数据库
  - 数据库 -》数据库
  - 数据表 -》集合（数组）
  - 表记录 -》 文档对象
  - mongoDB不需要设计表结构，也就是说你可以任意的往里面存数据，没有结构性这么一说。
  - 在操作mongodb时也可以把他想象成像mysql那样，表就相当于它的集合，属性就相当于表的字段，只不过mongodb变更表更加的灵活。

## 启动和关闭数据库

- 启动

```
mongodb 默认使用执行 mongod 命令所在磁盘的根目录下寻找/data/db 目录作为自己的数据存储目录
所以在第一次执行该命令钱要先自己手动新建一个 /data/db目录
mongod
```

- 如果想要修改默认的数据存储目录，可以

`mongod --dbpath = 数据存储路径`，不推荐改，很麻烦，它只是我们解决问题的工具。

- 停止：

```
在开启服务的控制台直接 ctrl+c
或者直接关闭控制台即可。
```

##  连接数据库

连接：

```
# 该命令默认连接本机的mongodb 服务
mongo
```

退出：

```
在连接状态输入 exit 退出连接
exit
```

## 基本命令

- `show dbs`
  - 查看显示所有数据库
- `db`
  - 查看当前操作的数据库
  - 默认是在test数据库，但是通过show dbs却又看不见，原因是因为Mongodb只会在你写入数据的时候自动新建数据库。
- `use 数据库名称`
  - 切换到指定的数据库，如果没有会新建，当然前提也是你在当前数据库名下写入了数据。

- 插入数据
  - 例，`db.students.insertOne({"name": "jack"})

- `show collections`
  - 查看当前数据库下的所有集合。

- `db.students.find()`，可以查看当前students的集合

# 在node中如何操作mongoDB数据

- 使用第三方包mongoose来操作mongodb数据库`npm install mongoose --save`

```javascript
//一个meow的简单开始

const mongoose = require('mongoose');
//连接本地的test数据库
mongoose.connect('mongodb://localhost:27017/test', {useNewUrlParser: true, useUnifiedTopology: true});
//实例化一个cat模型，类似于mysql的表并设置name字段，类似为string
const Cat = mongoose.model('Cat', { name: String });
//持久化保持kitty实例
const kitty = new Cat({ name: 'Zildjian' });
kitty.save().then(() => console.log('meow'));
```

# mongoose开始

## 设计Schema发布model

```javascript
//	连接数据库
//	该数据库不用必须存在，只要你往其中插入数据就会自动将其创建
mongoose.connect('mongodb://localhost/itcast',{useNewUrlParser: true, useUnifiedTopology: true})
//	设计集合结构（表结构），字段名称就是表结构中的属性名称
//	约束的目的是为了保证数据的完整性，不要有脏数据
var Schema = mongoose.Schema
var UserSchema = new Schema({
    username: {
        type: String,
        required: true //表示必须有的意思
    },
    password: {
        type: String,
        required: true
    },
    email: {
        type: String
    }
})
//	将文档结构发布为模型
//	mongoose.model方法就是将一个架构发布为model，第一个参数为集合名称会被自动转换成小写。第二个参数架构Schema，返回值为模型构造函数。
var User = mongoose.model('User',UserSchema)
// 当拿到模型构造函数，就可以对集合中的数据进行操作了
```

## 增删改查

- 新增数据

```javascript
// 参数对象的结构跟Schema中的结构要保持一致
var admin = new User({
	username: 'admin',
	password: '1223234',
	email: 'sdfj@163.com'
})
admin.save(function  (err,data) {
	if(err) {console.log('保存失败')}
	else{
	console.log('保存成功')
	console.log(data)	//data就是我们刚刚存储进去的一条信息，mongodb还会自动帮我们生成_id
	}
})
```

- 查询

```javascript
//	查询所有
User.find(function (err,ret) {
	if(err) {console.log('查询失败')}
    else{
        console.log(ret)	//结果为一个数组，数组中保存着数据
    }
})
//	按条件查，第一个参数为条件，可以传多个条件进行组合查询，返回值依旧是一个数组
User.find({
    username: 'admin'
},function (err,ret) {
    if(err) {console.log('查询失败')}
    else {console.log(ret)}
})
// 按照条件查询单个，返回值直接就是对象数据。如果不传条件返回值就是第一个数据
User.findOne({
    username: 'admin'
},function (err,ret) {
    if(err) {console.log('查询失败')}
    else {console.log(ret)}
})
```

- 删除

```javascript
// 只要符合条件，有多少个删多少个
User.remove({
	username: 'zs'
},function(err,ret) {
	if(err) {console.log('删除失败')}
	else{
		console.log(ret)
	}
})
//	根据条件删除一个
Model.findOneAndRemove(conditions,[options],[callback])
//	根据id删除一个
Model.findByIdAndRemove(id,[options],[callback])
```

- 更新数据

```javascript
//	根据id更新一个，第一个参数为id值，第二个为改动的内容，第三个为回调函数
User.findByIdAndUpdate('id值',{
	password:'123'
},function (err,ret) {
	if(err) {console.log('更新失败')}
	else{
	console.log('更新成功')
	}
})
//	根据条件更新所有
Model.update(conditions,doc,[options],[callback])
//	根据指定条件更新一个
Model.findOneAndUpdate([conditions],[update],[options],[callback])
//	还有更多的编辑方法请自行查阅官方文档
```

## 补充

- mongoose方法支持promise调用

