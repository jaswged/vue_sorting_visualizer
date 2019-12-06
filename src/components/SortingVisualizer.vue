<template>
  <v-container>
    <v-layout
      text-center
      wrap
    >
      <v-flex xs12>
        <v-img
          :src="require('../assets/logo.svg')"
          class="my-3"
          contain
          height="200"
        ></v-img>
      </v-flex>

      <v-flex mb-5>
        <h1 class="headline font-weight-bold mb-3">Hello world</h1>
      </v-flex>

      <v-layout justify-center>
        <div className="array-container"
          v-for="(eco, i) in ecosystem"
          :key="i"
          :href="eco.href"
          class="subheading mx-3"
          target="_blank"
        >
          {{ eco.text }}
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
    ecosystem: [
      {
        text: 'vuetify-loader',
        href: 'https://github.com/vuetifyjs/vuetify-loader',
      },
      {
        text: 'github',
        href: 'https://github.com/vuetifyjs/vuetify',
      },
      {
        text: 'awesome-vuetify',
        href: 'https://github.com/vuetifyjs/awesome-vuetify',
      },
    ],
  }),
  methods: {
    randomIntFromInterval(min, max) {
      // inclusive
      return Math.floor(Math.random() * (max - min + 1) + min);
    },
    resetArray(){
      const array = [];
      this.randomIntFromInterval(2, 5);
      for(let i = 0; i< 100; i++){
          array.push(this.randomIntFromInterval(5,1000));
      }
      this.setState(array);
    }
  },
  beforeMount(){
    this.resetArray()
 },
};
</script>

<style scoped>

</style>