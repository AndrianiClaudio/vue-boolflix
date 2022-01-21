<template>
  <div class="card"
  @mouseover="activeHover"
  @mouseleave="resetHover"
  >
    <!-- di base mostra immagine poster -->
    <img
    v-if="!hover"
    class="poster"
    :src="card.poster" alt="Immagine poster non trovata"
    >
    <!-- all'hover sulla card viene impostata a true la variabile hover -->
    <!-- mostra informazioni card -->
    <div v-else class="hover-container">
      <div class='card-detail card-title'>
        <strong>Titolo:</strong>
        <span>{{card.title}}</span>
      </div>
      <!-- mostra nome originale solo diverso dal titolo mostrato -->
      <div class="card-detail card-original-title" v-if="card.title !== card.original_title">
        <strong>Titolo originale:</strong>
        <span>{{card.original_title}}</span>
      </div>
      <div class='card-detail card-overview' v-if="card.overview">
        <strong>Overiew:</strong>
        <p>{{card.overview}}</p>
      </div>
      <div class='card-detail card-language'>
      <!-- ottieni immagine della bandiera, se presente, altrimenti scrive iniziale lingua -->
        <img :src="getFlag(card.language)" :alt="card.language">
      </div>
      <div class='card-detail card-vote' v-if="card.vote > 0">
        <!-- stelle gialle-->
        <strong>Voto:</strong>
          <!-- v-for = '(star,index) in Math.round(card.vote/2)' -->
          <font-awesome-icon
          v-for = '(star,index) in Math.round(card.vote/2)'
          :key= '"full_star-"+index'
          icon='star'
          class="fullStar"
          />
        <!-- stelle grigie -->
          <font-awesome-icon
          v-for = '(star,index) in 5 - Math.round(card.vote/2)'
          :key= '"empty_star-"+index'
          icon='star'
          class="emptyStar"
          />
      </div>
    </div>
  </div>
</template>

<script>
import { library } from '@fortawesome/fontawesome-svg-core';
import { faStar } from '@fortawesome/free-solid-svg-icons'; // fas fa-star
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';

library.add(faStar);

export default {
  name: 'Card',
  components: {
    FontAwesomeIcon,
  },
  data() {
    return {
      hover: false,
      flags: {
        it: 'https://img.icons8.com/office/italy.png',
        en: 'https://img.icons8.com/color/usa.png',
        fr: 'https://img.icons8.com/color/france.png',
      },
    };
  },
  props: {
    card: Object,
  },
  methods: {
    // ottieni bandiera da lingua, se presente nei data
    getFlag(lan) {
      if (this.flags[lan]) {
        return this.flags[lan];
      }
      return '';
    },
    // funzioni gestione hover
    activeHover() {
      this.hover = true;
    },
    resetHover() {
      this.hover = false;
    },
  },
};
</script>

<style lang='scss' scoped>
@import '../assets/scss/partials/_variables.scss';
@import '../assets/scss/partials/_mixins.scss';

.card {
  @include flex($ali:center,$jus:center);
  position: relative;
  background-color: $cardBgColor;
  width: calc($cardWidth + 1rem);
  height: $cardHeight;
  overflow: hidden;
  border: 1px solid white;
  img.poster {
    max-height: calc($cardHeight - 1rem);
  }
  .hover-container {
    .card-detail {
      font-size: 1.15rem;
      strong {
      padding-right: .5rem;
      }
      .card-language {
        img {
          margin: 0 auto;
        }
      }
      p {
        display: inline;
      }
    //stelle per il voto
    .fullStar {
      path{
        fill:gold;
      }
    }
    .emptyStar {
      path {
        fill:grey;
      }
    }
   }
  }
}
</style>
