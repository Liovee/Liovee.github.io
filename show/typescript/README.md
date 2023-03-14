# TYPESCRIPT

### 学习地址

https://ts.xcatliu.com/advanced/generics.html

### 安装

```
npm install typescript -g
```

### 编译

```
tsc --init
tsc -w  //编译
```

```
npm i ts-node -g
npm i @types/node -D //node.js中调试ts的包
在终端直接输入ts-node xx.ts直接调试
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

### any/unknow 类型

都是顶级类型,其次是 Object,再其次是 Number,String，Boolean,最后是 string,number,boolean,最后是 never，即上层类型可以包含下层类型,unknow 特殊，不能赋值给其他类型,除了自身和 any，他没有办法读任何属性，方法也不能调用

#### object Object {}

Object 表示包含所有类型
let a :Object = 1 / "1"
object 表示非原始类型，只支持引用类型
let a:object = []/{}/()=>
{} 即 new Object 同 Object,但是赋值之后无法增加属性

### interface

```
interface Axx{
    name:String,
    [anyName]:string:any, // 定义一个任意key，除必填属性其他属性可以不校验
    email?:string,  // 定义一个可选类型，可传可不传
    readonly age:string // 定义一个只读属性，不可修改
} //第一个字母大写
interface Bxx extengs Axx{ //继承，拥有继承接口的所有属性
    id:string
}
let a : Axx = {
    name:'Liovee'
} //不能多属性 也不能少属性 如果重名的接口 会重合，就是重载
interface Fn = {
    (name:string):number []
}
const fn:Fn = function(){ //function中的参数也已经约束好了 为string
    return [1]
}
```

### 数组

两种方式定义

```
let arr:number[] = [1,2,3]
let arr:Array<boolean> = [true,false]
```

定义对象数组

```
interface Liovee {
name : string
}
let arr:X[] = [{name:"Liovee"}]
```

二维数组

```
let arr:number[][] = [[1],[2]]
```

### 类型断言

```
interface A {
    run:string
}
interface B {
    build:number
}
let fn = (type A | B)=>{
    console.log((type as A).run)
    console.log(<A>type.run)
}
类型断言只能欺骗编译阶段，运行阶段会保错
```

### 内置对象

```
const regexp:RegExp = '\w\d\s' //正则声明

const data:Date = new Date() //日期类型

const err:Error = new Error("错误")
```

#### DOM 和 BOM

```
<div>
    <ul id="Liovee">
        <li>1</li>
        <li>2</li>
        <li>3</li>
    </ul>
</div>
const list:NodeList = document.querySelectorAll("#Liovee li")
const body:HTMLElement = document.body

