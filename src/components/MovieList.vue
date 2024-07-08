<template>
  <div class="container">
    <input type="text" v-model="searchQuery" @input="searchMovies" placeholder="Search" />
    <div v-if="showGenreFilter" class="filters">
      <label for="genreFilter">Filter by Genre:</label>
      <select id="genreFilter" v-model="selectedGenre" @change="filterMovies">
        <option value="">All</option>
        <option v-for="genre in popularGenres" :value="genre.id" :key="genre.id">
          {{ genre.name }}
        </option>
      </select>
    </div>
    <ul class="movies-grid" v-if="!loading && filteredMovies">
      <MovieItem v-for="movie in filteredMovies" :movie="movie" />
    </ul>
    <div v-else class="loader">
      <p v-if="loading">Loading Movies...</p>
      <p v-if="!filteredMovies">No results found</p>
    </div>
  </div>
</template>

<script>
import MovieItem from "./MovieItem.vue";

export default {
  name: "MovieList",
  components: {
    MovieItem,
  },
  props: {
    movies: {
      type: Array,
    },
    loading: {
      type: Boolean,
    },
    showGenreFilter: {
      type: Boolean,
      default: true,
    },
  },

  data() {
    return {
      filteredMovies: [...this.movies],
      searchQuery: "",
      selectedGenre: "",
      popularGenres: [
        { id: 28, name: "Action" },
        { id: 35, name: "Comedy" },
        { id: 18, name: "Drama" },
        { id: 878, name: "Sci-Fi" },
      ],
    };
  },

  watch: {
    movies(newMovies) {
      this.filteredMovies = [...newMovies];
    },
  },

  methods: {
    searchMovies() {
      if (this.searchQuery.trim() === "") {
        this.filteredMovies = [...this.movies];
      } else {
        const searchTerm = this.searchQuery.trim().toLowerCase();
        const result = this.movies.filter((movie) => movie.title.toLowerCase().includes(searchTerm));
        result.length > 0 ? (this.filteredMovies = result) : (this.filteredMovies = null);
      }
    },
    filterMovies() {
      let filtered = [...this.movies];

      if (this.selectedGenre) {
        filtered = filtered.filter((movie) => movie.genre_ids.includes(parseInt(this.selectedGenre)));
      }

      if (this.searchQuery.toLowerCase().trim() !== "") {
        const searchTerm = this.searchQuery.trim().toLowerCase();
        filtered = filtered.filter((movie) => movie.title.toLowerCase().includes(searchTerm));
      }
      this.filteredMovies = filtered;
    },
  },
};
</script>
<style scoped>
.container {
  width: 90%;
  max-width: 1200px;
  margin: 20px auto;
  padding: 20px;
  background-color: #14181c;
  border-radius: 10px;
}

.movies-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  row-gap: 2rem;
  list-style: none;
  padding: 0;
  justify-content: center;
  width: 100%;
  margin: 30px auto;
}

input[type="text"] {
  width: 100%;
  padding: 10px;
  margin-bottom: 20px;
  border: none;
  border-radius: 5px;
  background-color: #1c222a;
  color: #ffffff;
  font-size: 16px;
}

input[type="text"]::placeholder {
  color: white;
}

div.loader {
  margin-top: 3rem;
}

div.loader p {
  font-size: 3rem;
}

.filters {
  margin-bottom: 20px;
}

.filters label {
  display: block;
  font-size: 1.2rem;
  margin-bottom: 5px;
  font-weight: bold;
}

.filters select {
  width: 100%;
  padding: 10px;
  border: none;
  border-radius: 5px;
  background-color: #1c222a;
  color: #ffffff;
  font-size: 16px;
}

select option {
  font-size: 20px;
  padding: 10px;
}

input {
  width: 100% !important;
  margin: 1rem 0rem;
}
#genreFilte r {
  margin: 1rem 0rem;
  width: 100% !important;
}
</style>
