<template>
  <div class="movies-show">
    <h1>{{ movie.title }}</h1>
    <h2>{{ movie.director }}</h2>
    <p>{{ movie.genres[0].name }}</p>
    <p>{{ movie.actors }}</p>
    <p>{{ movie.plot }}</p>
    <p>{{ movie.year }}</p>
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
  },
};
</script>
