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
                    <div class="fret" v-bind:class="getFretNoteClass(fretIndex -1, stringIndex)" v-for="(item,fretIndex) in fretCount + 1" v-bind:key="fretIndex" v-on:click="fretClick(fretIndex, stringIndex);">
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
        <!-- <div class="notation-groups">
            <div class="notation-container" v-for="(currentLine, lineIndex) in lines.length" v-bind:key="lineIndex">
                <div>
                    <input type="text" class="text-notes" v-model="lineText[lineIndex]" placeholder="Text describing the current line...">
                </div>
                <div class="notation-string" v-for="(string, stringIndex) in stringCount" v-bind:key="stringIndex">
                    <div class="notation-string-tuning"> 
                        <pre class="notation-text">{{stringTuning[stringIndex]}}|</pre>
                    </div>
                    <div class="notation-bar" v-bind:class="activeLine === lineIndex && activeBar === barIndex ? 'active-bar' : ''"
                        v-for="(item,barIndex) in lines[lineIndex].length" :key="barIndex"
                        v-on:click="setBarActive(lineIndex, barIndex);" 
                        v-on:mouseover="barMouseOver(barIndex);" 
                        v-on:mouseleave="barMouseLeave();">

                    
                        <span class="notation-text"
                            v-bind:class="[hoverBar === barIndex ? 'hovered-bar' : '']">
                            {{lines[lineIndex][barIndex][stringIndex]}}</span>
                    </div>
                </div>
            </div> 
        </div> -->
