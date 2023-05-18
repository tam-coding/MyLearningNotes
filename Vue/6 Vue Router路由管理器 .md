# 6.Vue Router路由管理器

### ****vue-router的理解****

• vue 的一个插件库，专门用来实现SPA 应用

****对SPA应用的理解****

1. 单页 Web 应用（single page web application，SPA）
2. 整个应用只有一个完整的页面
3. **点击页面中的导航链接不会刷新页面，只会做页面的局部更新**
4. **数据需要通过ajax请求获取**

**路由的理解**

1. 什么是路由?
    1. 一个路由就是一组映射关系（key - value）
    2. key 为路径，value 可能是 function 或 componen

**路由分类**

1. 后端路由：
    1. 理解：**value 是 function**，用于处理客户端提交的请求
    2. 工作过程：服务器接收到一个请求时，根据请求路径找到匹配的函数来处理请求，返回响应数据
2. 前端路由：
    1. 理解：**value 是 component**，用于展示页面内容
    2. 工作过程：当浏览器的路径改变时，对应的组件就会显示

### ****基本路由****

下载`vue-router`：`npm i vue-router`

`src/router/index.js`:

```jsx
//该文件专门用于创建整个应用的路由器
import VueRouter from "vue-router";
//引入组件
import Home from '../components/Home'
import About from '../components/About'

//创建并暴露一个路由器
export default new VueRouter({
    routes:[
        {
            path:'/about',
            component:About
        },
        {
            path:'/home',
            component:Home
        }
    ]
})
```

`src/main.js`:

```jsx
import Vue from 'vue'
import App from './App.vue'
import VueRouter from 'vue-router'
import router from './router'

Vue.config.productionTip = false
Vue.use(VueRouter)

new Vue({
    el:"#app",
    render: h => h(App),
    router
})
```

`src/App.vue`:

```jsx
<template>
	<div>
		<div class="row">
			<div class="col-xs-offset-2 col-xs-8">
				<div class="page-header"><h2>Vue Router Demo</h2></div>
			</div>
		</div>
		<div class="row">
			<div class="col-xs-2 col-xs-offset-2">
				<div class="list-group">
					<!-- 原始html中我们使用a标签实现页面跳转 -->
					<!-- <a class="list-group-item active" href="./about.html">About</a>
					<a class="list-group-item" href="./home.html">Home</a> -->
					
					<!-- Vue中借助router-link标签实现路由的切换 -->
					<router-link class="list-group-item" active-class="active" to="/about"> 							About
    				</router-link>
					<router-link class="list-group-item" active-class="active" to="/home">
                        Home
    				</router-link>
				</div>
			</div>
			<div class="col-xs-6">
				<div class="panel">
					<div class="panel-body">
						<!-- 指定组件的呈现位置 -->
						<router-view></router-view>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
	export default {
		name:'App',
	}
</script>
```

`src/components/Home.vue`:

```jsx
<template>
  <h2>我是Home组件的内容</h2>
</template>

<script>
    export default {
        name:'Home'
    }
</script>
```

`src/components/About.vue`:

```jsx
<template>
  <h2>我是About组件的内容</h2>
</template>

<script>
    export default {
        name:'About'
    }
</script>
```

1. 安装`vue-router`，命令：`npm i vue-router`
2. 应用插件：`Vue.use(VueRouter)`
3. 编写router配置项：
    
    ```jsx
    //引入VueRouter
    import VueRouter from 'vue-router'
    //引入Luyou 组件
    import About from '../components/About'
    import Home from '../components/Home'
    
    //创建router实例对象，去管理一组一组的路由规则
    const router = new VueRouter({
    	routes:[
    		{
    			path:'/about',
    			component:About
    		},
    		{
    			path:'/home',
    			component:Home
    		}
    	]
    })
    
    //暴露router
    export default router
    ```
    
4. 实现切换（`active-class`可配置高亮样式）：
    
    `<router-link active-class="active" to="/about">About</router-link>`
    
5. 指定展示位：`**<router-view></router-view>**`

### ****几个注意事项****

