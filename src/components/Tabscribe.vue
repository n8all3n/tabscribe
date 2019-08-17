<template>
    <div class="tabscribe-container">
        <div class="tab-control-container sticky-top">
            <div class="tab-container">
                <div class="fret-number-container">
                    <div class="fret-number" v-for="(item, x) in fretCount + 3" v-bind:key="x">
                        <span v-if="x-1 > 0">{{x - 2}}</span>
                    </div>
                </div>
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
                    <div class="fret" v-bind:class="getFretNoteClass(fretIndex, stringIndex)" v-for="(item,fretIndex) in fretCount + 1" v-bind:key="fretIndex" v-on:click="fretClick(fretIndex, stringIndex);">
                    </div>
                </div>
            </div>
            <div class="btn-group control-buttons">
                <button type="button" class="btn btn-primary" v-on:click="addNewLine();">Add Line</button>
                <button type="button" class="btn btn-primary" v-on:click="deleteLine();">Delete Line</button>
                <button type="button" class="btn btn-primary" v-on:click="addNewBar();">Add Bar</button>
                <button type="button" class="btn btn-primary" v-on:click="backOneBar();">Back a Bar</button>
                <button type="button" class="btn btn-primary" v-on:click="nextBar();">Next Bar</button>
            </div>
        </div>
        <div class="notation-groups">
            <div class="notation-container" v-for="(currentLine, lineIndex) in lines.length" v-bind:key="lineIndex">
                <div class="notation-string" v-for="(string, stringIndex) in stringCount" v-bind:key="stringIndex">
                    <div class="notation-string-tuning"> 
                        <span class="notation-text">{{stringTuning[stringIndex]}}|</span>
                       
                        <div v-bind:class="itemIndex === selectedItem && activeLine === lineIndex ? 'active-bar' : ''" @click="itemClick(itemIndex, lineIndex)" class="notation-text" v-for="(currItem, itemIndex) in lines[lineIndex][stringIndex].split(',')" v-bind:key="itemIndex">
                            {{currItem}}
                        </div>
            
                    </div>
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
      stringTuning: {
        type: Array,
        required: true,
        default: () => []
      },
      lines: {
        type: Array,
        required: true,
        default: () => []
      },
      lineText: {
        type: Array,
        required: true,
        default: () => []
      },
      fretCount : {
        type: Number,
        required: false,
        default: 24
      },
      defaultBarCount :{
        type: Number,
        required: false,
        default: 150
      }
  },
  data() {
      return {
          activeBar: 0,
          activeLine: 0,
          selectedItem: 0
      }
  },
  mounted: function () {
      var vm = this;
      document.onkeydown = function(event) {
        switch (event.keyCode) {
           case 37: // Left arrow
                vm.backOneBar();
              break;
           case 39: // Right arrow
                vm.nextBar();
              break;
        }
    };
  },
  computed: {
      stringCount: function() {
          return this.stringTuning.length;
      }
  },
  created: function () {
    this.lines[0] = [];

    for (var i = 0; i < this.stringCount; i++) {
        var item = [];
        var defaultBar = this.createDefaultBar();    
        this.lines[0].push(defaultBar);
    }

    this.lineText.push('');
  },
  methods: {
    itemClick(itemIndex, lineIndex) {
        this.selectedItem = itemIndex;
        this.activeLine = lineIndex;
        
    },
    createDefaultBar() {
        var newBar = [];
        for(var i = 0; i < this.defaultBarCount; i++) {
            newBar.push('-');

            if (i + 1 !== this.defaultBarCount) {
                newBar.push(',');
            }
            
        }
        return newBar.join('');
    },

    setStringTuning(newTuning, stringIndex) {
        Vue.set(this.stringTuning, stringIndex, newTuning);
    },   
    addNewLine: function() {
        var newLine = [];

        for (var i = 0; i < this.stringCount; i++) {
            var item = [];
            var defaultBar = this.createDefaultBar();    
            newLine.push(defaultBar);
        }

        var nextIndex = this.lines.length;
        
        Vue.set(this.lines, nextIndex, newLine);


        this.lineText.push('');
    },
    deleteLine: function() {
        if (this.lines.length > 1) {
            
            this.$delete(this.lines, this.activeLine);
            this.$delete(this.lineText, this.activeLine);
            
            if (this.activeLine > 0) {
                this.activeLine--;
            }

            this.activeBar = 0;      
        }
    },
    setSpecialNotationValue(notation, stringIndex) {
        var currLine = this.lines[this.activeLine];
        var currString = currLine[stringIndex];
        
        var currArray = currString.split(',');

        var currItem = parseInt(currArray[this.selectedItem], 10);

        currArray[this.selectedItem] = notation;

        currLine[stringIndex] = currArray.join(',');
        
        Vue.set(this.lines, this.activeLine, currLine);
    },
    getSpecialNotationVal(stringIndex) {
        var currLine = this.lines[this.activeLine];
        var theString = currLine[stringIndex];

        var strSplit = theString.split(',');

        var item = strSplit[this.selectedItem];

        if (isNaN(item)) {
            return item;
        }

        return '-';
    },

      /**
       * Checks if the current fret and string are selected.  If they are
       * return a class that indicates the fret is active
       */
      getFretNoteClass: function(fretIndex, stringIndex) {

        var currLine = this.lines[this.activeLine];
        var theString = currLine[stringIndex];

        var strSplit = theString.split(',');

        var item = strSplit[this.selectedItem];

        var parsedItem = parseInt(item, 10);

        if (parsedItem >= 0 && parsedItem === fretIndex) {
             return 'fret-active';
        }

        return '';
      },
      fretClick: function(fretIndex, stringIndex) {
        var currLine = this.lines[this.activeLine];
        var currString = currLine[stringIndex];
        
        var currArray = currString.split(',');

        var currItem = parseInt(currArray[this.selectedItem], 10);

        if (currItem === fretIndex) {
            currArray[this.selectedItem] = '-';
        } else {
            currArray[this.selectedItem] = fretIndex;
        }

        currLine[stringIndex] = currArray.join(',');
        
        Vue.set(this.lines, this.activeLine, currLine);
      },
      nextBar: function() {
        if (this.selectedItem + 1 >= this.lines[this.activeLine][0].split(',').length) {
            return;
        }

        this.selectedItem +=1;
          
      },
      backOneBar: function() {
        if (this.selectedItem === 0) {
            return;
        }

        this.selectedItem -=1;     
      },
      setBarActive: function(lineIndex, barIndex) {
        this.activeBar = barIndex;
        this.activeLine = lineIndex;
      }
  }
}
</script>

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

    .fret-number-container {
        display: table-row;
        border-bottom: 2px solid #4E5555;
    }

    .fret-number {
        display: table-cell;
        border-right: 2px solid #4E5555;
        border-left: 2px solid #4E5555;
        width: 3rem;
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
        margin-top: .5em;
        margin-bottom: 1em;
    }

    .text-notes {
        width: 100%;
    }

    .notation-string {
        display: table-row;
    }

    .notation-bar {
        display: table-cell;
    }

    .notation-text {
        margin: 0 !important;
        display: inline-block;
    }

    .notation-text, .text-notes {
        font-family: Consolas,monospace;
        font-size: 14px;
        font-weight: 400;
    }


    .hovered-bar {
        color: red;
    }

    .active-bar {
        background-color: black;
        color: white;
    }

</style>
