# vue入门

## 入门准备
```bash
# 安装node、npm、vue、vue-cli

# 安装最新稳定版vue
npm install vue
npm install -g @vue/cli
```

浏览器插件 `vue DevTools`,可以在浏览器中查看当前vue页面的组件
## vue-cli搭建项目

```bash
# 安装package.json里的依赖
npm install
```

### 开发中编译并热加载（实时更新）
```
npm run serve
```

### 编译，并最小化，发布到生产环境
```
npm run build
```

### Run your tests
```
npm run test
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).

## 应用小实例

### 引入新页面

（1)创建Info.vue文件，并自定义内容

```javascript
<template>
  <div class="info">
    <h1>This is info page</h1>
  </div>
</template>
<script>

export default {
  name: 'info',
}
</script>
```

（2）在入口页面App.vue 中通过`router-link`在页面中显示链接入口

```javascript
// App.vue
<router-link to="/info">Info</router-link>
```
此时有入口，会跳转，但是跳转后的页面并没有加载Info.vue的数据。

（3）定义路由（让页面访问到Info组件）

```javascript
//router.js
// 引入Info组件
import Info from './views/Info.vue'

export default new Router({
    routes: [
        // 定义路由
        {
            path: '/info',
            name: 'info',
            component: Info
        },
  ]
})
```
保存后直接就可以在浏览器中看到Info.vue页面中定义的内容。（实时刷新）

## Vuex介绍

单向数据流：Actions -》 State =》 View
用户操作=》状态发生变更=》视图数据变化

### 使用场景：

多个视图依赖于同一状态（例：菜单导航）

来自不同视图的行为需要变更同一状态（例：评论弹幕）



### vuex介绍：

- vue的状态管理模式

- 作用：组件状态集中管理

- 组件状态改变遵循统一的规则

### 使用

`store.js`数据存储

```javascript
export default new Vuex.Store({
// 组件状态，集中管理
  state: {

  },
// vue里面唯一可以修改state状态的方法集
  mutations: {

  },
  actions: {

  }
})
```

（1）在vue中实现组件间状态传递

```html
// Info.vue
// 添加按钮，触发事件
<button type="button" @click="add()">添加</button>
```
```javascript
export default {
  name: 'info',
  methods: {
    // 添加触发事件
    add () {
      console.log('add Event from info');
    }
  }
}
```