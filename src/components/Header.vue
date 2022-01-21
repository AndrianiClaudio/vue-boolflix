<template>
  <header class="header">
    <h1>Boolflix</h1>
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
  @include padding($x:3rem);
  h1{
    font-size: $titleFont;
    color: $titleColor;
  }
}
</style>
