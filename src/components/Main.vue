<template>
<main class="main" v-if="films.length > 0">
    <ul>
        <li
        v-for="(film,index) in films"
        :key="'film-'+index"
        >
            <ul>
                <li>
                    Titolo: {{film.title}}
                </li>
                <li>
                    Titolo originale: {{film.originalTitle}}
                </li>
                <li>
                    Lingua: {{film.language}}
                </li>
                <li>
                    Voto: {{film.vote}}
                </li>
            </ul>
        </li>
    </ul>
</main>
</template>

<script>
export default {
  name: 'Main',
  props: {
    films: Array,
  },
};
</script>

<style>

</style>