1. 路由组件通常存放在`pages`文件夹，一般组件通常存放在`components`文件夹
    
    比如上一节的案例就可以修改为：
    
    `src/pages/Home.vue`:
    
    ```jsx
    <template>
      <h2>我是Home组件的内容</h2>
    </template>
    
    <script>
        export default {
            name:'Home'
        }
    </script>
    ```
    
    `src/pages/About.vue`:
    
    ```jsx
    <template>
      <h2>我是About组件的内容</h2>
    </template>
    
    <script>
        export default {
            name:'About'
        }
    </script>
    ```
    
    `src/router/index.js`:
    
    ```jsx
    //该文件专门用于创建整个应用的路由器
    import VueRouter from "vue-router";
    //引入组件
    import Home from '../pages/Home'
    import About from '../pages/About'
    
    //创建并暴露一个路由器
    export default new VueRouter({
        routes:[
            {
                path:'/about',
                component:About
            },
            {
                path:'/home',
                component:Home
            }
        ]
    })
    ```
    
    `src/components/Banner.vue`:
    
    ```jsx
    <template>
        <div class="col-xs-offset-2 col-xs-8">
            <div class="page-header"><h2>Vue Router Demo</h2></div>
        </div>
    </template>
    
    <script>
        export default {
            name:'Banner'
        }
    </script>
    ```
    
    `src/components/Banner.vue`:
    
    ```jsx
    <template>
        	<div>
        		<div class="row">
        			<Banner/>
        		</div>
        		<div class="row">
        			<div class="col-xs-2 col-xs-offset-2">
        				<div class="list-group">
        					<!-- 原始html中我们使用a标签实现页面跳转 -->
        					<!-- <a class="list-group-item active" href="./about.html">About</a>
        					<a class="list-group-item" href="./home.html">Home</a> -->
    					<!-- Vue中借助router-link标签实现路由的切换 -->
    					<router-link class="list-group-item" active-class="active" to="/about">
                            About
        				</router-link>
    					<router-link class="list-group-item" active-class="active" to="/home">
                            Home
        				</router-link>
    				</div>
    			</div>
    			<div class="col-xs-6">
    				<div class="panel">
    					<div class="panel-body">
    						<!-- 指定组件的呈现位置 -->
    						<router-view></router-view>
    					</div>
    				</div>
    			</div>
    		</div>
    	</div>
    </template>
    
    <script>
    	import Banner from './components/Banner.vue'
    	export default {
    		name:'App',
    		components:{Banner}
    	}
    </script>
    ```
    
2. 通过切换，“隐藏”了的路由组件，默认是被销毁掉的，需要的时候再去挂载
3. 每个组件都有自己的`$route`属性，里面存储着自己的路由信息
4. 整个应用只有一个router，可以通过组件的`$router`属性获取到

### ****多级路由（嵌套路由）****

`src/pages/Home.vue`:

```jsx
<template>
    <div>
        <h2>Home组件内容</h2>
		<div>
			<ul class="nav nav-tabs">
				<li>
					<router-link class="list-group-item" active-class="active" to="/home/news"> 
                        News
    				</router-link>
				</li>
				<li>
					<router-link class="list-group-item" active-class="active" to="/home/message">
                        Message
    				</router-link>
				</li>
			</ul>
			<router-view></router-view>
		</div>
    </div>
</template>

<script>
    export default {
        name:'Home'
    }
</script>
```

`src/pages/News.vue`:

```jsx
<template>
    <ul>
        <li>news001</li>
        <li>news002</li>
        <li>news003</li>
    </ul>
</template>

<script>
    export default {
        name:'News'
    }
</script>
```

`src/pages/Message.vue`:

```jsx
<template>
    <ul>
        <li>
            <a href="/message1">message001</a>&nbsp;&nbsp;
        </li>
        <li>
            <a href="/message2">message002</a>&nbsp;&nbsp;
        </li>
        <li>
            <a href="/message/3">message003</a>&nbsp;&nbsp;
        </li>
    </ul>
</template>

<script>
    export default {
        name:'News'
    }
</script>
```

`src/router/index.js`:

```jsx
//该文件专门用于创建整个应用的路由器
import VueRouter from "vue-router";
//引入组件
import Home from '../pages/Home'
import About from '../pages/About'
import News from '../pages/News'
import Message from '../pages/Message'

//创建并暴露一个路由器
export default new VueRouter({
    routes:[
        {
            path:'/about',
            component:About
        },
        {
            path:'/home',
            component:Home,
            children:[
                {
                    path:'news',
                    component:News
                },
                {
                    path:'message',
                    component:Message
                }
            ]
        }
    ]
})
```

