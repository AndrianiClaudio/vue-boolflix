<template>
  <div id="app">
    <Header
    @printFilm='printSearched($event)'
    />
    <Main
    :cards = cards
    />
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
      cards: {
        movie: [],
        tv: [],
      },
    };
  },
  methods: {
    printSearched(e) {
      this.cards.movie = e.movie;
      this.cards.tv = e.tv;
    },
  },
};
</script>

<style lang="scss">
</style>
