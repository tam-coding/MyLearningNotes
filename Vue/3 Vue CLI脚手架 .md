# 3.Vue CLI脚手架

### ****初始化脚手架****

**说明**

1. Vue 脚手架是 Vue 官方提供的标准化开发工具（开发平台）
2. 最新的版本是 4.x
3. 文档：[Vue CLI](https://cli.vuejs.org/zh/)

**具体步骤**

1. 如果下载缓慢请配置 npm 淘宝镜像：`npm config set registry http://registry.npm.taobao.org`
2. 全局安装@vue/cli：`npm install -g @vue/cli`
3. 切换到你要创建项目的目录，然后使用命令创建项目：`vue create xxxx`
4. 选择使用vue的版本
5. 启动项目：`npm run serve`
6. 暂停项目：Ctrl+C

Vue 脚手架隐藏了所有 webpack 相关的配置，若想查看具体的 webpack 配置，请执行：

`vue inspect > output.js`

****分析脚手架结构****

```
.文件目录
├── node_modules 
├── public
│   ├── favicon.ico: 页签图标
│   └── index.html: 主页面
├── src
│   ├── assets: 存放静态资源
│   │   └── logo.png
│   │── component: 存放组件
│   │   └── HelloWorld.vue
│   │── App.vue: 汇总所有组件
│   └── main.js: 入口文件
├── .gitignore: git版本管制忽略的配置
├── babel.config.js: babel的配置文件
├── package.json: 应用包配置文件 
├── README.md: 应用描述文件
└── package-lock.json: 包版本控制文件
```

`src/components/School.vue`:

```
<template>
  <div>
    <h2>{{name}}</h2>
  </div>
</template>

<script>
    export default {
        name:'School-1',
        data() {
            return {
                name:'wcyu'
            }
        },
    }
</script>
```

`src/components/Student.vue`:

```
<template>
  <div>
    <h2>{{name}}</h2>
  </div>
</template>

<script>
    export default {
        name:'Student-1',
        data() {
            return {
                name:'tam'
            }
        },
    }
</script>
```

`src/App.vue`:

```jsx
<template>
  <div>
    <School/>
    <Student/>
  </div>
</template>

<script>
    import Student from './components/Student.vue'
    import School from './components/School.vue'
    export default {
        name:'app',
        components:{
            School,
            Student
        }
    }
</script>

```

`src/main.js`:

```jsx
import Vue from 'vue'
import App from './App.vue'

Vue.config.productionTip = false

new Vue({
  render: h => h(App),
}).$mount('#app')
```

`public/index.html`:

```html
<!DOCTYPE html>
<html lang="">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <link rel="icon" href="<%= BASE_URL %>favicon.ico">
    <title><%= htmlWebpackPlugin.options.title %></title>
  </head>
  <body>
    <noscript>
      <strong>We're sorry but <%= htmlWebpackPlugin.options.title %> doesn't work properly without JavaScript enabled. Please enable it to continue.</strong>
    </noscript>
    <div id="app"></div>
    <!-- built files will be auto injected -->
  </body>
</html>
```

### ****render函数****

```jsx
import Vue from 'vue'
import App from './App.vue'

Vue.config.productionTip = false

new Vue({
    el:'#app',
    // 简写形式
	render: h => h(App),
    // 完整形式
	// render(createElement){
	//     return createElement(App)
	// }
})
```

关于不同版本的函数：

1. `vue.js` 与 `vue.runtime.xxx.js`的区别：
    1. `vue.js` 是完整版的 Vue，包含：核心功能+模板解析器
    2. `vue.runtime.xxx.js` 是运行版的 Vue，只包含核心功能，没有模板解析器
2.  因为 `vue.runtime.xxx.js` 没有模板解析器，所以不能使用 `template` 配置项，需要使用 `render`函数接收到的`createElement` 函数去指定具体内容

### ****修改默认配置****

- `vue.config.js` 是一个可选的配置文件，如果项目的（和 `package.json` 同级的）根目录中存在这个文件，那么它会被 `@vue/cli-service` 自动加载
- 使用 `vue.config.js` 可以对脚手架进行个性化定制，详见[配置参考 | Vue CLI](https://cli.vuejs.org/zh/config/#vue-config-js)

### ****ref属性****

```jsx
<template>
  <div>
    <School ref="sch"/>
    <span ref="n">span</span><br>
    <button @click="getN">getN</button>
    <button @click="getSch">getSch</button>
    <Student />
  </div>
</template>

<script>
    import Student from './components/Student.vue'
    import School from './components/School.vue'
    export default {
        name:'app',
        components:{
            School,
            Student
        },
         methods:{
          getN(){
            console.log(this.$refs.n)
          },
          getSch(){
            console.log(this.$refs.sch)
          }
        }
    }
</script>
```

`ref`属性：

1. 被用来（id的替代者）给元素或子组件注册引用信息
2. **应用在`html`标签上获取的是真实DOM元素，应用在组件标签上获取的是组件实例对象（vc）**
3. 使用方式：
    1. 打标识：`<h1 ref="xxx"></h1>` 或 `<School ref="xxx"></School>`
    2. 获取：`this.$refs.xxx`

### ****props配置项****

`props`配置项：

1. 功能：让组件接收外部传过来的数据
2. 传递数据：`<Demo name="xxx"/>`
3. 接收数据：
    1. 第一种方式（只接收）：`props:['name']`
    2. 第二种方式（限制数据类型）：`props:{name:String}`
    3. 第三种方式（限制类型、限制必要性、指定默认值）：
    
    ```jsx
    props:{
        name:{
        	type:String, //类型
            required:true, //必要性
            default:'JOJO' //默认值
        }
    }
    ```
    

注意：

**props是只读的**，Vue底层会监测你对props的修改，如果进行了修改，就会发出警告，**若业务需求确实需要修改，那么请复制props的内容到data中一份，然后去修改data中的数据**

`src/App.vue`:

```jsx
<template>
    <div>
        <Student name="tam" sex="boy" :age="20" />
    </div>
</template>

<script>
    import Student from './components/Student.vue'
    export default {
        name:'App',
        components: { Student },
    }
</script>
```

`src/components/Student.vue`:

```jsx
<template>
    <div>
        <h1>{{msg}}</h1>
        <h2>学生姓名：{{name}}</h2>
        <h2>学生性别：{{sex}}</h2>
        <h2>学生年龄：{{age}}</h2>     
    </div>
</template>

<script>
    export default {
        name:'Student',
        data() {
            return {
                msg:"我是一名来",
            }
        },
        // 简单声明接收
		// props:['name','age','sex']

        // 接收的同时对数据进行类型限制
		/* props:{
			name:String,
			age:Number,
			sex:String
		} */

        // 接收的同时对数据进行类型限制 + 指定默认值 + 限制必要性
		props:{
			name:{
				type:String,
				required:true,
			},
			age:{
				type:Number,
				default:99
			},
			sex:{
				type:String,
				required:true
			}
		}
    }
</script>
```

### **mixin混入**

**局部混入：**

`src/mixin.js`:

```jsx
export const mixin = {
    methods: {
        showName() {
            alert(this.name)
        }
    },
    mounted() {
        console.log("你好呀~")
    }
}
```

`src/components/School.vue`

```jsx
<template>
    <div>
        <h2 @click="showName">学校姓名：{{name}}</h2>
        <h2>学校地址：{{address}}</h2>   
    </div>
</template>

<script>
    //引入混入
    import {mixin} from '../mixin'
    
    export default {
        name:'School',
        data() {
            return {
                name:'尚硅谷',
				address:'北京'
            }
        },
        mixins:[mixin]
    }
</script>
```

`src/components/Student.vue`:

```jsx
<template>
    <div>
        <h2 @click="showName">学生姓名：{{name}}</h2>
        <h2>学生性别：{{sex}}</h2>   
    </div>
</template>

<script>
    //引入混入
    import {mixin} from '../mixin'
    
    export default {
        name:'Student',
        data() {
            return {
                name:'JOJO',
				sex:'男'
            }
        },
		mixins:[mixin]
    }
</script>
```

`src/App.vue`:

```jsx
<template>
    <div>
        <School/>
        <hr/>
        <Student/>
    </div>
</template>

<script>
    import Student from './components/Student.vue'
    import School from './components/School.vue'

    export default {
        name:'App',
        components: { Student,School },
    }
</script>
```

**全局混入：**

`src/main.js`:

```jsx
import Vue from 'vue'
import App from './App.vue'
import {mixin} from './mixin'

Vue.config.productionTip = false
Vue.mixin(mixin)

new Vue({
    el:"#app",
    render: h => h(App)
})
```

**总结：**

`mixin`（混入）：

1. 功能：可以把多个组件共用的配置提取成一个混入对象
    
    使用方式：
    
    1. 定义混入：
    
    ```jsx
    const mixin = {
        data(){....},
        methods:{....}
        ....
    }
    ```
    
    1. 使用混入：
        1. 全局混入：`Vue.mixin(xxx)`
        2. 局部混入：`mixins:['xxx']`

备注：

1. 组件和混入对象含有同名选项时，这些选项将以恰当的方式进行“合并”，在发生冲突时以组件优先。
    
    ```jsx
    mixin.js:
    var mixin = {
    	data: function () {
    		return {
        		message: 'hello',
                foo: 'abc'
        	}
      	}
    }
    -----------------------------
    调用minxin
    new Vue({
      	mixins: [mixin],
      	data () {
        	return {
          		message: 'goodbye',
                bar: 'def'
        	}
        },
      	created () {
        	console.log(this.$data)
        	// => { message: "goodbye", foo: "abc", bar: "def" }
      	}
    })
    ```
    
2. 同名生命周期钩子都将被调用。另外，混入对象的钩子将在组件自身钩子之前调用。
    
    ```jsx
    mixin.js:
    var mixin = {
      	created () {
        	console.log('混入对象的钩子被调用')
      	}
    }
    --------------------------------------------
    new Vue({
      	mixins: [mixin],
      	created () {
        	console.log('组件钩子被调用')
      	}
    })
    
    // => "混入对象的钩子被调用"
    // => "组件钩子被调用"
    ```
    

### ****plugin插件****

`src/plugin.js`:

```jsx
export default {
	install(Vue,x,y,z){
		console.log(x,y,z)
		//全局过滤器
		Vue.filter('mySlice',function(value){
			return value.slice(0,4)
		})

		//定义混入
		Vue.mixin({
			data() {
				return {
					x:100,
					y:200
				}
			},
		})

		//给Vue原型上添加一个方法（vm和vc就都能用了）
		Vue.prototype.hello = ()=>{alert('你好啊')}
	}
}
```

`src/main.js`:

```jsx
import Vue from 'vue'
import App from './App.vue'
import plugin from './plugin'

Vue.config.productionTip = false
Vue.use(plugin,1,2,3)

new Vue({
    el:"#app",
    render: h => h(App)
})
```

`src/components/School.vue`:

```jsx
<template>
    <div>
        <h2>学校姓名：{{name | mySlice}}</h2>
        <h2>学校地址：{{address}}</h2>   
    </div>
</template>

<script>
    export default {
        name:'School',
        data() {
            return {
                name:'尚硅谷atguigu',
				address:'北京'
            }
        }
    }
</script>
```

`src/components/Student.vue`:

```jsx
<template>
    <div>
        <h2>学生姓名：{{name}}</h2>
        <h2>学生性别：{{sex}}</h2> 
        <button @click="test">点我测试hello方法</button>  
    </div>
</template>

<script>
    export default {
        name:'Student',
        data() {
            return {
                name:'JOJO',
				sex:'男'
            }
        },
        methods:{
            test() {
                this.hello()
            }
        }
    }
</script>
```

插件：

1. 功能：用于增强Vue
2. 本质：**包含install方法的一个对象，install的第一个参数是Vue**，第二个以后的参数是插件使用者传递的数据
3. 定义插件：
    
    ```jsx
    plugin.install = function (Vue, options) {
            // 1. 添加全局过滤器
            Vue.filter(....)
        
            // 2. 添加全局指令
            Vue.directive(....)
        
            // 3. 配置全局混入
            Vue.mixin(....)
        
            // 4. 添加实例方法
            Vue.prototype.$myMethod = function () {...}
            Vue.prototype.$myProperty = xxxx
        }
    ```
    
4. 使用插件：main.js — `Vue.use(plugin)`

### ****scoped样式****

```jsx
<template>
  
    <div class="bgsc">
      <h2 class="fontw">{{name}}</h2>
      </div>
</template>
<script>
    export default {
        name:'School-1',
        data() {
            return {
                name:'wcyu'
            }
        },
}
</script>

<style scoped lang="less">
  .bgsc{
    background-color: skyblue;
    .fontw{
      color: aqua;
      font-size: 50px;
    }
  }
</style>
```

`scoped`样式：

1. 作用：让样式在局部生效，防止冲突
2. 写法：`<style scoped lang=“less”>` **lang指定语言**

注意：

`scoped`样式一般不会在`App.vue`中使用

### ****唯一ID生成类Nanoid库****

一个小巧、安全、URL友好、唯一的 JavaScript 字符串ID生成器。 **下面案例有使用**

[nanoid/README.zh-CN.md at main · ai/nanoid](https://github.com/ai/nanoid/blob/main/README.zh-CN.md)

### ****Todo-List案例****

`src/components/MyHeader.vue：`

```jsx
<template>
    <div class="todo-header">
        <input type="text" placeholder="请输入你的任务名称，按回车键确认" @keydown.enter="add" v-model="title"/>
    </div>
</template>

<script>
    import {nanoid} from 'nanoid'
    export default {
        name:'MyHeader',
        data() {
            return {
                title:''
            }
        },
        methods:{
            add(){
                if(!this.title.trim()) return
                const todoObj = {id:nanoid(),title:this.title,done:false}
                this.addTodo(todoObj)
                this.title = ''
            }
        },
        props:['addTodo']
    }
</script>

<style scoped>
    .todo-header input {
        width: 560px;
        height: 28px;
        font-size: 14px;
        border: 1px solid #ccc;
        border-radius: 4px;
        padding: 4px 7px;
    }

    .todo-header input:focus {
        outline: none;
        border-color: rgba(82, 168, 236, 0.8);
        box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 8px rgba(82, 168, 236, 0.6);
    }
</style>
```

`src/components/MyItem.vue`:

```jsx
<template>
    <li>
        <label>
            <input type="checkbox" :checked="todo.done" @click="handleCheck(todo.id)"/>
            <span>{{todo.title}}</span>
        </label>
        <button class="btn btn-danger" @click="handleDelete(todo.id,todo.title)">删除</button>
    </li>
</template>

<script>
    export default {
        name:'MyItem',
        props:['todo','checkTodo','deleteTodo'],
        methods:{
            handleCheck(id){
                this.checkTodo(id)
            },
            handleDelete(id,title){
                if(confirm("确定删除任务："+title+"吗？")){
                    this.deleteTodo(id)
                }
            }
        }
    }
</script>

<style scoped>
    li {
        list-style: none;
        height: 36px;
        line-height: 36px;
        padding: 0 5px;
        border-bottom: 1px solid #ddd;
    }

    li label {
        float: left;
        cursor: pointer;
    }

    li label li input {
        vertical-align: middle;
        margin-right: 6px;
        position: relative;
        top: -1px;
    }

    li button {
        float: right;
        display: none;
        margin-top: 3px;
    }

    li:before {
        content: initial;
    }

    li:last-child {
        border-bottom: none;
    }

    li:hover {
        background-color: #eee;
    }

    li:hover button{
        display: block;
    }
</style>
```

`src/components/MyList.vue`:

```jsx
<template>
    <ul class="todo-main">
        <MyItem 
            v-for="todo in todos" 
            :key="todo.id" 
            :todo="todo" 
            :checkTodo="checkTodo"
            :deleteTodo="deleteTodo"
        />
    </ul>
</template>

<script>
    import MyItem from './MyItem.vue'

    export default {
        name:'MyList',
        components:{MyItem},
        props:['todos','checkTodo','deleteTodo']
    }
</script>

<style scoped>
    .todo-main {
        margin-left: 0px;
        border: 1px solid #ddd;
        border-radius: 2px;
        padding: 0px;
    }

    .todo-empty {
        height: 40px;
        line-height: 40px;
        border: 1px solid #ddd;
        border-radius: 2px;
        padding-left: 5px;
        margin-top: 10px;
    }
</style>
```

`src/components/MyFooter.vue`:

```jsx
<template>
    <div class="todo-footer" v-show="total">
        <label>
            <input type="checkbox" v-model="isAll"/>
        </label>
        <span>
            <span>已完成{{doneTotal}}</span> / 全部{{total}}
        </span>
        <button class="btn btn-danger" @click="clearAll">清除已完成任务</button>
    </div>
</template>

<script>
    export default {
        name:'MyFooter',
        props:['todos','checkAllTodo','clearAllTodo'],
        computed:{
            doneTotal(){
                return this.todos.reduce((pre,todo)=> pre + (todo.done ? 1 : 0) ,0)
            },
            total(){
                return this.todos.length
            },
            isAll:{
                get(){
                    return this.total === this.doneTotal && this.total > 0
                },
                set(value){
                    this.checkAllTodo(value)
                }
            }
        },
        methods:{
            clearAll(){
                this.clearAllTodo()
            }
        }
    }
</script>

<style scoped>
    .todo-footer {
        height: 40px;
        line-height: 40px;
        padding-left: 6px;
        margin-top: 5px;
        }

    .todo-footer label {
        display: inline-block;
        margin-right: 20px;
        cursor: pointer;
    }

    .todo-footer label input {
        position: relative;
        top: -1px;
        vertical-align: middle;
        margin-right: 5px;
    }

    .todo-footer button {
        float: right;
        margin-top: 5px;
    }
</style>
```

`src/App.vue`:

```jsx
<template>
    <div id="root">
        <div class="todo-container">
            <div class="todo-wrap">
            <MyHeader :addTodo="addTodo"/>
            <MyList :todos="todos" :checkTodo="checkTodo" :deleteTodo="deleteTodo"/>
            <MyFooter :todos="todos" :checkAllTodo="checkAllTodo" :clearAllTodo="clearAllTodo"/>
            </div>
        </div>
    </div>
</template>

<script>
    import MyHeader from './components/MyHeader.vue'
    import MyList from './components/MyList.vue'
    import MyFooter from './components/MyFooter.vue'

    export default {
        name:'App',
        components: { MyHeader,MyList,MyFooter },
        data() {
            return {
                todos:[
                    {id:'001',title:'抽烟',done:false},
                    {id:'002',title:'喝酒',done:false},
                    {id:'003',title:'烫头',done:false},
                ]
            }
        },
        methods:{
            //添加一个todo
            addTodo(todoObj){
                this.todos.unshift(todoObj)
            },
            //勾选or取消勾选一个todo
            checkTodo(id){
                this.todos.forEach((todo)=>{
                    if(todo.id === id) todo.done = !todo.done
                })
            },
            //删除一个todo
            deleteTodo(id){
                this.todos = this.todos.filter(todo => todo.id !== id)
            },
            //全选or取消勾选
            checkAllTodo(done){
                this.todos.forEach(todo => todo.done = done)
            },
            //删除已完成的todo
            clearAllTodo(){
                this.todos = this.todos.filter(todo => !todo.done)
            }
        }
    }
</script>

<style>
    body {
    	background: #fff;
    }

    .btn {
        display: inline-block;
        padding: 4px 12px;
        margin-bottom: 0;
        font-size: 14px;
        line-height: 20px;
        text-align: center;
        vertical-align: middle;
        cursor: pointer;
        box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
        border-radius: 4px;
    }

    .btn-danger {
        color: #fff;
        background-color: #da4f49;
        border: 1px solid #bd362f;
    }

    .btn-danger:hover {
        color: #fff;
        background-color: #bd362f;
    }

    .btn:focus {
    	outline: none;
    }

    .todo-container {
        width: 600px;
        margin: 0 auto;
    }
    .todo-container .todo-wrap {
        padding: 10px;
        border: 1px solid #ddd;
        border-radius: 5px;
    }
</style>
```

- 组件化编码流程：
    1. 拆分静态组件：组件要按照功能点拆分，命名不要与html元素冲突
    2. 实现动态组件：考虑好数据的存放位置，数据是一个组件在用，还是一些组件在用：
        1. 一个组件在用：放在组件自身即可
        2. 一些组件在用：放在他们共同的父组件上（**状态提升**）
    3. 实现交互：从绑定事件开始
- `**props`适用于：**
    1. **父组件 ==> 子组件 通信 （父传子）**
    2. **子组件 ==> 父组件 通信（ 子传父）要求父组件通过props先给子组件一个函数 子组件调用函数通过参数给父组件传参数 类似于jsonp**
- **使用`v-model`时要切记：`v-model`绑定的值不能是`props`传过来的值，因为`props`是不可以修改的**
- `props`传过来的若是对象类型的值，修改对象中的属性时Vue不会报错，但不推荐这样做

### 自己的思考（案例心得）：

- 单选 与 全部勾选 总结
    
    footer添加一个isAll的**计算属性**（Boolean），用于判断是不是所有的checkbox都勾选上了，用v-model与全部勾选框绑定 计算属性用对象来写 因为存在isAll被修改的情况（全勾→非全勾、非全勾→全勾）get需要判断是否所有checkbox都勾选上了 set获取当前是否所有checkbox都勾选（即当前isAll的值 /get的返回值） 然后通过父组件传过来的函数把该值通过函数的参数传给父组件（因为调用set就是isAll改变的时候）
    
- **子传父**
    
    **要求父组件通过props先给子组件一个函数 子组件调用函数通过参数给父组件传参数 类似于jsonp**
    

### ****WebStorage****

1. 存储内容大小一般支持5MB左右（不同浏览器可能还不一样）
2. 浏览器端通过`Window.sessionStorage`和`Window.localStorage`属性来实现本地存储机制
3. 相关API：
    1. `xxxStorage.getItem('key')`：该方法接受一个键名作为参数，返回键名对应的值
    2. `xxxStorage.setItem('key', 'value')`：该方法接受一个键和值作为参数，会把键值对添加到存储中，如果键名存在，则更新其对应的值
    3. `xxxStorage.removeItem('key')`：该方法接受一个键名作为参数，并把该键名从存储中删除
    4. `xxxStorage.clear()`：该方法会清空存储中的所有数据
4. 备注：
    1. `LocalStorage`存储的内容，需要手动清除才会消失
    2. `SessionStorage`存储的内容会随着浏览器窗口关闭而消失
    3. `xxxStorage.getItem(xxx)`如果 xxx 对应的 value 获取不到，那么`getItem()`的返回值是null
    4. `JSON.parse(null)`的结果依然是null

### **使用本地存储优化Todo-List：**

`src/App.vue`:

```jsx
<template>
    <div id="root">
        <div class="todo-container">
            <div class="todo-wrap">
            <MyHeader :addTodo="addTodo"/>
            <MyList :todos="todos" :checkTodo="checkTodo" :deleteTodo="deleteTodo"/>
            <MyFooter :todos="todos" :checkAllTodo="checkAllTodo" :clearAllTodo="clearAllTodo"/>
            </div>
        </div>
    </div>
</template>

<script>
    import MyHeader from './components/MyHeader.vue'
    import MyList from './components/MyList.vue'
    import MyFooter from './components/MyFooter.vue'

    export default {
        name:'App',
        components: { MyHeader,MyList,MyFooter },
        data() {
            return {
                //若localStorage中存有'todos'则从localStorage中取出，否则初始为空数组
                todos:JSON.parse(localStorage.getItem('todos')) || []
            }
        },
        methods:{
            //添加一个todo
            addTodo(todoObj){
                this.todos.unshift(todoObj)
            },
            //勾选or取消勾选一个todo
            checkTodo(id){
                this.todos.forEach((todo)=>{
                    if(todo.id === id) todo.done = !todo.done
                })
            },
            //删除一个todo
            deleteTodo(id){
                this.todos = this.todos.filter(todo => todo.id !== id)
            },
            //全选or取消勾选
            checkAllTodo(done){
                this.todos.forEach(todo => todo.done = done)
            },
            //删除已完成的todo
            clearAllTodo(){
                this.todos = this.todos.filter(todo => !todo.done)
            }
        },
        watch:{
            todos:{
                //由于todos是对象数组，所以必须开启深度监视才能发现数组中对象的变化
                deep:true,
                handler(value){
                    localStorage.setItem('todos',JSON.stringify(value))
                }
            }
        }
    }
</script>

<style>
    body {
    	background: #fff;
    }

    .btn {
        display: inline-block;
        padding: 4px 12px;
        margin-bottom: 0;
        font-size: 14px;
        line-height: 20px;
        text-align: center;
        vertical-align: middle;
        cursor: pointer;
        box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
        border-radius: 4px;
    }

    .btn-danger {
        color: #fff;
        background-color: #da4f49;
        border: 1px solid #bd362f;
    }

    .btn-danger:hover {
        color: #fff;
        background-color: #bd362f;
    }

    .btn:focus {
    	outline: none;
    }

    .todo-container {
        width: 600px;
        margin: 0 auto;
    }
    .todo-container .todo-wrap {
        padding: 10px;
        border: 1px solid #ddd;
        border-radius: 5px;
    }
</style>
```

### ****自定义事件****

****绑定:****

`src/App.vue`:

```jsx
<template>
    <div class="app">
        <**!-- 通过父组件给子组件传递函数类型的props实现子给父传递数据 -->**
        <School :getSchoolName="getSchoolName"/>

   <!-- **通过父组件给子组件绑定一个自定义事件实现子给父传递数据（第一种写法，使用@或v-on）** -->
   <!-- <Student @jojo="getStudentName"/> -->

      <!-- **通过父组件给子组件绑定一个自定义事件实现子给父传递数据（第二种写法，使用ref）** -->
		<Student ref="student"/>
    </div>
</template>

<script>
    import Student from './components/Student.vue'
    import School from './components/School.vue'

    export default {
        name:'App',
        components: { Student,School },
        methods:{
            getSchoolName(name){
                console.log("已收到学校的名称："+name)
            },
            getStudentName(name){
                console.log("已收到学生的姓名："+name)      
            }
        },
        mounted(){
            this.$refs.student.$on('jojo',this.getStudentName)
        }
    }
</script>

<style scoped>
	.app{
		background-color: gray;
		padding: 5px;
	}
</style>
```

`src/components/Student.vue`:

```jsx
<template>
    <div class="student">
        <h2>学生姓名：{{name}}</h2>
        <h2>学生性别：{{sex}}</h2>
        <button @click="sendStudentName">点我传递学生姓名</button> 
    </div>
</template>

<script>
    export default {
        name:'Student',
        data() {
            return {
                name:'JOJO',
				sex:'男'
            }
        },
        methods:{
            sendStudentName(){
                this.$emit('jojo',this.name)
            }
        }
    }
</script>

<style scoped>
    .student{
        background-color: chartreuse;
        padding: 5px;
		margin-top: 30px;
    }
</style>
```

****解绑:****

`src/App.vue`:

```jsx
<template>
    <div class="app">
        <Student @jojo="getStudentName"/>
    </div>
</template>

<script>
    import Student from './components/Student.vue'

    export default {
        name:'App',
        components: { Student },
        methods:{
            getStudentName(name){
                console.log("已收到学生的姓名："+name)      
            }
        }
    }
</script>

<style scoped>
	.app{
		background-color: gray;
		padding: 5px;
	}
</style>
```

`src/components/Student.vue`:

```jsx
<template>
    <div class="student">
        <h2>学生姓名：{{name}}</h2>
        <h2>学生性别：{{sex}}</h2>
        <button @click="sendStudentName">点我传递学生姓名</button> 
        <button @click="unbind">解绑自定义事件</button> 
    </div>
</template>

<script>
    export default {
        name:'Student',
        data() {
            return {
                name:'JOJO',
				sex:'男'
            }
        },
        methods:{
            sendStudentName(){
                this.$emit('jojo',this.name)
            },
            unbind(){
                **// 解绑一个自定义事件
                // this.$off('jojo')
                // 解绑多个自定义事件
                // this.$off(['jojo'])
                // 解绑所有自定义事件
                this.$off()**
            }
        }
    }
</script>

<style scoped>
    .student{
        background-color: chartreuse;
        padding: 5px;
		margin-top: 30px;
    }
</style>
```

组件的自定义事件：

1. 一种组件间通信的方式，适用于：**子组件 ==> 父组件（子传父）**
2. 使用场景：A是父组件，B是子组件，B想给A传数据，那么就要在A中给B绑定自定义事件**（事件的回调在A中）A给B绑定自定义事件 B触发事件**
3.  **绑定自定义事件：**
    1. 第一种方式，在父组件中：`<Demo @atguigu="test"/>` 或 `<Demo v-on:atguigu="test"/>`
    2. 第二种方式，在父组件中：
        
        ```jsx
        <Demo ref="demo"/>
        ...
        mounted(){
            this.$refs.demo.$on('atguigu',data)
        }
        ```
        
    
     c. 若想让自定义事件只能触发一次，可以使用`once`修饰符，或`$once`方法
    
4. **触发自定义事件**：`this.$emit('atguigu',数据)`
5. **解绑自定义事件**：`this.$off('atguigu')`
6. 组件上也可以绑定原生DOM事件**(不加native会默认为自定义事件)**，需要使用`native`修饰符
7. 注意：通过`this.$refs.xxx.$on('atguigu',回调)`绑定自定义事件时，**回调要么配置在methods中，要么用箭头函数，否则this指向会出问题！**

### **使用自定义事件优化Todo-List：**

`src/App.vue`:

```jsx
<template>
    <div id="root">
        <div class="todo-container">
            <div class="todo-wrap">
            <MyHeader @addTodo="addTodo"/>
            <MyList :todos="todos" :checkTodo="checkTodo" :deleteTodo="deleteTodo"/>
            <MyFooter :todos="todos" @checkAllTodo="checkAllTodo" @clearAllTodo="clearAllTodo"/>
            </div>
        </div>
    </div>
</template>

<script>
    import MyHeader from './components/MyHeader.vue'
    import MyList from './components/MyList.vue'
    import MyFooter from './components/MyFooter.vue'

    export default {
        name:'App',
        components: { MyHeader,MyList,MyFooter },
        data() {
            return {
                todos:JSON.parse(localStorage.getItem('todos')) || []
            }
        },
        methods:{
            //添加一个todo
            addTodo(todoObj){
                this.todos.unshift(todoObj)
            },
            //勾选or取消勾选一个todo
            checkTodo(id){
                this.todos.forEach((todo)=>{
                    if(todo.id === id) todo.done = !todo.done
                })
            },
            //删除一个todo
            deleteTodo(id){
                this.todos = this.todos.filter(todo => todo.id !== id)
            },
            //全选or取消勾选
            checkAllTodo(done){
                this.todos.forEach(todo => todo.done = done)
            },
            //删除已完成的todo
            clearAllTodo(){
                this.todos = this.todos.filter(todo => !todo.done)
            }
        },
        watch:{
            todos:{
                deep:true,
                handler(value){
                    localStorage.setItem('todos',JSON.stringify(value))
                }
            }
        }
    }
</script>

<style>
    body {
    	background: #fff;
    }

    .btn {
        display: inline-block;
        padding: 4px 12px;
        margin-bottom: 0;
        font-size: 14px;
        line-height: 20px;
        text-align: center;
        vertical-align: middle;
        cursor: pointer;
        box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
        border-radius: 4px;
    }

    .btn-danger {
        color: #fff;
        background-color: #da4f49;
        border: 1px solid #bd362f;
    }

    .btn-danger:hover {
        color: #fff;
        background-color: #bd362f;
    }

    .btn:focus {
   		outline: none;
    }

    .todo-container {
    	width: 600px;
    	margin: 0 auto;
    }
    .todo-container .todo-wrap {
        padding: 10px;
        border: 1px solid #ddd;
        border-radius: 5px;
    }
</style>
```

`src/components/MyHeader.vue`:

```jsx
<template>
    <div class="todo-header">
        <input type="text" placeholder="请输入你的任务名称，按回车键确认" @keydown.enter="add" v-model="title"/>
    </div>
</template>

<script>
    import {nanoid} from 'nanoid'
    export default {
        name:'MyHeader',
        data() {
            return {
                title:''
            }
        },
        methods:{
            add(){
                if(!this.title.trim()) return
                const todoObj = {id:nanoid(),title:this.title,done:false}
                this.$emit('addTodo',todoObj)
                this.title = ''
            }
        }
    }
</script>

<style scoped>
    /*header*/
    .todo-header input {
        width: 560px;
        height: 28px;
        font-size: 14px;
        border: 1px solid #ccc;
        border-radius: 4px;
        padding: 4px 7px;
    }

    .todo-header input:focus {
        outline: none;
        border-color: rgba(82, 168, 236, 0.8);
        box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 8px rgba(82, 168, 236, 0.6);
    }
</style>
```

`src/components/MyFooter`:

```jsx
<template>
    <div class="todo-footer" v-show="total">
        <label>
            <input type="checkbox" v-model="isAll"/>
        </label>
        <span>
            <span>已完成{{doneTotal}}</span> / 全部{{total}}
        </span>
        <button class="btn btn-danger" @click="clearAll">清除已完成任务</button>
    </div>
</template>

<script>
    export default {
        name:'MyFooter',
        props:['todos'],
        computed:{
            doneTotal(){
                return this.todos.reduce((pre,todo)=> pre + (todo.done ? 1 : 0) ,0)
            },
            total(){
                return this.todos.length
            },
            isAll:{
                get(){
                    return this.total === this.doneTotal && this.total > 0
                },
                set(value){
                    this.$emit('checkAllTodo',value)
                }
            }
        },
        methods:{
            clearAll(){
                this.$emit('clearAllTodo')
            }
        }
    }
</script>

<style scoped>
    .todo-footer {
        height: 40px;
        line-height: 40px;
        padding-left: 6px;
        margin-top: 5px;
        }

    .todo-footer label {
        display: inline-block;
        margin-right: 20px;
        cursor: pointer;
    }

    .todo-footer label input {
        position: relative;
        top: -1px;
        vertical-align: middle;
        margin-right: 5px;
    }

    .todo-footer button {
        float: right;
        margin-top: 5px;
    }
</style>
```

### ****全局事件总线****

全局事件总线是一种可以在任意组件间通信的方式，本质上就是一个对象。

它必须满足以下条件：

1. 所有的组件对象都必须能看见他 

2. 这个对象必须能够使用`$on`、`$emit`和`$off`方法去绑定、触发和解绑事件

`src/main.js`:

```jsx
import Vue from 'vue'
import App from './App.vue'

Vue.config.productionTip = false

new Vue({
  render: h => h(App),
  beforeCreate(){
    Vue.prototype.$bus=this
  }
}).$mount('#app')
```

`src/App.vue`:

```jsx
<template>
  <div>
    <hr>
    <h2>app</h2>
    <School/>
    <Student/>
    <hr>
  </div>
</template>

<script>
    import Student from './components/Student.vue'
    import School from './components/School.vue'
    export default {
        name:'app',
        components:{
            School,
            Student
        },
         methods:{
          demo(){
            console.log('1111')
          }
        }
    }
</script>
```

`src/components/School.vue`:

```jsx
<template>
  <div>
    <h1>welcome to {{sname}}</h1>
    
  </div>
</template>

<script>
    export default {
        name:'School-1',
        data() {
            return {
                sname:'wcyu'
            }
        },
        mounted(){
          this.$bus.$on('hello',(name)=>{
            console.log('i am school component,i receive your name:',name)
          })
          
        },
        beforeDestroy() {
			this.$bus.$off('hello')
		},
    }
</script>

<style>

</style>
```

`src/components/Student.vue`:

```jsx
<template>
  <div>
    <h2>{{name}}</h2>
    <button @click="sendNameToSchool">把学生名传递到school组件</button>
  </div>
</template>

<script>
    export default {
        name:'Student-1',
        data() {
            return {
                name:'tam'
            }
        },
        methods:{
          sendNameToSchool(){
            this.$bus.$emit('hello',this.name)
          }
        }
        
    }
</script>

<style>

</style>
```

全局事件总线（GlobalEventBus）：

1. 一种组件间通信的方式，适用于任意组件间通信
2. 安装全局事件总线：
    
    ```jsx
    new Vue({
       	...
       	beforeCreate() {
       		Vue.prototype.$bus = this //安装全局事件总线，$bus就是当前应用的vm
       	},
        ...
    })
    ```
    
    **（自己总结）**this是对象vm 使用component都能通过原型链访问到Vue.prototype 所以在其身上设置一个对象$bus 让所有组件都能通过其完成组件间传数据
    
3. 使用事件总线：
    1. **接收数据：A组件想接收数据，则在A组件中给`$bus`绑定自定义事件，事件的回调留在A组件自身**
    
    ```jsx
    export default {
    1.
        methods(){
            demo(data){...}
        }
        ...
        mounted() {
            this.$bus.$on('xxx',this.demo)
        }
    2.
    mounted() {
            this.$bus.$on('xxx',()=>{}) //要用箭头函数
        }
    }
    ```
    
    b. 提供数据：`this.$bus.$emit('xxx',data)`
    
4.  最好在`beforeDestroy`钩子中，用`$off`去解绑当前组件所用到的事件

### **使用自定义事件优化Todo-List：**

`src/mian.js`:

```jsx
import Vue from 'vue'
import App from './App.vue'

Vue.config.productionTip = false

new Vue({
    el:"#app",
    render: h => h(App),
    beforeCreate() {
        Vue.prototype.$bus = this
    }
})
```

`src/components/App.vue`

```jsx
<template>
    <div id="root">
        <div class="todo-container">
            <div class="todo-wrap">
            <MyHeader @addTodo="addTodo"/>
            <MyList :todos="todos"/>
            <MyFooter :todos="todos" @checkAllTodo="checkAllTodo" @clearAllTodo="clearAllTodo"/>
            </div>
        </div>
    </div>
</template>

<script>
    import MyHeader from './components/MyHeader.vue'
    import MyList from './components/MyList.vue'
    import MyFooter from './components/MyFooter.vue'

    export default {
        name:'App',
        components: { MyHeader,MyList,MyFooter },
        data() {
            return {
                todos:JSON.parse(localStorage.getItem('todos')) || []
            }
        },
        methods:{
            //添加一个todo
            addTodo(todoObj){
                this.todos.unshift(todoObj)
            },
            //勾选or取消勾选一个todo
            checkTodo(id){
                this.todos.forEach((todo)=>{
                    if(todo.id === id) todo.done = !todo.done
                })
            },
            //删除一个todo
            deleteTodo(id){
                this.todos = this.todos.filter(todo => todo.id !== id)
            },
            //全选or取消勾选
            checkAllTodo(done){
                this.todos.forEach(todo => todo.done = done)
            },
            //删除已完成的todo
            clearAllTodo(){
                this.todos = this.todos.filter(todo => !todo.done)
            }
        },
        watch:{
            todos:{
                deep:true,
                handler(value){
                    localStorage.setItem('todos',JSON.stringify(value))
                }
            }
        },
        mounted(){
            this.$bus.$on('checkTodo',this.checkTodo)
            this.$bus.$on('deleteTodo',this.deleteTodo)
        },
        beforeDestroy(){
            this.$bus.$off(['checkTodo','deleteTodo'])
        }
    }
</script>

<style>
    body {
        background: #fff;
    }

    .btn {
        display: inline-block;
        padding: 4px 12px;
        margin-bottom: 0;
        font-size: 14px;
        line-height: 20px;
        text-align: center;
        vertical-align: middle;
        cursor: pointer;
        box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
        border-radius: 4px;
    }

    .btn-danger {
        color: #fff;
        background-color: #da4f49;
        border: 1px solid #bd362f;
    }

    .btn-danger:hover {
        color: #fff;
        background-color: #bd362f;
    }

    .btn:focus {
        outline: none;
    }

    .todo-container {
        width: 600px;
        margin: 0 auto;
    }
    .todo-container .todo-wrap {
        padding: 10px;
        border: 1px solid #ddd;
        border-radius: 5px;
    }
</style>
```

`src/components/MyItem.vue`:

```jsx
<template>
    <li>
        <label>
            <input type="checkbox" :checked="todo.done" @click="handleCheck(todo.id)"/>
            <span>{{todo.title}}</span>
        </label>
        <button class="btn btn-danger" @click="handleDelete(todo.id,todo.title)">删除</button>
    </li>
</template>

<script>
    export default {
        name:'MyItem',
        props:['todo'],
        methods:{
            handleCheck(id){
                this.$bus.$emit('checkTodo',id)
            },
            handleDelete(id,title){
                if(confirm("确定删除任务："+title+"吗？")){
                    this.$bus.$emit('deleteTodo',id)
                }
            }
        }
    }
</script>

<style scoped>
    li {
        list-style: none;
        height: 36px;
        line-height: 36px;
        padding: 0 5px;
        border-bottom: 1px solid #ddd;
    }

    li label {
        float: left;
        cursor: pointer;
    }

    li label li input {
        vertical-align: middle;
        margin-right: 6px;
        position: relative;
        top: -1px;
    }

    li button {
        float: right;
        display: none;
        margin-top: 3px;
    }

    li:before {
        content: initial;
    }

    li:last-child {
        border-bottom: none;
    }

    li:hover {
        background-color: #eee;
    }

    li:hover button{
        display: block;
    }
</style>
```

### ****消息的订阅与发布****

[https://github.com/mroderick/PubSubJS](https://github.com/mroderick/PubSubJS)

`src/components/School.vue`:

```jsx
<template>
	<div class="school">
		<h2>学校名称：{{name}}</h2>
		<h2>学校地址：{{address}}</h2>
	</div>
</template>

<script>
	import pubsub from 'pubsub-js'

	export default {
		name:'School',
		data() {
			return {
				name:'尚硅谷',
				address:'北京',
			}
		},
		methods:{
			demo(msgName,data) {
				console.log('我是School组件，收到了数据：',data)
			}
		},
		mounted() {
			this.pubId = pubsub.subscribe('demo',this.demo) //订阅消息
		},
		beforeDestroy() {
			pubsub.unsubscribe(this.pubId) //取消订阅
		}
	}
</script>

<style scoped>
	.school{
		background-color: skyblue;
		padding: 5px;
	}
</style>
```

`src/components/Student.vue`:

```jsx
<template>
	<div class="student">
		<h2>学生姓名：{{name}}</h2>
		<h2>学生性别：{{sex}}</h2>
		<button @click="sendStudentName">把学生名给School组件</button>
	</div>
</template>

<script>
	import pubsub from 'pubsub-js'

	export default {
		name:'Student',
		data() {
			return {
				name:'JOJO',
				sex:'男',
			}
		},
		methods: {
			sendStudentName(){
				pubsub.publish('demo',this.name) //发布消息
			}
		}
	}
</script>

<style scoped>
	.student{
		background-color: pink;
		padding: 5px;
		margin-top: 30px;
	}
</style>
```

消息订阅与发布（pubsub）：

1. 消息订阅与发布是一种组件间通信的方式，适用于任意组件间通信
2.  使用步骤：
    1. 安装pubsub：`npm i pubsub-js`
    2. 引入：`import pubsub from 'pubsub-js'`
    3. 接收数据：**A组件想接收数据，则在A组件中订阅消息，订阅的回调留在A组件自身**
        
        ```jsx
        export default {
            methods(){
                demo(data){...}
            }
            ...
            mounted() {
        		this.pid = pubsub.subscribe('xxx',this.demo)
            }
        }
        ```
        
3. 提供数据：`pubsub.publish('xxx',data)`
4. 最好在`beforeDestroy`钩子中，使用`pubsub.unsubscribe(pid)`取消订阅

### **使用消息的订阅与发布优化Todo-List：**

`src/App.vue`:

```jsx
<template>
    <div id="root">
        <div class="todo-container">
            <div class="todo-wrap">
            <MyHeader @addTodo="addTodo"/>
            <MyList :todos="todos"/>
            <MyFooter :todos="todos" @checkAllTodo="checkAllTodo" @clearAllTodo="clearAllTodo"/>
            </div>
        </div>
    </div>
</template>

<script>
    import pubsub from 'pubsub-js'
    import MyHeader from './components/MyHeader.vue'
    import MyList from './components/MyList.vue'
    import MyFooter from './components/MyFooter.vue'

    export default {
        name:'App',
        components: { MyHeader,MyList,MyFooter },
        data() {
            return {
                todos:JSON.parse(localStorage.getItem('todos')) || []
            }
        },
        methods:{
            //添加一个todo
            addTodo(todoObj){
                this.todos.unshift(todoObj)
            },
            //勾选or取消勾选一个todo
            checkTodo(_,id){
                this.todos.forEach((todo)=>{
                    if(todo.id === id) todo.done = !todo.done
                })
            },
            //删除一个todo
            deleteTodo(id){
                this.todos = this.todos.filter(todo => todo.id !== id)
            },
            //全选or取消勾选
            checkAllTodo(done){
                this.todos.forEach(todo => todo.done = done)
            },
            //删除已完成的todo
            clearAllTodo(){
                this.todos = this.todos.filter(todo => !todo.done)
            }
        },
        watch:{
            todos:{
                deep:true,
                handler(value){
                    localStorage.setItem('todos',JSON.stringify(value))
                }
            }
        },
        mounted(){
            this.pubId = pubsub.subscribe('checkTodo',this.checkTodo)
            this.$bus.$on('deleteTodo',this.deleteTodo)
        },
        beforeDestroy(){
            pubsub.unsubscribe(this.pubId)
            this.$bus.$off('deleteTodo')
        }
    }
</script>

<style>
    body {
        background: #fff;
    }

    .btn {
        display: inline-block;
        padding: 4px 12px;
        margin-bottom: 0;
        font-size: 14px;
        line-height: 20px;
        text-align: center;
        vertical-align: middle;
        cursor: pointer;
        box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
        border-radius: 4px;
    }

    .btn-danger {
        color: #fff;
        background-color: #da4f49;
        border: 1px solid #bd362f;
    }

    .btn-danger:hover {
        color: #fff;
        background-color: #bd362f;
    }

    .btn:focus {
        outline: none;
    }

    .todo-container {
        width: 600px;
        margin: 0 auto;
    }
    .todo-container .todo-wrap {
        padding: 10px;
        border: 1px solid #ddd;
        border-radius: 5px;
    }
</style>
```

`src/components/myItem.vue`:

```jsx
<template>
    <li>
        <label>
            <input type="checkbox" :checked="todo.done" @click="handleCheck(todo.id)"/>
            <span>{{todo.title}}</span>
        </label>
        <button class="btn btn-danger" @click="handleDelete(todo.id,todo.title)">删除</button>
    </li>
</template>

<script>
    import pubsub from 'pubsub-js'
    export default {
        name:'MyItem',
        props:['todo'],
        methods:{
            handleCheck(id){                    
                pubsub.publish('checkTodo',id)
            },
            handleDelete(id,title){
                if(confirm("确定删除任务："+title+"吗？")){
                    this.$bus.$emit('deleteTodo',id)
                }
            }
        }
    }
</script>

<style scoped>
    li {
        list-style: none;
        height: 36px;
        line-height: 36px;
        padding: 0 5px;
        border-bottom: 1px solid #ddd;
    }

    li label {
        float: left;
        cursor: pointer;
    }

    li label li input {
        vertical-align: middle;
        margin-right: 6px;
        position: relative;
        top: -1px;
    }

    li button {
        float: right;
        display: none;
        margin-top: 3px;
    }

    li:before {
        content: initial;
    }

    li:last-child {
        border-bottom: none;
    }

    li:hover {
        background-color: #eee;
    }

    li:hover button{
        display: block;
    }
</style>
```

### 编辑 edit 学习有感

- 为什么输入的内容的input框不能够用v-model绑定
    
    因为todos是在app.vue文件里 通过props传过来的 一般通过props传过来的数据都不建议修改 所有有全局数据总线调用函数修改todos的title
    
- 实现细节
    - 按钮原来设置亮色 hover后颜色变暗色调
    - 点击edit按钮后 this.$set响应式设置新值isEdit 通过v-show判定isEdit的值将input框、文本、edit按钮显示、隐藏
    - 失去焦点事件将title数据update
    - **点编辑按钮 input框自动获取focus 通过ref获取input标签 因为focus事件是在updated钩子之前执行的 所有需要通过nextTick/或者设置延时器 去设置input输入框focus**
        
        ```jsx
        // this.$nextTick(function(){
                        //     this.$refs.inputTitle.focus()
                        // })
                        setTimeout(()=>{
                            this.$refs.inputTitle.focus()
                        })
        ```
        

### ****$nextTick****

`$nextTick(回调函数)`可以将回调延迟到下次 DOM 更新循环之后执行

**使用$nextTick优化Todo-List：**

`src/App.vue`:

```jsx
<template>
    <div id="root">
        <div class="todo-container">
            <div class="todo-wrap">
            <MyHeader @addTodo="addTodo"/>
            <MyList :todos="todos"/>
            <MyFooter :todos="todos" @checkAllTodo="checkAllTodo" @clearAllTodo="clearAllTodo"/>
            </div>
        </div>
    </div>
</template>

<script>
    import pubsub from 'pubsub-js'
    import MyHeader from './components/MyHeader.vue'
    import MyList from './components/MyList.vue'
    import MyFooter from './components/MyFooter.vue'

    export default {
        name:'App',
        components: { MyHeader,MyList,MyFooter },
        data() {
            return {
                todos:JSON.parse(localStorage.getItem('todos')) || []
            }
        },
        methods:{
            //添加一个todo
            addTodo(todoObj){
                this.todos.unshift(todoObj)
            },
            //勾选or取消勾选一个todo
            checkTodo(_,id){
                this.todos.forEach((todo)=>{
                    if(todo.id === id) todo.done = !todo.done
                })
            },
            //删除一个todo
            deleteTodo(id){
                this.todos = this.todos.filter(todo => todo.id !== id)
            },
            //更新一个todo
			updateTodo(id,title){
				this.todos.forEach((todo)=>{
					if(todo.id === id) todo.title = title
				})
			},
            //全选or取消勾选
            checkAllTodo(done){
                this.todos.forEach(todo => todo.done = done)
            },
            //删除已完成的todo
            clearAllTodo(){
                this.todos = this.todos.filter(todo => !todo.done)
            }
        },
        watch:{
            todos:{
                deep:true,
                handler(value){
                    localStorage.setItem('todos',JSON.stringify(value))
                }
            }
        },
        mounted(){
            this.pubId = pubsub.subscribe('checkTodo',this.checkTodo)
            this.$bus.$on('deleteTodo',this.deleteTodo)
            this.$bus.$on('updateTodo',this.updateTodo)
        },
        beforeDestroy(){
            pubsub.unsubscribe(this.pubId)
            this.$bus.$off('deleteTodo')
            this.$bus.$off('updateTodo')
        }
    }
</script>

<style>
    body {
        background: #fff;
    }

    .btn {
        display: inline-block;
        padding: 4px 12px;
        margin-bottom: 0;
        font-size: 14px;
        line-height: 20px;
        text-align: center;
        vertical-align: middle;
        cursor: pointer;
        box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
        border-radius: 4px;
    }

    .btn-danger {
        color: #fff;
        background-color: #e04e49;
        border: 1px solid #bd362f;
    }

    .btn-danger:hover {
        color: #fff;
        background-color: #bd362f;
    }

    .btn-info {
        color: #fff;
        background-color: rgb(50, 129, 233);
        border: 1px solid rgb(1, 47, 212);
        margin-right: 5px;
    }

    .btn-info:hover {
        color: #fff;
        background-color: rgb(1, 47, 212);
    }

    .btn:focus {
        outline: none;
    }

    .todo-container {
        width: 600px;
        margin: 0 auto;
    }
    .todo-container .todo-wrap {
        padding: 10px;
        border: 1px solid #ddd;
        border-radius: 5px;
    }
</style>
```

`src/components/MyItem.vue`:

```jsx
<template>
    <li>
        <label>
            <input type="checkbox" :checked="todo.done" @click="handleCheck(todo.id)"/>
            <span v-show="!todo.isEdit">{{todo.title}}</span>
            <input type="text" v-show="todo.isEdit" :value="todo.title" @blur="handleBlur(todo,$event)" ref="inputTitle">
        </label>
        <button class="btn btn-danger" @click="handleDelete(todo.id,todo.title)">删除</button>
        <button class="btn btn-info" v-show="!todo.isEdit" @click="handleEdit(todo)">编辑</button>
    </li>
</template>

<script>
    import pubsub from 'pubsub-js'
    export default {
        name:'MyItem',
        props:['todo'],
        methods:{
            handleCheck(id){                    
                pubsub.publish('checkTodo',id)
            },
            handleDelete(id,title){
                if(confirm("确定删除任务："+title+"吗？")){
                    this.$bus.$emit('deleteTodo',id)
                }
            },
            handleEdit(todo){
                // 如果todo自身有isEdit属性就将isEdit改成true
				if(Object.prototype.hasOwnProperty.call(todo,'isEdit')){
					todo.isEdit = true
				}else{
                    // 如果没有就向todo中添加一个响应式的isEdit属性并设为true
					this.$set(todo,'isEdit',true)
				}
                // 当Vue重新编译模板之后执行$nextTick()中的回调函数
                this.$nextTick(function(){
                    // 使input框获取焦点
                    this.$refs.inputTitle.focus()
                })
			},
            // 当input框失去焦点时更新
            handleBlur(todo,event){
                todo.isEdit = false
				if(!event.target.value.trim()) return alert('输入不能为空！')
				this.$bus.$emit('updateTodo',todo.id,event.target.value)
            }
        }
    }
</script>

<style scoped>
    li {
        list-style: none;
        height: 36px;
        line-height: 36px;
        padding: 0 5px;
        border-bottom: 1px solid #ddd;
    }

    li label {
        float: left;
        cursor: pointer;
    }

    li label li input {
        vertical-align: middle;
        margin-right: 6px;
        position: relative;
        top: -1px;
    }

    li button {
        float: right;
        display: none;
        margin-top: 3px;
    }

    li:before {
        content: initial;
    }

    li:last-child {
        border-bottom: none;
    }

    li:hover {
        background-color: #eee;
    }

    li:hover button{
        display: block;
    }
</style>
```

$nextTick：

1. 语法：`this.$nextTick(回调函数)`
2. 作用：在下一次 DOM 更新结束后执行其指定的回调
3. 什么时候用：当改变数据后，要基于更新后的新DOM进行某些操作时，要在nextTick所指定的回调函数中执行

### ****过渡与动画****

Vue封装的过度与动画：

1. 作用：**在插入、更新或移除 DOM元素时，在合适的时候给元素添加样式类名**
2. 写法：
    1. 准备好样式：
        - 元素进入的样式：
            1. `v-enter`：进入的起点
            2. `v-enter-active`：进入过程中
            3. `v-enter-to`：进入的终点
        - 元素离开的样式：
            1. `v-leave`：离开的起点
            2. `v-leave-active`：离开过程中
            3. `v-leave-to`：离开的终点
    2. 使用`<transition>`包裹要过度的元素，并配置name属性：
        
        ```jsx
        <transition name="hello">
        	<h1 v-show="isShow">你好啊！</h1>
        </transition>
        ```
        
3. 备注：若有多个元素需要过度，则需要使用：`<transition-group>`，且每个元素都要指定`key`值
4. `<transition appear>` 让进网站、刷新就执行动画

手写css动画：

```jsx
<template>
  <div>
    <button @click="isShow=!isShow">显示/隐藏</button>
    <transition appear>
    <h1 v-show="isShow" >你好啊！</h1>
    </transition>
    
  </div>
</template>

<script>
export default {
    name:'Test',
    data() {
        return {
            isShow:true
        }
    },

}
</script>

<style scoped>
h1{
    background-color: orange;
}
.v-enter-active{
    animation: atguigu 0.5s linear;
}
.v-leave-active{
    animation: atguigu 0.5s linear reverse;
}

@keyframes atguigu {
    from {
        transform: translateX(-100%);
    }
    to{
        transform: translateX(0px);
    }
}
</style>

```

transition-group：

```
<template>
  <div>
    <button @click="isShow=!isShow">显示/隐藏</button>
    <transition-group name="hello" appear>
    <h1 v-show="isShow" key="1">你好啊！</h1>
    <h1 v-show="!isShow" key="2" >我不好！</h1>
    </transition-group>
    
  </div>
</template>

<script>
export default {
    name:'Test',
    data() {
        return {
            isShow:true
        }
    },

}
</script>

<style scoped>
h1{
    background-color: orange;
    
}
.hello-enter-active,.hello-leave-active{
    transition:0.5s linear;
}

.hello-enter,.hello-leave-to{
    transform:translateX(-100%);
}
.hello-enter-to,.hello-leave{
    transform: translateX(0px);
    
}

</style>
```

使用animate.css

```
<template>
  <div>
    <button @click="isShow=!isShow">显示/隐藏</button>
    <transition-group 
     name="animate__animated animate__bounce"
     appear
     enter-active-class="animate__backInDown"
     leave-active-class="animate__backOutDown"
     >
    <h1 v-show="isShow" key="1">你好啊！</h1>
    <h1 v-show="!isShow" key="2" >我不好！</h1>
    </transition-group>
    
  </div>
</template>

<script>
import 'animate.css'
export default {
    name:'Test',
    data() {
        return {
            isShow:true
        }
    },

}
</script>

<style scoped>
h1{
    background-color: orange;
    
}

</style>
```

### 动画完善todolist

```
<template>
    <ul class="todo-main" >
        <transition-group
        name="animate__animated animate__bounce"
        enter-active-class="animate__backInDown"
        leave-active-class="animate__backOutDown"
        appear
        >
        <MyItem v-for="todoObj in todos" 
        :key="todoObj.id" 
        :todo="todoObj" 
        :checkTodo="checkTodo"
        :deleteTodo="deleteTodo"
        />
        </transition-group>
        
        
    </ul>
</template>
```

### 插件 animate.css

下载[animate.css - npm (npmjs.com)](https://www.npmjs.com/package/animate.css)

插件document：[Animate.css | A cross-browser library of CSS animations.](https://animate.style/)

```jsx
<template>
  <div>
    <button @click="isShow=!isShow">显示/隐藏</button>
    <transition-group 
     name="animate__animated animate__bounce"
     appear
     enter-active-class="animate__backInDown"
     leave-active-class="animate__backOutDown"
     >
    <h1 v-show="isShow" key="1">你好啊！</h1>
    <h1 v-show="!isShow" key="2" >我不好！</h1>
    </transition-group>
    
  </div>
</template>

<script>
import 'animate.css'
export default {
    name:'Test',
    data() {
        return {
            isShow:true
        }
    },

}
</script>

<style scoped>
h1{
    background-color: orange;
    
}

</style>
```