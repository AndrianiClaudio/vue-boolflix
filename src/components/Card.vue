<template>
<!-- <div class="cards"> -->
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
<!-- </div> -->
</template>

<script>
export default {
  name: 'Card',
  props: {
    card: Object,
  },
};
</script>

<style>

</style>
