# 5.Vuex

### ****理解Vuex****

**Vuex是什么**

1. 概念：专门在 Vue 中实现集中式状态（数据）管理的一个 Vue 插件，对 vue 应用中多个组件的共享状态进行集中式的管理（读/写），也是一种组件间通信的方式，且适用于任意组件间通信

[https://github.com/vuejs/vuex](https://github.com/vuejs/vuex)

![Untitled](5%20Vuex%20441dff0e962a45d8b3598434a3030981/Untitled.png)

![Untitled](5%20Vuex%20441dff0e962a45d8b3598434a3030981/Untitled%201.png)

**什么时候使用Vuex**

1. 多个组件依赖于同一状态
2. 来自不同组件的行为需要变更同一状态

### ****Vuex工作原理图****

![Untitled](5%20Vuex%20441dff0e962a45d8b3598434a3030981/Untitled%202.png)

自己总结： ****

- **action中的context相对于一个小Store对象，里面有state dispatch commit**
- 如果业务逻辑简单 可以直接通过`this.$store.commit()`跳过action

### Vuex的基本使用：

1. 初始化数据`state`，配置`actions`、`mutations`，操作文件`store.js`

    

```jsx
//引入Vue核心库
import Vue from 'vue'
//引入Vuex
import Vuex from 'vuex'
//引用Vuex
Vue.use(Vuex)

const actions = {
    //响应组件中加的动作
	jia(context,value){
		// console.log('actions中的jia被调用了',miniStore,value)
		context.commit('JIA',value)
	},
}

const mutations = {
    //执行加
	JIA(state,value){
		// console.log('mutations中的JIA被调用了',state,value)
		state.sum += value
	}
}

//初始化数据
const state = {
   sum:0
}

//创建并暴露store
export default new Vuex.Store({
	actions,
	mutations,
	state,
})
```

1. 组件中读取vuex中的数据：`$store.state.sum`
2. 组件中修改vuex中的数据：`$store.dispatch('action中的方法名',数据）`或 `$store.commit('mutations中的方法名',数据)`

### ****求和案例 使用纯vue编写****

`src/App.vue`:

```jsx
<template>
	<div class="container">
		<Count/>
	</div>
</template>

<script>
	import Count from './components/Count'
	export default {
		name:'App',
		components:{Count}
	}
</script>
```

`src/components/Count.vue`:

```jsx
<template>
	<div>
		<h1>当前求和为：{{sum}}</h1>
		<select v-model.number="n">
			<option value="1">1</option>
			<option value="2">2</option>
			<option value="3">3</option>
		</select>
		<button @click="increment">+</button>
		<button @click="decrement">-</button>
		<button @click="incrementOdd">当前求和为奇数再加</button>
		<button @click="incrementWait">等一等再加</button>
	</div>
</template>

<script>
	export default {
		name:'Count',
		data() {
			return {
				n:1, //用户选择的数字
				sum:0 //当前的和
			}
		},
		methods: {
			increment(){
				this.sum += this.n
			},
			decrement(){
				this.sum -= this.n
			},
			incrementOdd(){
				if(this.sum % 2){
					this.sum += this.n
				}
			},
			incrementWait(){
				setTimeout(()=>{
					this.sum += this.n
				},500)
			},
		},
	}
</script>

<style>
	button{
		margin-left: 5px;
	}
</style>
```

****搭建Vuex环境****

1. 下载 Vuex：`npm i vuex`
2. 创建`src/store/index.js`：
    
    ```jsx
    import Vue from "vue"
    import Vuex from 'vuex'
    
    const actions={}
    const state={}
    const mutations={}
    
    Vue.use(Vuex)
    
    export default new Vuex.Store({
       actions,
       state,
       mutations
    })
    ```
    
3. 在`src/main.js`中创建 vm 时传入`store`配置项：
    
    ```jsx
    import Vue from 'vue'
    import App from './App.vue'
    import store from './store'
    
    Vue.config.productionTip = false
    
    new Vue({
        el:"#app",
        render: h => h(App),
        store,
        beforeCreate(){
            Vue.prototype.$bus = this
        },
        
    })
    
    ```
    

### 求和案例 ****使用Vuex编写****

`src/components/Count.vue`:

```jsx
<template>
  <div>
    <h3>sum:{{$store.state.sum}}</h3>
    <select v-model.number="n">
        <option value="1">1</option>
        <option value="2">2</option>
        <option value="3">3</option>
        <option value="4">4</option>
    </select>
    <button @click="increment">+</button>
    <button @click="decrement">-</button>
    <button @click="incrementOdd">当前求和为奇数再加</button>
    <button @click="incrementWait">等一等再加</button>
  </div>
</template>

<script>
export default {
    name:'CountOne',
    data() {
        return {
            n:1
        }
    },
    methods:{
        increment(){
            this.$store.commit('JIA',this.n)
        },
        decrement(){
            this.$store.commit('JIAN',this.n)
        },
        incrementOdd(){
           this.$store.dispatch('JIAODD',this.n)
        },
        incrementWait(){
           this.$store.dispatch('JIAWAIT',this.n)
        },

    }
}
</script>

<style>
button{
    margin-left: 5px;
}
</style>
```

`src/store/index.js`:

```jsx
import Vue from "vue"
import Vuex from 'vuex'

const actions={
    JIAODD(context,value){
        console.log('JIAODD被调用了');
        if (context.state.sum%2) {
            context.commit('JIA',value)
        }
        
    },
    JIAWAIT(context,value){
        console.log('JIAWAIT被调用了');
        setTimeout(() => {
            context.commit('JIA',value)
        }, 500);
    },
}
const state={
    sum:0,
    
}
const mutations={
    JIA(state,value){
        state.sum+=value
    },
    JIAN(state,value){
        state.sum-=value
    },
}

Vue.use(Vuex)

export default new Vuex.Store({
   actions,
   state,
   mutations
})
```

### ****getters配置项****

`src/Count.vue`:

```jsx
<template>
  <div>
    <h3>sum:{{$store.state.sum}}</h3>
    <h3>sum:{{$store.getters.bigNum}}</h3>
    <select v-model.number="n">
        <option value="1">1</option>
        <option value="2">2</option>
        <option value="3">3</option>
        <option value="4">4</option>
    </select>
    <button @click="increment">+</button>
    <button @click="decrement">-</button>
    <button @click="incrementOdd">当前求和为奇数再加</button>
    <button @click="incrementWait">等一等再加</button>
  </div>
</template>

<script>
export default {
    name:'CountOne',
    data() {
        return {
            n:1
        }
    },
    methods:{
        increment(){
            this.$store.commit('JIA',this.n)
        },
        decrement(){
            this.$store.commit('JIAN',this.n)
        },
        incrementOdd(){
           this.$store.dispatch('JIAODD',this.n)
        },
        incrementWait(){
           this.$store.dispatch('JIAWAIT',this.n)
        },

    }
}
</script>

<style>
button{
    border: 2px solid rgb(12, 203, 28);
    background-color: #fff;
    
    color: #16ed1d;
    width: 150px;
    height: 40px;
    margin-left: 5px;
}
</style>
```

`src/store/index.js`:

```jsx
import Vue from "vue"
import Vuex from 'vuex'

const actions={
    JIAODD(context,value){
        console.log('JIAODD被调用了');
        if (context.state.sum%2) {
            context.commit('JIA',value)
        }
        
    },
    JIAWAIT(context,value){
        console.log('JIAWAIT被调用了');
        setTimeout(() => {
            context.commit('JIA',value)
        }, 500);
    },
}
const state={
    sum:0,
    
}
const mutations={
    JIA(state,value){
        state.sum+=value
    },
    JIAN(state,value){
        state.sum-=value
    },
}
const getters={
    bigNum(state){
        return state.sum*10
    }
}

Vue.use(Vuex)

export default new Vuex.Store({
   actions,
   state,
   mutations,
   getters
})
```

`getters`配置项的使用：

1. 概念：当`state`中的数据需要经过加工后再使用时，可以使用`getters`加工
2. 在`store.js`中追加`getters`配置
    
    ```jsx
    ...
    const getters = {
    	bigSum(state){
    		return state.sum * 10
    	}
    }
    
    //创建并暴露store
    export default new Vuex.Store({
    	...
    	getters
    })
    ```
    
3. 组件中读取数据：`$store.getters.bigSum`

### ****四个map方法的使用****

****mapState与mapGetters****

`src/store/index.js`:

```jsx
import Vue from "vue"
import Vuex from 'vuex'

const actions={
    JIAODD(context,value){
        console.log('JIAODD被调用了');
        if (context.state.sum%2) {
            context.commit('JIA',value)
        }
        
    },
    JIAWAIT(context,value){
        console.log('JIAWAIT被调用了');
        setTimeout(() => {
            context.commit('JIA',value)
        }, 500);
    },
}
const state={
    sum:0,
    school:'wsyu',
    work:'打螺丝'
    
}
const mutations={
    JIA(state,value){
        state.sum+=value
    },
    JIAN(state,value){
        state.sum-=value
    },
}
const getters={
    bigNum(state){
        return state.sum*10
    }
}

Vue.use(Vuex)

export default new Vuex.Store({
   actions,
   state,
   mutations,
   getters
})
```

`src/components/Count.vue`:

```jsx
<template>
  <div>
    <h3>sum:{{$store.state.sum}}</h3>
    <h3>10*sum:{{bigNum}}</h3>
    <h3>我在{{school}}，{{work}}</h3>
    <select v-model.number="n">
        <option value="1">1</option>
        <option value="2">2</option>
        <option value="3">3</option>
        <option value="4">4</option>
    </select>
    <button @click="increment">+</button>
    <button @click="decrement">-</button>
    <button @click="incrementOdd">当前求和为奇数再加</button>
    <button @click="incrementWait">等一等再加</button>
  </div>
</template>

<script>
import { mapGetters, mapState } from 'vuex'
export default {
    name:'CountOne',
    data() {
        return {
            n:1
        }
    },
    methods:{
        increment(){
            this.$store.commit('JIA',this.n)
        },
        decrement(){
            this.$store.commit('JIAN',this.n)
        },
        incrementOdd(){
           this.$store.dispatch('JIAODD',this.n)
        },
        incrementWait(){
           this.$store.dispatch('JIAWAIT',this.n)
        },

    },
    computed:{
        // ...mapState(['sum','school','work'])
        ...mapState({
            sum:'sum',
            school:'school',
            work:'work'
        }),
        ...mapGetters(['bigNum'])
    }
}
</script>

<style>
button{
    border: 2px solid rgb(12, 203, 28);
    background-color: #fff;
    
    color: #16ed1d;
    width: 150px;
    height: 40px;
    margin-left: 5px;
}
</style>
```

- mapState方法：用于帮助我们映射`state`中的数据
    
    ```jsx
    computed: {
        //借助mapState生成计算属性：sum、school、subject（对象写法）
         ...mapState({sum:'sum',school:'school',subject:'subject'}),
             
        //借助mapState生成计算属性：sum、school、subject（数组写法）
        ...mapState(['sum','school','subject']),
    },
    ```
    
- mapGetters方法：用于帮助我们映射`getters`中的数据
    
    ```jsx
    computed: {
        //借助mapGetters生成计算属性：bigSum（对象写法）
        ...mapGetters({bigSum:'bigSum'}),
    
        //借助mapGetters生成计算属性：bigSum（数组写法）
        ...mapGetters(['bigSum'])
    },
    ```
    

### ****mapActions与mapMutations****

**备注：`mapActions`与`mapMutations`使用时，若需要传递参数，则需要在模板中绑定事件时传递好参数，否则参数是事件对象**

`src/components/Count.vue`:

```jsx
<template>
	<div>
		<h1>当前求和为：{{sum}}</h1>
		<h3>当前求和的10倍为：{{bigSum}}</h3>
		<h3>我是{{name}}，我在{{school}}学习</h3>
		<select v-model.number="n">
			<option value="1">1</option>
			<option value="2">2</option>
			<option value="3">3</option>
		</select>
		<button @click="increment(n)">+</button>
		<button @click="decrement(n)">-</button>
		<button @click="incrementOdd(n)">当前求和为奇数再加</button>
		<button @click="incrementWait(n)">等一等再加</button>
	</div>
</template>

<script>
	import {mapState,mapGetters,mapMutations,mapActions} from 'vuex'

	export default {
		name:'Count',
		data() {
			return {
				n:1, //用户选择的数字
			}
		},
		methods: {

			//原写法
				increment(){
            this.$store.commit('JIA',this.n)
        },
        decrement(){
            this.$store.commit('JIAN',this.n)
        },
        
        incrementOdd(){
           this.$store.dispatch('JIAODD',this.n)
        },
        incrementWait(){
           this.$store.dispatch('JIAWAIT',this.n)
        },

			// 借助mapActions生成：increment、decrement（对象形式）
			...mapMutations({increment:'ADD',decrement:'SUBTRACT'}),

			// 借助mapActions生成：incrementOdd、incrementWait（对象形式）
			...mapActions({incrementOdd:'addOdd',incrementWait:'addWait'})
		},
		computed:{		
			// 借助mapState生成计算属性（数组写法）
			// ...mapState(['sum','school','name']),
			// 借助mapState生成计算属性（对象写法）
			...mapState({sum:'sum',school:'school',name:'name'}),

			...mapGetters(['bigSum'])
		}
	}
</script>

<style>
	button{
		margin-left: 5px;
	}
</style>
```

- mapActions方法：用于帮助我们生成与`actions`对话的方法，即：包含`$store.dispatch(xxx)`
的函数
    
    ```jsx
    methods:{
        //靠mapActions生成：incrementOdd、incrementWait（对象形式）
        ...mapActions({incrementOdd:'jiaOdd',incrementWait:'jiaWait'})
    
        //靠mapActions生成：incrementOdd、incrementWait（数组形式）
        ...mapActions(['jiaOdd','jiaWait'])
    }
    ```
    
- mapMutations方法：用于帮助我们生成与`mutations`对话的方法，即：包含`store.commit(xxx)`
的函数
    
    ```jsx
    methods:{
        //靠mapActions生成：increment、decrement（对象形式）
        ...mapMutations({increment:'JIA',decrement:'JIAN'}),
        
        //靠mapMutations生成：JIA、JIAN（对象形式）
        ...mapMutations(['JIA','JIAN']),
    }
    ```
    

### ****多组件共享数据****

`src/App.vue`:

```jsx
<template>
	<div class="container">
		<Count/>
		<hr/>
		<Person/>
	</div>
</template>

<script>
	import Count from './components/Count'
	import Person from './components/Person'

	export default {
		name:'App',
		components:{Count,Person}
	}
</script>
```

`src/store/index.js`:

```jsx
//引入Vue核心库
import Vue from 'vue'
//引入Vuex
import Vuex from 'vuex'
//应用Vuex插件
Vue.use(Vuex)
   
//准备actions对象——响应组件中用户的动作
const actions = {
    addOdd(context,value){
        console.log("actions中的addOdd被调用了")
        if(context.state.sum % 2){
            context.commit('ADD',value)
        }
    },
    addWait(context,value){
        console.log("actions中的addWait被调用了")
        setTimeout(()=>{
			context.commit('ADD',value)
		},500)
    },
}
//准备mutations对象——修改state中的数据
const mutations = {
    ADD(state,value){
        state.sum += value
    },
    SUBTRACT(state,value){
        state.sum -= value
    },
	ADD_PERSON(state,value){
		console.log('mutations中的ADD_PERSON被调用了')
		state.personList.unshift(value)
	}
}
//准备state对象——保存具体的数据
const state = {
    sum:0, //当前的和
    name:'JOJO',
    school:'尚硅谷',
    personList:[
		{id:'001',name:'JOJO'}
	]
}
//准备getters对象——用于将state中的数据进行加工
const getters = {
    bigSum(){
        return state.sum * 10
    }
}
   
//创建并暴露store
export default new Vuex.Store({
    actions,
    mutations,
    state,
    getters
})
```

`src/components/Count.vue`:

```jsx
<template>
	<div>
		<h1>当前求和为：{{sum}}</h1>
		<h3>当前求和的10倍为：{{bigSum}}</h3>
		<h3>我是{{name}}，我在{{school}}学习</h3>
		<h3 style="color:red">Person组件的总人数是：{{personList.length}}</h3>
		<select v-model.number="n">
			<option value="1">1</option>
			<option value="2">2</option>
			<option value="3">3</option>
		</select>
		<button @click="increment(n)">+</button>
		<button @click="decrement(n)">-</button>
		<button @click="incrementOdd(n)">当前求和为奇数再加</button>
		<button @click="incrementWait(n)">等一等再加</button>
	</div>
</template>

<script>
	import {mapState,mapGetters,mapMutations,mapActions} from 'vuex'

	export default {
		name:'Count',
		data() {
			return {
				n:1, //用户选择的数字
			}
		},
		methods: {
			...mapMutations({increment:'ADD',decrement:'SUBTRACT'}),
			...mapActions({incrementOdd:'addOdd',incrementWait:'addWait'})
		},
		computed:{
			...mapState(['sum','school','name','personList']),,
			...mapGetters(['bigSum'])
		}
	}
</script>

<style>
	button{
		margin-left: 5px;
	}
</style>
```

`src/components/Person.vue`:

```jsx
<template>
	<div>
		<h1>人员列表</h1>
		<h3 style="color:red">Count组件求和为：{{sum}}</h3>
		<input type="text" placeholder="请输入名字" v-model="name">
		<button @click="add">添加</button>
		<ul>
			<li v-for="p in personList" :key="p.id">{{p.name}}</li>
		</ul>
	</div>
</template>

<script>
	import {nanoid} from 'nanoid'
	export default {
		name:'Person',
		data() {
			return {
				name:''
			}
		},
		computed:{
			personList(){
				return this.$store.state.personList
			},
			sum(){
				return this.$store.state.sum
			}
		},
		methods: {
			add(){
				const personObj = {id:nanoid(),name:this.name}
				this.$store.commit('ADD_PERSON',personObj)
				this.name = ''
			}
		}
	}
</script>
```

### ****模块化+命名空间****

`src/store/index.js`:

```jsx
//引入Vue核心库
import Vue from 'vue'
//引入Vuex
import Vuex from 'vuex'
//引入count
import countOptions from './count'
//引入person
import personOptions from './person'
//应用Vuex插件
Vue.use(Vuex)
   
//创建并暴露store
export default new Vuex.Store({
    modules:{
        countAbout:countOptions,
        personAbout:personOptions,
    }
})
```

`src/store/count.js`:

```jsx
export default{
    namespaced:true,
    actions:{
        addOdd(context,value){
            console.log("actions中的addOdd被调用了")
            if(context.state.sum % 2){
                context.commit('ADD',value)
            }
        },
        addWait(context,value){
            console.log("actions中的addWait被调用了")
            setTimeout(()=>{
                context.commit('ADD',value)
            },500)
        }
    },
    mutations:{
        ADD(state,value){
            state.sum += value
        },
        SUBTRACT(state,value){
            state.sum -= value
        }
    },
    state:{
        sum:0, //当前的和
        name:'JOJO',
        school:'尚硅谷',
    },
    getters:{
        bigSum(state){
            return state.sum * 10
        }
    }
}
```

`src/store/person.js`:

```jsx
import axios from "axios"
import { nanoid } from "nanoid"

export default{
    namespaced:true,
    actions:{
        addPersonWang(context,value){
            if(value.name.indexOf('王') === 0){
                context.commit('ADD_PERSON',value)
            }else{
                alert('添加的人必须姓王！')
            }
        },
        addPersonServer(context){
            axios.get('http://api.uixsj.cn/hitokoto/get?type=social').then(
                response => {
                    context.commit('ADD_PERSON',{id:nanoid(),name:response.data})
                },
                error => {
                    alert(error.message)
                }
            )
        }
    },
    mutations:{
        ADD_PERSON(state,value){
            console.log('mutations中的ADD_PERSON被调用了')
            state.personList.unshift(value)
        }
    },
    state:{
        personList:[
            {id:'001',name:'JOJO'}
        ]
    },
    getters:{
        firstPersonName(state){
            return state.personList[0].name
        }
    }
}
```

`src/components/Count.vue`:

```jsx
<template>
	<div>
		<h1>当前求和为：{{sum}}</h1>
		<h3>当前求和的10倍为：{{bigSum}}</h3>
		<h3>我是{{name}}，我在{{school}}学习</h3>
		<h3 style="color:red">Person组件的总人数是：{{personList.length}}</h3>
		<select v-model.number="n">
			<option value="1">1</option>
			<option value="2">2</option>
			<option value="3">3</option>
		</select>
		<button @click="increment(n)">+</button>
		<button @click="decrement(n)">-</button>
		<button @click="incrementOdd(n)">当前求和为奇数再加</button>
		<button @click="incrementWait(n)">等一等再加</button>
	</div>
</template>

<script>
	import {mapState,mapGetters,mapMutations,mapActions} from 'vuex'

	export default {
		name:'Count',
		data() {
			return {
				n:1, //用户选择的数字
			}
		},
		methods: {
			...mapMutations('countAbout',{increment:'ADD',decrement:'SUBTRACT'}),
			...mapActions('countAbout',{incrementOdd:'addOdd',incrementWait:'addWait'})
		},
		computed:{
			...mapState('countAbout',['sum','school','name']),
			...mapGetters('countAbout',['bigSum']),
			...mapState('personAbout',['personList'])
		}
	}
</script>

<style>
	button{
		margin-left: 5px;
	}
</style>
```

`src/components/Person.vue`:

```jsx
<template>
	<div>
		<h1>人员列表</h1>
		<h3 style="color:red">Count组件求和为：{{sum}}</h3>
        <h3>列表中第一个人的名字是：{{firstPersonName}}</h3>
		<input type="text" placeholder="请输入名字" v-model="name">
		<button @click="add">添加</button>
        <button @click="addWang">添加一个姓王的人</button>
        <button @click="addPerson">随机添加一个人</button>
		<ul>
			<li v-for="p in personList" :key="p.id">{{p.name}}</li>
		</ul>
	</div>
</template>

<script>
	import {nanoid} from 'nanoid'
	export default {
		name:'Person',
		data() {
			return {
				name:''
			}
		},
		computed:{
			personList(){
				return this.$store.state.personAbout.personList
			},
			sum(){
				return this.$store.state.countAbout.sum
			},
            firstPersonName(){
                return this.$store.getters['personAbout/firstPersonName']
            }
		},
		methods: {
			add(){
				const personObj = {id:nanoid(),name:this.name}
				this.$store.commit('personAbout/ADD_PERSON',personObj)
				this.name = ''
			},
            addWang(){
                const personObj = {id:nanoid(),name:this.name}
				this.$store.dispatch('personAbout/addPersonWang',personObj)
				this.name = ''   
            },
            addPerson(){
                this.$store.dispatch('personAbout/addPersonServer')
            }
		},
	}
</script>
```

模块化+命名空间：

1. 目的：让代码更好维护，让多种数据分类更加明确
2. 修改`store.js`：
    
    ```jsx
    const countAbout = {
    	namespaced:true,//开启命名空间
    	state:{x:1},
        mutations: { ... },
        actions: { ... },
      	getters: {
        	bigSum(state){
           		return state.sum * 10
        	}
      	}
    }
    
    const personAbout = {
      	namespaced:true,//开启命名空间
      	state:{ ... },
      	mutations: { ... },
      	actions: { ... }
    }
    
    const store = new Vuex.Store({
      	modules: {
        	countAbout,
        	personAbout
      	}
    })
    ```
    
3. 开启命名空间后，组件中读取`state`数据
    
    ```jsx
    //方式一：自己直接读取
    this.$store.state.personAbout.list
    //方式二：借助mapState读取：
    ...mapState('countAbout',['sum','school','subject']),
    ```
    
4. 开启命名空间后，组件中读取`getters`数据：
    
    ```jsx
    //方式一：自己直接读取
    this.$store.getters['personAbout/firstPersonName']
    //方式二：借助mapGetters读取：
    ...mapGetters('countAbout',['bigSum'])
    ```
    
5. 开启命名空间后，组件中调用`dispatch`：
    
    ```jsx
    //方式一：自己直接dispatch
    this.$store.dispatch('personAbout/addPersonWang',person)
    //方式二：借助mapActions：
    ...mapActions('countAbout',{incrementOdd:'jiaOdd',incrementWait:'jiaWait'})
    ```
    
6. 开启命名空间后，组件中调用`commit`：
    
    ```jsx
    //方式一：自己直接commit
    this.$store.commit('personAbout/ADD_PERSON',person)
    //方式二：借助mapMutations：
    ...mapMutations('countAbout',{increment:'JIA',decrement:'JIAN'}),
    ```