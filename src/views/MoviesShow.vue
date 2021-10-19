<template>
  <div class="movies-show">
    <h1>{{ movie.title }}</h1>
    <h2>Directed by {{ movie.director }}</h2>
    <p>Starring {{ movie.actors[0].first_name }} {{ movie.actors[0].last_name }}</p>
    <p>{{ movie.plot }}</p>
    <p>{{ movie.genres[0].name }}</p>
    <p>{{ movie.year }}</p>
    <button v-on:click="editMovie(movie)">Update</button>
    <button v-on:click="destroyMovie(movie)">Delete</button>
    <dialog id="movie-edit">
      <form method="dialog">
        <div>
          Movie Title:
          <input type="text" v-model="movie.title" />
        </div>
        <div>
          Year:
          <input type="integer" v-model="movie.year" />
        </div>
        <div>
          Plot:
          <input type="text" v-model="movie.plot" />
        </div>
        <div>
          Director:
          <input type="text" v-model="movie.director" />
        </div>
        <button v-on:click="editMovie(movie)">Submit</button>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      movie: {},
    };
  },
  created: function () {
    this.showMovie();
  },
  methods: {
    showMovie: function () {
      axios.get(`http://127.0.0.1:3000/movies/${this.$route.params.id}`).then((response) => {
        console.log("movies show", response);
        this.movie = response.data;
      });
    },
    editMovie: function (movie) {
      document.querySelector("#movie-edit").showModal();
      let editMovieParams = movie;
      axios
        .patch(`http://127.0.0.1:3000/movies/${movie.id}`, editMovieParams)
        .then((response) => {
          console.log("movies edit", response);
          this.$router.push(`/movies/${response.data.id}`);
        })
        .catch((error) => {
          console.log("movies update error", error.response);
        });
    },
    destroyMovie: function (movie) {
      axios.delete(`http://127.0.0.1:3000/movies/${movie.id}`).then((response) => {
        console.log("movie deleted", response.data);
        this.$router.push("/");
      });
    },
  },
};
</script>
