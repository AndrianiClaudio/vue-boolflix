<template>
  <div id="app">
    <!--
      Header invia tramite emit array contentente:
        cards: contenente film e serie TV ricercate
        nf_error: variabili pari a true se la ricerca restituisce array nullo
    -->
    <Header
    @printFilm='printSearched($event)'
    />
    <!--
      Main ottiene tramite props i dati ricevuti da Header
     -->
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
      // dati arrivati da header
      cards: {
        movie: [],
        tv: [],
      },
      nf_error: {
        movie: false,
        tv: false,
      },
      // eventuali altri dati ...
    };
  },
  methods: {
    /**
     * carica data con i dati ricevuti da Header
     */
    printSearched(e) {
      [this.cards, this.nf_error] = e;
    },
  },
};
</script>

<style lang="scss">
@import 'assets/scss/style.scss';
</style>
