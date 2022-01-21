<template>
    <ul class='card'>
      <li>
        <img :src="card.poster" alt="Immagine poster non trovata">
      </li>
      <li>
          <span>
              {{card.title}}
          </span>
      </li>
      <li>
          <span>
              {{card.original_title}}
          </span>
      </li>
      <li>
          <img :src="getUrl(card.language)" :alt="card.language">
      </li>
      <li>
          <span>
              {{card.vote}}
          </span>
      </li>
    </ul>
</template>

<script>
export default {
  name: 'Card',
  data() {
    return {
      flags: {
        it: 'https://img.icons8.com/office/italy.png',
        en: 'https://img.icons8.com/color/usa.png',
        fr: 'https://img.icons8.com/color/france.png',
      },
    };
  },
  props: {
    card: Object,
  },
  methods: {
    getUrl(lan) {
      if (this.flags[lan]) {
        return this.flags[lan];
      }
      return '';
    },
  },
};
</script>

<style lang='scss' scoped>
</style>