1. 配置路由规则，使用children配置项：
    
    ```jsx
    routes:[
    	{
    		path:'/about',
    		component:About,
    	},
    	{
    		path:'/home',
    		component:Home,
    		children:[ //通过children配置子级路由
    			{
    				path:'news', //此处一定不要写：/news
    				component:News
    			},
    			{
    				path:'message', //此处一定不要写：/message
    				component:Message
    			}
    		]
    	}
    ]
    ```
    
2. 跳转（要写完整路径）：`<router-link to="/home/news">News</router-link>`

### ****路由的query参数****

`src/router.index.js`:

```jsx
//该文件专门用于创建整个应用的路由器
import VueRouter from "vue-router";
//引入组件
import Home from '../pages/Home'
import About from '../pages/About'
import News from '../pages/News'
import Message from '../pages/Message'
import Detail from '../pages/Detail'

//创建并暴露一个路由器
export default new VueRouter({
    routes:[
        {
            path:'/about',
            component:About
        },
        {
            path:'/home',
            component:Home,
            children:[
                {
                    path:'news',
                    component:News
                },
                {
                    path:'message',
                    component:Message,
                    children:[
                        {
                            path:'detail',
                            component:Detail
                        }
                    ]
                }
            ]
        }
    ]
})
```

`src/pages/Detail.vue`:

```jsx
<template>
    <ul>
        <li>消息编号：{{$route.query.id}}</li>
        <li>消息标题：{{$route.query.title}}</li>
    </ul>
</template>

<script>
    export default {
        name:'Detail'
    }
</script>
```

`src/pages/Message.vue`:

```jsx
<template>
    <div>
        <ul>
            <li v-for="m in messageList" :key="m.id">
                <!-- 跳转路由并携带query参数，to的字符串写法 -->
                <!-- <router-link :to="`/home/message/detail?id=${m.id}&title=${m.title}`">
                    {{m.title}}
                </router-link>&nbsp;&nbsp; -->

                <!-- 跳转路由并携带query参数，to的对象写法 -->
                <router-link :to="{
                    path:'/home/message/detail',
                    query:{
                        id:m.id,
                        title:m.title
                    }
                }">
                    {{m.title}}
                </router-link>&nbsp;&nbsp;
            </li>
        </ul>
        <hr/>
        <router-view></router-view>
    </div>
</template>

<script>
    export default {
        name:'News',
        data(){
            return{
                messageList:[
                    {id:'001',title:'消息001'},
                    {id:'002',title:'消息002'},
                    {id:'003',title:'消息003'}
                ]
            }
        }
    }
</script>
```

- 传递参数：
    
    ```jsx
    <!-- 跳转并携带query参数，to的字符串写法 -->
    <router-link :to="/home/message/detail?id=666&title=你好">跳转</router-link>
    				
    <!-- 跳转并携带query参数，to的对象写法 -->
    <router-link :to="{
    	path:'/home/message/detail',
    		query:{
    		id:666,
            title:'你好'
    	}
    }">跳转</router-link>
    ```
    
- 接收参数：
    
    ```jsx
    $route.query.id
    $route.query.title
    ```
    

### ****命名路由****

`src/router/index.js`:

```jsx
//该文件专门用于创建整个应用的路由器
import VueRouter from "vue-router";
//引入组件
import Home from '../pages/Home'
import About from '../pages/About'
import News from '../pages/News'
import Message from '../pages/Message'
import Detail from '../pages/Detail'

//创建并暴露一个路由器
export default new VueRouter({
    routes:[
        {
            path:'/about',
            component:About
        },
        {
            path:'/home',
            component:Home,
            children:[
                {
                    path:'news',
                    component:News
                },
                {
                    path:'message',
                    component:Message,
                    children:[
                        {
                            //name配置项为路由命名
                            name:'xiangqing',
                            path:'detail',
                            component:Detail
                        }
                    ]
                }
            ]
        }
    ]
})
```

`src/pages/Message.vue`:

