<template>
  <div>
    <ul>
      <li v-for="(item, index) in list"
        @click="choose(index)"
        :class="{active: index === current}"
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
<script>
export default {
  name: 'Demo1',
  data () {
    return {
      list: [1, 2, 3, 4, 5, 6, 7, 8, 9],
      current: '',
      target: []
    }
  },
  methods: {
    choose (index) {
      this.current = index
      console.log(index)
    },
    add () {
      if (this.current === '') {
        return;
      }
      this.target.push(this.list[this.current])
      this.current = ''
    }
  }
}
</script>
<style scoped>
/* li{
  list-style: none
} */
li.active {
  background: green;
}
</style>
