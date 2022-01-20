<template>
  <main class="main">
    <section id="movie" v-if="cards.movie.length > 0">
        <h2>Film</h2>
        <Card
        v-for="(card,index) in cards.movie"
        :key='"movie-"+index'
        :card = card
        />
    </section>
    <strong v-else-if="nf_error.movie">Nessun film trovato</strong>
    <section id="series" v-if="cards.tv.length > 0">
        <h2>Serie TV</h2>
        <Card
        v-for="(card,index) in cards.tv"
        :key='"series-"+index'
        :card = card
        />
    </section>
    <strong v-else-if="nf_error.tv">Nessuna serie tv trovata</strong>
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

<style>

</style>
