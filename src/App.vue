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
// Milestone 5 (Opzionale):
// Partendo da un film o da una serie, richiedere all'API quali sono gli
// attori che fanno parte del cast aggiungendo alla nostra scheda Film /
// Serie SOLO i primi 5 restituiti dall’API con Nome e Cognome, e i generi
// associati al film con questo schema: “Genere 1, Genere 2, …”.

// Milestone 6 (Opzionale):
// Creare una lista di generi richiedendo quelli disponibili all'API e creare
// dei filtri con i generi tv e movie per mostrare/nascondere le schede
// ottenute con la ricerca.
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
#app {
  min-width: 500px;
}
</style>
