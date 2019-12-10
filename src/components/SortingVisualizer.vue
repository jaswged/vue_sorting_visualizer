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
            <span class="mr-2">Bubble Sort</span>
          </v-btn>
        </v-col>
        <v-col cols="5">
          <v-slider v-model="stateSize" label="Max error count" min="5" max="50" v-on:input="resetArray($event)"></v-slider>
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
            }"
          >
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
    PRIMARY_COLOR: 'turquoise',
    SECONDARY_COLOR: 'red'
  }),
  methods: {
    randomIntFromInterval(min, max) {
      return Math.floor(Math.random() * (max - min + 1) + min);
    },
    resetArray(){
      const array = [];
      for(let i = 0; i < this.stateSize; i++){
          array.push(this.randomIntFromInterval(5,1000));
      }
      this.setState(array);
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
      width: 3px;
      display: inline-block;
      margin: 0 1px;
  }
</style>