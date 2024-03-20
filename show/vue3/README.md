# VUE3
![avatar](http://baidu.com/pic/doge.png)
官网 https://cn.vuejs.org/

### 构建方式

1.vite 构建

```
npm init vite@latest 之后按需选择
```

2.vue 脚手架构建

```
npm init vue@latest
vue create <project>
```

3.父子组件传参

- 父传子

```
父
let a: number = 1
<Menu :title="a"></Menu>
```

```
子
js写法
defineProps({
     title: {
         type: Number
     }
 })
如果在业务里面使用这个title，就需要let props = defineProps({}),再通过props.title使用
ts独有写法 withDefaults 包括默认值(泛型字面量模式)
withDefaults(defineProps<{
    title: Number,
    arr: String[]
}>(), { arr: () => ['Liovee'] })
```

- 子传父

```
子
js方式
<button @click="send">给父组件传值</button>
const emit = defineEmits(['on-click'])
const send = () => {
    emit('on-click', '子给父组件传值')
}
ts方式
const emit = defineEmits<{
    (e: "on-click", props: string): void
}>()
const send = () => {
    emit('on-click', '子给父组件传值')
}
```

```
父
@on-click="getName"
const getName = (name: string) => {
   console.log(name, 'namename');
}
```

defineExpose 子组件向父组件暴露方法或者属性

```
子
defineExpose({
    things: 'Liovee'
})
```

```
父
<Content @on-click="getName" ref="content"></Content>
const content = ref<InstanceType<typeof Content>>()
onMounted(() => {
  console.log(content.value?.things);
})
```

- eventBus
  Bus.ts

```
type busClass = {
  emit:(name:string)=>void,
  on:(name:string,callback:Function)=>void
}
type ParamsKey = string | number | symbol
type List = {
  [key:ParamsKey]:Array<Function>
}
class Bus implements busClass{
  list:List
  constructor(){
    this.list = {}
  }
  emit (name:string,...args:Array<any>){
    let eventName:Array<Function> = this.list[name]
    eventName.forEach(fn=>{
      fn.apply(this,args)
    })
  }
  on (name:string,callback:Function){
    let fn:Array<Function> = this.list[name] || []
    fn.push(callback)
    this.list[name] = fn
  }
}
```

使用方式就是 Bus.emit('fn',variable) Bus.on('fn',(value:boolean)=>{
//事件
})

- Mitt 1.安装 Mitt
  npm i Mitt -S 2.初始化
  import mitt form 'mitt'
  const Mit = mitt() 3.挂载
  app.config.globalProperties.$Bus = Mit
4.声明
declare module 'vue' {
  export interface ComponentCustomProperties{
    $Bus:typeOf Mit
  }
} 
5.使用
import {getCurrentInstance} from 'vue'
const instance = getCurrentInstance()
instance?.proxy?.$Bus.emit

### 递归组件

数据类型

```
interface Tree {
  name: string,
  checked: boolean,
  children?: [Tree]
}
```

```
Tree.vue
<Tree v-if="item?.children?.length" :data="item?.children"></Tree>
直接在组件中使用组件名的组件
注意在使用递归组件绑定事件的时候要加上.stop修饰符，防止冒泡
```

### 动态组件

多个组件同时使用一个挂载点，并且动态切换

```
<component :is="A"></component>
```

### fragment

vue2 只允许 template 子节点有一个 div 标签，而 vue3 可以使用多个

### npm run dev

当我们执行 npm run dev 的时候发生了什么?

- 1.首先会去 package.json 的 scripts 里面找到 dev 执行 vite 命令，可以去 node_modules 底下的 vite 文件的 package.json,发现他做了一个软连接,指向 bin/vite.js，发现他有三个脚本分别对应 mac，windows，linux 1.查找规则是先从当前项目的 node_modlue /bin 去找,

  2.找不到去全局的 node_module/bin 去找

  3.再找不到 去环境变量去找

### Ref

- isRef 判断是否为 ref 对象

```
const person = ref({ name: "Liovee" })
const show = () => {
  console.log(isRef(person)); //true
}
```

- 直接写在 dom 上获取 dom,同 vue2 类似

```
<div ref = "name">
const name = ref<HTMLDivElement>()
```

- shallowRef 相对于 ref 只能做浅层次的响应

```
const person2 = shallowRef({ name: "Liovee2" })
const show = () => {
  person2.value.name = "小李"
  console.log(person2); //控制台值改变但是视图没有变化
}
```

// 如果 shallowRef 跟着 ref 一起写，那么它会被 ref 影响，上面的写法会成功，视图也会更新
如果要视图发生变化需要这样赋值

```
person2.value = {
    name : "小李"
}
```

- triggerRef
  shallowRef 修改值视图不更新的问题也可以通过 triggerRef 解决

```
const person2 = shallowRef({ name: "Liovee2" })
const show = () => {
  person2.value.name = "小李"
  triggerRef(person2)
  console.log(person2); //控制台值改变但是视图没有变化
}
```

- triggerRef
  可以让我们自定义 ref

```
function myRef<T>(value: T) {
  return customRef((track, trigger) => {
    return {
      get() {
        track()
        return value
      },
      set(newValue) {
        value = newValue
        trigger()
      }
    }
  })
}// 使用方式同ref
```

### reactive

注意 reactive 定义的数组不要直接将数组赋值，防止破坏他的 proxy，可以使用 push ＋解构,或者写成对象里面是数组属性的形式

- shallowReactive 同 shallowRef 即到对象的第一个层次

### toRef

- 只能修改响应式对象的值 非响应式只能改变值但是改变不了视图
  // 使用场景为需要使用 proxy 对象中的一个属性

```
const man = reactive({ name: "Liovee", age: 24, like: 'game' })
const like = toRef(man, 'like')
```

toRefs 写法

```
const toRefs = <T extends object>(object: T) => {
  const map: any = {}
  for (let key in object) {
    map[key] = toRef(object, key)
  }
  return map
}
```

- toRaw 解除响应式

### watch

```
const stop = watchEffect((oninvalidate) => {
  console.log("message=====>", message.value);
  console.log("message2=====>", message2.value);
  oninvalidate(() => {
    console.log("before");
  })
})
const stopWatch = () => stop() //点击按钮之后就不会再执行监听的行为了
```

- flush 配置
  1.pre 组件更新前执行 2.sync 强制效果始终同步触发 3.post 组件更新后执行

### 插槽

- 匿名插槽
  子组件

```
<main class="main">
  <slot></slot>
</main>
```

父组件

```
<Dialog>
  <template v-slot>
    <div>匿名插槽</div>
  </template>
</Dialog>
```

- 具名插槽
  子

```
<main class="main">
  <slot name="footer"></slot>
</main>
```

父

```
<Dialog>
  <template v-slot:footer>
      <div>具名插槽</div>
  </template>
</Dialog>
```

- 作用域插槽
  子

```
<div v-for="item in data">
  <slot :data="item"></slot>
</div>
```

父

```
<Dialog>
  <template v-slot="{data}">
    <div>{{ data.name }}</div>
  </template>
</Dialog>
```

v-slot 简写为#

- 动态插槽
  子

```
<footer class="footer">
  <slot name="footer"></slot>
</footer>
```

父

```
let name = ref('footer')
<template #[name]>
  <div>213123</div>
</template>
```

### 异步组件

即请求加载时显示一个组件，加载完毕再显示其他组件 常用于骨架屏
简单封装 ajax 来请求数据

```
export const axios = {
    get<T>(url: string):Promise<T> {
        return new Promise((resolve) => {
            const xhr = new XMLHttpRequest()
            xhr.open('Get', url)
            xhr.onreadystatechange = () => {
                if (xhr.readyState == 4 && xhr.status == 200) {
                    setTimeout(() => {
                        resolve(JSON.parse(xhr.responseText))
                    }, 2000)
                }
            }
            xhr.send(null)
        })
    }
}
```

子组件请求数据并渲染

```
import { axios } from '../server/axios'
interface Data {
    data: {
        name: string,
        age: string,
        url: string,
        desc: string,
    }
}
const { data } = await axios.get<Data>('./data.json')
```

父组件使用 suspense 来做异步组件

```
<Suspense>
    <template #default>
        <SyncVue></SyncVue>
    </template>
    <template #fallback>
        <skeletonVue></skeletonVue>
    </template>
</Suspense>
const SyncVue = defineAsyncComponent(() => import('./components/sync.vue'))//异步组需要使用defineAsyncComponent引入
```

### 同时 defineAsyncComponent 引入的包会在 npm run build 的时候被打包进其他包里面，不会被打入主包，这样我们可以减少首屏加载时间，是一个比较好的性能优化方式

### teleport

- 将将模板代码去选染至指定的 DOM 节点
  使用方式: <Teleport to="里面是各种选择器(类 属性 id)" :disabled="true(true前面的to属性不会起作用)/false"></Teleport>

### keep-alive

使用<keep-alive></keep-alive>标签包裹组件可以保存组件中输入的信息
:include 属性是字符串或者数组 表示你想缓存的组件名称
:exclude 作用与上面相反
:max 指定缓存组件数量
新增 onActivated 和 onDeactivated 生命周期函数
注意 keeplive 开启之后不会走到 onUnmounted 周期

### transition 动画组件

- v-enter-from：定义进入过渡的开始状态。在元素被插入之前生效，在元素被插入之后的下一帧移除。
- v-enter-active：定义进入过渡生效时的状态。在整个进入过渡的阶段中应用，在元素被插入之前生效，在过渡/动画完成之后移除。这个类可以被用来定义进入过渡的过程时间，延迟和曲线函数。
- v-enter-to：定义进入过渡的结束状态。在元素被插入之后下一帧生效 (与此同时 v-enter-from 被移除)，在过渡/动画完成之后移除。
- v-leave-from：定义离开过渡的开始状态。在离开过渡被触发时立刻生效，下一帧被移除。
- v-leave-active：定义离开过渡生效时的状态。在整个离开过渡的阶段中应用，在离开过渡被触发时立刻生效，在过渡/动画完成之后移除。这个类可以被用来定义离开过渡的过程时间，延迟和曲线函数。
- v-leave-to：离开过渡的结束状态。在离开过渡被触发之后下一帧生效 (与此同时 v-leave-from 被移除)，在过渡/动画完成之后移除。

```
.fade-enter-from{ //fade为transition包裹组件的name属性名称
}
.fade-enter-active{
    transition: all 1.4s ease; //ease为默认的
}
.fade-enter-to{
}
```

通过 transition 组件中的 enter-from-class 之类可以自定义类名,:duration 定义动画持续多久

如何结合 animate.css 进行动画呢

```
首先安装animate的库
npm i animate.css
其次在组件中引入animate
首先安装animate的库
接着在transition组件中利用刚刚的enter-from-class直接将animate.css的类名沾上去
```

transition 生命周期

```
@before-enter="beforeEnter" //对应enter-from
  @enter="enter"//对应enter-active
  @after-enter="afterEnter"//对应enter-to
  @enter-cancelled="enterCancelled"//显示过度打断
  @before-leave="beforeLeave"//对应leave-from
  @leave="leave"//对应enter-active
  @after-leave="afterLeave"//对应leave-to
  @leave-cancelled="leaveCancelled"//离开过度打断0
```

### transition-group

多个组件动画需要用到这个，同 transition 使用方式一致

### provide/inject

父组件:
const color = ref<string>('red')
provide("color":color)
子组件:
inject<Ref<string>>('color')

### css 可以直接使用 v-bind 绑定 setup 中定义的颜色变量 background:v-bind(color)

### tsx

- 插件安装
  npm install @vitejs/plugin-vue-jsx -D

```
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import vueJsx from '@vitejs/plugin-vue-jsx';
// https://vitejs.dev/config/
export default defineConfig({
  plugins: [vue(),vueJsx()]
})
```

三种使用 tsx 的方式

```
// 第一种方式
// export default function(){
//     return (<div>Liovee</div>)
// }
//第二种方式 options api
// import {defineComponent} from 'vue'
// export default defineComponent({
//     data(){
//         return {
//             age:22
//         }
//     },
//     render(){
//         return (<div>{this.age}</div>)
//     }
// })
// 第三种setup函数 支持v-show不支持v-if可以用三元表达式替换 map代替v-for
const A = (_:any,{slots}:any)=>(
    <>
    <div>{slots.default?slots.default() : '默认值'}</div>
    <div>{slots.foo?.()}</div>
    </>
)
interface Props{
    name?:String
}
import {defineComponent,ref} from 'vue'
export default defineComponent({
    props:{
        name:String
    },
    emits:['on-click'],
    setup(props:Props,{emit}){
        const flag = ref(false)
        const data = [
            {name:"Liovee1"},
            {name:"Liovee2"},
            {name:"Liovee3"},
        ]
        const fn = (item:any)=>{
            console.log('触发了事件',item);
            emit('on-click',item)
        }
        const slot = {
            default:()=>(
                <div>LioveeDefault slots</div>
            ),
            foo:()=>(
                <div>Liovee foo</div>
            )
        }
        const v = ref<String>('')
        return ()=>(
        <>
        <input v-model = {v.value} type="text" />
        <div>{v.value}</div>
        <A v-slots = {slot}></A>
        <div>props:{props?.name}</div>
        {data.map(v=>{return <div onClick={()=>fn(v)}>{v.name}</div>})}
        </>
        )
    }
})
```

### 自定义指令

```
Vue3指令的钩子函数
created 元素初始化的时候
beforeMount 指令绑定到元素后调用 只调用一次
mounted 元素插入父级dom调用
beforeUpdate 元素被更新之前调用
update 这个周期方法被移除 改用updated
beforeUnmount 在元素被移除前调用
unmounted 指令被移除后调用 只调用一次
Vue2 指令 bind inserted update componentUpdated unbind
```

```
type dir = {
  background:string
}
<A v-move:aaa.Liovee="{background:'red'}"></A>
const vMove: Directive = {
    mounted(el:HTMLElement,dir:DirectiveBinding<Dir>) {
        el.style.background = dir.value.background
    },
}
```

```
自定义指令简单实现按钮鉴权
<template>
    <div class="btns">
        <button v-has-show="'shop:edit'">创建</button>
        <button v-has-show="'shop:create'">编辑</button>
        <button v-has-show="'shop:delete'">删除</button>
    </div>
</template>
<script setup lang="ts">
import type { Directive } from 'vue';
localStorage.setItem('userId', 'Liovee')
const permission = [
    // 'Liovee:shop:edit',
    'Liovee:shop:create',
    'Liovee:shop:delete',
]
const userid = localStorage.getItem('userId') as string
const vHasShow: Directive<HTMLElement,string> = (el, bilding) => {
    if (!permission.includes(userid + ':' + bilding.value)) {
        el.style.display = 'none'
    }
}
</script>
<style lang="less">
.btns {
    button {
        margin: 10px;
    }
}
</style>
```

```
图片懒加载
<template>
  <div>
    <div>
      <img v-lazy="item" width="360" height="500" v-for="item in arr" alt="">
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, reactive } from 'vue'
import type { Directive } from 'vue'
let imageList: Record<string, { default: string }> = import.meta.glob('./assets/images/*.*', { eager: true })
let arr = Object.values(imageList).map(v => v.default)
let vLazy: Directive<HTMLImageElement, string> = async (el, bilding) => {
  const def = await import('./assets/vue.svg')
  el.src = def.default
  const observer = new IntersectionObserver((enr) => {
    //intersectionRatio属性判断在可视区域出现的比例
    if (enr[0].intersectionRatio > 0) {
      setTimeout(() => {
        el.src = bilding.value
      }, 200)
      observer.unobserve(el) // 停止监听
    }
  })
  observer.observe(el) //元素传入
  console.log(el);
}
</script>

<style lang="less" scoped></style>
```

### 自定义 hook

案例

```
App.vue
<template>
  <div>
    <img id="img" width="300" height="300" src="./assets/images/4.png" alt="">
  </div>
</template>

<script setup lang="ts">
import useBase64 from './hooks/index'
useBase64({
  el: "#img"
}).then(res => {
  console.log(res.baseURL);
})
</script>

<style lang="less" scoped></style>
hooks/index.tsx
import { onMounted } from 'vue'
type Options = {
    el: string
}
export default function (options: Options): Promise<{ baseURL: string }> {
    return new Promise((resolve) => {
        onMounted(() => {
            let img: HTMLImageElement = document.querySelector(options.el) as HTMLImageElement
            img.onload = () => {
                resolve({
                    baseURL: base64(img)
                })
            }

        })
        const base64 = (el: HTMLImageElement) => {
            const canvas = document.createElement('canvas')
            const ctx = canvas.getContext('2d')
            canvas.width = el.width
            canvas.height = el.height
            ctx?.drawImage(el, 0, 0, canvas.width, canvas.height)
            return canvas.toDataURL('image/png')
        }
    })

}
```

### 全局变量

vue2 使用 prototype 定义全局变量 而 vue3 使用 globalProperties 即 app.config.globalProperties.$http

```
定义全局过滤器
app.config.globalProperties.$filters = {
  format<T>(str:T){
    return `Liovee - ${str}`
  }
}
使用 如果在script中使用
const app = getCurrentInstance()
app?.proxy.$filter.format()

会报错是因为没有加声明文件

type Filter = {
    format<T>(str: T): string
}
declare module 'vue' {
    export interface ComponentCustomProperties {
        $filters: Filter
    }

```

### 插件

```
实例
loading/index.vue
<template>
    <div v-if="isShow" class="loading">
        <div class="loading-content">Loading...</div>
    </div>
</template>

<script setup lang='ts'>
import { ref } from 'vue';
const isShow = ref(false)//定位loading 的开关

const show = () => {
    isShow.value = true
}
const hide = () => {
    isShow.value = false
}
//对外暴露 当前组件的属性和方法
defineExpose({
    isShow,
    show,
    hide
})
</script>



<style scoped lang="less">
.loading {
    position: fixed;
    inset: 0;
    background: rgba(0, 0, 0, 0.8);
    display: flex;
    justify-content: center;
    align-items: center;
    &-content {
        font-size: 30px;
        color: #fff;
    }
}
</style>
loading/index.ts
import type { App, VNode } from 'vue'
import Loading from './index.vue'
import { createVNode, render } from 'vue'
export default {
    install(app: App) {
        console.log(app, 1232);
        const Vnode = createVNode(Loading)//将插件转换为vnode
        render(Vnode, document.body)//render函数进行挂载
        console.log(Vnode.component, 3213213);

        app.config.globalProperties.$loadingg = {
            show: Vnode.component?.exposed?.show,
            hide: Vnode.component?.exposed?.hide,
        }
    }
}
App.vue
const instance = getCurrentInstance()
instance?.proxy?.$loadingg?.show()
main.ts
type Lod = {
    show: () => void,
    hide: () => void
}
//编写ts loading 声明文件放置报错 和 智能提示
declare module '@vue/runtime-core' {
    export interface ComponentCustomProperties {
        $loadingg: Lod
    }
}
```

### 样式穿透

vue2 使用/deep/ vue3 使用:deep(类名)

css 式样 :slotted 假设 aclass 是组件 a 的类名 且啊是个插槽，b 是 a 的父组件且 b 中定义了 a 的样式，此时样式不生效如果使用:slotted(.a)这样就生效了
:global() 全局选择器
动态 css

```
const red = ref('red')
.div{
  color:v-bind(red)
}
const red = ref({
  color:'red'
})
.div{
  color:v-bind(red.color)（错误）
  color:v-bind("red.color")（正确）
}
```

```
<div :class="$style.div"></div>
<style module>
.div{color:red}
</style>
```

```
const css = useCssModule('Liovee') //css模块化
<style module="Liovee">
.div{color:red}
</style>
```

### vue 继承 tailwind

- 1.安装 tailwind 及其依赖项

```
npm install -D tailwindcss@latest postcss@latest autoprefixer@latest
```

- 2.生成配置项

```
npx tailwindcss init -p
```

- 3.修改配置文件 tailwind.config.js

```
module.exports = {
  content: ['./index.html', './src/**/*.{vue,js,ts,jsx,tsx}'],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

- 4. src 目录底下新建 index.css 并配置
- 5.main.ts 中引入即可

### EventLoop

1.宏任务
script(整体代码)、setTimeout、setInterval、UI 交互事件、postMessage、Ajax

2.微任务
Promise.then catch finally、MutaionObserver、process.nextTick(Node.js 环境)

### nextTick

使用 nextTick 是因为 vue 更新 dom 是异步的，数据更新是同步的,所以有时候数据更新 dom 还没更新
使用方式

```
1.回调函数模式
nextTick(()=>{})
2.async await模式
const send = async ()=>{
    await nextTick()
    // 同步代码
}
```

### vue 开发移动端

- 1.安装依赖

```
npm install postcss-px-to-viewport -D //将px转为vw或者vh
```

### unocss(原子化)

- 1.npm i unocss
- 2.vite.config.js 的配置(静态)

```
import unoCss from 'unocss/vite'
plugins: [vue(),vueJsx(),unoCss({
    rules:[
      ['flex',{display:"flex"}],
      ['red',{color:'red'}]
    ]
  })],
```

(动态)

```
rules: [
  [/^m-(\d+)$/, ([, d]) => ({ margin: `${Number(d) * 10}px` })],
  ['flex', { display: "flex" }]
]
```

shortcuts 可以自定义组合样式

```
plugins: [vue(), vueJsx(), unocss({
    rules: [
      [/^m-(\d+)$/, ([, d]) => ({ margin: `${Number(d) * 10}px` })],
      ['flex', { display: "flex" }],
      ['pink', { color: 'pink' }]
    ],
    shortcuts: {
      btn: "pink flex"
    }
```

- 3.main.ts 引入
  import 'uno.css'
- 4.预设

```
presets:[presetIcons(),presetAttributify(),presetUno()]
```
图标库安装
```
npm i -D @iconify-json/ic
```
### h函数
h 接收三个参数
- type 元素的类型
- propsOrChildren 数据对象, 这里主要表示(props, attrs, dom props, class 和 style)
- children 子节点
使用props传递参数
```
<template>
    <Btn text="按钮"></Btn>
</template>
  
<script setup lang='ts'>
import { h, } from 'vue';
type Props = {
    text: string
}
const Btn = (props: Props, ctx: any) => {
    return h('div', {
        class: 'p-2.5 text-white bg-green-500 rounded shadow-lg w-20 text-center inline m-1',
 
    }, props.text)
}
</script>
```
接受emit
```
<template>
    <Btn @on-click="getNum" text="按钮"></Btn>
</template>
  
<script setup lang='ts'>
import { h, } from 'vue';
type Props = {
    text: string
}
const Btn = (props: Props, ctx: any) => {
    return h('div', {
        class: 'p-2.5 text-white bg-green-500 rounded shadow-lg w-20 text-center inline m-1',
        onClick: () => {
            ctx.emit('on-click', 123)
        }
    }, props.text)
}
 
const getNum = (num: number) => {
    console.log(num);
}
</script>
```
定义插槽
```
<template>
    <Btn @on-click="getNum">
        <template #default>
            按钮slots
        </template>
    </Btn>
</template>
  
<script setup lang='ts'>
import { h, } from 'vue';
type Props = {
    text?: string
}
const Btn = (props: Props, ctx: any) => {
    return h('div', {
        class: 'p-2.5 text-white bg-green-500 rounded shadow-lg w-20 text-center inline m-1',
        onClick: () => {
            ctx.emit('on-click', 123)
        }
    }, ctx.slots.default())
}
 
const getNum = (num: number) => {
    console.log(num);
}
</script>
```

### 开发环境
```
npm run dev 就是开发环境
npm run build 就是生产环境
```