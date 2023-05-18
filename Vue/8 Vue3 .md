# 8.Vue3

### vue3简介

**性能的提升**

- 打包大小减少**41%**
- 初次渲染快**55%**, 更新渲染快**133%**
- 内存减少**54%**

**源码的升级**

- 使用`Proxy`代替`defineProperty`实现响应式
- 重写虚拟`DOM`的实现和`Tree-Shaking`

**支持TypeScript**

- Vue3可以更好的支持`TypeScript`

**Vue3新的特性**

1. Composition API（组合API）
    - `setup`配置
    - `ref`与`reactive`
    - `watch`与`watchEffect`
    - `provide`与 `inject`
2. 新的内置组件
    - `Fragment`
    - `Teleport`
    - `Suspense`
3. 其他改变
    - 新的生命周期钩子
    - `data` 选项应始终被声明为一个函数
    - 移除`keyCode`支持作为 `v-on` 的修饰符

### ****创建Vue3.0工程****

- **使用 vue-cli 创建**

```
## 查看@vue/cli版本，确保@vue/cli版本在4.5.0以上
vue --version
## 安装或者升级你的@vue/cli
npm install -g @vue/cli
## 创建
vue create vue_test
## 启动
cd vue_test
npm run serve
```

- ****使用vite创建****

官方文档：[https://v3.cn.vuejs.org/guide/installation.html#vite](https://v3.cn.vuejs.org/guide/installation.html#vite)

vite官网：[https://vitejs.cn](https://vitejs.cn/)

- 什么是vite？—— 新一代前端构建工具。
- 优势如下：
    - 开发环境中，无需打包操作，可快速的冷启动。
    - 轻量快速的热重载（HMR）。
    - 真正的按需编译，不再等待整个应用编译完成。

```
## 创建工程
npm init vite-app <project-name>
## 进入工程目录
cd <project-name>
## 安装依赖
npm install
## 运行
npm run dev
```

### ****分析文件目录****

**main.js**

Vue2项目的`main.js`

```jsx
import Vue from 'vue'
import App from './App.vue'

Vue.config.productionTip = false

new Vue({
  render: h => h(App),
}).$mount('#app')
```

Vue3项目中的`main.js`

```jsx
// 引入的不再是Vue构造函数了，引入的是一个名为createApp的工厂函数
import { createApp } from 'vue'
import App from './App.vue'

// 创建应用实例对象——app(类似于之前Vue2中的vm，但app是轻量级（部分API），vm是重量级（所有的API）)
const app = createApp(App)
console.log(app)
// 挂载
app.mount('#app')
```

****App.vue**** 我们再来看看组件，在template标签里可以没有根标签了

```jsx
<template>
	<!-- Vue3组件中的模板结构可以没有根标签 -->
	<img alt="Vue logo" src="./assets/logo.png">
	<HelloWorld msg="Welcome to Your Vue.js App"/>
</template>
```

### ****常用 Composition API****

- **setup**
    1. setup是Vue3中一个新的配置项，值为一个函数。
    2. setup是所有Composition API（组合API）表演的舞台 。
    3. **组件中所用到的：数据、方法等等配置项，均要配置在setup中。**
    4. setup函数的两种返回值：
        1. 若返回一个对象，则对象中的属性、方法，在模板（`template`标签）中均可以直接使用。**setup中的重点**
        2. 若返回一个渲染函数：则可以自定义渲染内容。（了解的内容）
    5. 注意点：
        1. 尽量不要与Vue2的配置混用
            - Vue2.x配置（data、methos、computed…）中**可以访问到**setup中的属性、方法。
            - 但在setup中**不能访问到**Vue2.x配置（data、methos、computed…）。
            - 如果有重名, setup优先。
        2.  setup不能是一个async函数，因为返回值不再是return的对象, 而是promise, 模板看不到return对象中的属性。（后期也可以返回一个Promise实例，但需要Suspense和异步组件的配合）
- setup函数配置具体实现：
    
    ```jsx
    <!--Vue3的setup函数配置-->
    <template>
      <h1>Vue3的setup配置</h1>
      <h2>姓名：{{username}}</h2>
      <h2>性别：{{sex}}</h2>
      <h2>个人介绍：我是{{username}},性别{{sex}}</h2>
    </template>
    <script>
      export default {
        name: 'App',
        setup() {
          // 定义
          let username = 'Shier'
          let sex = '男'
          // 定义函数调用
          function personInfo() {
            alert('我是' + username + '性别为:' + sex)
          }
    // 将变量或者函数返回 ,返回的是一个对象 ，放回的属性就可以直接在template标签中直接使用，不需要使用this
          return {
            username,
            sex,
            personInfo
          }
        }
      }
    </script>
    ```
    

### ****ref函数****

- 作用: 定义一个 ref 响应式的数据
- 语法: `const xxx = ref(initValue)`
    - 创建一个包含响应式数据的**引用对象（reference对象，简称ref对象）。**
    - **JS中操作数据： `xxx.value`**
    - **模板中读取数据: 不需要`.value`，直接：`<div>{{xxx}}</div>`**
- 使用ref函数需要注意三点：
    1. 接收的数据可以是：基本类型、也可以是对象类型。
    2. 基本类型的数据：响应式依然是靠`Object.defineProperty()`的`get`与`set`完成的。
    3. 对象类型的数据：内部求助了Vue3.0中的一个新函数—— `reactive`函数。

`src/App.vue`:

```jsx
<template>
    <h2>name:{{ name}}</h2>
    <h2>school:{{ school }}</h2>
    <h2>type:{{ job.type }}</h2>
    <h2>salary:{{ job.salary }}</h2>
    <button @click="info">click</button>
</template>

<script>
import {ref} from 'vue'

export default {
  
  name: 'App',
  setup(){
    let name=ref('tam')
    let school =ref('wsyu')
    let job=ref({
      type:'front-edge enginner',
      salary:1500
    })
    
    function info(){
      alert('i am'+name.value+',i am from '+school.value)
      job.value.salary=2300
    }
    return {
      name,
      school,
      job,
      info
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

### **reactive函数**

- **reactive只能定义一个对象、数组**类型的响应式数据（基本类型不要用它，要用`ref`函数）
- 语法：`const 代理对象= reactive(源对象)`接收一个对象（或数组），返回一个**代理对象（Proxy的实例对象，简称proxy对象）**
- reactive定义的响应式数据是**深层次的。**
- 内部基于 ES6 的 Proxy 实现，通过代理对象操作源对象内部数据进行操作。
- **对数组的修改可以直接通过索引**

```jsx
<!--Vue3的reactive-->
<template>
  <h1>个人信息</h1>
  <h2>姓名：{{ person.username }}</h2>
  <h2>性别：{{ person.sex }}</h2>
  <h2>工作：{{ person.type }}</h2>
  <h2>薪水：{{ person.salary }}</h2>
  <button @click="changeInfo">修改信息</button>
</template>
<script>
  // 导入reactive
  import {reactive} from "vue";

  export default {
    name: 'App',
    setup() {
      // 使用reactive创建对象数据类型
      let person = reactive({
        username: 'Shier',
        sex: '男',
        type: '全栈工程师',
        salary: '40k',
        // 使用reactive创建数组数据类型
        hobby: reactive(['学习', '跑步', '编程'])
      })

      // 定义修改信息函数
      function changeInfo() {
        person.username = 'shier22'
        person.sex = '女'
        // reactive修改对象数据
        person.salary = '45k'
        person.type = '产品经理'
        // reactive 修改数组数据：通过索引值修改
        person.hobby[0] = '打王者'
      }

      // 返回值，上面定义的变量、函数、方法
      return {
        person,
        changeInfo,
      }
    }
  }
</script>
```

## ****Vue3中的响应式原理****

### ****vue2.x的响应式****

实现原理：

- 对象类型：通过`Object.defineProperty()`对属性的读取、修改进行拦截（数据劫持）。
- 数组类型：通过重写更新数组的一系列方法来实现拦截。（对数组的变更方法进行了包裹）。
    
    ```jsx
    Object.defineProperty(data, 'count', {
        get () {}, 
        set () {}
    })
    ```
    
- 存在问题：
    - 新增属性、删除属性, 界面不会更新。
    - 直接通过下标修改数组, 界面不会自动更新。

### ****Vue3.0的响应式****

- 实现原理:
    - 通过Proxy（代理）: 拦截对象中任意属性的变化, 包括：属性值的读写、属性的添加、属性的删除等。
    - 通过Reflect（反射）: 对源对象的属性进行操作。
- MDN文档中描述的Proxy与Reflect：

Proxy：[https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy)

Reflect：[https://developer.mozilla.org/zh-N/docs/Web/JavaScript/Reference/Global_Objects/Reflect](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect)

```jsx
new Proxy(data, {
	// 拦截读取属性值
    get (target, prop) {
    	return Reflect.get(target, prop)
    },
    // 拦截设置属性值或添加新属性
    set (target, prop, value) {
    	return Reflect.set(target, prop, value)
    },
    // 拦截删除属性
    deleteProperty (target, prop) {
    	return Reflect.deleteProperty(target, prop)
    }
})

proxy.name = 'tom'
```

## ****reactive对比ref****

- 从定义数据角度对比：
    - ref用来定义：基本类型数据。
    - reactive用来定义：对象（或数组）类型数据。
    - 备注：ref也可以用来定义对象（或数组）类型数据, 它内部会自动通过`reactive`转为**代理对象**。
- 从原理角度对比：
    - ref通过`Object.defineProperty()`的`get`与`set`来实现响应式（数据劫持）。
    - reactive通过使用**Proxy**来实现响应式（数据劫持）, 并通过**Reflect**操作**源对象**内部的数据。
- 从使用角度对比：
    - ref定义的数据：操作数据**需要**`.value`，读取数据时模板中直接读取**不需要**`.value`。
    - reactive定义的数据：操作数据与读取数据：**均不需要**`.value`。

## ****setup的两个注意点****

- setup执行的时机
    - **在beforeCreate之前执行一次，this是undefined。**
- setup的参数
    - props：值为对象，包含：组件外部传递过来，且组件内部声明接收了的属性。
    - context：上下文对象
        - attrs: 值为对象，包含：组件外部传递过来，但没有在props配置中声明的属性, 相当于 `this.$attrs`。
        - slots: 收到的插槽内容, 相当于 `this.$slots`。
        - emit: 分发自定义事件的函数, 相当于 `this.$emit`。

**`App.vue`：**

```jsx
<template>
  <Demo @hello="showhelloMsg" msg="你好啊" school="尚硅谷">
    <template v-slot:qwe>
      <span>尚硅谷</span>
    </template>
  </Demo>
</template>

<script>
import Demo from './components/Demo.vue'
export default {
  name: 'App',
  components: {
    Demo
  },
  setup() {
    function showhelloMsg(value) {
      alert(`你好，你触发了hello事件，我收到的参数是：${value}`)
    }
    return {
      showhelloMsg
    }
  }
}
</script>

<style>

</style>
```

**`Demo.vue:`**

```jsx
<template>
    <h1>Demo</h1>
    <button @click="test()">测试触发一下Demo组件的Hello事件</button>
</template>

<script>
export default {
    name: 'Demo',
    props: ['msg', 'school'],
    emits: ['hello'],
    setup(props, context) {
        console.log('---setup---', props)
        console.log('---setup---', context)
        console.log('---setup---', context.attrs) //相当于Vue2中的$attrs
        console.log('---setup---', context.emit) //触发自定义事件
        console.log('---setup---', context.slots) //插槽
        function test() {
            context.emit('hello', 666)
        }
        // 返回一个对象
        return {
            test
        }
    }
}
</script>

<style>

</style>
```

## ****计算属性与监视****

### ****computed函数****

- 与Vue2.x中computed配置功能一致
- 在Vue3中的计算属性属于一个组合，需要引入，computed函数
    
    ```jsx
    import {computed} from 'vue'
    
    setup(){
        ...
    	//计算属性——简写
        let fullName = computed(()=>{
            return person.firstName + '-' + person.lastName
        })
        //计算属性——完整
        let fullName = computed({
            get(){
                return person.firstName + '-' + person.lastName
            },
            set(value){
                const nameArr = value.split('-')
                person.firstName = nameArr[0]
                person.lastName = nameArr[1]
            }
        })
    }
    ```
    

案例：

```jsx
<template>
  <h1>个人信息</h1>
  姓：<input type="text" v-model="person.firstName"><br><br>
  名：<input type="text" v-model="person.lastName"><br><br>
  <span>全名： {{person.fullName}}</span><br><br>
  修改名字：<input type="text" v-model="person.fullName">
</template>

<script>
  // 导入reactive,在
  import {reactive, computed} from "vue";

  export default {
    name: "ComputedDemo",
    setup() {
      // 使用reactive创建对象数据类型
      let person = reactive({
        firstName: '十',
        lastName: '二',
      })
      /*// 简单写法（只读）将computed组合创建在person对象中,将值返回
      person.fullName = computed(() => {
        return person.firstName + '-' + person.lastName
      })*/
      // 完整写法（读写）将computed组合创建在person对象中,将值返回
      person.fullName = computed({
        get(){
          return person.firstName + '-' + person.lastName
        },
        set(value){
          const nameArray = value.split('-')
          person.firstName = nameArray[0]
          person.lastName = nameArray[1]
        }
      })
      // 返回值，上面定义的变量、函数、方法
      return {
        person
      }
    }
  }
</script>

```

### **watch函数**

**自己总结 不知道对不对：watch(xxx, (newValue, oldValue))若xxx是proxy对象，则强制开启深度监视，不能正常获取oldValue**

- 与Vue2.x中watch配置功能一致
- 两个小“坑”：
    - 监视reactive定义的响应式数据时：oldValue无法正确获取、强制开启了深度监视（deep配置失效）。
    - 监视reactive定义的响应式数据中某个属性时：deep配置有效。

```jsx
<template>
  <h1 v-show="sum">当前求和结果{{ sum }}</h1><br>
  <button @click="sum++">sum+1</button>
  <hr>
  <h1 v-show="msg">当前信息：{{ msg }}</h1><br>
  <button @click="msg+='@'">信息修改</button>
  <hr>
  <h1 v-show="person.name">姓名:{{person.name}}</h1><br>
  <h1 v-show="person.age">年龄:{{person.age}}</h1><br>
  <h1 v-show="person.job.salary">薪资:{{person.job.salary}}K</h1><br>
  <button @click="person.name+='~'">姓名修改</button>
  <button @click="person.age++">年龄增加</button>
  <button @click="person.job.salary++">薪资++</button>
</template>

<script>
  // 引入需要的组合
  import {reactive, ref, watch} from "vue";

  export default {
    name: "WatchDemo",
    setup() {
      // 监视的数据
      let sum = ref(1)
      let msg = ref('Vue3')
      let person = reactive({
        name: 'Shier',
        age: 19,
        job: {
          job1: '全栈',
          salary:88
        }
      })

      **//情况一：监视ref定义的响应式数据
      /*watch(sum, (newValue, oldValue) => {
        console.log('sum变化了', newValue, oldValue)
      }, {immediate: true})

      //情况二：监视多个ref定义的响应式数据
      watch([sum, msg], (newValue, oldValue) => {
        console.log('sum或msg变化了', newValue, oldValue)
      })

      /!* 情况三：监视reactive定义的响应式数据(也就是监视的是person整个对象（proxy代理的对象），则会自动强制开启深度监视)
            若watch监视的是reactive定义的响应式数据，则无法正确获得oldValue！！
            若watch监视的是reactive定义的响应式数据，则强制开启了深度监视
      *!/
      watch(person, (newValue, oldValue) => {
        console.log('person变化了', newValue, oldValue)
      }, {immediate: true, deep: false}) //此处的deep配置不再奏效

      //情况四：监视reactive定义的响应式数据中的某个属性
      watch(() => person.job, (newValue, oldValue) => {
        console.log('person的job变化了', newValue, oldValue)
      }, {immediate: true, deep: true})

      //情况五：监视reactive定义的响应式数据中的某些属性
      watch([() => person.job, () => person.name], (newValue, oldValue) => {
        console.log('person的job变化了', newValue, oldValue)
      }, {immediate: true, deep: true})
*/**
      //特殊情况
      watch(() => person.job, (newValue, oldValue) => {
        console.log('person的job变化了', newValue, oldValue)
      }, {deep: true}) //此处由于监视的是reactive素定义的对象中的某个属性，所以deep配置有效

      return {
        sum,
        msg,
        person
      }
    }
  }
</script>
```

### **watch函数的value使用问题 自己总结**

- person是通过ref定义的，因为数据是对象，所有会借助reactive生成proxy对象，此时若监视的是person，则实际监视的是{ proxy{…} }，此时可以开启深度监视检测变量变化（若person是通过reactive定义的 则深度监视是强制开启的）
- 若监视的是person.value,则实际监视的是 proxy{…} ,此时深度监视是默认开启的

```jsx
let person = ref({
        name: 'Shier',
        age: 19,
        job: {
          job1: '全栈',
          salary:88
        }
      })

1.
**watch(person, (newValue, oldValue) => {
        console.log('person变化了', newValue, oldValue)
      }, {deep: false}) 
2.
watch(person.value, (newValue, oldValue) => {
        console.log('person变化了', newValue, oldValue)
      })** 

```

### ****watchEffect函数****

- watch的套路是：既要指明监视的属性，也要指明监视的回调。
- watchEffect的套路是：不用指明监视哪个属性，监视的回调中用到哪个属性，那就监视哪个属性。
- watchEffect有点像computed：
    - 但computed注重的计算出来的值（回调函数的返回值），所以必须要写返回值。
    - 而watchEffect更注重的是过程（回调函数的函数体），所以不用写返回值。

## ****Vue3生命周期****

- Vue3.0中可以继续使用Vue2.x中的生命周期钩子，但有有两个被更名：
    - `beforeDestroy`改名为 `beforeUnmount`
    - `destroyed`改名为 `unmounted`

Vue3.0也提供了 Composition API 形式的生命周期钩子，与Vue2.x中钩子对应关系如下：

- `beforeCreate`==>`setup()`
- `created` ==>`setup()`
- `beforeMount` ==>`onBeforeMount`
- `mounted`==>`onMounted`
- `beforeUpdate`==>`onBeforeUpdate`
- `updated` ==>`onUpdated`
- `beforeUnmount` ==>`onBeforeUnmount`
- `unmounted` ==>`onUnmounted`
- 使用组合式 API 名称时，在前面添加 `onXxxx`

![Untitled](8%20Vue3%20209a3e5a592b46a49debd3b4dcca9913/Untitled.png)

> 当我们同时使用Vue2中配置项的形式与Vue3中的组合API形式编写生命周期时，组合API会比配置项形式出现在前
> 

```jsx
<template>
  <h1>当前求和结果:{{ sum }}</h1><br>
  <button @click="sum++">sum+1</button>
</template>

<script>
  // 引入需要的组合
  import {ref, onBeforeMount, onMounted, onBeforeUpdate, onUpdated, onBeforeUnmount, onUnmounted} from "vue";

  export default {
    name: "WatchDemo",
    setup() {
      console.log('setup--------')
      // 监视的数据
      let sum = ref(0)

      // 组合式API形式使用生命钩子
      onBeforeMount(() => {
        console.log('onBeforeMount----')
      })

      onMounted(() => {
        console.log('onMounted----')
      })

      onBeforeUpdate(() => {
        console.log('onBeforeMount----')
      })

      onUpdated(() => {
        console.log('onUpdated----')
      })

      onBeforeUnmount(() => {
        console.log('onBeforeUnmount----')
      })

      onUnmounted(() => {
        console.log('onUnmounted----')
      })
      return {sum}
    },

    // 配置项形式Vue3中的生命钩子
    beforeCreate() {
      console.log('beforeCreate--------')
    }
    ,
    created() {
      console.log('created--------')
    }
    ,
    beforeMount() {
      console.log('beforeMount--------')
    }
    ,
    mounted() {
      console.log('mounted--------')
    }
    ,
    beforeUpdate() {
      console.log('beforeUpdate--------')
    }
    ,
    updated() {
      console.log('updated--------')
    }
    ,
    beforeUnmount() {
      console.log('beforeUnmount--------')
    }
    ,
    unmounted() {
      console.log('unmounted--------')
    }

  }
</script>
```

## ****自定义hook函数****

- 什么是hook？—— **本质是一个函数**，把setup函数中使用的Composition API进行了封装。
- 类似于vue2.x中的mixin。
- 自定义hook的优势: **复用代码**, 让setup中的逻辑更清楚易懂。
- 封装目录：`src\hooks\xxx.js`

> 把setup函数中使用的Composition API封装到别的文件中，然后通过导入引用的方式，提高代码复用性
> 

**`App.vue`:**

```jsx
<template>
        <button @click="isShowDemo = !isShowDemo">click</button>
        <Demo v-if="isShowDemo"></Demo>
</template>

<script>
import { ref} from 'vue'
import Demo from './components/Demo.vue'
export default {
  
  name: 'App',
  components:{Demo},
  setup(){
    let isShowDemo=ref(true)
    return {
      isShowDemo
    }
  }
}
</script>
```

`components/**Demo.vue**`

```jsx
<template>
  <h2>当前点击鼠标时的坐标为：x={{ point.x }},y={{ point.y }}</h2>
</template>

<script>
import usePoint from '../hooks/usePoint'
export default {
    name:'Demo',
    setup() {
        let point=usePoint()
        return{
            point
        }
    }
}
</script>

<style>

</style>
```

`hooks/**usePoint.js**`

```jsx
import { onMounted,reactive,onBeforeUnmount} from 'vue';
export default function(){
    
        
        let point =reactive({
            x:0,
            y:0
      
         })

        function savePoint(event){
        point.x=event.pageX,
        point.y=event.pageY,
        console.log(point.x,point.y);

        }
        onMounted(()=>{
            window.addEventListener('click',savePoint)
        })
        onBeforeUnmount(() => {
            window.removeEventListener('click',savePoint)
        })
        
    return point
}
```

### ****toRef toRefs****

- 作用：**创建一个 ref 对象，其value值指向另一个对象中的某个属性**。
- 语法：`const name = toRef(person,'name')`
- 应用: 要将响应式对象中的某个属性单独提供给外部使用时。
- 扩展：`toRefs`与`toRef`功能一致，但可以批量创建多个 ref 对象，语法：`toRefs(person)`

****`toRefs`:**

```jsx
<template>
  <h2>姓名：{{ name }}</h2>
  <h2>年龄：{{ age }}</h2>
  <h2>薪资：{{ job.job1.salary }}K</h2>
  <button @click="name += '!'">修改姓名</button>
  <button @click="age++">修改年龄</button>
  <button @click="job.job1.salary += 10">修改薪资</button>
</template>

<script>
import { reactive, toRefs } from 'vue';

export default {
  name: 'App',
  setup() {
    let person = reactive({
      name: '张三',
      age: 18,
      job: {
        job1: {
          salary: 20
        }
      }
    })
    return {
      // ...为扩展运算符
      ...toRefs(person)
    }
  }
}
</script>

<style>

</style>
```

****`toRef:`****

```jsx
<template>
  <h2>姓名：{{ name }}</h2>
  <h2>年龄：{{ age }}</h2>
  <h2>薪资：{{ salary }}K</h2>
  <button @click="name += '!'">修改姓名</button>
  <button @click="age++">修改年龄</button>
  <button @click="salary += 10">修改薪资</button>
</template>

<script>
import { reactive, toRef } from 'vue';

export default {
  name: 'App',
  setup() {
    let person = reactive({
      name: '张三',
      age: 18,
      job: {
        job1: {
          salary: 20
        }
      }
    })
    return {
      name:toRef(person,'name'),
      age:toRef(person,'age'),
      salary:toRef(person.job.job1,'salary')
    }
  }
}
</script>

<style>

</style>
```

### ****shallowReactive与shallowRef****

- shallowReactive：只处理对象最外层属性的响应式（浅响应式）。
- shallowRef：只处理基本数据类型的响应式, 不进行对象的响应式处理。
- 什么时候使用?
    - 如果有一个对象数据，结构比较深, 但变化时只是外层属性变化 ===> shallowReactive。
    - 如果有一个对象数据，后续功能不会修改该对象中的属性，而是生新的对象来替换 ===> shallowRef。

****shallowReactive:****

```jsx
<template>
  <h2>姓名：{{ name }}</h2>
  <h2>年龄：{{ age }}</h2>
  <h2>薪资：{{ job.job1.salary }}K</h2>
  <button @click="name += '!'">修改姓名</button>
  <button @click="age++">修改年龄</button>
  <button @click="job.job1.salary += 10">修改薪资</button>
</template>

<script>
import { reactive, toRefs, shallowReactive } from 'vue';

export default {
  name: 'App',
  setup() {
    // **shallowReactive只考虑第一层数据的响应式**
    let person = shallowReactive({
      name: '张三',
      age: 18,
      job: {
        job1: {
          salary: 20
        }
      }
    })
    return {
      // ...为扩展运算符
      ...toRefs(person)
    }
  }
}
</script>

<style>

</style>
```

> 点击修改姓名和修改年龄会增加，但是点击深层次的修改薪资就不会增加。
> 
> 
> 此处还有一个注意点和疑问点：点完修改薪资不会变动，但是点完修改薪资以后，再次点击修改姓名或者修改年龄，则薪资会增加。
> 

****shallowRef****

```jsx
<template>
  <h2>当前x下的y的值为{{ x.y }}</h2>
  <button @click="x.y++">y+1</button>
</template>

<script>
import { reactive, toRefs, shallowRef } from 'vue';

export default {
  name: 'App',
  setup() {
    let x = shallowRef({
      y:0
    })

    return {
      x
    }
  }
}
</script>

<style>

</style>
```

> 点击y+1没有变化
> 

### ****readonly与shallowReadonly****

- readonly: 让一个响应式数据变为只读的（深只读）。
- shallowReadonly：让一个响应式数据变为只读的（浅只读）。
- 应用场景: 不希望数据被修改时。

```jsx
let person = reactive({
      name:'张三',
      age:18,
      job:{
        job1:{
          salary:20
        }
      }
    })

    // readonly: 让一个响应式数据变为只读的（深只读）
    person= readonly(person)
		// shallowReadonly：让一个响应式数据变为只读的（浅只读）。
    person = shallowReadonly(person)
```

### ****toRaw与markRaw****

- toRaw：
    - 作用：将一个由`**reactive`(ref不行)**生成的**响应式对象**转为**普通对象**。
    - 使用场景：用于读取响应式对象对应的普通对象，对这个普通对象的所有操作，不会引起页面更新。
- markRaw：
    - 作用：标记一个**对象**，使其永远不会再成为响应式对象。
    - 应用场景:
        1. 有些值不应被设置为响应式的，例如复杂的第三方类库等。
        2. 当渲染具有不可变数据源的大列表时，跳过响应式转换可以提高性能。

自己总结 ：**区别在于toRaw是将reactive生成的对象转为普通对象，而markRaw是让对象不能成为响应式对象。**

****toRaw：****

```jsx
<template>
  <h2>姓名：{{ name }}</h2>
  <h2>年龄：{{ age }}</h2>
  <h2>薪资：{{ job.job1.salary }}</h2>
  <button @click="name += '!'">修改姓名</button> <br>
  <button @click="age++">修改年龄</button><br>
  <button @click="job.job1.salary++">修改薪资</button><br>
  <button @click="showRawPerson">输出最原始的person</button>
</template>

<script>
import { reactive, toRefs, ref, shallowReadonly, toRaw } from 'vue';

export default {
  name: 'App',
  setup() {
    let person = reactive({
      name: '张三',
      age: 18,
      job: {
        job1: {
          salary: 20
        }
      }
    })

    // 输出最原始的person
    function showRawPerson(){
      console.log('代理对象的person',person)
      const p = toRaw(person)
      console.log('原始的对象的person',p)
    }

    return {
      ...toRefs(person),
      showRawPerson
    }
  }
}
</script>

<style>

</style>
```

****markRaw:****

```jsx
<template>
  <h2>姓名：{{ person.name }}</h2>
  <h2>年龄：{{ person.age }}</h2>
  <h2>薪资：{{ person.job.job1.salary }}</h2>
  <h2>资产：{{ person.car }}</h2>
  <h2></h2>
  <button @click="person.name += '!'">修改姓名</button> <br>
  <button @click="person.age++">修改年龄</button><br>
  <button @click="person.job.job1.salary++">修改薪资</button><br>

  <button @click="addCar">给人添加一台车</button><br>
  <button v-if="person.car" @click="person.car.name += '!'">修改车名</button><br>
  <button v-if="person.car" @click="changePrice()">修改车价格</button><br>
</template>

<script>
import { reactive, markRaw } from 'vue';

export default {
  name: 'App',
  setup() {
    let person = reactive({
      name: '张三',
      age: 18,
      job: {
        job1: {
          salary: 20
        }
      }
    })
    // 响应式对象身上追加一个car属性
    function addCar() {
      let car = {
        name: '奔驰',
        price: 40
      }
      // 标记对象不会成为响应式对象，对其更改以后，页面不会发生变法，但是他的值是变化的。
      person.car = markRaw(car)
    }
    // 更改车的价格
    function changePrice() {
      person.car.price++
      console.log('车的价格变为', person.car.price)
    }

    return {
      person,
      addCar,
      changePrice
    }
  }
}
</script>

<style>

</style>
```

### ****customRef****

- 作用：创建一个自定义的 ref，并对其依赖项跟踪和更新触发进行显式控制。
- 实现防抖效果

```jsx
<template>
        
        <input type="text" v-model="keyValue">
        <h3>{{ keyValue }}</h3>
</template>

<script>
import { track } from '@vue/reactivity'
import { trigger } from '@vue/reactivity'
import { customRef} from 'vue'

export default {
  
  name: 'App',
 
  setup(){
    function myRef(value,delay){
      let timer
      return customRef((track,trigger)=>{
        return {
          get(){
            track()
            return value
          },
          set(newValue){
            clearTimeout(timer)
            timer=setTimeout(() => {
              value=newValue
              trigger()
            }, delay);
            
          }
        }
      })
    }

    let keyValue=myRef('hello',300)
    return {
      keyValue
    }
  }
}
</script>
```

### **provide与inject**

- 作用：实现**祖组件与后代组件间**通信
- 套路：父组件有一个 `provide` 选项来提供数据，后代组件有一个 `inject` 选项来开始使用这些数据

App.vue

```jsx
<template>
  <div class="app">
    <h3>我是App组件（祖组件）</h3>
    <h4>{{ name }}---{{ price }}</h4>
    <ChildCom></ChildCom>
  </div>
</template>

<script>
import { provide, reactive, toRefs } from 'vue';
import ChildCom from './components/ChildCom.vue';
export default {
  name: 'App',
  components: {
    ChildCom
  },
  setup() {
    let car = reactive({
      name: '奔驰',
      price: '40W'
    })
    // provide() 给自己的后代组件传递数据
    provide('car',car)
    return {
      ...toRefs(car)
    }
  }

}
</script>

<style>
.app {
  background-color: #ccc;
  padding: 10%;
}
</style>
```

ChildCom.vue

```jsx
<template>
    <div class="child">
        <h3>我是Child组件（子组件）</h3>
        <h4>{{ name }}--{{ price }}</h4>
        <SonCom></SonCom>
    </div>
</template>

<script>
import { inject, toRefs } from 'vue';
import SonCom from './SonCom.vue';
export default {
    name: 'child',
    components: {
        SonCom
    },
    setup(){
        let car = inject('car')
        console.log('child--',car)
        return {
            ...toRefs(car)
        }
    }
}
</script>

<style>
.child {
    background-color: skyblue;
    padding: 10%;
}
</style>
```

SonCom.vue

```jsx
<template>
    <div class="son">
        <h3>我是Son组件（孙组件）</h3>
        <h4>{{ car.name }}--{{ car.price }}</h4>
    </div>
</template>

<script>
import { inject } from 'vue';

export default {
    name: 'son',
    setup(){
        let car = inject('car')
        console.log('son--',car)
        return {
            car
        }
    }
}
</script>

<style>
.son {
    background-color: orange;
    padding: 10%;
}
</style>
```

### ****响应式数据的判断API****

- isRef: 检查一个值是否为一个 ref 对象
- isReactive: 检查一个对象是否是由 `reactive` 创建的响应式代理
- isReadonly: 检查一个对象是否是由 `readonly` 创建的只读代理
- isProxy: 检查一个对象是否是由 `reactive` 或者 `readonly` 方法创建的代理

```jsx
<!--Vue3的API-->
<template>
  <h2> APP</h2>
</template>
<script>
  // 引入组件
  import {reactive, provide, ref, readonly, isProxy, isReadonly, isRef, isReactive} from "vue";

  export default {
    name: 'App',
    setup() {
      let demo = reactive({name: '张三', age: 88})
      let sum = ref(0)
      // 将数据提供出去
      let demoRef = isRef(demo)
      let demoReactive = isReactive(demo)
      let sumReactive = isReactive(sum)
      let sumReadOnly = isReadonly(demo)
      let demoProxy = isProxy(demo)

      console.log('demoReactive---', demoReactive)  // true
      console.log('demoRef---', demoRef) // false  由reactive创建
      console.log('sumReactive---', sumReactive) // false 由reactive创建
      console.log('sumReadOnly---', sumReadOnly) // false 可修改的
      console.log('demoProxy---', demoProxy) // true 由reactive创建 都是使用代理的
      return {...demo}
    }
  }
</script>
```

### ****Composition API 的优势****

- **Options API(配置项API) 存在的问题（Vue2）**

使用传统OptionsAPI中，新增或者修改一个需求，就需要分别在data，methods，computed里修改 

![Untitled](8%20Vue3%20209a3e5a592b46a49debd3b4dcca9913/Untitled%201.png)

- **Composition API(组合API) 的优势（Vue3）**

我们可以更加优雅的组织我们的代码，函数。让相关功能的代码更加有序的组织在一起。

![Untitled](8%20Vue3%20209a3e5a592b46a49debd3b4dcca9913/Untitled%202.png)

## ****新的组件****

### ****Fragment****

- 在Vue2中: 组件必须有一个根标签
- 在Vue3中: 组件可以没有根标签, 内部会将多个标签包含在一个Fragment虚拟元素中
- 好处: 减少标签层级, 减小内存占用

### Teleport

什么是Teleport？—— `Teleport` 是一种能够将我们的**组件html结构**移动到指定位置的技术。

**App.vue：**

```jsx
<template>
  <div class="app">
    <h3>我是App组件</h3>
    <ChildCom></ChildCom>
  </div>
</template>

<script>
import ChildCom from './components/ChildCom.vue';
export default {
  name: 'App',
  components: {
    ChildCom
  },
}
</script>

<style>
.app {
  background-color: #ccc;
  padding: 10%;
}
</style>
```

**ChildCom.vue**

```jsx
<template>
    <div class="child">
        <h3>我是Child组件</h3>
        <SonCom></SonCom>
    </div>
</template>

<script>
import SonCom from './SonCom.vue';
export default {
    name: 'child',
    components: {
        SonCom
    },
}
</script>

<style>
.child {
    background-color: skyblue;
    padding: 10%;
}
</style>
```

**SonCom.vue**

```jsx
<template>
    <div class="son">
        <h3>我是Son组件</h3>
    </div>
    <Dialog></Dialog>
</template>

<script>
import Dialog from './Dialog.vue';
export default {
    name: 'son',
    components:{
        Dialog
    }
}
</script>

<style>
.son {
    background-color: orange;
    padding: 10%;
}
</style>
```

**Dialog.vue**

```jsx
<template>
    <div>
        <button @click="isShow = true">点我弹个窗</button>
        <teleport to="body">
            <div v-if="isShow" class="mask">
                <div  class="dialog">
                <h3>我是一个弹窗</h3>
                <h4>写一些内容</h4>
                <button @click="isShow = false">关闭弹窗</button>
            </div>
            </div>
        </teleport>
    </div>
</template>

<script>
import { ref } from 'vue';

export default {
    name: 'Dialog',
    setup() {
        let isShow = ref(false)
        return {
            isShow
        }
    }
}
</script>

<style>
/* 遮罩层 */
.mask{
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: rgb(0,0,0,0.5);
}
.dialog {
    position:absolute;
    top: 50%;
    left: 50%;
    text-align: center;
    transform: translate(-50%,-50%);
    width: 300px;
    height: 300px;
    background-color: green;
}
</style>
```

**运行效果：弹窗没有把父组件撑开**

![Untitled](8%20Vue3%20209a3e5a592b46a49debd3b4dcca9913/Untitled%203.png)

### Suspense

- 等待异步组件时渲染一些额外内容，让应用有更好的用户体验。
- 使用步骤：
    - 异步引入组件(动态引入)
    
    ```jsx
    // import Child from 'vue'// 静态引入
    
    import {defineAsyncComponent} from 'vue'
    const Child = defineAsyncComponent(()=>import('./components/Child.vue'))
    ```
    
    - 使用`Suspense`包裹组件，并配置好`default`与 `fallback`
    
    ```jsx
    <template>
    	<div class="app">
    		<h3>我是App组件</h3>
    		<Suspense>
    			<template v-slot:default>
    				<Child/>
    			</template>
    			<template v-slot:fallback>
    				<h3>加载中.....</h3>
    			</template>
    		</Suspense>
    	</div>
    </template>
    ```
    

Child.vue:

```jsx

<template>
  <div class="child">
    <h2> Child儿子组件 </h2>
    {{sum}}
  </div>
</template>

<script>
  import {ref} from "vue";
  export default {
    name: "Child",
    // 异步
    async setup(){
      let sum = ref(0)
      let p = new Promise((resolve, reject)=>{
        setTimeout(()=>{
          resolve({sum})
        },2000)
      })
      return await p
    }
  }
</script>

<style scoped>
  .child {
    background-color: orange;
    padding: 10px;
  }
</style>
```

Child.vue:

```jsx
<!--Vue3的自定义hook-->
<template>
  <div class="app">
    <h3>我是App组件</h3>
    <Suspense>
      <template v-slot:default>
        <Child/>
      </template>
      <template v-slot:fallback>
        <h3>加载中.....</h3>
      </template>
    </Suspense>
  </div>
</template>
<script>
  // 引入组件
  //import Child from "./components/Child"; // 静态引入
  // 异步引入
  import {defineAsyncComponent} from 'vue'
  const Child = defineAsyncComponent(()=>import('./components/Child'))
  export default {
    name: 'App',
    components: {Child},
  }
</script>

<style scoped>
  .app {
    background-color: gray;
    padding: 10px;
  }
</style>
```

### ****全局API的转移****

Vue 2.x 有许多全局 API 和配置。

- 例如：注册全局组件、注册全局指令等。

```jsx
//注册全局组件
Vue.component('MyButton', {
  data: () => ({
    count: 0
  }),
  template: '<button @click="count++">Clicked {{ count }} times.</button>'
})

//注册全局指令
Vue.directive('focus', {
  inserted: el => el.focus()
}
```

- Vue3.0中对这些API做出了调整：
    - 将全局的API，即：`Vue.xxx`调整到应用实例（`app`）上
    
    | 2.x 全局 API（Vue） | 3.x 实例 API (app) |
    | --- | --- |
    | Vue.config.xxxx | app.config.xxxx |
    | Vue.config.productionTip | 移除 |
    | Vue.component | app.component |
    | Vue.directive | app.directive |
    | Vue.mixin | app.mixin |
    | Vue.use | app.use |
    | Vue.prototype | app.config.globalProperties |
    
    ![Untitled](8%20Vue3%20209a3e5a592b46a49debd3b4dcca9913/Untitled%204.png)
    

### ****其他的改变****

- data选项应始终被声明为一个函数。
- 过度类名的更改：
    - Vue2.x写法
    
    ```jsx
    .v-enter,
    .v-leave-to {
      opacity: 0;
    }
    .v-leave,
    .v-enter-to {
      opacity: 1;
    }
    ```
    
    - Vue3.x写法
    
    ```jsx
    .v-enter-from,
    .v-leave-to {
      opacity: 0;
    }
    
    .v-leave-from,
    .v-enter-to {
      opacity: 1;
    }
    ```
    
- **移除**keyCode作为 v-on 的修饰符，同时也不再支持`config.keyCodes`
- **移除**`v-on.native`修饰符
    - 父组件中绑定事件
    
    ```jsx
    <my-component
      v-on:close="handleComponentEvent"
      v-on:click="handleNativeClickEvent"
    />
    ```
    
    - 子组件中声明自定义事件
    
    ```jsx
    <script>
      export default {
        emits: ['close']
      }
    </script>
    ```
    
- **移除**过滤器（filter）
    
    > 过滤器虽然这看起来很方便，但它需要一个自定义语法，打破大括号内表达式是 “只是 JavaScript” 的假设，这不仅有学习成本，而且有实现成本！建议用方法调用或计算属性去替换过滤器。
    >