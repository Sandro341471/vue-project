<template>
  <div id="app">
=    <slotmachine/>
  </div>
</template>

<script>
import slotmachine from './components/slotmachine.vue'

export default {
  name: 'app',
  components: {
    slotmachine
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
}
</style>
