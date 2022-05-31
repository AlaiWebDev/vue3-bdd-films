<template>
  <div class="home">
    <div class="feature-card">
        <img :src="image" class="featured-img" alt="image Logo">
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
// @ est un alias à /src (uniquement lorsque webpack est incous dans les modules du projet)
import env from '@/env.js';
import image from "@/assets/logo.jpg";
import afficheDefaut from '@/assets/defaut.jpg'

export default {
  data: function () {
    return {
      search: "",
      movies: [],
      afficheUrl: env.imgMovieLinks,
      image,
      afficheDefaut,
      apikey: env.apikey
    }
  },
  methods: {
    SearchMovies: function () {
      if (this.search != "") {
        fetch(`https://api.themoviedb.org/3/search/movie?api_key=${this.apikey}&query=${this.search}&language=fr`)
          .then(response => response.json())
          .then(data => {
            this.movies = data.results;
            this.search = ""; // remet à zéro la saisie
            console.log(this.movies[0].adult);
            return this.movies;
          });
      }
    }
  }
}
</script>
<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Amatic+SC:wght@700&family=Ubuntu:wght@700&display=swap');
  @media screen and (min-width: 800px) {
  .home {
    .feature-card {
      display: flex;
      flex-direction: column;
      .featured-img {
        display: block;
        width: fit-content;
        margin: auto;
        height: 19rem;
        object-fit: cover;
      }
      .details {
        text-align: center;
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
          width: 30%;
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
      width: 60%;
      flex-wrap: wrap;
      margin: 0 auto;

      .movie {
        max-width: 20%;
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
  
}
@media screen and (max-width: 767px) {

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
} 

</style>
