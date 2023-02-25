# EXPRESS

### 导入 express

```
const express = require('express')
```

### 创建 web 服务器

```
const app = express()
```

### 监听客户端的请求，并响应具体内容

```
app.get(客户端请求地址('/user'), (req, res) => {
    // 向客户端响应对象
    res.send()
})
app.post(客户端请求地址('/user'), function () {
})

```

### 启动 web 服务器

```
app.listen(80, () => {
    //req.query 获取客户端通过查询字符串的形式，发送到服务器的参数
    //req.params url中:匹配到的动态参数
    //返回内容
})
```

### 托管静态资源 express.static

创建静态资源服务器，express 在指定的静态目录中查找文件，并对外提供资源的访问路径，因此，存放静态资源的目录名不会出现在 URL 中

```
app.use(express.static('public'))  // 指定目录,可以访问public地下所有的文件
```

### express 路由

三部分 请求的类型、请求的 URL 地址、处理函数

```
app.Method(GET、POST)(PATH,HANDLER)
#example
app.get(post)('/', function(req,res) {
    res.send("hello world")
})
```

### 中间件

#### 分类

- 应用级别中间件(使用 app.use,app.get(post)绑定到 app 上面的中间件)
- 路由级别中间件(绑定到 express.Router()实例上的中间件,使用方式同上)
- 错误级别中间件(捕获项目中发生的错误，四个形参(err,req,res,next),放在路由之后)
- Express 内置中间件(express.static 快速托管静态资源、express.json 解析 json 请求体的数据、express.urlencoded,解析 url-encoded 格式的请求体数据(application/x-www-form-urlencoded))
- 第三方中间件(在路由之前配置 cors 中间件解决跨域问题 app.use(cors()))
  把流转关系交给下一个页面或者路由

```
const mw = function(req,res,next){
    next()
}
// 注册全局中间件
app.use(mw)
// 注册局部生效中间件
app.get('/', mw, function(req,res) {
    res.send("hello world")
})
```

### cors 跨域

1.setHeader('Access-Control-Allow-origin','http://baidu.com(*)')
表示只支持来自http://baidu.com的请求

2.cors 仅支持客户端向服务器发送如下 9 个请求头
Accept, Accept-Language, Content-Language, DPR, Downlink, Save-Data, View-Width, Width, Content-Type
其中 Content-Type`仅`限于`text/plain, multipart/form-data, application/x-www-form-urlencoded` 三者之一。
如果需要加入`额外请求头信息`，须在`服务器端`，通过 Access-Control-Allow-Headers`对额外请求头进行声明`，否则请求会失败。
例如：如果要加一个请求头为 Content-Type: X-Custom-Header 则需要在服务器端声明
res.setHeader('Access-Control-Allow-Headers','Content-Type, X-Custom-Header')

3.默认情况下，CORS`仅`支持客户端发起`GET, POST, HEAD`请求。
如果客户端希望通过`PUT, DELETE`等方法请求服务器资源，则需要在服务器端，通过 Access-Control-Allow-Methods 来`指明实际请求所允许使用的HTTP方法`

### 连接 mysql

```
const mysql = require('mysql')
const db = mysql.createPool({
    host,user,password,database
})
db.query() 指定执行的语句
```
