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

