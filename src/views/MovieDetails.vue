<template>
  <div class="movie-details">
      <h2>{{ movie.title }}</h2>
      <p>Date de sortie : {{ movie.release_date }}</p>
      <img :src="afficheUrl + movie.backdrop_path" alt="Affiche du film" class="featured-img" />
      <div class="synoptique">
          {{ movie.overview }}
      </div>
      <div class="titre"></div>
  </div>
</template>

<script>
import { ref, onBeforeMount } from 'vue';
import { useRoute } from 'vue-router';
import env from '@/env.js';

export default {
    setup () {
        const movie = ref({});
        const route = useRoute();
        const afficheUrl = env.imgMovieLinks
        // const affiche = ref("");

        onBeforeMount(() => {
            fetch(`https://api.themoviedb.org/3/movie/${route.params.id}?api_key=${env.apikey}&language=fr-FR`)
            .then(response => response.json())
            .then(data => {
                console.log(data);
                movie.value = data;
            })
        });

        return {
            movie,
            afficheUrl
        }
    }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Rowdies:wght@300&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Amatic+SC:wght@700&display=swap');
    .movie-details {
        h2, p, .synoptique {
            padding: 1rem .5rem;
            font-family: 'Rowdies', cursive;
        }

        .synoptique {
            font-family: 'Rowdies', cursive;
        }
        img {
            display: block;
            width: 100%;
            object-fit: scale-down;
        }
    }

</style>