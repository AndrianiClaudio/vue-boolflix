<template>
<div class="searchbar">
  <!--
    ricerca di film e serie tv alla pressione del tasto enter o al click sul bottone
    se non si digita niente compare span di errore
  -->
  <div class="error-container">
    <span v-if="input.emptyError" class="emptyError">Digita qualcosa da ricercare!!</span>
  </div>
  <!-- valore della input e del button salvato nei data -->
  <div class="container">
    <input
    :type="input.type"
    v-model="input.value"
    :placeholder="input.placeholder"
    @keyup.enter="searchFilmByValue(),
    $emit('printFilm',[cards,nf_error])"
    >
    <button id="searchbar-btn" name="searchbar-btn"
    @click="searchFilmByValue(),
    $emit('printFilm',[cards,nf_error])"
    >
        {{button.value}}
    </button>
  </div>
</div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Searchbar',
  data() {
    return {
      // dati trasmessi ad header tramite emit
      cards: {
        movie: [],
        tv: [],
      },
      nf_error: {
        movie: false,
        tv: false,
      },
      // dati della Api tramite la quale si accede ai dati dei film
      queryApi: {
        prefix: 'https://api.themoviedb.org/3/search/',
        params: {
          api_key: 'c22bfc860e4fafc65337bd37d36134e0',
          language: 'it-IT',
          // query: verrá riempita con input.value
          query: '',
        },
      },
      apiCast: {
        prefix: 'https://api.themoviedb.org/3/',
        post: '/credits',
        params: {
          api_key: 'c22bfc860e4fafc65337bd37d36134e0',
          language: 'it-IT',
          // query: verrá riempita con input.value
          // query: '',
        },
      },
      apiGen: {
        prefix: 'https://api.themoviedb.org/3/genre/',
        post: '/list',
        params: {
          api_key: 'c22bfc860e4fafc65337bd37d36134e0',
          language: 'it-IT',
          // query: verrá riempita con input.value
          // query: '',
        },
      },
      // poster_path contiene
      // il prefisso del link contenente le immagini poster dei film
      // la dimensione del poster dei film
      poster_path: {
        prefix: 'https://image.tmdb.org/t/p/',
        dim: 'w342',
      },
      // dati relativi alla searchbar
      input: {
        type: 'text',
        placeholder: 'Ricerca film o serie tv',
        value: '',
        emptyError: false,
      },
      button: {
        value: 'cerca',
      },
    };
  },
  methods: {

    getGenres(f, arrayGen) {
      const genresNames = [];
      axios.get(`${this.apiGen.prefix}${f}/${this.apiGen.post}`, { params: this.apiGen.params })
        .then((r) => {
          r.data.genres.forEach((el) => {
            if (arrayGen.includes(el.id)) {
              console.log('arrayGenIncludes: ', el.name);
              genresNames.push(el.name);
            }
          });
        });
      return genresNames;
    },
    apiCastRequest(array, i, id, f) {
      axios.get(`${this.apiCast.prefix}${f}/${id}${this.apiCast.post}`, { params: this.apiCast.params })
        .then((r) => {
          r.data.cast.forEach((el) => {
            array[i].cast.push(el.original_name);
          });
          // console.log(array[i].cast);
          // r.data.cast.forEach((el) => {
          // array[i].cast.push(el.original_name);
          // console.log(el);
        });
    },
    /**
     * funzione contenente una chiamata axios.
     * array sará uno degli array di this.cards
     * folder sara una stringa che indica la cartella in cui la api dovra cercare
     */
    axiosCall(array, folder) {
      // elimina ogni eventuale ricerca precedente
      array.splice(0, array.length);
      // elimina ogni eventuale spazio e divide le parole con +
      this.queryApi.params.query = this.input.value.trim().split(' ').join('+');
      axios.get(`${this.queryApi.prefix}${folder}`, { params: this.queryApi.params })
        .then((r) => {
          r.data.results.forEach((el, index) => {
            const genresNames = this.getGenres(folder, el.genre_ids);
            console.log('genresNames', genresNames);
            // carica i dati ricevuti negli array in base alla folder
            // diverse folder hanno attributi di nome differente!!
            // console.log(this.getGenreNameById(folder, el.genre_ids));
            if (folder === 'movie') {
              this.nf_error.movie = false;
              array.push({
                // id: el.id,
                title: el.title,
                original_title: el.original_title,
                language: el.original_language,
                vote: el.vote_average,
                poster: (el.poster_path !== null) ? (this.poster_path.prefix + this.poster_path.dim + el.poster_path) : '',
                overview: el.overview,
                // type: folder,
                cast: [],
                genres: genresNames,
                // genres: Object.assign(this.getGenreNameById(folder, el.genre_ids)),
              });
              // console.log(array.genres);
              // console.log(el.genre_ids);
              this.apiCastRequest(array, index, el.id, folder);
            } else if (folder === 'tv') {
              this.nf_error.tv = false;
              array.push({
                // id: el.id,
                title: el.name,
                original_title: el.original_name,
                language: el.original_language,
                vote: el.vote_average,
                poster: (el.poster_path !== null) ? (this.poster_path.prefix + this.poster_path.dim + el.poster_path) : '',
                overview: el.overview,
                // type: folder,
                cast: [],
                // genres: el.genre_ids,
                genres: genresNames,
              });
              // console.log(array.genres);
              // console.log(this.getGenreNameById);
              // consolethis.getGenreNameById(el.genre_ids, folder),.log(el.genre_ids);
              this.apiCastRequest(array, index, el.id, folder);
            }
          });
        })
        .catch((e) => { console.error(e); }) // se ci sono errori nella chiamata axios
        .then(() => {
          // imposta a true variabili di nf_error se un array riceve 0 dati
          if (array.length === 0) {
            if (folder === 'movie') {
              this.nf_error.movie = true;
            } else if (folder === 'tv') {
              this.nf_error.tv = true;
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
    /**
     * Funzione chiamata al momento del click sulla searchbar
     * restituisce e invia a Header this.cards e this.nf_error
     *  */
    searchFilmByValue() {
      // ad ogni nuova ricerca resetta ogni errore
      this.input.emptyError = false;
      this.nf_error.movie = false;
      this.nf_error.tv = false;
      if (this.input.value.length === 0) { // se si ricerca una stringa vuota
        this.input.emptyError = true;
      } else {
        this.getMovie();
        this.getTv();
        // reset valore input searchbar
        this.input.value = '';
      }
      return [this.cards, this.nf_error];
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
  input,
  button {
    padding: .25rem .5rem
  }
}
</style>
