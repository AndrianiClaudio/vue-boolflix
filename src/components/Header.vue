<template>
<header class="header">
    <Searchbar
    @printFilm = 'sendFilms($event)'
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
  data() {
    return {
      films: [],
    };
  },
  methods: {
    sendFilms(e) {
      this.$emit('getFilms', e);
    },
  },
};
</script>

<style lang="scss" scoped>

</style>
