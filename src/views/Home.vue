<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <h1>Movies</h1>

    <div>
      Movie Title:
      <input type="text" v-model="newMovieParams.title" />
    </div>
    <div>
      Year:
      <input type="integer" v-model="newMovieParams.year" />
    </div>
    <div>
      Plot:
      <input type="text" v-model="newMovieParams.plot" />
    </div>
    <div>
      Director:
      <input type="text" v-model="newMovieParams.director" />
    </div>
    <p>{{ newMovieParams }}</p>
    <button v-on:click="createMovie()">Create Movie</button>

    <div v-for="movie in movies" v-bind:key="movie.id">
      <h2>{{ movie.title }}</h2>
      <p>{{ movie.year }}</p>
      <button v-on:click="showMore()">Show More</button>
      <p v-if="moreInfo === true">{{ movie.plot }}</p>
      <p v-if="moreInfo === true">{{ movie.director }}</p>
      <br />
      <br />
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Movies",
      movies: [],
      moreInfo: false,
      newMovieParams: {},
    };
  },
  created: function () {
    this.indexMovies();
  },
  methods: {
    indexMovies: function () {
      axios.get("http://127.0.0.1:3000/movies").then((response) => {
        console.log(response.data);
        this.movies = response.data;
      });
    },
    showMore: function () {
      this.moreInfo = !this.moreInfo;
    },
    createMovie: function () {
      axios
        .post("http://127.0.0.1:3000/movies", this.newMovieParams)
        .then((response) => {
          console.log(response.data);
          this.movies.push(response.data);
        })
        .catch((error) => {
          console.log(error.response.data.error);
        });
    },
  },
};
</script>
