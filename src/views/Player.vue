<template>
  
  
  <div class="flex justify-center items-center h-screen">
    <div id="player"></div>
    <a @click="goBack" class="text-white font-bold">Back</a>
  </div>
</template>


<script setup>
import { onMounted, ref } from "vue";
import { useRouter } from "vue-router";

const loadYouTubePlayerAPI = () => {
  return new Promise((resolve) => {
    if (window.YT && typeof window.YT.Player === "function") {
      // If the YouTube Player API is already loaded, resolve immediately
      resolve();
    } else {
      // Otherwise, load the API dynamically
      const tag = document.createElement("script");
      tag.src = "https://www.youtube.com/iframe_api";
      const firstScriptTag = document.getElementsByTagName("script")[0];
      firstScriptTag.parentNode.insertBefore(tag, firstScriptTag);
      window.onYouTubeIframeAPIReady = resolve;
    }
  });
};

const router = useRouter();
const videoKey = ref(router.currentRoute.value.params.videoKey);

onMounted(async () => {
  await loadYouTubePlayerAPI();
  const player = new window.YT.Player("player", {
    videoId: videoKey.value,
    width: "720",
    height: "460",
  });
});

const goBack = () => {
  router.back();
};
</script>
