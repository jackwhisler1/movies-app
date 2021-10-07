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
      <router-link :to="`/movies/${movie.id}`"><button class="button">See Details</button></router-link>

      <br />
      <br />
    </div>
  </div>
</template>
<style scoped>
.button {
  background-color: #dde9de; /* Green */
  font-family: "Lucida Sans", "Lucida Sans Regular", "Lucida Grande", "Lucida Sans Unicode", "Geneva", "Verdana",
    "sans-serif";
  border: none;
  color: #28942c;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 14px;
  border-radius: 3px;
  transition-duration: 0.07s;
  cursor: pointer;
}

.button:hover {
  background-color: #4caf50; /* Green */
  color: white;
  text-decoration: none;
}
</style>

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
