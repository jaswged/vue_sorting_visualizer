<template>
  <v-container>
      <v-row>
        <v-col lg="2" xs="12" sm="12" md="4">
          <v-btn target="_blank" outlined v-on:click="resetArray" :disabled=isSorting>
            <span class="mr-2">Reset Array</span>
          </v-btn>
        </v-col>
        <v-col lg="2" xs="12" sm="12" md="4">
          <v-btn target="_blank" outlined v-on:click="mergeSort" :disabled=isSorting>
            <span class="mr-2">Merge Sort</span>
          </v-btn>
        </v-col>
        <v-col lg="2" xs="12" sm="12" md="4">
          <v-btn target="_blank" outlined v-on:click="quickSort" :disabled=isSorting>
            <span class="mr-2">Quick Sort</span>
          </v-btn>
        </v-col>
        <v-col lg="2" xs="12" sm="12" md="4">
          <v-btn target="_blank" outlined v-on:click="bubbleSort" :disabled=isSorting>
            <span class="mr-2">Bubble Sort</span>
          </v-btn>
        </v-col>
        <!--<v-col lg="2" xs="12" sm="6" md="4">
          <v-btn target="_blank" outlined v-on:click="bubbleSort" :disabled=isSorting>
            <span class="mr-2">Heap Sort</span>
          </v-btn>
        </v-col>
        <v-col lg="2" xs="12" sm="6" md="4">
          <v-btn target="_blank" outlined v-on:click="bubbleSort" :disabled=isSorting>
            <span class="mr-2">Smooth Sort</span>
          </v-btn>
        </v-col>
        <v-col lg="2" xs="12" sm="6" md="4">
          <v-btn target="_blank" outlined v-on:click="bubbleSort" :disabled=isSorting>
            <span class="mr-2">Cube Sort</span>
          </v-btn>
        </v-col>-->
        <v-col lg="2" xs="12" sm="6" md="4">
          <v-slider v-model="ANIMATION_SPEED_MS" label="Animation Speed" min="5" max="1000" :disabled=isSorting></v-slider>
        </v-col>
        <v-col lg="2" xs="12" sm="6" md="4">
          <v-slider v-model="stateSize" label="Array Size" min="5" max="50" v-on:input="resetArray($event)" :disabled=isSorting></v-slider>
        </v-col>
        <v-col cols ="1" lg="1" xs="12" sm="6" md="4">
          {{stateSize}}
        </v-col>
      </v-row>
      <v-spacer/>
      <v-row>
        <v-col cols="12" justify="center" id="barContainer">
          <Bar v-for="(val, kee) in state" :id="kee" :key="kee" :val="val" :barWidth="barWidth"/>
        </v-col>
      </v-row>
      <v-row>
        <v-col cols=12>
          <a href="https://en.wikipedia.org/wiki/Sorting_algorithm">Wikipedia Article</a>
        </v-col>
      </v-row>
  </v-container>
</template>

<script>
import Bar from './Bar';

