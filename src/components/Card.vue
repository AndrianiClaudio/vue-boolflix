<template>
<div>

    <ul class='card' >
      <li
      @mouseover="activeHover"
      @mouseleave="resetHover">
        <img :src="card.poster" alt="Immagine poster non trovata">
      </li>
      <li v-if="hover">
          <span>
              {{card.title}}
          </span>
      </li>
      <li v-if="hover">
          <span>
              {{card.original_title}}
          </span>
      </li>
      <li v-if="hover">
          <img :src="getUrl(card.language)" :alt="card.language">
      </li>
      <li v-if="hover">
          <span>
              ({{card.vote}} / 10)
              <font-awesome-icon
              v-for = '(star,index) in Math.round(card.vote/2)'
              :key= '"full_star-"+index'
              icon='star'
              class="fullStar"
              />
              <font-awesome-icon
              v-for = '(star,index) in 5 - Math.round(card.vote/2)'
              :key= '"empty_star-"+index'
              icon='star'
              class="emptyStar"
              />
              ({{card.vote/2}} / 5)
          </span>
      </li>
    </ul>
</div>
</template>

<script>
import { library } from '@fortawesome/fontawesome-svg-core';
import { faStar } from '@fortawesome/free-solid-svg-icons';
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
    getUrl(lan) {
      if (this.flags[lan]) {
        return this.flags[lan];
      }
      return '';
    },
    getPath() {
      return this.card.poster;
    },
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
.card {
  width: 300px;
  .fullStar {
    // font-size: 1.15rem;
    path{
      fill:gold;
    }
  }
  .emptyStar {
    // font-size: 1.15rem;
    path {
      fill:grey;
    }
  }
}
</style>
