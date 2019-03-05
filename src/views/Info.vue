<template>
  <div class="info">
    <h1>This is info page</h1>
    <button type="button" @click="add()">添加</button>
  </div>
</template>
<script>

export default {
  name: 'info',
  methods: {
    add () {
      console.log('add Event from info');
    }
  }
}
</script>