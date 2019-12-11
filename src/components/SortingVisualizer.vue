<template>
  <v-container>
      <v-row>
        <v-col cols="1">
          <v-btn target="_blank" outlined v-on:click="resetArray">
            <span class="mr-2">Reset Array</span>
          </v-btn>
        </v-col>
        <v-col cols="1">
          <v-btn target="_blank" outlined v-on:click="mergeSort">
            <span class="mr-2">Merge Sort</span>
          </v-btn>
        </v-col>
        <v-col cols="1">
          <v-btn target="_blank" outlined v-on:click="quickSort">
            <span class="mr-2">Quick Sort</span>
          </v-btn>
        </v-col>
        <v-col cols="1">
          <v-btn target="_blank" outlined v-on:click="bubbleSort">
            <span class="mr-2">Bubble Sort</span>
          </v-btn>
        </v-col>
        <v-col cols="1">
          <v-btn target="_blank" outlined v-on:click="bubbleSort">
            <span class="mr-2">Heap Sort</span>
          </v-btn>
        </v-col>
        <v-col cols="1">
          <v-btn target="_blank" outlined v-on:click="bubbleSort">
            <span class="mr-2">Smooth Sort</span>
          </v-btn>
        </v-col>
        <v-col cols="1">
          <v-btn target="_blank" outlined v-on:click="bubbleSort">
            <span class="mr-2">Cube Sort</span>
          </v-btn>
        </v-col>
        <v-col cols="2">
          <v-slider v-model="ANIMATION_SPEED_MS" label="Animation Speed" min="5" max="1000"></v-slider>
        </v-col>
        <v-col cols="2">
          <v-slider v-model="stateSize" label="Array Size" min="5" max="50" v-on:input="resetArray($event)"></v-slider>
        </v-col>
        <v-col cols ="1">
          {{stateSize}}
        </v-col>
      </v-row>
      <v-spacer/>
      <v-row>
        <v-col cols="12" justify="center">
          <div className="array-container"
            v-for="(val, kee) in state"
            :key="kee"
            class="subheading mx-3 array-bar"
            target="_blank"
            :style="{
              backgroundColor: PRIMARY_COLOR,
              height: `${val}px`,
              width: `${screenWidth/stateSize - 2}px`,
              margin: `1px !important`
            }"
          >
          {{val}}
          </div>
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
export default {
  name: 'SortingVisualizer',
  data: () => ({
    state: [],
    stateSize: 25,
    PRIMARY_COLOR: 'grey',
    SECONDARY_COLOR: 'red',
    screenWidth: screen.width * .9,
    isSorted: false,
    ANIMATION_SPEED_MS: 75
  }),
  methods: {
    randomIntFromInterval(min, max) {
      return Math.floor(Math.random() * (max - min + 1) + min);
    },
    resetArray(){
      const array = [];
      for(let i = 0; i < this.stateSize; i++){
          array.push(this.randomIntFromInterval(5,750));
      }
      this.setState(array);
      this.isSorted = false;
    },
    setState(array){
      this.state = array;
    },
    bubbleSort(){
      alert("Bubble sort");
    },
    mergeSort(){
      const animations = this.getMergeSortAnimations(this.state);

      for (let i = 0; i < animations.length; i++) {
        const arrayBars = document.getElementsByClassName('array-bar');
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
    },
    quickSort(){
      alert("quick sort");
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
  .array-bar {
      display: inline-block;
      
  }
</style>