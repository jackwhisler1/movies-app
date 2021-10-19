<template>
  <div class="movies-new">
    <div>
      <label>Movie Title:</label>
      <br />
      <input type="text" v-model="newMovieParams.title" />
    </div>
    <div>
      <label>Year:</label>
      <br />
      <input type="integer" v-model="newMovieParams.year" />
    </div>
    <div>
      <label>Plot:</label>
      <br />
      <textarea type="text" class="form-control" v-model="newMovieParams.plot" />
      <br />
      <small>{{ 500 - newMovieParams.plot.length }} characters remaining.</small>
    </div>
    <div>
      <label>Director:</label>
      <br />
      <input type="text" v-model="newMovieParams.director" />
    </div>
    <button class="button" v-on:click="createMovie()">Create Movie</button>
    <br />
    <br />
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      movies: [],
      newMovieParams: {
        title: "",
        diretor: "",
        year: "",
        plot: "",
      },
      errors: [],
    };
  },
  methods: {
    createMovie: function () {
      axios
        .post("/movies", this.newMovieParams)
        .then((response) => {
          console.log(response.data);
          // this.movies.year = parseInt(this.movies.year);
          // this.movies.push(response.data);
        })
        .catch((error) => {
          console.log(error.response.data.error);
        });
    },
  },
};
</script>
