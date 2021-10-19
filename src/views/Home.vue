<template>
  <div class="home">
    <img
      alt="Vue logo"
      src="https://images.unsplash.com/photo-1478720568477-152d9b164e26?ixid=MnwxMjA3fDB8MHxzZWFyY2h8Nnx8bW92aWV8ZW58MHx8MHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=500&q=60"
    />
    <h1>JW Movies</h1>

    <div>
      Search:
      <input v-model="titleFilter" type="text" list="titles" />
      <datalist id="titles">
        <option v-for="movie in movies" v-bind:key="movie.id">{{ movie.title }}</option>
      </datalist>
    </div>
    <button class="button" v-on:click="setSortOrder('title')">Sort by Title</button>

    <button class="button" v-on:click="setSortOrder('year')">Sort by Year</button>
    <div
      v-for="movie in orderBy(filterBy(movies, titleFilter, 'title'), sortAttribute, sortOrder)"
      v-bind:key="movie.id"
    >
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
  color: rgb(5, 83, 18);
  padding: 15px 15px;
  margin: 10px 10px;
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

textarea {
  height: calc(2.5em + 4.75rem + 2px);
  font-family: Verdana;
  font-size: 14;
  width: 40%;
}

label {
  display: inline-block;
  max-width: 45%;
  text-align: center;
  color: rgb(15, 56, 104);
  padding: 0.2em 0.5em;
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
    setSortOrder: function (attribute) {
      this.sortOrder = this.sortOrder * -1;
      this.sortAttribute = attribute;
    },
  },
};
</script>
