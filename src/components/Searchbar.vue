<template>
<div class="searchbar">
    <span v-if="input.emptyError" class="emptyError">Digita qualcosa da ricercare!!</span>
    <input
    :type="input.type"
    v-model="input.value"
    :placeholder="input.placeholder"
    @keyup.enter="searchFilmByValue(),
    $emit('printFilm',[cards,input.nf_error])"
    >
    <!-- <button id="searchbar-btn" name="searchbar-btn"
    @click="searchFilmByValue(),
    $emit('printFilm',[cards,input.nf_error])"
    > -->
        <!-- {{button.value}} -->
    <!-- </button> -->
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
          language: 'it-IT',
          query: '',
        },
      },
      input: {
        type: 'text',
        placeholder: 'Ricerca film o serie tv',
        value: '',
        emptyError: false,
        nf_error: {
          movie: false,
          tv: false,
        },
      },
      // button: {
      //   value: 'cerca',
      // },
      cards: {
        movie: [],
        tv: [],
      },
      poster_path: { prefix: 'https://image.tmdb.org/t/p/', dim: 'w300' },
    };
  },
  methods: {
    axiosCall(array, folder) {
      array.splice(0, array.length);
      this.queryApi.folder = folder;
      this.queryApi.params.query = this.input.value.split(' ').join('+');
      axios.get(`${this.queryApi.prefix}${this.queryApi.folder}`, { params: this.queryApi.params })
        .then((r) => {
          r.data.results.forEach((el) => {
            if (folder === 'movie') {
              this.input.nf_error.movie = false;
              array.push({
                title: el.title,
                original_title: el.original_title,
                language: el.original_language,
                vote: el.vote_average,
                poster: (el.poster_path !== null) ? (this.poster_path.prefix + this.poster_path.dim + el.poster_path) : '',
              });
            } else if (folder === 'tv') {
              this.input.nf_error.tv = false;
              array.push({
                title: el.name,
                original_title: el.original_name,
                language: el.original_language,
                vote: el.vote_average,
                poster: (el.poster_path !== null) ? (this.poster_path.prefix + this.poster_path.dim + el.poster_path) : '',
              });
            }
          });
        })
        .catch((e) => { console.error(e); })
        .then(() => {
          if (array.length === 0) {
            if (folder === 'movie') {
              this.input.nf_error.movie = true;
            } else if (folder === 'tv') {
              this.input.nf_error.tv = true;
            }
          }
        });
    },
    getMovie() {
      this.axiosCall(this.cards.movie, 'movie');
    },
    getTv() {
      this.axiosCall(this.cards.tv, 'tv');
    },
    searchFilmByValue() {
      this.input.emptyError = false;
      this.input.nf_error.movie = false;
      this.input.nf_error.tv = false;
      if (this.input.value.length === 0) {
        this.input.emptyError = true;
      } else {
        this.getMovie();
        this.getTv();
        this.input.value = '';
      }
      return [this.cards, this.input.nf_error];
    },
  },
};
</script>

<style lang='scss' scoped>
@import '../assets/scss/partials/_variables.scss';
@import '../assets/scss/partials/_mixins.scss';
.searchbar {
  color: white;
  padding-right: 3rem;
  .emptyError {
    text-transform: uppercase;
    padding-right: 2rem;
  }
  input {
    padding: .25rem .5rem
  }
}
</style>
