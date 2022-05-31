<template>
    <div class="movie-details">
        <h2 id="titre">{{ movie.title }}</h2>
        <div class="genres" ><span v-for="genre in genresFilm" :key="genre.name">{{ genre }}&nbsp;</span></div>
        <h2 id="date_sortie">Date de sortie : <span>{{ dateSortie }}</span></h2>
        <img :src="afficheUrl + movie.backdrop_path" alt="Affiche du film" class="featured-img" />
        <div class="synoptique">
            {{ movie.overview }}
        </div>
        <div class="titre"></div>
    </div>
</template>

<script>
// import { ref, onBeforeMount } from 'vue';
import { useRoute } from 'vue-router';
import env from '@/env.js';

export default {
    data: function () {
        return {
            movie: {},
            route: useRoute(),
            afficheUrl: env.imgMovieLinks,
            genresFilm: [],
            dateSortie: ""
        }
    },
    beforeMount() {

            fetch(`https://api.themoviedb.org/3/movie/${this.route.params.id}?api_key=${env.apikey}&language=fr-FR`)
            .then(response => response.json())
            .then(data => {
                this.movie = data;
                console.log(this.movie);
                for (const genre of this.movie.genres) {
                    this.genresFilm.push(genre.name);
                }
                console.log("L'objet GenresFilm contient : " + this.genresFilm);
                var timestamp = Date.parse(this.movie.release_date);
                var dateTime = new Date(timestamp);
                console.log("Jour : " + dateTime.getDate());
                console.log("Mois : " + dateTime.getMonth());
                this.dateSortie = ((dateTime.getDate().toString().length === 1 ? ("0" +(dateTime.getDate())) : dateTime.getDate())) + "/" + ((((dateTime.getMonth()+1).toString().length === 1) ? ("0" + (dateTime.getMonth()+1)) : dateTime.getMonth()+1)) + "/" + dateTime.getFullYear();
                console.log("Date Sortie : ", this.dateSortie);
            });
        }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Rowdies:wght@300&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Amatic+SC:wght@700&display=swap');
@media screen and (min-width: 800px) {
    .movie-details {
        width: 80%;
        margin: auto;
        text-align: center;
        .synoptique {
        width: 50%;
        margin: 1rem auto;
        }
        img {
            display: block;
            margin: auto;
            width: 50%;
        }
        h2, #genres, #date_sortie {
            margin: 1rem auto;
        padding: .5rem .5rem
    }
    }
}
@media screen and (max-width: 767px) {
    .movie-details {
        display: flex;
        flex-direction: column;
        align-items: center;

        .synoptique {
        padding: 1rem .5rem;
        font-family: 'Rowdies', cursive;
        }

        #titre {
            font-size: 1.5rem;
            padding: 0 .5rem;
            text-align: center;
        }

        span {
            font-weight: 200;
        }

        #genres, #date_sortie {
            padding: .5rem .5rem
        }

        .genres {
            display: flex;
            flex-direction: column;
            justify-content: space-evenly;
            align-items: center;
            padding: .5rem 1rem;
            bottom: 1rem;
            left: 0;
            text-transform: capitalize;
            }

        .synoptique {
            font-family: 'Rowdies', cursive;
        }
        img {
            display: block;
            width: 100%;
            padding: .5rem;
            object-fit: scale-down;
        }
    }
}
</style>
