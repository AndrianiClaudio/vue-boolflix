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
        <strong>Language: </strong>
        <!-- <img :src="getFlag(card.language)" :alt="card.language"> -->
        <!-- <span class="flag">{{getFlag(card.language)}}</span> -->
          <!-- :src="`https://flagcdn.com/${card.language}.svg`" -->
        <img
          :src="`https://flagcdn.com/${getLanguage}.svg`"
          width="30"
          :alt="card.language">
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
          <span>({{card.vote/2}})</span>
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
  computed: {
    getLanguage() {
      return (this.card.language === 'en') ? 'us' : this.card.language;
    },
  },
  data() {
    return {
      hover: false,

    };
  },
  props: {
    card: Object,
  },
  methods: {
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
  // @include flex($ali:center,$jus:center);
  @include flex();
  position: relative;
  background-color: $cardBgColor;
  width: calc($cardWidth + 1rem);
  height: $cardHeight;
  overflow: hidden;
  border: 1px solid white;
  img.poster {
    max-height: calc($cardHeight - 1rem);
    margin: auto auto;
  }
  .hover-container {
    margin: 1.25rem;
    .card-detail {
      font-size: 1.1rem;
      padding-bottom: .35rem;
      width: calc($cardWidth - 2rem);
      //titolo di ogni card-detail
      strong {
        font-size: 1.22rem;
        padding-right: .5rem;
      }
      //paragrafo contenente descrzione 'overview'
      &.card-overview {
        max-height: 320px;
        overflow: auto;
        p {
          display: inline;
        }
      }
      &.card-language {
        @include flex($ali:center);
        .flag {
          font-size: 1.5rem;
        }
      }
      &.card-vote {
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
        :last-child {
          padding-left: .5rem;

        }
      }
   }
  }
}
</style>
