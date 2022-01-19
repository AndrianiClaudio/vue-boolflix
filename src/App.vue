<template>
  <div id="app">
    <Header
    @printFilm='printSearched($event)'
    />
    <Main
    :films = films
    />
    <!-- :searched = searched -->
  </div>
</template>

<script>
import Header from './components/Header.vue';
import Main from './components/Main.vue';

export default {
  name: 'App',
  components: {
    Header,
    Main,
  },
  data() {
    return {
      films: [],
    };
  },
  methods: {
    printSearched(e) {
      this.films = e;
    },
  },
};
</script>

<style lang="scss">
</style>
