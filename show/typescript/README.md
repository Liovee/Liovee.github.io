# TYPESCRIPT

### 安装

```
    npm install typescript -g
```

### 编译

```
    tsc --init
    tsc -w  //编译
```

### 基本数据类型

原始数据类型包括：布尔值、数值、字符串、null、undefined 以及 ES6 中的新类型 Symbol 和 ES10 中的新类型 BigInt
注意:NaN、Infinity、二/十/八/十六进制 属于 number 类型
```
let bool:boolean = new Bealean(1) //会报错 因为他会返回一个Boolean()对象,需写成如下形式
let bool:Boolean = new Bealean()
或
let bool1:boolean = Bealean(1)
let n:null = null
let u:undefined = undefined
```