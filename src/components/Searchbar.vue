<template>
<div class="searchbar">
    <input
    :type="input.type"
    v-model="input.value"
    :placeholder="input.placeholder"
    @keyup.enter="searchFilmByValue(),$emit('printFilm',films)"
    >
    <button id="searchbar-btn" name="searchbar-btn"
    @click="searchFilmByValue(),$emit('printFilm',films)"
    @keyup.enter="searchFilmByValue(),$emit('printFilm',films)"
    >
        {{button.value}}
    </button>
    <span v-if="input.error.emptyError">Digita qualcosa da ricercare</span>
    <!-- eslint-disable-next-line max-len -->
    <span v-if="input.error.notFound">Non &eacute; stato trovato nessun film contente il nome richiesto</span>
</div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Searchbar',
  data() {
    return {
      queryApi: {
        prefix: 'https://api.themoviedb.org/3/search/movie?api_key=',
        apiKey: 'c22bfc860e4fafc65337bd37d36134e0',
        query: {
          prefix: '&query=',
          queryRequest: '',
        },
      },
      input: {
        type: 'text',
        placeholder: 'Ricerca qui un film',
        value: '',
        error: {
          emptyError: false,
          notFound: false,
        },
      },
      button: {
        value: 'cerca',
      },
      films: [],
    };
  },
  methods: {
    searchFilmByValue() {
      if (this.input.value.length === 0) {
        this.input.error.notFound = false;
        this.input.error.emptyError = true;
      } else { // ricerca
        this.input.error.emptyError = false;
        this.input.error.notFound = false;
        if (this.films.length > 0) {
          this.films = [];
        }
        this.queryRequest = this.input.value.split(' ').join('+');
        // eslint-disable-next-line max-len
        const fullQueryApi = this.queryApi.prefix + this.queryApi.apiKey + this.queryApi.query.prefix + this.queryRequest;
        // chiamata axios
        axios.get(fullQueryApi)
          .then((r) => {
            r.data.results.forEach((el) => {
              this.films.push({
                title: el.title,
                originalTitle: el.original_title,
                language: el.original_language,
                vote: el.vote_average,
              });
            });
          })
          .catch((e) => { console.error(e); })
          .then(() => {
            if (this.films.length === 0) {
              this.input.error.notFound = true;
              this.input.error.emptyError = false;
            }
            return this.films;
          });
      }
    },
  },
};
</script>

<style>

</style>
