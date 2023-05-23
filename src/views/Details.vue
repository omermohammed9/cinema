<template>
  <div class="w-full flex justify-center">
    <div class="w-1/2">
      <transition name="fade" mode="out-in">
        <movie v-if="details" :movie="details"></movie>

        <span v-else>Loading Details...</span>
      </transition>
      <div v-if="details" class="mt-4">
        <h1 class="text-2xl font-bold">{{ details.title }}</h1>
        <p class="mb-2">Release Date: {{ formatDate(details.releaseDate) }}</p>
        <p class="mb-2">Genre: {{ getGenreNames() }}</p>
        <p class="mb-2">Rating: {{ details.vote_average }}</p>
        <p class="mb-2">Popularity: {{ details.popularity }}</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import axios from "@/http";
import { useRoute } from "vue-router";
import { onMounted, ref } from "vue";
import Movie from "@/components/Movie.vue";

const details = ref();
const route = useRoute();
const fetchMovieDetails = async () => {
  const { data } = await axios.get(`/movie/${route.params.id}`);
  details.value = data;
};
const getGenreNames = () => {
  if (details.value && details.value.genres) {
    return details.value.genres.map((genre) => genre.name).join(", ");
  }
  return "Not Available";
};

const formatDate = (date) => {
  const releaseDate = new Date(date);
  return releaseDate.toLocaleDateString("en-US", {
    year: "numeric",
    month: "long",
    day: "numeric"
  });
};
onMounted(() => {
  fetchMovieDetails();
});
</script>
