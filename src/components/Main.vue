<template>
<main class="main">
    <!-- <ul class="cards"> -->
        <section id="movie">
            <h2 v-if="cards.movie.length > 0">Film</h2>
            <Card
            v-for="(card,index) in cards.movie"
            :key='"movie-"+index'
            :card = card
            />
        </section>
        <section id="series">
            <h2 v-if="cards.tv.length > 0">Serie TV</h2>
            <Card
            v-for="(card,index) in cards.tv"
            :key='"series-"+index'
            :card = card
            />
        </section>
    <!-- </ul> -->
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
  },
};
</script>

<style>

</style>