```jsx
<template>
    <div>
        <ul>
            <li v-for="m in messageList" :key="m.id">
                <!-- 跳转路由并携带query参数，to的字符串写法 -->
                <!-- <router-link :to="`/home/message/detail?id=${m.id}&title=${m.title}`">
                    {{m.title}}
                </router-link>&nbsp;&nbsp; -->

                <!-- 跳转路由并携带query参数，to的对象写法 -->
                <router-link :to="{
                    //使用name进行跳转
                    name:'xiangqing',
                    query:{
                        id:m.id,
                        title:m.title
                    }
                }">
                    {{m.title}}
                </router-link>&nbsp;&nbsp;
            </li>
        </ul>
        <hr/>
        <router-view></router-view>
    </div>
</template>

<script>
    export default {
        name:'News',
        data(){
            return{
                messageList:[
                    {id:'001',title:'消息001'},
                    {id:'002',title:'消息002'},
                    {id:'003',title:'消息003'}
                ]
            }
        }
    }
</script>
```

命名路由：

1. 作用：可以简化路由的跳转
2. 如何使用：
    1. 给路由命名：
        
        ```jsx
        {
        	path:'/demo',
        	component:Demo,
        	children:[
        		{
        			path:'test',
        			component:Test,
        			children:[
        				{
                            name:'hello' //给路由命名
        					path:'welcome',
        					component:Hello,
        				}
        			]
        		}
        	]
        }
        ```
        
    2. 简化跳转：
        
        ```jsx
        <!--简化前，需要写完整的路径 -->
        <router-link to="/demo/test/welcome">跳转</router-link>
        
        <!--简化后，直接通过名字跳转 -->
        <router-link :to="{name:'hello'}">跳转</router-link>
        
        <!--简化写法配合传递参数 -->
        <router-link 
        	:to="{
        		name:'hello',
        		query:{
        		    id:666,
                    title:'你好'
        		}
        	}"
        >跳转</router-link>
        ```
        

### ****路由的params参数****

`src/router/index.js`:

```jsx
//该文件专门用于创建整个应用的路由器
import VueRouter from "vue-router";
//引入组件
import Home from '../pages/Home'
import About from '../pages/About'
import News from '../pages/News'
import Message from '../pages/Message'
import Detail from '../pages/Detail'

//创建并暴露一个路由器
export default new VueRouter({
    routes:[
        {
            path:'/about',
            component:About
        },
        {
            path:'/home',
            component:Home,
            children:[
                {
                    path:'news',
                    component:News
                },
                {
                    path:'message',
                    component:Message,
                    children:[
                        {
                            name:'xiangqing',
                            path:'detail/:id/:title',//使用占位符声明接收params参数
                            component:Detail
                        }
                    ]
                }
            ]
        }
    ]
})
```

`src/pages/Message.vue`:

```jsx
<template>
    <div>
        <ul>
            <li v-for="m in messageList" :key="m.id">
                <!-- 跳转路由并携带params参数，to的字符串写法 -->
                <!-- <router-link :to="`/home/message/detail/${m.id}/${m.title}`">
                    {{m.title}}
                </router-link>&nbsp;&nbsp; -->

                <!-- 跳转路由并携带params参数，to的对象写法 -->
                <router-link :to="{
                    name:'xiangqing',
                    params:{
                        id:m.id,
                        title:m.title
                    }
                }">
                    {{m.title}}
                </router-link>&nbsp;&nbsp;
            </li>
        </ul>
        <hr/>
        <router-view></router-view>
    </div>
</template>

<script>
    export default {
        name:'News',
        data(){
            return{
                messageList:[
                    {id:'001',title:'消息001'},
                    {id:'002',title:'消息002'},
                    {id:'003',title:'消息003'}
                ]
            }
        }
    }
</script>
```

`src/pages/Detail.vue`:

```jsx
<template>
    <ul>
        <li>消息编号：{{$route.params.id}}</li>
        <li>消息标题：{{$route.params.title}}</li>
    </ul>
</template>

<script>
    export default {
        name:'Detail'
    }
</script>
```

1. 配置路由，声明接收`params`参数：
    
    ```jsx
    {
    	path:'/home',
    	component:Home,
    	children:[
    		{
    			path:'news',
    			component:News
    		},
    		{
    			component:Message,
    			children:[
    				{
    					name:'xiangqing',
    					path:'detail/:id/:title', //使用占位符声明接收params参数
    					component:Detail
    				}
    			]
    		}
    	]
    }
    ```
    
2. 传递参数：
    
    ```jsx
    <!-- 跳转并携带params参数，to的字符串写法 -->
    <router-link :to="/home/message/detail/666/你好">跳转</router-link>
    				
    <!-- 跳转并携带params参数，to的对象写法 -->
    <router-link 
    	:to="{
    		name:'xiangqing',
    		params:{
    		   id:666,
                title:'你好'
    		}
    	}"
    >跳转</router-link>
    ```
    

