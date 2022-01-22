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
<<<<<<< HEAD
        <span class="flag">{{getFlag(card.language)}}</span>
=======
        {{getFlag(card.language)}}
>>>>>>> d3239560260f9fe4dcc4aeb70e0eebcc40e203aa
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
  data() {
    return {
      hover: false,
      java_flags: {
        de: '\uD83C\uDDE9\uD83C\uDDEA',
        fr: '\uD83C\uDDEB\uD83C\uDDF7',
        it: '\uD83C\uDDEE\uD83C\uDDF9',
        nl: '\uD83C\uDDF3\uD83C\uDDF1',
        en: '\uD83C\uDDFA\uD83C\uDDF8',
      },

    };
  },
  props: {
    card: Object,
  },
  methods: {
    // ottieni bandiera da lingua, se presente nei data, altrimenti ottieni lingua
    getFlag(lan) {
      if (this.java_flags[lan]) {
        return this.java_flags[lan];
      }
      return lan;
    },
    // getFlag(lan) {
    //   if (this.flags[lan]) {
    //     return this.flags[lan];
    //   }
    //   return '';
    // },
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
      font-size: 1.1rem;
      padding-bottom: .35rem;
      width: calc($cardWidth - 2rem);
      //titolo di ogni card-detail
      strong {
        font-size: 1.22rem;
        padding-right: .5rem;
      }
      //paragrafo contenente descrzione 'overview'
      p {
        display: inline;
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
<<<<<<< HEAD
=======
    }
    &.card-language {
      @include flex($ali:center);
      text-align: center;
    }
>>>>>>> d3239560260f9fe4dcc4aeb70e0eebcc40e203aa
   }
  }
}
</style>
