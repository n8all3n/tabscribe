<template>
    <div>
        <div class="tab-container">
            <div class="string" v-for="(item, stringIndex) in stringCount" v-bind:key="stringIndex">
                <div class="string-tuning">
                    {{stringTuning[stringIndex]}}
                </div>
                <!-- <div class="string-tuning">
                    -
                </div> -->
                <div class="fret" v-bind:class="getFretNoteClass(fretIndex, stringIndex)" v-for="(item,fretIndex) in fretCount" v-bind:key="fretIndex" v-on:click="fretClick(fretIndex + 1, stringIndex);">
                </div>
            </div>
        </div>
        <span>Active bar is {{activeBar}}</span>
        <button type="button" v-on:click="addNewBar();">Add Bar</button>
        <button type="button" v-on:click="deleteBar();">Delete Bar</button>
        <div class="notation-container">
            <div class="notation-string" v-for="(item, stringIndex) in stringCount" v-bind:key="stringIndex">
                <div class="notation-string-tuning"> 
                    <pre class="notation-text">{{stringTuning[stringIndex]}}|</pre>
                </div>
                <div class="notation-bar" v-on:click="setBarActive(barIndex);" v-on:mouseover="barMouseOver(barIndex);"  v-on:mouseleave="barMouseLeave();" v-for="(item,barIndex) in barText.length" :key="barIndex" v-bind:data-string="stringIndex + 1" v-bind:data-bar="barIndex + 1">
                    <pre class="notation-text" v-bind:class="[hoverBar === barIndex ? 'hovered-bar' : '', activeBar === barIndex ? 'active-bar' : '']">{{barText[barIndex][stringIndex]}}</pre>
                </div>
            </div>
        </div>

        
    </div>
</template>

<script>
/* eslint-disable */
import Vue from 'vue'
export default {
  name: 'Tabscribe',
  props: {
    msg: String
  },
  data() {
      return {
          stringCount: 4,
          fretCount: 24,
          stringTuning: [],
          hoverBar: null,
          activeBar: 0,
          barText: []
      }
  },
  created: function () {
      // Set default strings and tunings
      if (this.stringCount === 7) {
        this.stringTuning = ["E", "B", "G", "D", "A", "E", "B"];
      } else if (this.stringCount == 6) {
        this.stringTuning = ["E", "A", "D", "G", "B", "E"];
      } else if (this.stringCount === 4) {
          this.stringTuning = ["G", "D", "E", "A"];
      }

      // default some text into the bars
      this.barText[0] = ['-','-','-','-'];

  },
  methods: {
      /**
       * Checks if the current fret and string are selected.  If they are
       * return a class that indicates the fret is active
       */
      getFretNoteClass: function(fretIndex, stringIndex) {
        var currBar = this.barText[this.activeBar];
        var theString = currBar[stringIndex];

        if (parseInt(theString, 10) - 1 === fretIndex) {
            return 'fret-active';
        }

        return '';
      },
      fretClick: function(fretIndex, stringIndex) {
        // find the current bar
        var currentBarText = this.barText[this.activeBar];

        // clicking on an item that is already selected...clear the item
        if (currentBarText[stringIndex] === fretIndex.toString()) {
            currentBarText[stringIndex] = '-';
        } else {
            // clicked on a new fret set the fret
            currentBarText[stringIndex] = fretIndex.toString(); 
        }

        Vue.set(this.barText, this.activeBar, currentBarText);
      },
      deleteBar: function () {
        // don't delete if there's just one bar
        if (this.activeBar === 0) {
            return;
        }
          
        // get whatever the next bar would be after this one is deleted
        var nextActiveBar = this.activeBar - 1;
        this.$delete(this.barText, this.activeBar);

        // update the active bar now that a bar is deleted
        this.activeBar = nextActiveBar;
      },
      addNewBar: function() {
        var newBar = [];
        // add an array with the number of strings with dashes
        for(var i = 0; i < this.stringCount; i++) {
            newBar.push('-');
        }

        Vue.set(this.barText, this.barText.length, newBar);
      },
      barMouseOver: function(barIndex) {
        this.hoverBar = barIndex;
      },
      barMouseLeave: function() {
        this.hoverBar = null;
      },
      setBarActive: function(index) {
        this.activeBar = index;
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .fret-active {
        background:lightseagreen;
    }

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

    .fret:hover {
        background-color: red;
    }


    .notation-container {
        display: table;
    }

    .notation-string {
        display: table-row;
    }

    .notation-bar {
        display: table-cell;
    }

    .notation-text {
        margin: 0 !important;
    }

    .hovered-bar {
        color: red;
    }

    .active-bar {
        color: lightsalmon;
    }

</style>
