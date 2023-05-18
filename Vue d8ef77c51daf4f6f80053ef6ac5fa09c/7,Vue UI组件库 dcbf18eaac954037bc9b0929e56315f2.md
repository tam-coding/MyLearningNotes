# 7,Vue UI组件库

### ****常用UI组件库****

- ****移动端常用UI组件库****
1. [Vant](https://youzan.github.io/vant) 
2. [Cube UI](https://didi.github.io/cube-ui) 
3. [Mint UI](http://mint-ui.github.io/) 
- **PC端常用UI组件库**
1. [Element UI](https://element.eleme.cn/)
2. [IView UI](https://www.iviewui.com/)

### ****element-ui基本使用****

1. 安装 element-ui：`npm i element-ui -S`

`src/main.js`:

```jsx
import Vue from 'vue'
import App from './App.vue'
//引入ElementUI组件库
import ElementUI from 'element-ui';
//引入ElementUI全部样式
import 'element-ui/lib/theme-chalk/index.css';

Vue.config.productionTip = false
//使用ElementUI
Vue.use(ElementUI)

new Vue({
    el:"#app",
    render: h => h(App),
})
```

`src/App.vue`:

```jsx
<template>
	<div>
		<br>
		<el-row>
			<el-button icon="el-icon-search" circle></el-button>
			<el-button type="primary" icon="el-icon-edit" circle></el-button>
			<el-button type="success" icon="el-icon-check" circle></el-button>
			<el-button type="info" icon="el-icon-message" circle></el-button>
			<el-button type="warning" icon="el-icon-star-off" circle></el-button>
			<el-button type="danger" icon="el-icon-delete" circle></el-button>
		</el-row>
	</div>
</template>

<script>
	export default {
		name:'App',
	}
</script>
```

### ****element-ui按需引入****

[ElementUI/babel-plugin-component: Modular element-ui build plugin for babel. (github.com)](https://github.com/ElementUI/babel-plugin-component)

1. 安装 babel-plugin-component：`npm install babel-plugin-component -D`
2. 修改 `babel-config-js`：
    
    ```jsx
    module.exports = {
      presets: [
        '@vue/cli-plugin-babel/preset',
        ["@babel/preset-env", { "modules": false }]
      ],
      plugins: [
        [
          "component",
          {
            "libraryName": "element-ui",
            "styleLibraryName": "theme-chalk"
          }
        ]
      ]
    }
    ```
    
3. `src/main.js`:
    
    ```jsx
    import Vue from 'vue'
    import App from './App.vue'
    //按需引入
    import { Button,Row } from 'element-ui'
    
    Vue.config.productionTip = false
    
    Vue.component(Button.name, Button);
    Vue.component(Row.name, Row);
    /* 或写为
     * Vue.use(Button)
     * Vue.use(Row)
     */
    
    new Vue({
        el:"#app",
        render: h => h(App),
    })
    ```