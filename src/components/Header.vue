<template>
  <header class="header">
    <h1>Boolflix</h1>
    <!--
      Searchbar invia tramite emit
      ...attiva funzione per passare i dati al componente App.vue
     -->
    <Searchbar
    @printFilm = 'printFilm($event)'
    />
  </header>
</template>

<script>
import Searchbar from './Searchbar.vue';

export default {
  name: 'Header',
  components: {
    Searchbar,
  },
  methods: {
    // funzione di passaggio dati
    printFilm(e) {
      this.$emit('printFilm', e);
    },
  },
};
</script>

<style lang="scss" scoped>
@import '../assets/scss/partials/_variables.scss';
@import '../assets/scss/partials/_mixins.scss';
.header {
  @include flex(center,space-between);
  height: $headerHeight;
  background-color: $headerBgColor;
  h1{
    padding-left: 3rem;
    font-size: $titleFont;
    color: $titleColor;
    text-transform: uppercase;
    font-weight: normal;
  }
}
</style>
