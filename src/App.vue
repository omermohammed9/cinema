<template>
  <h1 class="text-3xl font-bold">Cinema</h1>

  <!-- <transition name="slide-fade">
    <router-view></router-view>
  </transition> -->
  <router-view v-slot="{ Component }">
    <transition name="slide-fade" mode="out-in">
      <component :is="Component"> </component>
    </transition>
  </router-view>
  
</template>
<script setup></script>
