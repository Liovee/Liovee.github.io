# NODE

### node.js
#### fs 模块(文件系统)
```
const fs = require('fs')
fs.readFile(路径,编码格式,回调函数）
fs.writeFile(路径,内容,回调函数）
```
#### path 模块(文件系统)
``` 
const path = require('path')
path.basename():返回文件名
例如 path.basename('C:\\temp\\myfile.html'); // 返回: 'myfile.html'
```
```
path.basename('/foo/bar/baz/asdf/quux.html', '.html'); // 返回: 'quux'

path.dirname():返回 path 的目录名
path.extname():方法返回 path 的扩展名
路径的拼接使用 path.join 方法

path.join('/foo', 'bar', 'baz/asdf', 'quux', '..'); // 返回:
'/foo/bar/baz/asdf' 
```