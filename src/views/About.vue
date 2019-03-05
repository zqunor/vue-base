<template>
  <div class="about">
    <h1>This is an about page</h1>
    <p>{{zqunor}}</p>
    <p>{{msg}}</p>
  </div>
</template>
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