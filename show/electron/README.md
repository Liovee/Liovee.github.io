暂时记录一下初学electron遇到的问题和解决方法
electron使用
https://xiaoman.blog.csdn.net/article/details/126063804
问题
1.打包报错
package.js中的build下面的file修改成
    "files": [
      "dist",
      "dist-electron"
    ],
2.应用安装之后白屏
index.ts 修改
win.loadFile(path.join(__dirname, "../dist/index.html"));
3.关于package.main中的main标签 设置 打包完后一直打包到dist-electron/index.js
"main": "dist-electron/index.js", main做响应修改即可