<svg
   xmlns:dc="http://purl.org/dc/elements/1.1/"
   xmlns:cc="http://creativecommons.org/ns#"
   xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
   xmlns:svg="http://www.w3.org/2000/svg"
   xmlns="http://www.w3.org/2000/svg"
   xmlns:sodipodi="http://sodipodi.sourceforge.net/DTD/sodipodi-0.dtd"
   xmlns:inkscape="http://www.inkscape.org/namespaces/inkscape"
   version="1.2"
   width="317.7525mm"
   height="23.57753mm"
   viewBox="0 0 180.81869 13.416915"
   id="svg348"
   sodipodi:docname="GuitTab.svg"
   inkscape:version="0.92.1 r15371">
  <metadata
     id="metadata354">
    <rdf:RDF>
      <cc:Work
         rdf:about="">
        <dc:format>image/svg+xml</dc:format>
        <dc:type
           rdf:resource="http://purl.org/dc/dcmitype/StillImage" />
        <dc:title></dc:title>
      </cc:Work>
    </rdf:RDF>
  </metadata>
  <defs
     id="defs352" />
  <sodipodi:namedview
     pagecolor="#ffffff"
     bordercolor="#666666"
     borderopacity="1"
     objecttolerance="10"
     gridtolerance="10"
     guidetolerance="10"
     inkscape:pageopacity="0"
     inkscape:pageshadow="2"
     inkscape:window-width="1920"
     inkscape:window-height="1001"
     id="namedview350"
     showgrid="false"
     inkscape:zoom="2.8284271"
     inkscape:cx="133.71628"
     inkscape:cy="72.615461"
     inkscape:window-x="-9"
     inkscape:window-y="-9"
     inkscape:window-maximized="1"
     inkscape:current-layer="svg348"
     fit-margin-top="0"
     fit-margin-left="0"
     fit-margin-right="0"
     fit-margin-bottom="0"
     showguides="false" />
  <rect
     style="fill:#ff0035;fill-opacity:0.38039216;stroke:#ff0000;stroke-width:0.39944384;stroke-miterlimit:4;stroke-dasharray:none;stroke-dashoffset:0;stroke-opacity:0.58823529"
     id="rect4672"
     width="3.2900002"
     height="13.017471"
     x="5.5"
     y="0.19972192" />
  <!-- Page: 1/1 -->
  <text
     font-size="2.2001"
     id="text4"
     style="font-size:2.20009995px;font-family:'Century Schoolbook L';text-anchor:start;fill:currentColor"
     x="14.211811"
     y="-14.297421">
    <tspan
       id="tspan2" />
  </text>
  <line
     x1="0.25316724"
     y1="12.048759"
     x2="180.70462"
     y2="12.048759"
     id="line50"
     style="stroke:currentColor;stroke-width:0.22496055;stroke-linecap:round;stroke-linejoin:round" />
  <line
     x1="0.25316724"
     y1="9.8882132"
     x2="180.70462"
     y2="9.8882132"
     id="line52"
     style="stroke:currentColor;stroke-width:0.22496055;stroke-linecap:round;stroke-linejoin:round" />
  <line
     x1="0.25316802"
     y1="7.7276664"
     x2="180.70462"
     y2="7.7276664"
     id="line54"
     style="stroke:currentColor;stroke-width:0.22496054;stroke-linecap:round;stroke-linejoin:round" />
  <line
     x1="0.25316724"
     y1="5.567194"
     x2="180.70462"
     y2="5.567194"
     id="line56"
     style="stroke:currentColor;stroke-width:0.22496055;stroke-linecap:round;stroke-linejoin:round" />
  <line
     x1="0.25316724"
     y1="3.406517"
     x2="180.70462"
     y2="3.406517"
     id="line58"
     style="stroke:currentColor;stroke-width:0.22496055;stroke-linecap:round;stroke-linejoin:round" />
  <line
     x1="0.24860002"
     y1="1.2460279"
     x2="180.70004"
     y2="1.2460279"
     id="line60"
     style="stroke:currentColor;stroke-width:0.22496054;stroke-linecap:round;stroke-linejoin:round" />
  <text
     font-weight="bold"
     font-size="1.7461"
     id="text70"
     style="font-weight:bold;font-size:1.74609995px;font-family:'Century Schoolbook L';text-anchor:start;fill:currentColor"
     x="59.611511"
     y="2.9625783">
    <tspan
       id="tspan68" />
  </text>
  <rect
     x="0"
     y="1.2815297"
     width="0.23045826"
     height="10.839248"
     ry="0.029676242"
     id="rect274"
     style="fill:currentColor;stroke-width:0.92460364" />
  <path
     d="m 2.7770226,9.0960119 c 0,0.138275 -0.132513,0.1555593 -0.247742,0.1555593 -0.259266,0 -0.483963,-0.1786051 -0.483963,-0.4321092 0,-0.4724394 0.679852,-0.7489893 1.348181,-0.7489893 0.599192,0 1.238713,0.126752 1.238713,0.59343 0,0.2938343 -0.178605,0.4954852 -0.39754,0.6337602 0.334164,0.063376 0.587668,0.2074124 0.587668,0.5242925 0,0.5185306 -0.541577,0.9103096 -1.0313,0.9103096 -0.23622,0 -0.460917,-0.08642 -0.622238,-0.299595 -0.01152,-0.01152 -0.01728,-0.02881 -0.01728,-0.04609 0,-0.06914 0.09218,-0.144036 0.161321,-0.144036 0.02305,0 0.04609,0.0058 0.05761,0.02305 0.06338,0.08642 0.15556,0.120991 0.253504,0.120991 0.247743,0 0.547339,-0.230458 0.547339,-0.5358153 0,-0.2707884 -0.282312,-0.3629717 -0.581907,-0.3629717 -0.03457,0 -0.04609,-0.046092 -0.04609,-0.092183 0,-0.063376 0.02881,-0.1382749 0.103706,-0.1382749 0.207413,0 0.397541,-0.2246968 0.397541,-0.4436322 0,-0.2592655 -0.207413,-0.391779 -0.460917,-0.4378706 -0.06338,0.3572103 -0.178605,0.9967319 -0.230458,1.2675204 -0.115229,0.5876683 -0.483962,1.1638133 -0.916072,1.1638133 -0.253504,0 -0.391779,-0.195889 -0.553099,-0.305357 -0.01729,-0.01152 -0.02305,-0.02881 -0.02305,-0.05185 0,-0.06338 0.06914,-0.149798 0.138275,-0.149798 0.01728,0 0.02881,0 0.04033,0.01152 0.115229,0.0749 0.207412,0.201651 0.374495,0.201651 0.374494,0 0.512769,-0.5703836 0.512769,-1.1234834 v -0.046092 c 0,-0.2016509 0.05185,-0.6279987 0.103706,-0.9391174 -0.224696,0.074899 -0.403302,0.2304583 -0.403302,0.403302 0,0.086422 0.04609,0.1613208 0.109468,0.1958895 0.02305,0.017284 0.04033,0.051853 0.04033,0.092183 z m 0.19589,-1.9704181 c -0.247743,0.2362197 -0.656806,0.6049529 -0.996732,0.6049529 -0.190128,0 -0.299596,-0.1209906 -0.449394,-0.1843666 -0.01728,-0.00576 -0.02881,-0.028807 -0.02881,-0.051853 0,-0.063376 0.06338,-0.1613208 0.144036,-0.1613208 0.01152,0 0.02881,0.00576 0.04033,0.011523 0.09218,0.04033 0.172843,0.1037063 0.288072,0.1037063 0.19589,0 0.374495,-0.1555594 0.524293,-0.3111187 0.576146,-0.5876685 1.008255,-1.261759 1.232952,-1.9358494 0.05185,-0.1555593 0.190128,-0.2131739 0.328403,-0.2131739 0.167082,0 0.328403,0.086422 0.328403,0.1786052 0,0.011523 -0.0058,0.023046 -0.01152,0.034569 -0.02305,0.668329 -0.05185,1.3424194 -0.09218,2.0165098 v 0.051853 c 0,0.092183 0.01729,0.1728437 0.120991,0.1728437 0.09218,0 0.201651,-0.074899 0.305357,-0.1094677 l 0.01152,-0.00576 c 0.02881,0 0.05185,0.046092 0.05185,0.097945 0,0.046092 -0.01728,0.092183 -0.06914,0.1094677 -0.167083,0.051853 -0.311119,0.1958895 -0.501247,0.1958895 -0.184367,0 -0.259266,-0.1670822 -0.305357,-0.3341644 -0.09218,-0.3514489 -0.149798,-0.7201821 -0.178605,-1.1004382 -0.15556,0.2016509 -0.322642,0.391779 -0.501247,0.5819071 0.06338,0.1152291 0.190128,0.190128 0.368733,0.190128 0.02305,0 0.04609,-0.00576 0.06914,-0.00576 0.02881,0 0.04609,0.04033 0.04609,0.086422 0,0.1440364 -0.195889,0.1670823 -0.322641,0.1670823 -0.184367,0 -0.322642,-0.069138 -0.403302,-0.1901281 z m -0.0749,-3.5893874 c 0,0.1325135 -0.132514,0.138275 -0.241981,0.138275 -0.282312,0 -0.524293,-0.2074125 -0.524293,-0.4839624 0,-0.5473383 0.720182,-0.6913747 1.336658,-0.6913747 0.282311,0 0.685613,0.2362197 0.985209,0.2362197 0.218935,0 0.443632,-0.086422 0.63376,-0.2362197 0.01729,-0.011523 0.03457,-0.017284 0.05185,-0.017284 0.06338,0 0.132514,0.063376 0.132514,0.1094677 0,0.011523 -0.0058,0.017284 -0.01152,0.023046 -0.230458,0.2592655 -0.547338,0.4090634 -0.852696,0.4090634 -0.08642,0 -0.178605,-0.011523 -0.253504,-0.034569 -0.05185,0.3399259 -0.149798,0.8699799 -0.201651,1.1119611 -0.126752,0.5530998 -0.507008,1.0658694 -0.939117,1.0658694 -0.299596,0 -0.466678,-0.2419812 -0.662568,-0.3687332 -0.01728,-0.011523 -0.02304,-0.028807 -0.02304,-0.051853 0,-0.063376 0.06914,-0.1497979 0.132513,-0.1497979 0.01152,0 0.02881,0.00576 0.04033,0.011523 0.132514,0.086422 0.247743,0.265027 0.43211,0.265027 0.322641,0 0.472439,-0.4436321 0.489723,-0.8642185 0.01152,-0.2592655 0.0749,-0.8642184 0.126752,-1.1983829 -0.06914,-0.017284 -0.132513,-0.028807 -0.184366,-0.028807 -0.293834,0 -0.651045,0.1209906 -0.651045,0.403302 0,0.1037062 0.05761,0.2074124 0.138275,0.2477426 0.02881,0.017284 0.04609,0.057615 0.04609,0.1037062 z"
     id="path286"
     inkscape:connector-curvature="0"
     style="fill:currentColor;stroke-width:0.00576146" />
  <text
     font-weight="bold"
     font-size="1.7461"
     id="text252-7"
     style="font-weight:bold;font-size:1.74609995px;font-family:'Century Schoolbook L';text-anchor:start;fill:currentColor;stroke-width:1"
     x="32.094929"
     y="3.8305764">
    <tspan
       style="stroke-width:1"
       id="tspan250-8" />
  </text>
