<template>
    <div class="tabscribe-container">
        <div class="tab-control-container sticky-top">
            <div class="tab-container">
                <div class="string" v-for="(item, stringIndex) in stringCount" v-bind:key="stringIndex">
                    <div class="string-tuning">
                        {{stringTuning[stringIndex]}}
                    </div>
                    <div class="adv-container">
                    <div class="btn-group">
                            <button class="btn btn-primary btn-sm dropdown-toggle" type="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                                {{getSpecialNotationVal(stringIndex)}}
                            </button>
                            <div class="dropdown-menu">
                                <a class="dropdown-item" href="javascript:void(0);" @click="setSpecialNotationValue('-', stringIndex);">-</a>
                                <a class="dropdown-item" href="javascript:void(0);" @click="setSpecialNotationValue('h', stringIndex);">h</a>
                                <a class="dropdown-item" href="javascript:void(0);" @click="setSpecialNotationValue('p', stringIndex);">p</a>
                                <a class="dropdown-item" href="javascript:void(0);" @click="setSpecialNotationValue('b', stringIndex);">b</a>
                                <a class="dropdown-item" href="javascript:void(0);" @click="setSpecialNotationValue('r', stringIndex);">r</a>
                                <a class="dropdown-item" href="javascript:void(0);" @click="setSpecialNotationValue('/', stringIndex);">/</a>
                                <a class="dropdown-item" href="javascript:void(0);" @click="setSpecialNotationValue('\\', stringIndex);">\</a>
                                <a class="dropdown-item" href="javascript:void(0);" @click="setSpecialNotationValue('v', stringIndex);">v</a>
                                <a class="dropdown-item" href="javascript:void(0);" @click="setSpecialNotationValue('t', stringIndex);">t</a>
                                <a class="dropdown-item" href="javascript:void(0);" @click="setSpecialNotationValue('x', stringIndex);">x</a>                  
                            </div>
                        </div>
                    </div>
                    <div class="fret" v-bind:class="getFretNoteClass(fretIndex, stringIndex)" v-for="(item,fretIndex) in fretCount" v-bind:key="fretIndex" v-on:click="fretClick(fretIndex + 1, stringIndex);">
                    </div>
                </div>
            </div>
            <div class="btn-group control-buttons">
                <button type="button" class="btn btn-primary" v-on:click="addNewLine();">Add Line</button>
                <button type="button" class="btn btn-primary" v-on:click="deleteLine();">Delete Line</button>
                <button type="button" class="btn btn-primary" v-on:click="addNewBar();">Add Bar</button>
                <button type="button" class="btn btn-primary" v-on:click="backOneBar();">Back a Bar</button>
                <button type="button" class="btn btn-primary" v-on:click="nextBar();">Next Bar</button>
                <button type="button" class="btn btn-primary" v-on:click="deleteBar();">Delete Bar</button>
            </div>
        </div>
         

        <div class="notation-groups">
            <div class="notation-container" v-for="(currentLine, lineIndex) in lines.length" v-bind:key="lineIndex">
                <div class="notation-string" v-for="(string, stringIndex) in stringCount" v-bind:key="stringIndex">
                    <div class="notation-string-tuning"> 
                        <pre class="notation-text">{{stringTuning[stringIndex]}}|</pre>
                    </div>

                    <div class="notation-bar" v-bind:class="activeLine === lineIndex && activeBar === barIndex ? 'active-bar' : ''"
                        v-for="(item,barIndex) in lines[lineIndex].length" :key="barIndex"
                        v-on:click="setBarActive(lineIndex, barIndex);" 
                        v-on:mouseover="barMouseOver(barIndex);" 
                        v-on:mouseleave="barMouseLeave();">

                        <debug :item="lines[lineIndex][barIndex][stringIndex]"/>
                        <span class="notation-text"
                            v-bind:class="[hoverBar === barIndex ? 'hovered-bar' : '']">
                            {{lines[lineIndex][barIndex][stringIndex]}}</span>
                    </div>
                </div>
            </div> 
        </div>

    </div>
</template>

