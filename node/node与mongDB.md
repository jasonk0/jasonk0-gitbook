## node与mongoDB

1. 首先安装node上mongoDB的模块

`npm i mongodb --save`

2. 引入模块

`const {MongoClient} = require('mongodb')`

3. 定义数据库的连接地址

`const url = 'mongodb://127.0.0.1:27017'`

4. 定义要操作的数据库

`const dbName = 'foo_name'`

5. 实例化mongoclient

`const client = new MongoClient(url)`

6. 连接数据库

`client.connect((err)=>{})`