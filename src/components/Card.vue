<template>
    <ul class='card'>
      <li>
        <img :src="card.poster" alt="Immagine poster non trovata">
      </li>
      <li>
          <span>
              {{card.title}}
          </span>
      </li>
      <li>
          <span>
              {{card.original_title}}
          </span>
      </li>
      <li>
          <img :src="getUrl(card.language)" :alt="card.language">
      </li>
      <li>
          <span>
              {{card.vote}}
              <font-awesome-icon
              v-for = '(star,index) in Math.round(card.vote)'
              :key= '"full_star-"+index'
              icon='star'
              class="fullStar"
              />
              <font-awesome-icon
              v-for = '(star,index) in 10 - Math.round(card.vote)'
              :key= '"empty_star-"+index'
              icon='star'
              class="emptyStar"
              />
          </span>
      </li>
    </ul>
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
  },
};
</script>

<style lang='scss' scoped>
.svg-inline--fa.fullStar {
  color: gold;
}
.svg-inline--fa.emptyStar {
  color:grey;
}
</style>
