<template>
  <div class="App">
    <Header :title="'Composition API'" />
    <Search :search="state.search" @search="handleSearch" />
    <p class="App-intro">Sharing a few of our favourite movies</p>
    <div class="movies">
      <Movie v-for="movie in state.movies" :movie="movie" :key="movie.imdbID" />
    </div>
  </div>
</template>

<script>
import { reactive, watchEffect } from "@vue/composition-api";
import Header from "./components/Header.vue";
import Search from "./components/Search.vue";
import Movie from "./components/Movie.vue";

const API_KEY = "c091ea39";

export default {
  name: "app",
  components: {
    Header,
    Search,
    Movie,
  },
  setup() {
    const state = reactive({
      search: "Joker",
      loading: true,
      movies: [],
      errorMessage: null,
    });

    watchEffect(() => {
      const MOVIE_API_URL = `https://www.omdbapi.com/?s=${state.search}&apikey=${API_KEY}`;

    fetch(MOVIE_API_URL)
      .then(response => response.json())
      .then(jsonResponse => {
        state.movies = jsonResponse.Search;
        state.loading = false;
      });
    });

    return {
      state,
      handleSearch(searchTerm) {
        state.loading = true;
        state.search = searchTerm;
      },
    };
  },
};
</script>
