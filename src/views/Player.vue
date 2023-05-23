<template>
  <div>
    <div ref="youtube" />
    <!-- <YoutubeIframe :video-id="VideoID" /> -->
  </div>
</template>

<script setup>

// import { ref } from 'vue';
// import { useRoute } from 'vue-router';
// import { YoutubeIframe } from '@vue-youtube/component';



// const route = useRoute();
 const VideoID = ref(route.params.videoKey);
import { usePlayer } from '@vue-youtube/core';
import { ref } from 'vue';
const youtube = ref();

usePlayer(VideoID, youtube);
</script>


<!-- <template>
  <div>
    <div id="player"></div>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue';
import { useRouter } from 'vue-router';

const loadYouTubePlayerAPI = () => {
  return new Promise((resolve) => {
    const tag = document.createElement('script');
    tag.src = 'https://www.youtube.com/iframe_api';
    const firstScriptTag = document.getElementsByTagName('script')[0];
    firstScriptTag.parentNode.insertBefore(tag, firstScriptTag);
    window.onYouTubeIframeAPIReady = resolve;
  });
};

const router = useRouter();
const videoKey = ref(router.currentRoute.value.params.videoKey);

onMounted(() => {
  loadYouTubePlayerAPI().then(() => {
    const player = new window.YT.Player('player', {
      videoId: videoKey.value,
      width: '640',
      height: '360',
    });
  });
});
</script> -->
