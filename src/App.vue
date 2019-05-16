<template>
  <div id="app">
    <input type="button" value="Click Me" @click="btnClick();"/>
    <Tabscribe :stringTuning="['G', 'D', 'A', 'E']" :lines="lines" :lineText="lineText" :defaultBarCount="20" />
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
    btnClick() {
      console.log(this.lines);
      console.log(this.lineText);
    }
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
