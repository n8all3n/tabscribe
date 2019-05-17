<template>
  <div id="app">
    <!-- https://tabs.ultimate-guitar.com/tab/nirvana/smells_like_teen_spirit_tabs_202727 -->
    <input type="button" value="Click Me" @click="btnClick();"/>
    <input type="button" value="Get Json" @click="getJson();"/>
    <Tabscribe :stringTuning="['E','B','G', 'D', 'A', 'E']" :lines="lines" :lineText="lineText" />
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
    getJson() {
      var currFile = require('./songs/Song1.json');
      this.lines = currFile.lines; //Object.freeze(currFile.lines);
      this.lineText = currFile.lineText; //Object.freeze(currFile.lineText);
    },
    btnClick() {
      var finalObj ={
        lines: this.lines,
        lineText: this.lineText
      }

      console.log(JSON.stringify(finalObj));
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