</svg>





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
        default: 10
      }
  },
  data() {
      return {
          hoverBar: null,
          activeBar: 0,
          activeLine: 0,
      }
  },
  mounted: function () {
      var vm = this;
      document.onkeydown = function(event) {
        switch (event.keyCode) {
           case 37: // Left arrow
                var initX = $('#rect4672').attr('x');
                initX = parseFloat(initX) - 4;
                if (initX >= 5.5) {
                    $('#rect4672').attr('x', initX);
                }
              break;
           case 39: // Right arrow
                var initX = $('#rect4672').attr('x');
                initX = parseFloat(initX) + 4;

                if (initX <= 177.5) {
                    $('#rect4672').attr('x', initX);
                }

                //Place 2 digit element .3 more than tracker
                //Place 1 digit element 1 more than tracker


                //Y values for strings
                //2
                //4
                //6
                //8.5
                //10.5
                //12.5


                var svg = document.getElementById('svg348');
                var element = document.createElementNS('http://www.w3.org/2000/svg', 'text');
                element.setAttributeNS(null,'data-fret-1-string-1', '');
                element.setAttributeNS(null, 'x', 5.8);
                element.setAttributeNS(null, 'y', 12.5);
                element.setAttributeNS(null, 'font-weight', 'bold');
                element.setAttributeNS(null, 'font-size', 1.7);
                element.setAttributeNS(null, 'style', "font-weight:bold;font-size:2.51501966px;font-family:'Century Schoolbook L';text-anchor:start;fill:currentColor;stroke-width:1.44036412");
                var txt = document.createTextNode("2");
                element.appendChild(txt);
                svg.appendChild(element);

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

    for (var i = 0; i < this.defaultBarCount; i++) {
        this.lines[0].push(this.createDefaultBar());
    }

    this.lineText.push('');
  },
  methods: {
    setStringTuning(newTuning, stringIndex) {
        Vue.set(this.stringTuning, stringIndex, newTuning);
    },   
    createDefaultBar() {
        var barArray = [];
        for (var i = 0; i < this.stringCount; i++) {
            barArray.push('-');
        }

        return barArray;
    },
    addNewLine: function() {
        var newBar = this.createDefaultBar();
        var newArray = [];
        for (var i = 0; i < this.defaultBarCount; i++) {
            newArray.push(this.createDefaultBar());
        }
        
        var nextIndex = this.lines.length;

        Vue.set(this.lines, nextIndex, newArray);

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
        var newBar = this.createDefaultBar();
        var nextish = this.lines[this.activeLine];
        nextish.push(newBar);

        Vue.set(this.lines, this.activeLine, nextish);
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
