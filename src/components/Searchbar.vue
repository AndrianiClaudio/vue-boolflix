<template>
<div class="searchbar">
    <input
    :type="input.type"
    v-model="input.value"
    :placeholder="input.placeholder"
    @keyup.enter="$emit('printFilm',searchFilm),searchFilmByValue()"
    >
    <button id="searchbar-btn" name="searchbar-btn"
    @click="$emit('printFilm',searchFilm),searchFilmByValue()"
    @keyup.enter="$emit('printFilm',searchFilm),searchFilmByValue()"
    >
        {{button.value}}
    </button>
    <span v-if="input.emptyError">Digita qualcosa da ricercare</span>
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
        //   queryRequest: '',
        },
      },
      input: {
        type: 'text',
        placeholder: 'Ricerca qui un film',
        emptyError: false,
        value: '',
      },
      button: {
        value: 'cerca',
      },
      searchFilm: [],
    };
  },
  methods: {
    searchFilmByValue() {
      let fullQueryApi = this.queryApi.prefix + this.queryApi.apiKey + this.queryApi.query.prefix;
      if (this.input.value.length === 0) { this.input.emptyError = true; } else { // ricerca
        if (this.input.emptyError) { this.input.emptyError = false; }
        if (this.searchFilm.length > 0) { this.searchFilm = []; }
        fullQueryApi += this.input.value;
        // chiamata axios
        axios.get(fullQueryApi)
          .then((r) => {
            r.data.results.forEach((el) => {
              this.searchFilm.push({
                title: el.title,
                originalTitle: el.original_title,
                language: el.original_language,
                vote: el.vote_average,
              });
            });
          })
          .catch((e) => { console.error(e); })
          .then(() => this.searchFilm);
      }
    },
  },
};
</script>

<style>

</style>
