<template>
  <div class="home">
    <h1 class="display-4">J.Whis. Films</h1>
    <br />
    <span></span>

    <br />

    <div>
      Search:
      <input v-model="titleFilter" type="text" list="titles" />
      <datalist id="titles">
        <option v-for="movie in movies" v-bind:key="movie.id">{{ movie.title }}</option>
      </datalist>
    </div>
    <button class="button" v-on:click="setSortOrder('title')">Sort by Title</button>

    <button class="button" v-on:click="setSortOrder('year')">Sort by Year</button>
    <div class="container">
      <!-- <div class="card-group"> -->
      <div class="row row-cols-2 row-cols-md-4 g-4">
        <div
          class="col"
          v-for="movie in orderBy(filterBy(movies, titleFilter, 'title'), sortAttribute, sortOrder)"
          v-bind:key="movie.id"
        >
          <div class="card">
            <img
              v-bind:src="`https://source.unsplash.com/random/300x200?sig=${movie.id}`"
              v-bind:alt="movie.title"
              class="card-img-top"
            />
            <div class="card-body">
              <h5 class="card-title">{{ movie.title }}</h5>
              <p class="card-text">
                {{ movie.director }}
              </p>
              <p>{{ movie.year }}</p>
              <router-link :to="`/movies/${movie.id}`">
                <button class="btn btn-success">See Details</button>
              </router-link>
            </div>
            <!-- </div> -->
          </div>
        </div>
      </div>
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

h1 {
  margin: 1.2em;
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
