# Vue入门到TodoList练手

## 项目安装

```bash
npm install
```

### 编译和热加载(开发调试)

```bash
npm run serve
```

### 编译并压缩(生产环境)

```bash
npm run build
```

### 测试

```bash
npm run test
```

### 自定义配置

See [Configuration Reference](https://cli.vuejs.org/config/).

## TodoList

### 学习资料

[慕课网 - vue2.5入门](https://www.imooc.com/learn/980)

### 基础语法

#### 示例代码1

```html
<div id="root">
    <h1>hello {{msg}}</h1>
</div>
<script>
    new Vue({
        el: "#root",
        template: '<h1>hello {{msg}}</h1>',
        data: {
            msg: "world"
        }
    })
</script>
```

**挂载点**：vue实例绑定的dom节点

**模板**：挂载点输出的内容，可以直接在挂载点内部编写，也可以通过template属性实现。

> 示例代码1中div标签内部的`<h1>hello {{msg}}</h1>`和vue中的 ` template: '<h1>hello {{msg}}</h1>'` 效果一致

**实例**: 指定挂载点、指定模板、绑定数据后可以自动结合模板、数据生成最终要展示的内容，并放到挂载点之中

**插值表达式**：两个花括号包裹一个变量

> {{msg}} 就是一个插值表达式

#### 示例代码2

```html
<div id="root">
    <div v-html="msg" v-on:click="handleClick"></div>
</div>
<script>
    new Vue({
        el: "#root",
        data: {
            msg: "<h1>hello</h1>"
        },
        methods: {
            handleClick: function () {
                this.msg = 'world'
            }
        }
    })
</script>
```

**指令**

- `v-html`: 以html格式解析变量
- `v-text`: 以文本格式输出变量
- `v-on`: 事件绑定，简写为`@`
- `v-bind`: 属性绑定，简写为`:`
- `v-model`: 双向数据绑定
- `v-if`: 不符合条件时整个元素dom都清除，符合条件时再重新创建该dom
- `v-show`: 不符合条件时，dom元素增加`display:none`css属性，
- `v-for`: 用法`(item, index) in/of list`，`item`是元素列表每个元素值，`index`是每个元素的索引值

**事件**

> `click`就是一个点击事件, v-on:click表示绑定一个点击事件，简写方式为@click

#### 示例代码3

```html
<div id='app'>
    <h1 v-html='msg' v-on:click='handleClick' v-bind:title='title1'></h1>
    <input v-model='content'/>
    <div>{{content}}</div>
</div>

<script>
    new Vue({
        el: "#app",
        data: {
            msg: 'hello world',
            title1: 'this is a title',
            content: 'this is content'
        },
        methods: {
            handleClick: function () {
                this.msg = 'ready to learn'
            }
        }
    })
</script>
```

**双向数据绑定**: 当页面数据变化时，变量的值也会同时变化

#### 示例代码4

```html
<div id='app'>
    姓 <input v-model="firstName">
    名 <input v-model="lastName">
    <div>{{fullName}}</div>
</div>

<script>
    new Vue({
        el: "#app",
        data: {
            firstName: '',
            lastName: '',
        },
        // 计算属性
        computed: {
            fullName : function () {
                return this.firstName + ' ' + this.lastName
            }
        },
        // 侦听器
        watch: {
            firstName: function () {
                this.count ++
            },
            lastName: function () {
                this.count ++
            },
            //等价于
            fullName: function () {
                this.count ++
            },
        }
    })
</script>
```

**计算属性**: 一个属性的值是通过其他属性计算得来

**侦听器**: 监听一个属性的变化后进行数据处理

#### 示例代码5

```html
<input v-model="todo"/>
    <button @click="submit">提交</button>
    <div v-for="(item, index) in todoList" :key="index" v-model="todoList">
        <input type="checkbox" /> {{item}}
    </div>
</div>

<script>
    new Vue({
        el: "#app",
        data: {
            todo: '',
            todoList: []
        },
        methods: {
            submit: function () {
                this.todoList.push(this.todo)
                this.todo = ''
            }
        },

    })
</script>
```

效果图

![](https://ws1.sinaimg.cn/mw690/005EgYNMly1g6d7waupmcj30dx04smx6.jpg)

- 给列表增加元素: `push()`

### 组件

页面中的某一部分

**全局组件**: 在挂载点中可以直接使用

```js
Vue.component('todo-item', {
    template: "<li>item</li>"
})
```

**局部组件**: 需要在实例中声明才能在挂载点中使用

```js
var TodoItem = {
    template: "<li>item</li>"
}
new Vue({
    // ...
    // 注册局部组件
    components: {
        'todo-item': todoItem
    },
    // ...
})
```

**组件传值**: 接收外部传递的属性值

外部传值

```html
<todo-item v-for="(item, index) in todoList" :key="index" :content="item"></todo-item>
```

组件接收

```js
Vue.component('todo-item', {
    props: ["content"],
    template: "<li>{{content}}</li>"
})
```

**父子组件通信**：

子组件=>父组件：子组件通过发布订阅模式，向父组件传递数据

父组件=>子组件：父组件的模板中增加属性，子组件中接收属性

父组件的模板中使用子组件模板:

```html
<!-- @delete="checkout"用于订阅子组件的delete事件，并触发父组件的checkout方法-->
<todo-item
    v-for="(item, index) in todoList"
    :key="index"
    :content="item"
    :index="index"
    @delete="checkout"
></todo-item>
```

子组件：

```js
Vue.component('todo-item', {
    // 接收属性值
    props: ["content", "index"],
    template: "<li @click='checkout'>{{content}}</li>",
    methods: {
        // 子组件模板中的checkout事件
        checkout: function () {
            // 发布订阅模式, 发布delete事件
            this.$emit('delete', this.index)
        }
    }
})
```

父组件：

```js
new Vue({
    el: "#app",
    data: {
        todo: '',
        todoList: []
    },
    methods: {
        submit: function () {
            this.todoList.push(this.todo)
            this.todo = ''
        },
        checkout: function (index) {
            this.todoList.splice(index, 1)
        }
    },

})
```
