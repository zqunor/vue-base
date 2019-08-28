<template>
  <div id="app">
    <input v-model='content' type="text">
    <button @click='submit'>提交</button>
    <todo-item
      v-for="(item, index) of todoList"
      :key="index"
      :item="item"
      :index="index"
      @delete="finish"
      >
    </todo-item>
  </div>
</template>

<script>
import TodoItem from './components/TodoItem.vue'

export default {
  name: 'app',
  components: {
    TodoItem
  },
  data () {
    return {
      content: '',
      todoList: [],
    }
  },
  methods: {
    submit: function () {
      this.todoList.push(this.content)
      this.content = ''
    },
    finish: function (index) {
      this.todoList.splice(index, 1)
    }
  }
}
</script>

<style>

</style>
