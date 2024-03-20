### Pinia 使用介绍

- 1.根目录新建文件夹 store,并创建一个 ts 文件

- 2.定义仓库 import {defineStore} from 'pinia'
- 3.需要知道存储是使用定义的 defineStore()，并且它需要一个唯一的名称，作为第一个参数传递
  创建一个新的文件 store.name.ts 并储存一个枚举

```
store-name
export const enum Names {
    Test = 'TEST'
}
store.js
import { defineStore } from 'pinia'
import { Names } from './store-namespce'

export const useTestStore = defineStore(Names.Test, {
     state:()=>{
         return {
             current:1
         }
     },
     //类似于computed 可以帮我们去修饰我们的值
     getters:{

     },
     //可以操作异步 和 同步提交state
     actions:{

     }
App.vue
const Test = UseTestStore()
Test.name
```

- 修改 state 的值的几种方式 1.直接修改

```const Add = () => {
  Test.current++
}
```

2.批量修改 State 的值

```
Test.$patch({
    current:200,
    age:300
 })
```

3.批量修改函数形式

```
Test.$patch((state)=>{
       state.current++;
       state.age = 40
    })
```

4.通过原始对象修改整个实例

```
Test.$state = {
       current:10,
       age:30
    }
```

5.通过 actions 修改

```
actions:{
         setCurrent () {
             this.current++
         }
     }
使用方式是在实例中直接调用
const Add = () => {
     Test.setCurrent()
}
```
store里面的值是可以解构的，但是结构出来的值不是响应式的，可以引入pinia中的storetoRefs让它成为响应式的，从而在页面中使用
对于异步的action，需要使用async await
```
async setUser() {
            const result = await Login()
        }
```