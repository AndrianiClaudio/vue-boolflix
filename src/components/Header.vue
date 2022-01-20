<template>
  <header class="header">
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

</style>
