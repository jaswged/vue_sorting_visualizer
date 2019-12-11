<template>
  <v-container>
      <v-row>
        <v-col cols="2">
          <v-btn target="_blank" outlined v-on:click="resetArray">
            <span class="mr-2">Reset Array</span>
          </v-btn>
        </v-col>
        <v-col cols="2">
          <v-btn target="_blank" outlined>
            <span class="mr-2">Merge Sort</span>
          </v-btn>
        </v-col>
        <v-col cols="2">
          <v-btn target="_blank" outlined>
            <span class="mr-2">Quick Sort</span>
          </v-btn>
        </v-col>
        <v-col cols="2">
          <v-btn target="_blank" outlined>
            <span class="mr-2">Bubble Sort</span>
          </v-btn>
        </v-col>
        <v-col cols="3">
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
    isSorted: false
  }),
  methods: {
    randomIntFromInterval(min, max) {
      return Math.floor(Math.random() * (max - min + 1) + min);
    },
    resetArray(){
      const array = [];
      for(let i = 0; i < this.stateSize; i++){
          array.push(this.randomIntFromInterval(5,800));
      }
      this.setState(array);
      this.isSorted = false;
    },
    setState(array){
      this.state = array;
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