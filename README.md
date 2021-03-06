# vue入门

学习资源

[慕课网](https://www.imooc.com/learn/1091)

## 基本使用语法

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

### 使用场景

多个视图依赖于同一状态（例：菜单导航）

来自不同视图的行为需要变更同一状态（例：评论弹幕）

### vuex介绍

- vue的状态管理模式

- 作用：组件状态集中管理

- 组件状态改变遵循统一的规则

### 使用

`store.js`数据状态变更文件，默认格式

```javascript
import Vue from 'vue'
import Vuex from 'vuex'

// 给vue对象绑定Vuex组件
Vue.use(Vuex)

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

（1）vue中添加事件

```html
<!-- Info.vue -->
<!-- 添加按钮，触发事件 -->
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

(2)在Info组件中使用vuex，实现组件间状态传递

```javascript
// store.js中定义状态变更事件
export default new Vuex.Store({
  state: {
    // 定义变量，组件中公用的状态
    count: 0
  },
  mutations: {
    // 定义改变状态的方法
    increase () {
      this.state.count++
    }
  },
  actions: {
  }
})
```

```javascript
// Info.vue中引入store.js, @符号表示src目录
import store from '@/store'
export default {
  name: 'info',
  // 引入store
  store,
  methods: {
    add () {
      console.log('add Event from info');
      // 触发vuex的mutations事件，提交组件状态的修改
      store.commit('increase')
    }
  }
}
```

实现效果（借助chrome插件vue DevTools）
![](http://ww1.sinaimg.cn/large/005EgYNMly1g0se98lh7dj31rw0u0420.jpg)

（3）将Info.vue组件中`count`的值传递到About.vue中

```html
<templete>
   <p>{{zqunor}}</p>
   <p>{{msg}}</p>
</templete>

<!-- About.vue中引入store.js, @符号表示src目录 -->
<script>
import store from '@/store'
export default {
    name: 'about',
    store,
    data () {
        return {
            zqunor: store.state.count,
            msg: ++store.state.count
        }
    }
}
</script>
```

浏览器查看，可观察到info中点击事件，变更count值后，切换到About页面的时候，也会显示info中count对应的值

![](https://ws1.sinaimg.cn/large/005EgYNMly1g0sesoo2k1j322m0uedkr.jpg)

### 调试

1.Vue的chrome插件`Vue DevTools`

[chrome网上应用商店入口](https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd)

2.常用的语法：`console.log` `alert` `debuger`

`debuger`会设置成断点，这样运行时就可以在浏览器的console面板中输出需要调试的变量

3.定义全局变量，直接在console面板输出需要调试的变量的值

```javascript
// vue的生命周期，组件挂载完成之后会执行该方法
mounted () {
  // 代表windows.vue代表this所在文件的组件
  windows.vue = this
}
```

### Git版本管理

(1) 回退版本

1) 回退到上一次提交之前

```bash
git reset --hard head^
```

2) 回退错了，要撤销回退

```bash
#查看所有提交历史（git log只能看到当前版本之前的提交，回退后就看不到回退前的提交历史了）
git relog
#然后再回退到相应版本
git reset --hard HEAD@{1}
```

(2) 删除远程分支

```bash
#删除本地dev分支
git branch -D dev
#将本地dev分支的修改提交到远程
git push origin :dev
```

## 应用Demo1

1、需求描述

```info
三个部分
1.展示列表 （可以被点击选中）
2.添加按钮 （将选中的列表元素添加到被展示列）
3.展示被添加的列表  （选中的元素动态添加到这里）
```

2、实现效果
![](https://ws1.sinaimg.cn/large/005EgYNMly1g0vwpbk3vqj30t80ns3zf.jpg)

3、需求梳理

(1)业务拆分
- 展示列表
  - 列表元素展示
  - 点击事件
    - 点击后改变背景色
    - 获取点击的元素值
- 按钮
  - 按钮元素
  - 点击事件
    - 获取被点击元素的值
- 被添加列表
  - 数组
  - 追加被点击的元素

(2)元素|方法定义

变量：

- 被点击元素索引值
- 展示列表（数组）
- 被添加列表（数组）

方法：

- 元素选择事件
- 按钮点击事件

4、代码实现

(1)创建组件`view/Demo.vue`，添加路由, 添加首页入口

```javascript
// router.js
// 导入组件
import Demo1 from './views/Demo1.vue'
// 添加路由
{
  path: '/demo1',
  name: 'demo1',
  component: Demo1
}
```

```html
<!-- App.vue -->
| <router-link to="/demo1">Demo1</router-link>
```

(2)定义模板

```html
<template>
  <div>
    <ul>
      <li v-for="(item, index) in list"
        :key="index">
        {{item}}
      </li>
    </ul>
  <button type="button" @click="add()">添加</button>
  <ul>
    <li v-for="(item, index) in target"
      :key="index">
      {{item}}
    </li>
    </ul>
  </div>
</template>
```

(3)增加样式（选中的元素的背景变化）

