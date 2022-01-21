<template>
  <main class="main">
    <div class="container" v-if="cards.movie.length > 0 || cards.tv.length > 0">
      <section id="movie">
          <h2>Film</h2>
          <Card
          v-for="(card,index) in cards.movie"
          :key='"movie-"+index'
          :card = card
          />
      </section>
      <strong v-if="nf_error.movie">Nessun film trovato</strong>
      <section id="series">
          <h2>Serie TV</h2>
          <Card
          v-for="(card,index) in cards.tv"
          :key='"series-"+index'
          :card = card
          />
      </section>
      <strong v-if="nf_error.tv">Nessuna serie tv trovata</strong>
    </div>
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
        // gap:0 2.5rem;
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
