<template>
  <div id="app">
    <TreeEx/>
  </div>
</template>

<script>
import TreeEx from './components/TreeEx.vue'

export default {
  name: 'app',
  components: {
      TreeEx
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  width: 30vw;
}
</style>
