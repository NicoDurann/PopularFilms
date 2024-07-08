<template>
  <section class="home">
    <h1>Popular Films</h1>

    <MovieList v-if="!error" :movies="movies" :loading="loading" />
    <div class="error" v-else>
      <p>It seems that our server is having problems</p>
    </div>
  </section>
</template>

<script>
import MovieList from "../components/MovieList.vue";
import axios from "axios";

export default {
  name: "Home",
  components: {
    MovieList,
  },
  data() {
    return {
      loading: false,
      error: false,
      movies: [],
      filteredMovies: [],
    };
  },
  mounted() {
    this.fetchMovies();
  },
  methods: {
    async fetchMovies() {
      const apiKey = "0556df4be304f9f5266c4a449774afda";
      this.loading = true;
      try {
        const response = await axios.get(
          `https://api.themoviedb.org/3/movie/popular?api_key=${apiKey}&language=en`
        );
        this.movies = response.data.results.slice(0, 20);
        this.filteredMovies = [...this.movies];
      } catch (e) {
        console.log("Error connecting to the server:", e);
        this.error = true;
      } finally {
        this.loading = false;
      }
    },
  },
};
</script>

<style scoped>
.home {
  padding-top: 4rem;
}
div.error {
  margin-top: 3rem;
}
div.error p {
  font-size: 2rem;
}
</style>
