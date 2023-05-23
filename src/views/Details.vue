<template>
  <div class="w-full flex justify-center">
    <transition name="fade" mode="out-in">
      <div class="flex flex-col w-1/2">
        <div>
          <a @click="goBack" class="text-white font-bold my-2">Back</a>
        </div>

        <movie v-if="details" :movie="details"></movie>
        <span v-else>Loading Details...</span>
        <div v-if="details" class="mt-4">
          <h1 class="text-2xl font-bold">{{ details.title }}</h1>
          <p class="mb-2">Release Date: {{ details.release_dates }}</p>
          <p class="mb-2">Genre: {{ getGenreNames() }}</p>
          <p class="mb-2">Rating: {{ details.vote_average }}</p>
          <p class="mb-2">Popularity: {{ details.popularity }}</p>
        </div>
        <div class="grid grid-cols-4 gap-4">
          <div v-for="video in videos">
            <router-link
              :to="{ name: 'Player', params: { videoKey: video.key } }"
            >
              <img
                :src="`https://img.youtube.com/vi/${video.key}/0.jpg`"
                alt=""
              />
            </router-link>
          </div>
        </div>
      </div>
    </transition>
  </div>
</template>

<script setup>
import axios from "@/http";
import { useRoute, useRouter } from "vue-router";
import { onMounted, ref } from "vue";
import Movie from "@/components/Movie.vue";

const props = defineProps(["id"]);
const details = ref();
const videos = ref([]);
const route = useRoute();
const router = useRouter();

const fetchMovieDetails = async () => {
  const { data } = await axios.get(`/movie/${props.id}`);
  details.value = data;
};

const getGenreNames = () => {
  if (details.value && details.value.genres) {
    return details.value.genres.map((genre) => genre.name).join(", ");
  }
  return "Not Available";
};

const formatReleaseDate = (releaseDate) => {
  const date = new Date(releaseDate);
  const formattedDate = date.toLocaleDateString("en-US", {
    year: "numeric",
    month: "long",
    day: "numeric",
  });

  return formattedDate !== "Invalid Date" ? formattedDate : "Not Available";
};

const fetchMovieVideos = async () => {
  const { data } = await axios.get(`/movie/${props.id}/videos`);
  videos.value = data.results;
};
const goBack = () => {
  router.back();
};
onMounted(() => {
  fetchMovieDetails();
  fetchMovieVideos();
});
</script>
