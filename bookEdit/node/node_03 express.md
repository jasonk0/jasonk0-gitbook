#### 中间件express- session

1、配置express-session

```javascript
app.use(session({
  secret: 'keyboard cat', // 服务端生成的session签名
  resave: false, // 强制重新保存session，即使它并没有变化
  saveUninitialized: true, // 强制将未初始化的session存储
  rolling: true, // 在未过期时，发送请求将重置session的过期时间
  cookie: { maxAge: 1000 * 60, secure: false },
}))
```

2、设置和获取session

设置了username为zhangsan，在回到首页后就可以读取到session

```javascript
app.get('/', (req, res) => {
  if (req.session.username) {
    res.send(req.session.username + '已登陆')
  } else {
    res.send('请先登录')
  }
})
app.get('/login', (req, res) => {
  req.session.username = 'zhangsan'
  res.send('denglu')
})
```

3、销毁session

三种方法，在进入此页面就执行销毁session，回到首页时将无法读取session

```javascript
app.get('/loginOut', (req, res) => {
  // 1、将cookie的过期时间设为0。 将销毁所有的session
  req.session.cookie.maxAge = 0 
  // 2、将session设为空
  req.session.username = ''
  // 3、 destroy方法，销毁session
  req.session.destroy()
  res.send('退出登录')
})
```

#### 分布式系统，多服务器共享session

问题描述：不同服务器之间共享session，只需要将session保存到数据库（mongoDB、redis、MySql）中即可

```mermaid
graph LR

nginx --> A[服务器A]
nginx --> B[服务器B]
nginx --> C[服务器C]
A --保存session1--> D[mongodb数据库]
B --共享session1--> D
C --共享session1--> D


```

安装connet-mongo

`npm install connect-mongo`

引入，注意需要在引入session语句的下边

`const MongoStore = require('connect-mongo')(session)`

配置

```javascript
app.use(session({
  ... //这里参考上边的session配置
  store: MongoStore.create({
    mongoUrl: 'mongodb://user12345:foobar@localhost/test-app?authSource=admin&w=1', // 此处配置数据库的地址，用户名及密码
    touchAfter: 24 * 3600 // 不管多少次请求，在24小时内只更新一次session，除非更改session
  }),
}))
```