export default {
  name: 'SortingVisualizer',
  components: {
    Bar,
  },
  data: () => ({
    state: [],
    stateSize: 25,
    PRIMARY_COLOR: 'grey',
    SECONDARY_COLOR: 'red',
    clientWidth: document.body.clientWidth * .9,
    clientHeight: document.body.clientHeight - 100,
    isSorted: false,
    isSorting: false,
    ANIMATION_SPEED_MS: 45,
    barWidth: 0
  }),
  methods: {
    randomIntFromInterval(min, max) {
      return Math.floor(Math.random() * (max - min + 1) + min);
    },
    resetArray(){
      // Header is 50px tall.
      const array = [];
      for(let i = 0; i < this.stateSize; i++){
          array.push(this.randomIntFromInterval(25,750));
      }
      this.setState(array);
      this.isSorted = false;
      this.barWidth = this.getBarWidth();

      // Set the color back to Primary
      document.getElementsByClassName('array-bar').forEach(element => {
        element.style.backgroundColor = this.PRIMARY_COLOR;
      });
    },
    setState(array){
      this.state = array;
    },
    quickSort(){
      //var barContainer = document.getElementById("barContainer");
      var header = document.getElementById("header");
      window.console.log(header.clientHeight);
      window.console.log(this.clientHeight);
      
      const animations = this.getBubbleSortAnimations();
      window.console.log(animations);
// TODO
      const arrayBars = document.getElementsByClassName('array-bar');
      for (let i = 0; i < animations.length; i++) {
        var [barOneIdx, barTwoIdx] = animations[i];
        
        // Change the colors of those being compared
        setTimeout(() => {
          arrayBars[barOneIdx].style.backgroundColor = this.SECONDARY_COLOR;
          arrayBars[barTwoIdx].style.backgroundColor = this.SECONDARY_COLOR;
        }, i * this.ANIMATION_SPEED_MS);
        i++;

        // Swap the elements if not -1
        [barOneIdx, barTwoIdx] = animations[i];
        if(barOneIdx != -1){
          setTimeout(() => {
            window.console.log("Swap these two bars: " + barOneIdx + " : " + barTwoIdx);
            var oneHeight = arrayBars[barOneIdx].style.height;
            var twoHeight = arrayBars[barOneIdx].style.height;
            arrayBars[barOneIdx].style.height = `${twoHeight}px`;
            arrayBars[barTwoIdx].style.height = `${oneHeight}px`;
          }, i * this.ANIMATION_SPEED_MS);
        }
        i++;

        // Reset the colors of those that were compared
        [barOneIdx, barTwoIdx] = animations[i];
        // Change the colors of those being compared
        arrayBars[barOneIdx].style.color = this.PRIMARY_COLOR;
        arrayBars[barTwoIdx].style.color = this.PRIMARY_COLOR;
      }

      // Set timeout for setting sorted true and done sorting
      setTimeout(() => {
        this.isSorted = true;
        this.isSorting = false;
      },animations.length * this.ANIMATION_SPEED_MS);


    },
    bubbleSort(){
      //this.isSorting = true;

      const arrayBars = document.getElementsByClassName('array-bar');
      var length = arrayBars.length;

      var swapped = false;
      var animCount = 0;

      for(var i = 0; i < length -1; i++){
        swapped = false;
        for(var j = 0; j < length-i-1; j++){
          // Highlight the comparing bars
          arrayBars[j].style.backgroundColor = this.SECONDARY_COLOR;
          arrayBars[j+1].style.backgroundColor = this.SECONDARY_COLOR;

          if(arrayBars[j].clientHeight > arrayBars[j+1].clientHeight){
            animCount++;
            window.console.log("Swap bars " + j + " and " + (j+1));
            //setTimeout(() => {this.swapBubble(arrayBars[j], arrayBars[j+1]);}, animCount * this.ANIMATION_SPEED_MS);
            window.console.log(animCount);
            this.swapBubble(arrayBars[j], arrayBars[j+1]);
            swapped = true;
          }
          // Return the comparing bars to their original colors
          animCount++;
          // setTimeout(() => {
          //   arrayBars[j].style.backgroundColor = this.PRIMARY_COLOR;
          //   arrayBars[j+1].style.backgroundColor = this.PRIMARY_COLOR;
          // },animCount * this.ANIMATION_SPEED_MS);
        }
        if(!swapped) break;
      }
    },
    getBubbleSortAnimations(){
      var animations = [];

      const arrayBars = document.getElementsByClassName('array-bar');
      var length = arrayBars.length;
      var swapped = false;

      for(var i = 0; i < length -1; i++){
        swapped = false;
        for(var j = 0; j < length-i-1; j++){
          // Highlight the comparing bars
          //arrayBars[j].style.backgroundColor = this.SECONDARY_COLOR;
          //arrayBars[j+1].style.backgroundColor = this.SECONDARY_COLOR;
          animations.push([j, j+1]);

          if(arrayBars[j].clientHeight > arrayBars[j+1].clientHeight){

            //window.console.log("Swap bars " + j + " and " + (j+1));
            //setTimeout(() => {this.swapBubble(arrayBars[j], arrayBars[j+1]);}, animCount * this.ANIMATION_SPEED_MS);
            //this.swapBubble(arrayBars[j], arrayBars[j+1]);
            swapped = true;
            animations.push([j, j+1]);
          }else{
            animations.push([-1, -1]);
          }
          // Return the comparing bars to their original colors

          // setTimeout(() => {
          //   arrayBars[j].style.backgroundColor = this.PRIMARY_COLOR;
          //   arrayBars[j+1].style.backgroundColor = this.PRIMARY_COLOR;
          // },animCount * this.ANIMATION_SPEED_MS);
          // push animation again to reset the color
          animations.push([j, j+1]);
        }
        if(!swapped) break;
      }

      return animations;
    },
   swapBubble(node1, node2) {
        // Jquery $("#div1").insertAfter("#div2");
        node1.parentNode.replaceChild(node1, node2);
        node1.parentNode.insertBefore(node2, node1);
    },
    getBarWidth(){
      return this.clientWidth/this.stateSize - 2;
    },
    mergeSort(){
      this.isSorting = true;
      const animations = this.getMergeSortAnimations(this.state);

      const arrayBars = document.getElementsByClassName('array-bar');
      for (let i = 0; i < animations.length; i++) {
        const isColorChange = i % 3 !== 2;
        if (isColorChange) {
          const [barOneIdx, barTwoIdx] = animations[i];
          const barOneStyle = arrayBars[barOneIdx].style;
          const barTwoStyle = arrayBars[barTwoIdx].style;
          const color = i % 3 === 0 ? this.SECONDARY_COLOR : this.PRIMARY_COLOR;
          setTimeout(() => {
            barOneStyle.backgroundColor = color;
            barTwoStyle.backgroundColor = color;
          }, i * this.ANIMATION_SPEED_MS);
        } else {
          setTimeout(() => {
            const [barOneIdx, newHeight] = animations[i];
            const barOneStyle = arrayBars[barOneIdx].style;
            barOneStyle.height = `${newHeight}px`;
          }, i * this.ANIMATION_SPEED_MS);
        }
      }
      setTimeout(() => {
        this.isSorted = true;
        this.isSorting = false;
      },animations.length * this.ANIMATION_SPEED_MS);
    },
    getMergeSortAnimations(array) {
      const animations = [];
      if (array.length <= 1) return array;
      // Slice creates a new array with same values
      const auxiliaryArray = array.slice();
      this.mergeSortHelper(array, 0, array.length - 1, auxiliaryArray, animations);
      return animations;
    },
    mergeSortHelper(mainArray, startIdx, endIdx, auxiliaryArray, animations) {
      if (startIdx === endIdx) return;
      const middleIdx = Math.floor((startIdx + endIdx) / 2);
      this.mergeSortHelper(auxiliaryArray, startIdx, middleIdx, mainArray, animations);
      this.mergeSortHelper(auxiliaryArray, middleIdx + 1, endIdx, mainArray, animations);
      this.doMerge(mainArray, startIdx, middleIdx, endIdx, auxiliaryArray, animations);
    },
    doMerge(mainArray, startIdx, middleIdx, endIdx, auxiliaryArray, animations) {
      let k = startIdx;
      let i = startIdx;
      let j = middleIdx + 1;
      while (i <= middleIdx && j <= endIdx) {
          // These are the values that we're comparing; we push them once to change their color.
          animations.push([i, j]);
          // These are the values that we're comparing; we push them a second time to revert their color.
          animations.push([i, j]);
          if (auxiliaryArray[i] <= auxiliaryArray[j]) {
          // We overwrite the value at index k in the original array with the value at index i in the auxiliary array.
          animations.push([k, auxiliaryArray[i]]);
          mainArray[k++] = auxiliaryArray[i++];
          } else {
          // We overwrite the value at index k in the original array with the value at index j in the auxiliary array.
          animations.push([k, auxiliaryArray[j]]);
          mainArray[k++] = auxiliaryArray[j++];
          }
      }
      while (i <= middleIdx) {
          // These are the values that we're comparing; we push them once to change their color.
          animations.push([i, i]);
          // These are the values that we're comparing; we push them a second time to revert their color.
          animations.push([i, i]);
          // We overwrite the value at index k in the original array with the value at index i in the auxiliary array.
          animations.push([k, auxiliaryArray[i]]);
          mainArray[k++] = auxiliaryArray[i++];
      }
      while (j <= endIdx) {
          // These are the values that we're comparing; we push them once to change their color.
          animations.push([j, j]);
          // These are the values that we're comparing; we push them a second time to revert their color.
          animations.push([j, j]);
          // We overwrite the value at index k in the original array with the value at index j in the auxiliary array.
          animations.push([k, auxiliaryArray[j]]);
          mainArray[k++] = auxiliaryArray[j++];
      }
    }
  },
  beforeMount(){
    this.resetArray()
 },
};
</script>

<style scoped>

</style>