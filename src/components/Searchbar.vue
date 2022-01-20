<template>
<div class="searchbar">
    <input
    :type="input.type"
    v-model="input.value"
    :placeholder="input.placeholder"
    @keyup.enter="searchFilmByValue(),$emit('printFilm',cards)"
    >
    <!-- @keyup.enter="searchFilmByValue(),$emit('printFilm',cards)" -->
    <button id="searchbar-btn" name="searchbar-btn"
    @click="searchFilmByValue(),$emit('printFilm',cards)"
    @keyup.enter="searchFilmByValue(),$emit('printFilm',cards)"
    >
        {{button.value}}
    </button>
    <span v-if="input.error.emptyError">Digita qualcosa da ricercare</span>
    <!-- eslint-disable-next-line max-len -->
    <span v-if="input.error.movie.notFound">Non &eacute; stato trovato nessun film contente il nome richiesto</span>
    <!--  eslint-disable-next-line max-len -->
    <span v-if="input.error.tv.notFound">Non &eacute; stato trovato nessuna serie contente il nome richiesto</span>
</div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Searchbar',
  data() {
    return {
      queryApi: {
        prefix: 'https://api.themoviedb.org/3/search/',
        folder: '',
        params: {
          api_key: 'c22bfc860e4fafc65337bd37d36134e0',
          language: 'en-US',
          query: '',
        },
      },
      input: {
        type: 'text',
        placeholder: 'Ricerca qui un film',
        value: '',
        error: {
          emptyError: false,
          movie: {
            notFound: false,
          },
          tv: {
            notFound: false,
          },
        },
      },
      button: {
        value: 'cerca',
      },
      cards: {
        movie: [],
        tv: [],
      },
    };
  },
  methods: {
    getMovie() {
      this.cards.movie = [];
      this.input.error.emptyError = false;
      this.input.error.movie.notFound = false;
      // if (this.input.error.movie.notFound) {
      //   this.cards.movie = [];
      // }
      this.queryApi.folder = 'movie';
      this.queryApi.params.query = this.input.value.split(' ').join('+');
      // chiamata axios
      axios.get(`${this.queryApi.prefix}${this.queryApi.folder}`, { params: this.queryApi.params })
        .then((r) => {
          r.data.results.forEach((el) => {
            // console.log(el);
            this.cards.movie.push({
              title: el.title,
              original_title: el.original_title,
              language: el.original_language,
              vote: el.vote_average,
            });
          });
        })
        .catch((e) => { console.error(e); })
        .then(() => {
          if (this.cards.movie.length === 0) {
            this.input.error.movie.notFound = true;
            this.input.error.emptyError = false;
          }
        });
    },
    getTv() {
      this.cards.tv = [];
      this.input.error.emptyError = false;
      this.input.error.tv.notFound = false;

      this.queryApi.folder = 'tv';
      this.queryApi.params.query = this.input.value.split(' ').join('+');
      // chiamata axios
      axios.get(`${this.queryApi.prefix}${this.queryApi.folder}`, { params: this.queryApi.params })
        .then((r) => {
          r.data.results.forEach((el) => {
            this.cards.tv.push({
              title: el.name,
              original_title: el.name,
              language: el.original_language,
              vote: el.vote_average,
            });
          });
        })
        .catch((e) => { console.error(e); })
        .then(() => {
          if (this.cards.tv.length === 0) {
            this.input.error.tv.notFound = true;
            this.input.error.emptyError = false;
          }
        });
    },
    searchFilmByValue() {
      if (this.input.value.length === 0) {
        this.input.error.movie.notFound = false;
        this.input.error.tv.notFound = false;
        this.input.error.emptyError = true;
      } else {
        this.getMovie();
        this.getTv();
      }
      return this.cards;
    },
  },
};
</script>

<style>

</style>
