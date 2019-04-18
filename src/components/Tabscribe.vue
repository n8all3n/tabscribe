<template>
  <div class="tab-container">
    <div class="string" v-for="(item, stringIndex) in stringCount" :key="stringIndex">
        <div class="string-tuning">
            {{stringTuning[0]}}
        </div>
        <div class="fret" v-for="(item,fretIndex) in fretCount" :key="fretIndex">
            <div>
                String ({{stringIndex}} - Fret - {{fretIndex}}
            </div>
        </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Tabscribe',
  props: {
    msg: String
  },
  data() {
      return {
          stringCount: 7,
          fretCount: 24,
          stringTuning: []
      }
  },
  created: function () {
      for(var i = 0; i < this.stringCount; i++) {
          this.stringTuning[i] = 'E';
      }
  },
  methods: {
      
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .tab-container {
        display: table;
        width: 100%;
        border-top: 2px solid #4E5555;
        border-bottom: 2px solid #4E5555;
        border-collapse: collapse
    }

    .string {
        display: table-row;
        border-bottom: 2px solid #4E5555;
    }

    .fret, .string-tuning {
        display: table-cell;
        border-right: 2px solid #4E5555;
        border-left: 2px solid #4E5555;
        width: 3rem;
    }

</style>
