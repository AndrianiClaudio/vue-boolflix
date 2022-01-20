<template>
    <ul class='card'>
        <li>
            <span>
                Titolo: {{card.title}}
            </span>
        </li>
        <li>
            <span>
                Titolo originale: {{card.original_title}}
            </span>
        </li>
        <li>
            <span>
                Lingua: {{card.language}}
            </span>
        </li>
        <li>
            <span>
                Voto: {{card.vote}}
            </span>
        </li>
    </ul>
</template>

<script>
export default {
  name: 'Card',
  data() {
    return {
      dataLoad: false,
    };
  },
  props: {
    card: Object,
  },
  created() {
    this.dataLoad = true;
  },
};
</script>

<style>

</style>