> **特别注意：路由携带params参数时，若使用to的对象写法，则不能使用path配置项，必须使用name配置！**
> 
1. 接收参数：
    
    ```jsx
    $route.params.id
    $route.params.title
    ```
    

### ****路由的props配置****

`src/pages/Message.vue`:

```jsx
<template>
    <div>
        <ul>
            <li v-for="m in messageList" :key="m.id">
                <router-link :to="{
                    name:'xiangqing',
                    params:{
                        id:m.id,
                        title:m.title
                    }
                }">
                    {{m.title}}
                </router-link>&nbsp;&nbsp;
            </li>
        </ul>
        <hr/>
        <router-view></router-view>
    </div>
</template>

<script>
    export default {
        name:'News',
        data(){
            return{
                messageList:[
                    {id:'001',title:'消息001'},
                    {id:'002',title:'消息002'},
                    {id:'003',title:'消息003'}
                ]
            }
        }
    }
</script>
```

`src/pages/Message.vue`:

```jsx
<template>
    <div>
        <ul>
            <li v-for="m in messageList" :key="m.id">
                <router-link :to="{
                    name:'xiangqing',
                    params:{
                        id:m.id,
                        title:m.title
                    }
                }">
                    {{m.title}}
                </router-link>&nbsp;&nbsp;
            </li>
        </ul>
        <hr/>
        <router-view></router-view>
    </div>
</template>

<script>
    export default {
        name:'News',
        data(){
            return{
                messageList:[
                    {id:'001',title:'消息001'},
                    {id:'002',title:'消息002'},
                    {id:'003',title:'消息003'}
                ]
            }
        }
    }
</script>
```

`src/pages/Detail.vue`:

```jsx
//该文件专门用于创建整个应用的路由器
import VueRouter from "vue-router";
//引入组件
import Home from '../pages/Home'
import About from '../pages/About'
import News from '../pages/News'
import Message from '../pages/Message'
import Detail from '../pages/Detail'

//创建并暴露一个路由器
export default new VueRouter({
    routes:[
        {
            path:'/about',
            component:About
        },
        {
            path:'/home',
            component:Home,
            children:[
                {
                    path:'news',
                    component:News
                },
                {
                    path:'message',
                    component:Message,
                    children:[
                        {
                            name:'xiangqing',
                            path:'detail/:id/:title',
                            component:Detail,
                            //props的第一种写法，值为对象，该对象中的所有key-value都会以props的形式传给Detail组件。
							// props:{a:1,b:'hello'}

							//props的第二种写法，值为布尔值，若布尔值为真，就会把该路由组件收到的所有params参数，以props的形式传给Detail组件。
							// props:true

							//props的第三种写法，值为函数
							props($route){
								return {
									id:$route.params.id,
									title:$route.params.title,
								}
							}
                        }
                    ]
                }
            ]
        }
    ]
})
```

- 作用：让路由组件更方便的收到参数
    
    ```jsx
    {
    	name:'xiangqing',
    	path:'detail/:id',
    	component:Detail,
    
    	//第一种写法：props值为对象，该对象中所有的key-value的组合最终都会通过props传给Detail组件
    	// props:{a:900}
    
    	//第二种写法：props值为布尔值，布尔值为true，则把路由收到的所有params参数通过props传给Detail组件
    	// props:true
    	
    	//第三种写法：props值为函数，该函数返回的对象中每一组key-value都会通过props传给Detail组件
    	props(route){
    		return {
    			id:route.query.id,
    			title:route.query.title
    		}
    	}
    }
    ```
    

### ****路由跳转的replace方法****

`src/pages/Home.vue`:

```jsx
<template>
    <div>
        <h2>Home组件内容</h2>
		<div>
			<ul class="nav nav-tabs">
				<li>
					<router-link replace class="list-group-item" active-class="active" to="/home/news">News</router-link>
				</li>
				<li>
					<router-link replace class="list-group-item" active-class="active" to="/home/message">Message</router-link>
				</li>
			</ul>
			<router-view></router-view>
		</div>
    </div>
</template>

<script>
    export default {
        name:'Home'
    }
</script>
```

