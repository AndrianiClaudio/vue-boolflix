<template>
  <div id="app">
    <Header
    @printFilm='printSearched($event)'
    />
    <Main
    :cards = cards
    :nf_error = nf_error
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
      nf_error: {
        movie: false,
        tv: false,
      },
    };
  },
  methods: {
    printSearched(e) {
      [this.cards, this.nf_error] = e;
    },
  },
};
</script>

<style lang="scss">
</style>
