<template>
<header class="header">
    <Searchbar
    @printFilm = 'printSearched($event)'
    />
    <ul>
        <li
        v-for="(film,index) in films"
        :key="'film-'+index"
        >
        <ul>
            <!-- <li v-for="(data,index) in film"
            :key = "'data-'+index">
                {{data}}
            </li> -->
            <li>
                Titolo: {{film.title}}
            </li>
            <li>
                Titolo originale: {{film.originalTitle}}
            </li>
            <li>
                Lingua: {{film.language}}
            </li>
            <li>
                Voto: {{film.vote}}
            </li>
        </ul>
        </li>
    </ul>
</header>
</template>

<script>
import Searchbar from './Searchbar.vue';

export default {
  name: 'Header',
  components: {
    Searchbar,
  },
  data() {
    return {
      films: [],
    };
  },
  methods: {
    printSearched(e) {
      this.films = e;
    },
  },
};
</script>

<style lang="scss" scoped>

</style>