function promise(): Promise<number> {
    return new Promise<number>((resolve, reject) => {
        resolve(1)
    })
}
//dom元素的映射表
interface HTMLElementTagNameMap {
    "a": HTMLAnchorElement;
    "abbr": HTMLElement;
    "address": HTMLElement;
    "applet": HTMLAppletElement;
    "area": HTMLAreaElement;
    "article": HTMLElement;
    "aside": HTMLElement;
    "audio": HTMLAudioElement;
    "b": HTMLElement;
    "base": HTMLBaseElement;
    "bdi": HTMLElement;
    "bdo": HTMLElement;
    "blockquote": HTMLQuoteElement;
    "body": HTMLBodyElement;
    "br": HTMLBRElement;
    "button": HTMLButtonElement;
    "canvas": HTMLCanvasElement;
    "caption": HTMLTableCaptionElement;
    "cite": HTMLElement;
    "code": HTMLElement;
    "col": HTMLTableColElement;
    "colgroup": HTMLTableColElement;
    "data": HTMLDataElement;
    "datalist": HTMLDataListElement;
    "dd": HTMLElement;
    "del": HTMLModElement;
    "details": HTMLDetailsElement;
    "dfn": HTMLElement;
    "dialog": HTMLDialogElement;
    "dir": HTMLDirectoryElement;
    "div": HTMLDivElement;
    "dl": HTMLDListElement;
    "dt": HTMLElement;
    "em": HTMLElement;
    "embed": HTMLEmbedElement;
    "fieldset": HTMLFieldSetElement;
    "figcaption": HTMLElement;
    "figure": HTMLElement;
    "font": HTMLFontElement;
    "footer": HTMLElement;
    "form": HTMLFormElement;
    "frame": HTMLFrameElement;
    "frameset": HTMLFrameSetElement;
    "h1": HTMLHeadingElement;
    "h2": HTMLHeadingElement;
    "h3": HTMLHeadingElement;
    "h4": HTMLHeadingElement;
    "h5": HTMLHeadingElement;
    "h6": HTMLHeadingElement;
    "head": HTMLHeadElement;
    "header": HTMLElement;
    "hgroup": HTMLElement;
    "hr": HTMLHRElement;
    "html": HTMLHtmlElement;
    "i": HTMLElement;
    "iframe": HTMLIFrameElement;
    "img": HTMLImageElement;
    "input": HTMLInputElement;
    "ins": HTMLModElement;
    "kbd": HTMLElement;
    "label": HTMLLabelElement;
    "legend": HTMLLegendElement;
    "li": HTMLLIElement;
    "link": HTMLLinkElement;
    "main": HTMLElement;
    "map": HTMLMapElement;
    "mark": HTMLElement;
    "marquee": HTMLMarqueeElement;
    "menu": HTMLMenuElement;
    "meta": HTMLMetaElement;
    "meter": HTMLMeterElement;
    "nav": HTMLElement;
    "noscript": HTMLElement;
    "object": HTMLObjectElement;
    "ol": HTMLOListElement;
    "optgroup": HTMLOptGroupElement;
    "option": HTMLOptionElement;
    "output": HTMLOutputElement;
    "p": HTMLParagraphElement;
    "param": HTMLParamElement;
    "picture": HTMLPictureElement;
    "pre": HTMLPreElement;
    "progress": HTMLProgressElement;
    "q": HTMLQuoteElement;
    "rp": HTMLElement;
    "rt": HTMLElement;
    "ruby": HTMLElement;
    "s": HTMLElement;
    "samp": HTMLElement;
    "script": HTMLScriptElement;
    "section": HTMLElement;
    "select": HTMLSelectElement;
    "slot": HTMLSlotElement;
    "small": HTMLElement;
    "source": HTMLSourceElement;
    "span": HTMLSpanElement;
    "strong": HTMLElement;
    "style": HTMLStyleElement;
    "sub": HTMLElement;
    "summary": HTMLElement;
    "sup": HTMLElement;
    "table": HTMLTableElement;
    "tbody": HTMLTableSectionElement;
    "td": HTMLTableDataCellElement;
    "template": HTMLTemplateElement;
    "textarea": HTMLTextAreaElement;
    "tfoot": HTMLTableSectionElement;
    "th": HTMLTableHeaderCellElement;
    "thead": HTMLTableSectionElement;
    "time": HTMLTimeElement;
    "title": HTMLTitleElement;
    "tr": HTMLTableRowElement;
    "track": HTMLTrackElement;
    "u": HTMLElement;
    "ul": HTMLUListElement;
    "var": HTMLElement;
    "video": HTMLVideoElement;
    "wbr": HTMLElement;
```

### 类

- public 修饰的属性或方法是公有的，可以在任何地方被访问到，默认所有的属性和方法都是 public 的
- private 修饰的属性或方法是私有的，不能在声明它的类的外部访问
- protected 修饰的属性或方法是受保护的，它和 private 类似，区别是它在子类中也是允许被访问的

```
class Person {
    protected name: string
    age: number
    sub: boolean
    constructor(name: string, age: number, sub: boolean) {
        this.name = name,
            this.age = age,
            this.sub = sub
    }
}
let a = new Person("123", 10, true)
console.log(a.name); //保错 实例化的对象访问不到 只有子类继承了Person类才可以访问
```

static 直接通过类名去访问

```
class Person {
    static name: string = '123'
    constructor(name: string) {
        this.name = name,
    }
}
Person.name
```

同时，类中的静态方法是访问不到 static 修饰之外的属性的，同理 constructor 也是访问不到,即静态只能调用静态 static 修饰的属性的原因是因为静态方法是先加载的，所以无法调用类属性，类方法是对象调用

```
class Person {
    age:20
    static name: string = '123'
    constructor(name: string,age:number) {
        this.name = name,
    }
    static run(){
        this.age //保错
    }
}
```

类实现接口

```
interface Person{
    eat(type:boolean):boolean
}
class A {
    params:string
    constructor(params:string){
        this.params = params
    }
}
class man extends A implements Person {
    eat(type:boolean):boolean{
        return type
    }
}
```

抽象类(abstract)

```
abstract class A{ //抽象类是不能实例化的 就是不能new
    name:string
    constructor(name:string){
        this.name = name
    }
    abstract getName():string // 抽象方法 这里是不能写具体的实现的，需要也必须再去写一个子类去实现
}
class B extends A{
    constructor(){
        super("Liovee")
    }
    getName():sting{
        return this.name
    }
}
```

### 迭代器

```
let set: Set<number> = new Set([1, 1, 2, 2, 3, 4, 5]) //天然去重
let map: Map<any, any> = new Map()
let arr = [1, 2, 3]
map.set(arr, 'Liovee')
console.log(map.get(arr)); //与对象的区别就是 可以将引用类型当作key
```

一些伪数组例如 document.querySelectAll('div') 或者方法的 arguments 可以利用迭代器实现遍历，因为他们不是数组，所以没有数组的方法

```
const each = (value:any)=>{
    let params:any = value[Symbol.iterator]()
    let next:any =  = {done :false}
    while(!next.done){
        next = params.next()
        if(!next.done)
         console.log(next.value)
    }
}
```

它可以写成 for ... of 的方法，即语法糖，但是他不能用于对象，因为对象没有 ymbol.iterator，如果非要用于对象，可以给对象手动加上 Symbol.iterator

```
 let obj = {
    max:5,
    current:0,
    [Symbol.iterator](){
        return {
            max:this.max,
            current:this.current,
            next(){
                if(this.current == this.max){
                    return {
                        value:undefined,
                        done:true
                    }
                }else{
                    return {
                        value:this.current++,
                        done:false
                    }
                }
            }
        }
    }
 }
```

### 泛型

个人理解泛型就是类型的变量 即类型的占位符

泛型约束
<T extends number> //用法 限制泛型为 number 类型

```
function fn<T extends Len>(a:T){
    return a.length //保错 可以定义一个接口
}
interface Len{
    length:number
}
```

keyof

```
let obj = {
    name:'Liovee',
    age:16
}
type Key =keyof typeof obj //此时这个类型key相当于 "name"|"age"
function cb<T extends obj,K>(obj:T,key:K extends keyof T){
    return obj[key]
}
```

keyof 其他用法

```
interface Data {
    name:string,
    age:number,
    sex:string
}
type Option <T extends object>{
    [Key in keyof T]?:T[Key]
}
```

### tsconfig.json

```
"compilerOptions": {
  "incremental": true, // TS编译器在第一次编译之后会生成一个存储编译信息的文件，第二次编译会在第一次的基础上进行增量编译，可以提高编译的速度
  "tsBuildInfoFile": "./buildFile", // 增量编译文件的存储位置
  "diagnostics": true, // 打印诊断信息
  "target": "ES5", // 目标语言的版本
  "module": "CommonJS", // 生成代码的模板标准
  "outFile": "./app.js", // 将多个相互依赖的文件生成一个文件，可以用在AMD模块中，即开启时应设置"module": "AMD",
  "lib": ["DOM", "ES2015", "ScriptHost", "ES2019.Array"], // TS需要引用的库，即声明文件，es5 默认引用dom、es5、scripthost,如需要使用es的高级版本特性，通常都需要配置，如es8的数组新特性需要引入"ES2019.Array",
  "allowJS": true, // 允许编译器编译JS，JSX文件
  "checkJs": true, // 允许在JS文件中报错，通常与allowJS一起使用
  "outDir": "./dist", // 指定输出目录
  "rootDir": "./", // 指定输出文件目录(用于输出)，用于控制输出目录结构
  "declaration": true, // 生成声明文件，开启后会自动生成声明文件
  "declarationDir": "./file", // 指定生成声明文件存放目录
  "emitDeclarationOnly": true, // 只生成声明文件，而不会生成js文件
  "sourceMap": true, // 生成目标文件的sourceMap文件
  "inlineSourceMap": true, // 生成目标文件的inline SourceMap，inline SourceMap会包含在生成的js文件中
  "declarationMap": true, // 为声明文件生成sourceMap
  "typeRoots": [], // 声明文件目录，默认时node_modules/@types
  "types": [], // 加载的声明文件包
  "removeComments":true, // 删除注释
  "noEmit": true, // 不输出文件,即编译后不会生成任何js文件
  "noEmitOnError": true, // 发送错误时不输出任何文件
  "noEmitHelpers": true, // 不生成helper函数，减小体积，需要额外安装，常配合importHelpers一起使用
  "importHelpers": true, // 通过tslib引入helper函数，文件必须是模块
  "downlevelIteration": true, // 降级遍历器实现，如果目标源是es3/5，那么遍历器会有降级的实现
  "strict": true, // 开启所有严格的类型检查
  "alwaysStrict": true, // 在代码中注入'use strict'
  "noImplicitAny": true, // 不允许隐式的any类型
  "strictNullChecks": true, // 不允许把null、undefined赋值给其他类型的变量
  "strictFunctionTypes": true, // 不允许函数参数双向协变
  "strictPropertyInitialization": true, // 类的实例属性必须初始化
  "strictBindCallApply": true, // 严格的bind/call/apply检查
  "noImplicitThis": true, // 不允许this有隐式的any类型
  "noUnusedLocals": true, // 检查只声明、未使用的局部变量(只提示不报错)
  "noUnusedParameters": true, // 检查未使用的函数参数(只提示不报错)
  "noFallthroughCasesInSwitch": true, // 防止switch语句贯穿(即如果没有break语句后面不会执行)
  "noImplicitReturns": true, //每个分支都会有返回值
  "esModuleInterop": true, // 允许export=导出，由import from 导入
  "allowUmdGlobalAccess": true, // 允许在模块中全局变量的方式访问umd模块
  "moduleResolution": "node", // 模块解析策略，ts默认用node的解析策略，即相对的方式导入
  "baseUrl": "./", // 解析非相对模块的基地址，默认是当前目录
  "paths": { // 路径映射，相对于baseUrl
    // 如使用jq时不想使用默认版本，而需要手动指定版本，可进行如下配置
    "jquery": ["node_modules/jquery/dist/jquery.min.js"]
  },
  "rootDirs": ["src","out"], // 将多个目录放在一个虚拟目录下，用于运行时，即编译后引入文件的位置可能发生变化，这也设置可以虚拟src和out在同一个目录下，不用再去改变路径也不会报错
  "listEmittedFiles": true, // 打印输出文件
  "listFiles": true// 打印编译的文件(包括引用的声明文件)
}

// 指定一个匹配列表（属于自动指定该路径下的所有ts相关文件）
"include": [
   "src/**/*"
],
// 指定一个排除列表（include的反向操作）
 "exclude": [
   "demo.ts"
],
// 指定哪些文件使用该配置（属于手动一个个指定文件）
 "files": [
   "demo.ts"

```

- 1.include
  指定编译文件默认是编译当前目录下所有的 ts 文件

```
"include":["./first.ts"], //表示只编译first.ts
```

- 2.exclude
  指定排除的文件

```
"exclude":["./first.ts"], //表示除了编译first.ts,其他都编译
```

- 3.target
  指定编译 js 的版本例如 es5 es6

```
"target": "es2016", //编译为es2016的版本
```

- 4.allowJS
  是否允许编译 js 文件

```
"allowJs": true,
```

- 5.removeComments
  是否在编译过程中删除文件中的注释

- 6.rootDir
  编译文件的目录

- 7.outDir
  输出的目录

- 8.sourceMap
  代码源文件
  编译之后的代码会被打包成一行，sourcemap 会帮助记录代码所在行数
- 9.strict
  严格模式

- 10.module
  默认 common.js 可选 es6 模式 amd umd 等

### 声明文件(d.ts)

声明文件的作用是在你引入某些库的时候，他因为有声明文件会导致保错和没有代码提示，这时我们的解决方法是执行 npm i --save-dev @types/(库的名字)或者手写声明文件
例如

```
import express from 'express'
const app = express()
const router = express.Router()
app.use('./api', router)
router.get('/api', (req:any, res:any) => {
    res.json({
        code: 200
    })
})
app.listen(80, () => {
    console.log(111);
})
```

express.d.ts

```
declare module 'express' {
    interface Router {
        get(path:string,cb:(req:any,res:any)=>void):void
    }
    interface Express {
        (): App,
        Router():Router
    }
    interface App {
        use(path: string, router: any): void
        listen(port: number, cb?: () => void)
    }
    const express: Express
    export default express
}
```

### decorator 装饰器

首先在 tsconfig.js 中将下面两项注释打开

```
"experimentalDecorators": true,
"emitDecoratorMetadata": true,
```

类装饰器

```
const Base:ClassDecorator = (target)=>{ //此时的target是Http的构造函数
    console.log(target); //[class Http]
    target.prototype.name = "Liovee" //可以在不用关注Http这个类的情况下往里面添加属性
}
@Base // 或者写成Base(Http)但是要写在类下面
class Http{
    //xxx
}
```

柯里化

```
const Base = (name:string) => { //此时的target是Http的构造函数
    const fn:ClassDecorator = (target) => {
        target.prototype.name = name
        target.prototype.fn = () => {
            console.log("你好");
        }
    }
    return fn
}
@Base("学习") // 或者写成Base(Http)但是要写在类下面
class Http {
    //xxx
}
const http = new Http() as any
http.fn()
```

方法装饰器

```
const Get = ()=>{
    const fn :MethodDecorate = (target,key:any,descriptor:propertyDescriptor)=>{ //此时的target是原型对象 key是方法的名字 descriptor.value就是getList函数
        axios.get(url).then(res=>{
            descriptor.value(res.data)
        })
    }
    return fn
}
@Get(//传入url参数)
getList(data:any){
    console.log(data.result.list)
}
```

参数装饰器

```
npm i reflect-metadata  //需先安装这个库

const Get = ()=>{
    const fn :MethodDecorate = (target,_:any,descriptor)=>{
        const key = Reflect.getMetadata("key",target)
        axios.get(url).then(res=>{
            descriptor.value(key?res.data[key]:res.data)
        })
    }
    return fn
}
const Result = ()=>{
    const fn:ParameterDecorator = (target,key,index) =>{ //index就是下面@Result所在参数的位置
        reflect.defineMetadata("key","result",target)
    }
    return fn
}
@Get(//传入url参数)
getList(@Result() data:any){    //此时我们可以定义一个参数装饰器帮我们完成data.result的获取操作 他的执行顺序在@Get之前
    console.log( data )
}
```

### rollup 构建 ts 项目

```
import path from 'path'
import ts from 'rollup-plugin-typescript2' //插件 用来帮助rollup解析ts
import serve from 'rollup-plugin-serve'
import livereload from 'rollup-plugin-livereload'
import {
    terser
} from 'rollup-plugin-terser'
import replace from 'rollup-plugin-replace'
const isDev = () => {
    return process.env.NODE_ENV === 'development'
}
export default {
    input: './src/index.ts',
    output: {
        file: path.resolve(__dirname, './lib/index.js'),
        format: 'umd',
        sourcemap: true
    },
    plugins: [
        ts(),
        isDev() && livereload(),
        terser({
            drop_console:true //删除代码中的console.log
        }),
        replace({
            'process.env.NODE_ENV': JSON.stringify(process.env.NODE_ENV)
        }),
        isDev() && serve({
            open: true,
            port: 1988,
            openPage: "/public/index.html"
        })
    ]
}
```

- rollup-plugin-typescript2 插件 用来帮助 rollup 解析 ts
- package.json 的 build 中-c 就是让 rollup 解析 rollup.config.js
- package.json 的 build 中-w 就是--watch 即代码一改变就能接收到
- rollup-plugin-serve 启动前端服务到

```
serve({
            open: true,
            port: 1988,
            openPage: "/public/index.html"
        })
```

- rollup-plugin-terser 压缩打包之后的代码
- cross-env 区分生产模式还是开发者模式 结果会放到 process.env.NODE_ENV 中 但是浏览器是没有这个东西的 得在 node 环境里查询
- rollup-plugin-replace 在浏览器中也能查询什么模式

```
replace({
            'process.env.NODE_ENV':JSON.stringify(process.env.NODE_ENV)
        }),
```

### webpack 构建 ts 项目

```
const path = require("path")
const htmlwebpackplugin = require('html-webpack-plugin')
module.exports = {
    entry: "./src/index.ts",
    mode:'development',
    output: {
        path: path.resolve(__dirname, './dist'),
        filename: "index.js"
    },
    module: {
        rules: [{
            test: /\.ts$/,
            use: "ts-loader"
        }]
    },
    devServer: {
        port: 1877,
        proxy: {
        }
    },
    resolve: {
        extensions: ['.js', '.ts']
    },
    plugins: [
        new htmlwebpackplugin({
            template: './public/index.html'
        })
    ]
}
```

### ts 实现发布订阅模式

```
interface EventFace {
    on: (name: string, callback: Function) => void,
    emit: (name: string, ...args: Array<any>) => void,
    off: (name: string, fn: Function) => void,
    once: (name: string, fn: Function) => void
}

interface List {
    [key: string]: Array<Function>,
}
class Dispatch implements EventFace {
    list: List
    constructor() {
        this.list = {}
    }
    on(name: string, callback: Function) {
        const callbackList: Array<Function> = this.list[name] || [];
        callbackList.push(callback)
        this.list[name] = callbackList
    }
    emit(name: string, ...args: Array<any>) {
        let evnetName = this.list[name]
        if (evnetName) {
            evnetName.forEach(fn => {
                fn.apply(this, args)
            })
        } else {
            console.error('该事件未监听');
        }
    }
    off(name: string, fn: Function) {
        let evnetName = this.list[name]
        if (evnetName && fn) {
            let index = evnetName.findIndex(fns => fns === fn)
            evnetName.splice(index, 1)
        } else {
            console.error('该事件未监听');
        }
    }
    once(name: string, fn: Function) {
        let decor = (...args: Array<any>) => {
            fn.apply(this, args)
            this.off(name, decor)
        }
        this.on(name, decor)
    }
}
const o = new Dispatch()


o.on('abc', (...arg: Array<any>) => {
    console.log(arg, 1);
})

o.once('abc', (...arg: Array<any>) => {
    console.log(arg, 'once');
})
// let fn = (...arg: Array<any>) => {
//     console.log(arg, 2);
// }
// o.on('abc', fn)
// o.on('ddd', (aaaa: string) => {
//     console.log(aaaa);
// })
//o.off('abc', fn)

o.emit('abc', 1, true, 'Liovee')

o.emit('abc', 2, true, 'Liovee')

// o.emit('ddd', 'addddddddd')
```

### ts 的 proxy，reflect

```
const list: Set<Function> = new Set()

const autorun = (cb: Function) => {
    if (cb) {
        list.add(cb)
    }
}

const observable = <T extends object>(params: T) => {
    return new Proxy(params, {
        set(target, key, value, receiver) {
            const result = Reflect.set(target, key, value, receiver)
            list.forEach(fn => fn())
            console.log(list)
            return result
        }
    })
}

const person = observable({ name: "Liovee", attr: "你好" })

autorun(()=>{
    console.log('我变化了')
})

person.attr = '你不好'

```
