<template>
  <div class="home">
    <div class="feature-card">
        <img :src="image" class="featured-img">
        <div class="details">
          <h3>Fan de cinéma...</h3>
          <p>Je suis un amoureux du cinéma et je vous souhaite la bienvenue sur mon appli... encyclopédique</p>
        </div>
    </div>
    <form @submit.prevent="SearchMovies()" class="search-box">
      <input
      type="text"
      placeholder="Que recherchez-vous ?"
      v-model="search" />
      <input type="submit" value="Rechercher" />
    </form>
    <div class="movies-list">
      <div class="movie" v-for="movie in movies" :key="movie.id">
        <router-link :to="'/movie/' + movie.id" class="movie-link">
          <div class="product-image">
            <img :src="movie.backdrop_path ? (afficheUrl + movie.backdrop_path) : afficheDefaut" alt="Affiche du film" />
            <div v-if="movie.genre_ids.length" class="genre"><span v-for="(genre, index) in movies.genre_ids" :key="index">{{ genre.id }}</span></div>
          </div>
          <div class="detail">
            <p class="annee">{{ movie.release_date }}</p>
            <h3>{{ movie.title }}</h3>
          </div>
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import { ref } from 'vue';
import env from '@/env.js';
import image from "@/assets/logo.jpg";
import afficheDefaut from '@/assets/defaut.jpg'

export default {
  setup () {
    const search = ref("");
    const movies = ref([]);
    const genres = ref([]);
    const afficheUrl = env.imgMovieLinks
    // const filmGenre = ref("");

    fetch(`https://api.themoviedb.org/3/genre/movie/list?api_key=${env.apikey}&language=fr-FR`)
          .then(response => response.json())
          .then(data => {
            genres.value = data;
            // console.log(data);
            // console.log(genres.value);
          })

    const SearchMovies = () => {
      if (search.value != "") {
        fetch(`https://api.themoviedb.org/3/search/movie?api_key=${env.apikey}&query=${search.value}&language=fr`)
          .then(response => response.json())
          .then(data => {
            movies.value = data.results;
            search.value = ""; //remet à zéro la saisie
            console.log(movies.value[0].adult);
            for (let index = 0; index < movies.value.length; index++) {
              const element = movies.value[index];
              console.log("Film : " + element.genre_ids);
            }
            
            // console.log("Genres des films " + data.results.genre_ids);
            for (const film of data.results) {
              console.log(film.genre_ids);
            }
            // filmGenre = data.results
            // movies = data;
            
          });
          movies.value.forEach(element => console.log("Élément" + element.genre_ids));
      }
    }

    return {
      search,
      movies,
      SearchMovies,
      afficheUrl,
      image,
      afficheDefaut
    }
  }
}
</script>
<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Amatic+SC:wght@700&family=Ubuntu:wght@700&display=swap');
  .home {

    .feature-card {
      position: relative;

      .featured-img {
        display: block;
        width: 100%;
        height: 19rem;
        object-fit: cover;
        position: relative;
        z-index: 0;
      }

      .details {
        position: absolute;
        left: 0;
        right: 0;
        bottom: 0;
        background-color: rgba(0, 0, 0, .6);
        padding: 1rem;
        z-index: 1;

        h3 {
        color: #fff;
        margin-bottom: 1rem;
        }

        p, h3 {
          color: #fff;
          font-family: 'Amatic SC', cursive;
          font-size: 1.5rem;
        }
      }
    }
    .search-box {
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      padding: 1rem;

      input {
        display: block;
        appearance: none;
        border: none;
        outline: none;
        background: none;

        &[type="text"] {
          width: 100%;
          color: #fff;
          background-color: #496583;
          font-size: 1.1rem;
          padding: .75rem 1rem;
          border-radius: .5rem;
          margin-bottom: 1rem;
          transition: 0.4s;

          &::placeholder {
            color: #f3f3f3;
          }

          &:focus {
            box-shadow: 0px 3px 6px rgba(0, 0, 0, 0.2);
          }
        }
        &[type="submit"] {
          width: 100%;
          max-width: 19rem;
          background-color: #42B883;
          padding: 1rem;
          border-radius: .5rem;
          color:#fff;
          font-size: 1.1rem;
          text-transform: uppercase;
          transition: 0.4s;

          &:active {
            background-color: #3B8070;
          }
        }
      }
    }

    .movies-list {
      display: flex;
      flex-wrap: wrap;
      margin: 0 .5rem;

      .movie {
        max-width: 50%;
        flex: 1 1 50%;
        padding: 1rem .5rem;

        .movie-link {
          display: flex;
          flex-direction: column;
          height: 100%;

          .product-image {
            position: relative;
            display: block;

            img {
              display: block;
              width: 100%;
              // height: 17rem;
              object-fit: scale-down;
            }

            .genre {
              position: absolute;
              padding: .5rem 1rem;
              background-color: #42B883;
              color: #fff;
              bottom: 1rem;
              left: 0;
              text-transform: capitalize;
            }
          }

          .detail {
            background-color: #496583;
            padding: 1rem .5rem;
            flex: 1 1 100%;
            border-radius: 0px 0px .5rem .5rem;

            .annee {
              color: #AAA;
              font-size: .75rem;
            }

            h3 {
              color: #fff;
              font-weight: 600;
              font-size: 1.1rem;
            }
          }
        }
      }
    }
  }
  

</style>
