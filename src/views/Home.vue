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
      <br />
      <small>{{ 500 - newMovieParams.plot.length }} characters remaining.</small>
    </div>
    <div>
      Director:
      <input type="text" v-model="newMovieParams.director" />
    </div>
    <p>{{ newMovieParams }}</p>
    <button v-on:click="createMovie()">Create Movie</button>
    <br />
    <br />
    <div>
      Search:
      <input v-model="titleFilter" type="text" list="titles" />
      <datalist id="titles">
        <option v-for="movie in movies" v-bind:key="movie.id">{{ movie.title }}</option>
      </datalist>
    </div>
    <div
      v-for="movie in orderBy(filterBy(movies, titleFilter, 'title'), sortAttribute, sortOrder)"
      v-bind:key="movie.id"
    >
      <button v-on:click="setSortOrder('title')">Sort by Title</button>

      <button v-on:click="setSortOrder('year')">Sort by Year</button>
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
import Vue2Filters from "vue2-filters";
export default {
  mixins: [Vue2Filters.mixin],

  data: function () {
    return {
      message: "Movies",
      movies: [],
      moreInfo: false,
      titleFilter: "",
      newMovieParams: {
        plot: "",
      },
      sortOrder: 1,
      sortAttribute: "",
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
          this.movies.year = parseInt(this.movies.year);
          this.movies.push(response.data);
        })
        .catch((error) => {
          console.log(error.response.data.error);
        });
    },
    setSortOrder: function (attribute) {
      this.sortOrder = this.sortOrder * -1;
      this.sortAttribute = attribute;
    },
  },
};
</script>