1. 作用：控制路由跳转时操作浏览器历史记录的模式
2. 浏览器的历史记录有两种写入方式：`push`和`replace`，其中`push`是追加历史记录，`replace`是替换当前记录。**路由跳转时候默认为`push`方式**
3.  开启`replace`模式：
    
    `<router-link replace ...>News</router-link>`
    
    `<router-link replace='true'>News</router-link>`
    

### ****编程式路由导航****

`src/components/Banner.vue:`

```jsx
<template>
	<div class="col-xs-offset-2 col-xs-8">
		<div class="page-header">
			<h2>Vue Router Demo</h2>
			<button @click="back">后退</button>
			<button @click="forward">前进</button>
			<button @click="test">测试一下go</button>
		</div>
	</div>
</template>

<script>
	export default {
		name:'Banner',
		methods:{
			back(){
				this.$router.back()
			},
			forward(){
				this.$router.forward()
			},
			test(){
				this.$router.go(3)
			}
		},
	}
</script>
```

`src/pages/Message.vue`:

```jsx
<template>
    <div>
        <ul>
            <li v-for="m in messageList" :key="m.id">
                <router-link :to="{
                    name:'xiangqing',
                    params:{
                        id:m.id,
                        title:m.title
                    }
                }">
                    {{m.title}}
                </router-link>
                <button @click="showPush(m)">push查看</button>
                <button @click="showReplace(m)">replace查看</button>
            </li>
        </ul>
        <hr/>
        <router-view></router-view>
    </div>
</template>

<script>
    export default {
        name:'News',
        data(){
            return{
                messageList:[
                    {id:'001',title:'消息001'},
                    {id:'002',title:'消息002'},
                    {id:'003',title:'消息003'}
                ]
            }
        },
        methods:{
            showPush(m){
                this.$router.push({
                    name:'xiangqing',
                    query:{
                        id:m.id,
                        title:m.title
                    }
                })
            },
            showReplace(m){
                this.$router.replace({
                    name:'xiangqing',
                    query:{
                        id:m.id,
                        title:m.title
                    }
                })
            }
        }
    }
</script>
```

`src/router/index.js`:

```jsx
//该文件专门用于创建整个应用的路由器
import VueRouter from "vue-router";
//引入组件
import Home from '../pages/Home'
import About from '../pages/About'
import News from '../pages/News'
import Message from '../pages/Message'
import Detail from '../pages/Detail'

//创建并暴露一个路由器
export default new VueRouter({
    routes:[
        {
            path:'/about',
            component:About
        },
        {
            path:'/home',
            component:Home,
            children:[
                {
                    path:'news',
                    component:News
                },
                {
                    path:'message',
                    component:Message,
                    children:[
                        {
                            name:'xiangqing',
                            path:'detail',
                            component:Detail,
							props($route){
								return {
									id:$route.query.id,
									title:$route.query.title,
								}
							}
                        }
                    ]
                }
            ]
        }
    ]
})
```

`src/pages/Detail.vue`:

```jsx
<template>
    <ul>
        <li>消息编号：{{id}}</li>
        <li>消息标题：{{title}}</li>
    </ul>
</template>

<script>
    export default {
        name:'Detail',
        props:['id','title']
    }
</script>
```

1. 作用：不借助`<router-link>`实现路由跳转，让路由跳转更加灵活
2. 具体编码：
    
    ```jsx
    this.$router.push({
    	name:'xiangqing',
        params:{
            id:xxx,
            title:xxx
        }
    })
    
    this.$router.replace({
    	name:'xiangqing',
        params:{
            id:xxx,
            title:xxx
        }
    })
    this.$router.forward() //前进
    this.$router.back() //后退
    this.$router.go(-2)  //后退两步 //可前进也可后退
    ```
    

### ****缓存路由组件 keep-alive 重定向返回时仍有输入记录****

`src/pages/News.vue`:

```jsx
<template>
    <ul>
        <li>news001 <input type="text"></li>
        <li>news002 <input type="text"></li>
        <li>news003 <input type="text"></li>
    </ul>
</template>

<script>
    export default {
        name:'News'
    }
</script>
```

`src/pages/Home.vue`:

```jsx
<template>
    <div>
        <h2>Home组件内容</h2>
		<div>
			<ul class="nav nav-tabs">
				<li>
					<router-link replace class="list-group-item" active-class="active" to="/home/news">News</router-link>
				</li>
				<li>
					<router-link replace class="list-group-item" active-class="active" to="/home/message">Message</router-link>
				</li>
			</ul>
			<keep-alive include="News">
				<router-view></router-view>
			</keep-alive>
		</div>
    </div>
</template>

<script>
    export default {
        name:'Home'
    }
</script>
```

