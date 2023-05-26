<template>
  <div class="flex flex-col bg-white rounded-lg text-black">
    <router-link :to="{ name: 'Details', params: { id: movie.id } }">
      <div class="w-full overflow-hidden rounded-t-lg">
        <img class="w-full transition hover:scale-150" :src="imageURL" alt="" />
      </div>
      <div class="p-2 flex flex-col justify-between">
        <h1 class="text-xl font-bold">{{ movie.title }}</h1>
        <div class="flex flex-col space-y-3">
          <span class="font-bold"
            >Rating: {{ movie.vote_average.toFixed(1) * 10 }}%
          </span>

          <span class="font-bold">
            Release Date: {{ formatDate(movie.release_date) }}</span
          >
          <router-link :to="{ name: 'Details', params: { id: movie.id } }">
            <span class="font-bold cursor-pointer hover:underline">
              More Details
            </span>
          </router-link>
          <!-- <span>
            {{ overview }}
          </span>
          <a
            @click.stop="isOverviewExpanded = true"
            class="text-sm text-indigo-500 font-bold"
            v-if="overview.length != movie.overview.length"
            href="#"
          >
            Read more...
          </a> -->
        </div>
      </div>
    </router-link>
  </div>
</template>
<script setup>
import { computed, ref } from "vue";
import { fullImageUrl } from "@/helpers";

const props = defineProps({
  movie: Object,
});
const isOverviewExpanded = ref(false);
const imageURL = computed(() => {
  return fullImageUrl(props.movie.backdrop_path);
});

const overview = computed(() => {
  if (isOverviewExpanded.value) {
    return props.movie.overview;
  }
  return props.movie.overview.substr(0, 80) + "...";
});

const formatDate = (date) => {
  const options = { year: "numeric", month: "long", day: "numeric" };
  return new Date(date).toLocaleDateString(undefined, options);
};
</script>
