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
把流转关系交给下一个页面或者路由

```
const mw = function(req,res,next){
    next()
}
// 注册全局中间件
app.use(mw)
```