<script>
/* eslint-disable */
import Vue from 'vue'
import debug from '../components/Debug.vue'
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
          activeLine: 0,
          lines:[],
          barText: []
      }
  },
  components: {
    debug
  },
  created: function () {
    // Set default strings and tunings
    if (this.stringCount === 7) {
    this.stringTuning = ["E", "B", "G", "D", "A", "E", "B"];
    } else if (this.stringCount == 6) {
    this.stringTuning = ["E", "A", "D", "G", "B", "E"];
    } else if (this.stringCount === 4) {
        this.stringTuning = ["G", "D", "A", "E"];
    }

    this.lines[0] = [];
    this.lines[0].push(['-','-','-','-']);


    //this.lines[0][0] = ['-','-','-','-'];

    // default some text into the bars
    this.barText[0] = ['-','-','-','-'];

  },
  methods: {
    addNewLine: function() {
        var newBar = ['-','-','-','-'];
        
        var newArray = [];
        newArray[0] = newBar;
        
        var nextIndex = this.lines.length;

        Vue.set(this.lines, nextIndex, newArray);
    },
    deleteLine: function() {
        if (this.lines.length > 1) {
            this.$delete(this.lines, this.activeLine);

            if (this.activeLine > 0) {
                this.activeLine--;
            }

            this.activeBar = 0;
        }
    },
    setSpecialNotationValue(notation, stringIndex) {
        var currLine = this.lines[this.activeLine];
        currLine[this.activeBar][stringIndex] = notation;
        Vue.set(this.lines, this.activeLine, currLine);

    },
    getSpecialNotationVal(stringIndex) {
        var currBar = this.lines[this.activeLine][this.activeBar];
        var theString = currBar[stringIndex];

        var parsedVal = parseInt(theString, 10);
        if (isNaN(parsedVal) && theString !== '-') {
            return theString;
        }

        return '';
    },

      /**
       * Checks if the current fret and string are selected.  If they are
       * return a class that indicates the fret is active
       */
      getFretNoteClass: function(fretIndex, stringIndex) {
        var currBar =  this.lines[this.activeLine][this.activeBar];
        var theString = currBar[stringIndex];

        if (parseInt(theString, 10) - 1 === fretIndex) {
            return 'fret-active';
        }

        return '';
      },
      fretClick: function(fretIndex, stringIndex) {
        var currLine = this.lines[this.activeLine];

        if (currLine[this.activeBar][stringIndex] === fretIndex) {
            currLine[this.activeBar][stringIndex] = '-';
        } else {
            currLine[this.activeBar][stringIndex] = fretIndex;
        }

        Vue.set(this.lines, this.activeLine, currLine);      


        // // find the current bar
        // var currentBarText = this.barText[this.activeBar];

        // // clicking on an item that is already selected...clear the item
        // if (currentBarText[stringIndex] === fretIndex.toString()) {
        //     currentBarText[stringIndex] = '-';
        // } else {
        //     // clicked on a new fret set the fret
        //     currentBarText[stringIndex] = fretIndex.toString(); 
        // }

        // Vue.set(this.barText, this.activeBar, currentBarText);
      },
      deleteBar: function () {
        // don't delete if there's just one bar
        if (this.lines[this.activeLine].length === 1) {
            return;
        }
          
        // get whatever the next bar would be after this one is deleted
        var nextActiveBar;
        if (this.activeBar === 0) {
            nextActiveBar = 0;
        } else {
            nextActiveBar = this.activeBar - 1;
        }
        
        this.$delete(this.lines[this.activeLine], this.activeBar);
        
        // update the active bar now that a bar is deleted
        this.activeBar = nextActiveBar;
      },
      addNewBar: function() {
        var newBar = [];
        // add an array with the number of strings with dashes
        for(var i = 0; i < this.stringCount; i++) {
            newBar.push('-');
        }

        //this.lines[this.activeLine].push(newBar);

        var nextish = this.lines[this.activeLine];
        nextish.push(newBar);

        Vue.set(this.lines, this.activeLine, nextish);


        //Vue.set(this.barText, this.barText.length, newBar);
      },
      nextBar: function() {
        if (this.activeBar + 1 >= this.lines[this.activeLine].length) {
            return;
        }

        this.activeBar +=1;
          
      },
      backOneBar: function() {
        if (this.activeBar === 0) {
            return;
        }

        this.activeBar -=1;     
      },
      barMouseOver: function(barIndex) {
        this.hoverBar = barIndex;
      },
      barMouseLeave: function() {
        this.hoverBar = null;
      },
      setBarActive: function(lineIndex, barIndex) {
        this.activeBar = barIndex;
        this.activeLine = lineIndex;
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .control-buttons {
        padding-top: 1em;
    }

    .adv-container {
        padding: .1em;
    }
    .fret-active {
        background:#007BFF;
    }

    .tab-control-container {
        background-color: white;
    }

    .tab-container {
        display: table;
        width: 100%;
        border-top: 2px solid #4E5555;
        border-bottom: 2px solid #4E5555;
        border-collapse: collapse;
        
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
        margin-bottom: 1rem;
    }

    /* .notation-groups {
        height: 15em;
        overflow: auto;
    } */

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
        background-color: black;
        color: white;
    }

</style>
