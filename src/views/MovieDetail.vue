<template>
  <section class="movie-detail">
    <div v-if="!error && !loading" class="movie-info-container">
      <img :src="'https://image.tmdb.org/t/p/original' + movie.poster_path" :alt="movie.title" />
      <div class="info-text">
        <h2>
          {{ movie.title }} <span class="bold">{{ movie.release_date }}</span>
        </h2>
        <p>{{ movie.overview }}</p>
        <button class="btn" @click="toggleFavorite">
          {{ isFavorite ? "Remove from favorites" : "Add to favorites" }}
        </button>
      </div>
    </div>
    <div v-if="!movie" class="messages">
      <p v-if="loading">Loading movies....</p>
      <p v-if="error">Something went wrong</p>
    </div>
  </section>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      movie: {},
      loading: false,
      error: false,
      isFavorite: false,
    };
  },
  mounted() {
    this.fetchData();
    this.checkFavorite();
  },
  watch: {
    movie() {
      this.checkFavorite();
    },
  },
  methods: {
    async fetchData() {
      const movieId = this.$route.params.id;
      const apiKey = "0556df4be304f9f5266c4a449774afda";
      const language = "en";
      this.loading = true;
      try {
        const response = await axios.get(
          `https://api.themoviedb.org/3/movie/${movieId}?api_key=${apiKey}&language=${language}`
        );
        this.movie = response.data;
      } catch (e) {
        console.log("Error connecting to the API: ", e);
        this.error = true;
      } finally {
        this.loading = false;
      }
    },

    checkFavorite() {
      const favorites = JSON.parse(localStorage.getItem("favorites")) || [];
      const result = favorites.find((favorite) => favorite.id === this.movie.id);
      result ? (this.isFavorite = true) : (this.isFavorite = false);
    },

    toggleFavorite() {
      let favorites = JSON.parse(localStorage.getItem("favorites")) || [];
      const index = favorites.findIndex((favorite) => favorite.id === this.movie.id);

      if (index === -1) {
        favorites.push(this.movie);
      } else {
        favorites.splice(index, 1);
      }

      localStorage.setItem("favorites", JSON.stringify(favorites));
      this.isFavorite = !this.isFavorite;
    },
  },
};
</script>

<style scoped>
.movie-detail {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  padding: 20px;
  background-color: #1f252c;
  color: #ffffff;
}

.movie-info-container {
  display: flex;
  flex-direction: row;
  gap: 4rem;
  align-items: center;
  background-color: #14181c;
  border-radius: 10px;
  padding: 20px;
  max-width: 800px;
  width: 100%;
}

@media (max-width: 900px) {
  .movie-info-container {
    display: flex;
    flex-direction: column;
  }
}

.movie-info-container img {
  border-radius: 10px;
  margin-bottom: 20px;
  width: 100%;
  max-width: 250px;
}

.info-text {
  text-align: center;
}

.info-text h2 {
  font-size: 24px;
  margin-bottom: 10px;
}

.info-text h2 .bold {
  font-weight: 300;
  color: #ffab00;
  font-size: 1rem;
  padding-left: 1rem;
}

.info-text p {
  font-size: 16px;
  margin-bottom: 20px;
}

.btn {
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  background-color: #ffab00;
  color: #1c1c1c;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.btn:hover {
  background-color: #e69a00;
}

.messages {
  text-align: center;
  padding: 20px;
  font-size: 18px;
}

.messages p {
  margin: 10px 0;
}
</style>
