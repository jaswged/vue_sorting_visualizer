<template>
  <v-app>
    <v-app-bar app dark color="secondary" >
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
        />

        <v-img
          alt="Vuetify Name"
          class="shrink mt-1 hidden-sm-and-down"
          contain
          min-width="100"
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-name-dark.png"
          width="100"
        />
      </div>

      Sorting Visualizer

      <v-spacer></v-spacer>

      <v-btn
        href="https://github.com/jaswged/vue_sorting_visualizer"
        target="_blank"
        text
      >
        <span class="mr-2">By: Jason Gedamke</span>
        <v-icon large color="black" class="fab fa-github"></v-icon>
      </v-btn>
    </v-app-bar>

    <v-content>
      <SortingVisualizer/>
    </v-content>
  </v-app>
</template>

<script>
import SortingVisualizer from './components/SortingVisualizer';

export default {
  name: 'App',
  components: {
    SortingVisualizer,
  },
  data: () => ({
    //
  }),
};
</script>
