# 2.Vue组件化编程

### ****模块与组件、模块化与组件化****

![Untitled](2%20Vue%E7%BB%84%E4%BB%B6%E5%8C%96%E7%BC%96%E7%A8%8B%207fb2b87d656e4dce935f2af97dfbfe1d/Untitled.png)

![Untitled](2%20Vue%E7%BB%84%E4%BB%B6%E5%8C%96%E7%BC%96%E7%A8%8B%207fb2b87d656e4dce935f2af97dfbfe1d/Untitled%201.png)

**模块**

1. 理解：向外提供特定功能的 js 程序，一般就是一个 js 文件
2. 为什么：js 文件很多很复杂
3. 作用：复用 js，简化 js 的编写，提高 js 运行效率

**组件**

1. 定义：用来实现局部功能的代码和资源的集合（html/css/js/image…）
2. 为什么：一个界面的功能很复杂
3. 作用：复用编码，简化项目编码，提高运行效率

**模块化**

当应用中的 js 都以模块来编写的，那这个应用就是一个模块化的应用

**组件化**

当应用中的功能都是多组件的方式来编写的，那这个应用就是一个组件化的应用

### ****非单文件组件****

```html
<body>
    <div id="root">
        <h2>{{name}}</h2>
        //使用主机
        <school></school>
        <student></student>
        <hello></hello>
    </div>
    <script>
        //创建组件
        const school=Vue.extend({
            data() {
                return {
                    name:'wsyu',
                }
            },
            template:`
            <div v-text="name"><div>
            <h2>shool</h2>
            <hr>
            `
        })
        const student=Vue.extend({
            data() {
                return {
                    name:'tam',
                }
            },
            template:`
            <div v-text="name"><div>
            <h2>student</h2>
            <hr>
            `
        })
        
        const hello=Vue.extend({
            data() {
                return {
                    name:'hello',
                }
            },
            template:`
            <div v-text="name"><div>
            <h2>hello</h2>
            <hr>
            `
        })
        
        Vue.component('hello',hello)
        //创建Vue
        new Vue({
            el:'#root',
            data:{
                name:'Vue',

            },
            //局部主存
            components:{
          //**两种写法**
                school:school,
                student
            }
        })
    </script>
</body>
```

- Vue中使用组件的三大步骤：
    1. 定义组件(创建组件)
    2. 注册组件
    3. 使用组件(写组件标签)
- 如何定义一个组件？
    
    使用`Vue.extend(options)`创建，其中`options`和`new Vue(options)`时传入的`options`几乎一样，但也有点区别：
    
    1. el不要写，为什么？
        
        最终所有的组件都要经过一个vm的管理，由vm中的el决定服务哪个容器
        
    2. **data必须写成函数**，为什么？
        
        避免组件被复用时，数据存在引用关系
        
- 如何注册组件？
    1. 局部注册：`new Vue`的时候传入`components`选项
    2. 全局注册：`Vue.component('组件名',组件)`
- 编写组件标签：`<school></school>` 组件名

### ****组件注意事项****

- 关于组件名：
    - 一个单词组成：
        - 第一种写法（首字母小写）：school
        - 第二种写法（首字母大写）：School
    - 多个单词组成：
        - 第一种写法（kebab-case命名）：my-school
        - 第二种写法（CamelCase命名）：MySchool （需要Vue脚手架支持）
    - 备注：
        - 尽可能**回避HTML中已有的元素名称组件名**，例如：h2、H2都不行
        - 可以使用name配置项指定组件在开发者工具中呈现的名字
- 关于组件标签：
    - 第一种写法：`<school></school>`
    - 第二种写法：`<school/>`
    - 备注：不使用脚手架时，`<school/>`会导致后续组件不能渲染
- 一个简写方式：`const school = Vue.extend(options)`可简写为：`const school = options`
- 给最终渲染的HTML组件标签起名字
    
    ```jsx
    const hello=Vue.extend({
    name:'dddd';
    )}
    
    最后vue插件显示的是<dddd><dddd/>
    ```
    

### ****组件的嵌套****

