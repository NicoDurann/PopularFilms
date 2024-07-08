<template>
  <li class="movie-item">
    <router-link :to="'/movie-detail/' + movie.id">
      <img :src="`https://image.tmdb.org/t/p/w200${movie.poster_path}`" :alt="movie.title" />
    </router-link>
  </li>
</template>

<script>
export default {
  props: {
    movie: {
      type: Object,
      required: true,
    },
  },
};
</script>

<style scoped>
.movie-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin: 0 auto;
  transition: all 0.3s ease;
}

.movie-item:hover {
  transform: scale(1.05);
  transition: transform 0.3s;
}

.movie-item img {
  max-width: 100%;
  height: auto;
  border-radius: 4px;
}
</style>
