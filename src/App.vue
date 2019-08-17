<template>
  <div id="app">
    <h1>Tabscribe</h1>
    <Tabscribe :stringTuning="['E','B','G', 'D', 'A', 'E']" :lines="lines" :lineText="lineText" :defaultBarCount="20" />
  </div>
</template>

<script>
import Tabscribe from './components/Tabscribe.vue'
/* eslint-disable */
export default {
  name: 'app',
  components: {
    Tabscribe
  },
   data() {
      return {
        lines: [],
        lineText: []
      }
  },
  methods: {
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
