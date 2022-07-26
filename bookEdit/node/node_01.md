### node的模块

#### 核心模块

node内置的包，如http

#### 自定义模块

 第三方包，是由很多自定义模块构成的，其本身也属于自定义模块

> 自定义模块一般放在node_moudles文件夹下，由package.json来管理

第三方包的目录结构一般如下：

* package.json	包描述文件
* lib    用于存放JavaScript代码的目录
* bin    用于存放可执行二进制文件的目录
* doc    用于存放文档的目录

#### 通过commonJS来导入包

export导出

require引入

#### 常用的一些包

##### nodemon

热更新模块， 无须ctrl + c来终止程序再运行。

#####	http



核心模块，直接可以导入使用，用来接收一些网络请求、设置请求头等。

#####	fs

进行一些文件操作	

__fs. stat__ 检测是文件还是目录

__fs.mkdir__ 创建目录

__fs.writeFile__ 创建、写入文件

**fs.appendFile** 追加文件内容

**fs.readFile** 读取文件

**fs.readdir** 读取目录

**fs.rename** 重命名、移动位置

**fs.rmdir** 删除目录

**fs.unlink**  删除文件

__fs.createReadStream__ 创建文件读取流

~~~javascript
// 以流的方式读取文件 createReadStream
const fs = require('fs')
var readStream = fs.createReadStream('./data/input.txt')
var str = ''
var count = 0
readStream.on('data', (data) => {
  str += data
})
readStream.on('end', () => console.log(str))
~~~

__fs.createWriteStream__ 创建文件写入流

~~~js
var data = '疯疯癫癫闯天涯'
var writeStream = fs.createWriteStream('./data/output.txt')
writeStream.write(data)
writeStream.end()
writeStream.on('finish', () => console.log('写入完成'))
~~~

__pipe__ 管道流

~~~JavaScript
// 管道流，用来复制一些大文件
var readStream1 = fs.createReadStream('./data/input.txt')
var writeStream1 = fs.createWriteStream('./bigout')
readStream1.pipe(writeStream1)
~~~






### npm的一些命令

安装包

```shell
npm install <name> --save
```

初始化（即添加package.json文件）

~~~shell
npm init --yes
~~~

查看已安装包列表

~~~shell
npm list
~~~

查看包的版本

~~~shell
npm info <name>
~~~

指定安装包的版本

~~~shell
npm install <name>@1.0.0 --save
~~~

> ​	--save其作用是将包的版本信息（依赖）加入到package.json文件里(dependencies)
>
> ​	方便不同环境下启动
>
> ​	--dev 意为开发时依赖，在打包时，不包含带有dev的包

-----

### package.json

~~~json
 "dependencies": {
    "core-js": "^3.6.5",
  },
  "devDependencies": {
    "@vue/cli-plugin-babel": "~4.5.0",
  },
~~~

> ^	表示第一位版本号不变，后边两位取最新的版本
>
> ~	表示前两位不变，最后一位取最新的版本
>
> " * "	表示全部取最新版本

