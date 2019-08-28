<template>
  <div>
    <input type="checkbox" @click="checkout"/> {{item}}
  </div>
</template>

<script>
export default {
  name: 'todo-item',
  props: {
    item: String,
    index: Number,
  },
  methods: {
    checkout: function () {
      this.$emit('delete', this.index)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
