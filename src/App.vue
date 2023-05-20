<template>
  <h1 class="text-3xl font-bold">Cinema</h1>
  <div class="w-full flex flex-col items-center">
    <div class="w-2/5 mb-6">
      <input
        placeholder="Search for a movie ..."
        class="w-full p-2 rounded-lg"
        type="text"
      />
    </div>

    <div class="w-3/4 grid grid-cols-4 gap-4">
      <Movies v-for="movie in movies" :movie="movie" />
    </div>
  </div>
</template>
<script setup>
import axios from "axios";
import { onMounted, ref } from "vue";
import Movies from "./components/Movies.vue";
const movies = ref([]);
const searchInputValue = ref();

const apiKey =
  "Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiJjYWExZjIwNzAxYWY2MzFhMTNlNWY2NGI5ZmM2OWUxYiIsInN1YiI6IjVhN2QyYzQ3MGUwYTI2M2U4NTAwMDhkYiIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.3NO9tDVEdi2KYhccQ-XLOA-uvsoH3felcoLi5MIdL0M";

const headers = {
  Authorization: apiKey,
};
const searchMovies = async () => {
  const { data } = await axios.get(
    "https://api.themoviedb.org/3/search/movie",
    {
      params: {
        query: searchQueryValue.value,
      },
      headers,
    }
  );
};
const fetchMovies = async () => {
  //const response
  //  movies.value = response.data.results;

  const { data, status } = await axios.get(
    "https://api.themoviedb.org/3/trending/movie/day",
    {
      headers,
    }
  );
  movies.value = data.results;
};
onMounted(() => {
  fetchMovies();
});
</script>