1. 作用：让不展示的路由组件保持挂载，不被销毁
2. 具体编码：
    
    ```jsx
    //**缓存一个路由组件**
    <keep-alive include="News"> //include中写想要缓存的组件名，不写表示全部缓存
        <router-view></router-view>
    </keep-alive>
    
    **//缓存多个路由组件**
    **<keep-alive :include="['News','Message']">** 
        <router-view></router-view>
    </keep-alive>
    ```
    

### ****activated和deactivated****

`src/pages/News.vue`:

```jsx
<template>
    <ul>
        <li :style="{opacity}">欢迎学习vue</li>
        <li>news001 <input type="text"></li>
        <li>news002 <input type="text"></li>
        <li>news003 <input type="text"></li>
    </ul>
</template>

<script>
    export default {
        name:'News',
        data(){
            return{
                opacity:1
            }
        },
        activated(){
            console.log('News组件被激活了')
            this.timer = setInterval(() => {
                this.opacity -= 0.01
                if(this.opacity <= 0) this.opacity = 1
            },16)
        },
        deactivated(){
            console.log('News组件失活了')
            clearInterval(this.timer)
        }
    }
</script>
```

1. `activated`和`deactivated`是路由组件所独有的两个钩子，用于捕获路由组件的激活状态
2. 具体使用：
    1. `activated`路由组件被激活时触发
    2. `deactivated`路由组件失活时触发

### **路由守卫**

### **全局路由守卫 beforeEach afterEach**

`src/router/index.js`:

```jsx
//该文件专门用于创建整个应用的路由器
import VueRouter from "vue-router";
//引入组件
import Home from '../pages/Home'
import About from '../pages/About'
import News from '../pages/News'
import Message from '../pages/Message'
import Detail from '../pages/Detail'

//创建一个路由器
const router = new VueRouter({
    routes:[
        {
            name:'guanyv',
            path:'/about',
            component:About,
            meta:{title:'关于'}
        },
        {
            name:'zhuye',
            path:'/home',
            component:Home,
            meta:{title:'主页'},
            children:[
                {
                    name:'xinwen',
                    path:'news',
                    component:News,
                    meta:{isAuth:true,title:'新闻'}
                },
                {
                    name:'xiaoxi',
                    path:'message',
                    component:Message,
                    meta:{isAuth:true,title:'消息'},
                    children:[
                        {
                            name:'xiangqing',
                            path:'detail',
                            component:Detail,
                            meta:{isAuth:true,title:'详情'},
							props($route){
								return {
									id:$route.query.id,
									title:$route.query.title,
								}
							}
                        }
                    ]
                }
            ]
        }
    ]
})

//全局前置路由守卫————初始化的时候、每次路由切换之前被调用
router.beforeEach((to,from,next) => {
    console.log('前置路由守卫',to,from)
    if(to.meta.isAuth){
        if(localStorage.getItem('school')==='atguigu'){
            next()
        }else{
            alert('学校名不对，无权限查看！')
        }
    }else{
        next()
    }
})

//全局后置路由守卫————初始化的时候被调用、每次路由切换之后被调用
router.afterEach((to,from)=>{
	console.log('后置路由守卫',to,from)
	document.title = to.meta.title || '硅谷系统'
})

//导出路由器
export default router
```

### ****独享路由守卫****

`src/router/index.js`:

```jsx
//该文件专门用于创建整个应用的路由器
import VueRouter from "vue-router";
//引入组件
import Home from '../pages/Home'
import About from '../pages/About'
import News from '../pages/News'
import Message from '../pages/Message'
import Detail from '../pages/Detail'

//创建一个路由器
const router = new VueRouter({
    routes:[
        {
            name:'guanyv',
            path:'/about',
            component:About,
            meta:{title:'关于'}
        },
        {
            name:'zhuye',
            path:'/home',
            component:Home,
            meta:{title:'主页'},
            children:[
                {
                    name:'xinwen',
                    path:'news',
                    component:News,
                    meta:{title:'新闻'},
                    //独享守卫，特定路由切换之后被调用
                    beforeEnter(to,from,next){
                        console.log('独享路由守卫',to,from)
                        if(localStorage.getItem('school') === 'atguigu'){
                            next()
                        }else{
                            alert('暂无权限查看')
                        }
                    }
                },
                {
                    name:'xiaoxi',
                    path:'message',
                    component:Message,
                    meta:{title:'消息'},
                    children:[
                        {
                            name:'xiangqing',
                            path:'detail',
                            component:Detail,
                            meta:{title:'详情'},
							props($route){
								return {
									id:$route.query.id,
									title:$route.query.title,
								}
							}
                        }
                    ]
                }
            ]
        }
    ]
})

//全局后置路由守卫————初始化的时候被调用、每次路由切换之后被调用
router.afterEach((to,from)=>{
	console.log('后置路由守卫',to,from)
	document.title = to.meta.title || '硅谷系统'
})

//导出路由器
export default router
```

