<template>
  <v-container>
      <v-row>
        <v-col cols="1">
          <v-btn target="_blank" outlined v-on:click="resetArray" :disabled=isSorting>
            <span class="mr-2">Reset Array</span>
          </v-btn>
        </v-col>
        <v-col cols="1">
          <v-btn target="_blank" outlined v-on:click="mergeSort" :disabled=isSorting>
            <span class="mr-2">Merge Sort</span>
          </v-btn>
        </v-col>
        <v-col cols="1">
          <v-btn target="_blank" outlined v-on:click="quickSort" :disabled=isSorting>
            <span class="mr-2">Quick Sort</span>
          </v-btn>
        </v-col>
        <v-col cols="1">
          <v-btn target="_blank" outlined v-on:click="bubbleSort" :disabled=isSorting>
            <span class="mr-2">Bubble Sort</span>
          </v-btn>
        </v-col>
        <v-col cols="1">
          <v-btn target="_blank" outlined v-on:click="bubbleSort" :disabled=isSorting>
            <span class="mr-2">Heap Sort</span>
          </v-btn>
        </v-col>
        <v-col cols="1">
          <v-btn target="_blank" outlined v-on:click="bubbleSort" :disabled=isSorting>
            <span class="mr-2">Smooth Sort</span>
          </v-btn>
        </v-col>
        <v-col cols="1">
          <v-btn target="_blank" outlined v-on:click="bubbleSort" :disabled=isSorting>
            <span class="mr-2">Cube Sort</span>
          </v-btn>
        </v-col>
        <v-col cols="2">
          <v-slider v-model="ANIMATION_SPEED_MS" label="Animation Speed" min="5" max="1000" :disabled=isSorting></v-slider>
        </v-col>
        <v-col cols="2">
          <v-slider v-model="stateSize" label="Array Size" min="5" max="50" v-on:input="resetArray($event)" :disabled=isSorting></v-slider>
        </v-col>
        <v-col cols ="1">
          {{stateSize}}
        </v-col>
      </v-row>
      <v-spacer/>
      <v-row>
        <v-col cols="12" justify="center">
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
    screenWidth: screen.width * .9,
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
      alert("Quick sort clicked but doing anim bubble sort.");

      var animations = [];
      window.console.log(animations);
    },
    bubbleSort(){
      //this.isSorting = true;

      const arrayBars = document.getElementsByClassName('array-bar');
      //var arr = [].slice.call(arrayBars);
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
            // setTimeout(() => {this.swapBubble(arrayBars[j], arrayBars[j+1]);}, animCount * this.ANIMATION_SPEED_MS);
            window.console.log(animCount);
            this.sleep(1000);
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
   swapBubble(node1, node2) {
        // Jquery $("#div1").insertAfter("#div2");
        node1.parentNode.replaceChild(node1, node2);
        node1.parentNode.insertBefore(node2, node1);
    },
    getBarWidth(){
      return this.screenWidth/this.stateSize - 2;
    },
    sleep(milliseconds) {
      const date = Date.now();
      let currentDate = null;
      do {
        currentDate = Date.now();
      } while (currentDate - date < milliseconds);
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