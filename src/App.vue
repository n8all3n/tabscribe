<template>
  <div id="app">
    <Tabscribe />
  </div>
</template>

<script>
import Tabscribe from './components/Tabscribe.vue'

export default {
  name: 'app',
  components: {
    Tabscribe
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