### **组件内路由守卫**

`src/pages/About.vue`:

```jsx
<template>
    <h2>我是About组件的内容</h2>
</template>

<script>
    export default {
        name:'About',
        //通过路由规则，离开该组件时被调用
        beforeRouteEnter (to, from, next) {
            console.log('About--beforeRouteEnter',to,from)
            if(localStorage.getItem('school')==='atguigu'){
                next()
            }else{
                alert('学校名不对，无权限查看！')
            }
        },
        //通过路由规则，离开该组件时被调用
        beforeRouteLeave (to, from, next) {
            console.log('About--beforeRouteLeave',to,from)
            next()
        }
    }
</script>
```

1. 作用：对路由进行权限控制
2. 分类：全局守卫、独享守卫、组件内守卫
3. 全局守卫：
    
    ```jsx
    //全局前置守卫：初始化时执行、每次路由切换前执行
    router.beforeEach((to,from,next)=>{
    	console.log('beforeEach',to,from)
    	if(to.meta.isAuth){ //判断当前路由是否需要进行权限控制
    		if(localStorage.getItem('school') === 'atguigu'){ //权限控制的具体规则
    			next() //放行
    		}else{
    			alert('暂无权限查看')
    		}
    	}else{
    		next() //放行
    	}
    })
    
    //全局后置守卫：初始化时执行、每次路由切换后执行
    router.afterEach((to,from) => {
    	console.log('afterEach',to,from)
    	if(to.meta.title){ 
    		document.title = to.meta.title //修改网页的title
    	}else{
    		document.title = 'vue_test'
    	}
    })
    ```
    
4. 独享守卫：
    
    ```jsx
    beforeEnter(to,from,next){
    	console.log('beforeEnter',to,from)
        if(localStorage.getItem('school') === 'atguigu'){
            next()
        }else{
            alert('暂无权限查看')
        }
    }
    ```
    
5. 组件内守卫：
    
    ```jsx
    //进入守卫：通过路由规则，进入该组件时被调用
    beforeRouteEnter (to, from, next) {...},
    //离开守卫：通过路由规则，离开该组件时被调用
    beforeRouteLeave (to, from, next) {...},
    ```
    

### ****路由器的两种工作模式****

1. 对于一个url来说，什么是hash值？—— #及其后面的内容就是hash值
2. **hash值不会包含在 HTTP 请求中，即：hash值不会带给服务器**
3. hash模式：
    1. 地址中永远带着#号，不美观
    2. 若以后将地址通过第三方手机app分享，若app校验严格，则地址会被标记为不合法
    3. 兼容性较好
4. history模式：
    1. 地址干净，美观
    2. 兼容性和hash模式相比略差
    3. 应用部署上线时需要后端人员支持，解决刷新页面服务端404的问题

### 把项目给后端的步骤

`npm run build` 把dist项目给后端

自己用express搞个服务器测试

- 新建文件demo 然后`npm init`
- npm i express下载express模块
- 新建server.js文件
    
    ```jsx
    const express= require('express')
    
    const app=express()
    app.use(express.static(__dirname+'/static'))
    app.get('/person',(req,res)=>{
        res.send({
            name:'tam',
            age:21
        })
    })
    
    app.listen(5005,(err)=>{
        if(!err) console.log('服务器启动成功了');
    })
    ```
    
- 运行server node server.js /nodemon server.js
- 把dist文件的内容放到新建的static文件夹下

解决history mode下路径匹配问题

[connect-history-api-fallback - npm (npmjs.com)](https://www.npmjs.com/package/connect-history-api-fallback)