```jsx
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>组件的嵌套</title>
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		<div id="root">
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false
		
		//定义student组件
		const student = Vue.extend({
			template:`
				<div>
					<h2>学生名称：{{name}}</h2>	
					<h2>学生年龄：{{age}}</h2>	
				</div>
			`,
			data(){
				return {
					name:'JOJO',
					age:20
				}
			}
		})

		//定义school组件
		const school = Vue.extend({
			template:`
				<div>
					<h2>学校名称：{{name}}</h2>	
					<h2>学校地址：{{address}}</h2>	
					<student></student>
				</div>
			`,
			components:{
				student
			},
			data(){
				return {
					name:'尚硅谷',
					address:'北京'
				}
			}
		})

		//定义hello组件
		const hello = Vue.extend({
			template:`
				<h1>{{msg}}</h1>
			`,
			data(){
				return {
					msg:"欢迎学习尚硅谷Vue教程！"
				}
			}
		})

		//定义app组件
		const app = Vue.extend({
			template:`
				<div>
					<hello></hello>
					<school></school>
				</div>
			`,
			components:{
				school,
				hello
			}
		})

		//创建vm
		new Vue({
			template:`
				<app></app>
			`,
			el:'#root',
			components:{
				app
			}
		})
	</script>
</html>
```

### ****VueComponent****

关于VueComponent：

1. **school组件本质是一个名为`VueComponent`的构造函数**，且不是程序员定义的，是`Vue.extend`生成的
2. **我们只需要写`<school/>`或`<school></school>`，Vue解析时会帮我们创建school组件的实例对象，即Vue帮我们执行的：`new VueComponent(options)`**
3. 特别注意：**每次调用`Vue.extend`，返回的都是一个全新的VueComponent！**
4. 关于this指向：
    1. **组件配置中：`data`函数、`methods`中的函数、`watch`中的函数、`computed`中的函数 它们的this均是VueComponent实例对象**
    2.  `new Vue(options)`配置中：`data`函数、`methods`中的函数、`watch`中的函数、`computed`中的函数 它们的this均是Vue实例对象
5. `VueComponent`的实例对象，以后简称vc（也可称之为：组件实例对象）
    
    `Vue`的实例对象，以后简称vm (仅限本笔记中）
    

### ****一个重要的内置关系****

1. 一个重要的内置关系：`**VueComponent.prototype.__proto__ === Vue.prototype**`
2. 为什么要有这个关系：让组件实例对象（vc）可以访问到 Vue 原型上的属性、方法
    
    ![Untitled](2%20Vue%E7%BB%84%E4%BB%B6%E5%8C%96%E7%BC%96%E7%A8%8B%207fb2b87d656e4dce935f2af97dfbfe1d/Untitled%202.png)
    

### 单文件组件

1. Student.vue
    
    ```
    <template>
      <div>
        <h2>{{name}}</h2>
      </div>
    </template>
    
    <script>
        export default {
            name:'Student',
            data() {
                return {
                    name:'tam'
                }
            },
        }
    </script>
    
    <style>
    
    </style>
    ```
    
2. School.vue
    
    ```
    <template>
      <div>
        <h2>{{name}}</h2>
      </div>
    </template>
    
    <script>
        export default {
            name:'School',
            data() {
                return {
                    name:'wcyu'
                }
            },
        }
    </script>
    
    <style>
    
    </style>
    ```
    
3. App.vue
    
    ```
    <template>
      <div>
        <School/>
        <Student/>
      </div>
    </template>
    
    <script>
        import Student from './Student.vue'
        import School from './School.vue'
        export default {
            name:'app',
            components:{
                School,
                Student
            }
        }
    </script>
    
    <style>
    
    </style>
    ```
    
4. main.js
    
    ```jsx
    import App from './app.vue'
    
    new Vue({
        el:'#root',
        template:`<app></app>`,
        components:{
            App,
        }
    })
    ```
    
5. index.html
    
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
        <script src="/js/vue.js"></script>
    </head>
    <body>
        <div id="root">
            
        </div>
        <script src="main.js"></script>
    </body>
    </html>
    ```