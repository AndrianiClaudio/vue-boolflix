<template>
  <main class="main">
    <!-- container che si mostra soltanto se ci sono film o serie tv da mostrare -->
    <div class="container" v-if="cards.movie.length > 0 || cards.tv.length > 0">
      <!-- FILM -->
      <section id="movie">
        <h2>Film</h2>
        <!-- passa al componente Card tutti i film -->
        <Card
        v-for="(card,index) in cards.movie"
        :key='"movie-"+index'
        :card = card
        />
        <!-- Array film vuoto -->
        <strong v-if="nf_error.movie">Nessun film trovato</strong>
      </section>
      <!-- SERIE TV -->
      <section id="series">
        <h2>Serie TV</h2>
        <!-- passa al componente Card tutte le serie tv -->
        <Card
        v-for="(card,index) in cards.tv"
        :key='"series-"+index'
        :card = card
        />
        <!-- Array serie tv vuoto -->
        <strong v-if="nf_error.tv">Nessuna serie tv trovata</strong>
      </section>
    </div>
    <!-- errore: risultano essere vuoti tutti gli array(solo se la ricerca é stata effettuata) -->
    <div v-if='nf_error.movie && nf_error.tv'>
      <strong>Non &eacute; stato trovato nessun film e nessuna serie</strong>
    </div>
  </main>
</template>

<script>
import Card from './Card.vue';

export default {
  name: 'Main',
  components: {
    Card,
  },
  props: {
    cards: Object,
    nf_error: Object,
  },
};
</script>

<style lang='scss' scoped>
@import '../assets/scss/partials/_variables.scss';
@import '../assets/scss/partials/_mixins.scss';
.main {
  background-color: $mainBgColor;
  min-height: calc(100vh - $headerHeight);
  color: white;
  .container {
    section {
      @include flex($wra:wrap,$gap:0 2.5rem);
      padding: 1rem;
      min-width: 500px;
      gap:1.5rem 2.5rem;
      h2 {
        width:100%;
        text-align: center;
        text-transform: uppercase;
        font-size: 2rem;
        padding: 1.5rem;
      }
  }

  }
}
</style>
