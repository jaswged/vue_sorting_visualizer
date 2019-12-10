<template>
  <v-container>
    <v-layout
      text-center
      wrap
    >
      <v-btn target="_blank" outlined v-on:click="resetArray">
        <span class="mr-2">Reset Array</span>
      </v-btn>
      <v-btn target="_blank" outlined>
        <span class="mr-2">Merge Sort</span>
      </v-btn>
      <v-btn target="_blank" outlined>
        <span class="mr-2">Sort!</span>
      </v-btn>

      <v-slider v-model="stateSize" label="Max error count" min="5" max="100" thumb-label="always" v-on:input="resetArray($event)"></v-slider>

      <v-flex mb-5>
        <h1 class="headline font-weight-bold mb-3">Hello world</h1>
      </v-flex>

      <v-layout justify-center>
        <div className="array-container"
          v-for="(val, kee) in state"
          :key="kee"
          class="subheading mx-3 array-bar"
          target="_blank"
        >
          {{ val }}
        </div>
      </v-layout>

    </v-layout>
  </v-container>
</template>

<script>
export default {
  name: 'SortingVisualizer',

  data: () => ({
    state: [],
    stateSize: 50
  }),
  methods: {
    randomIntFromInterval(min, max) {
      // inclusive
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

</style>