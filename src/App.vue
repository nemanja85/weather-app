<script setup>
import { RouterView } from 'vue-router';
import Navigation from './components/navigation.vue';
</script>

<template>
  <div class="flex flex-col min-h-screen font-Roboto bg-gray-dark">
    <Navigation />
    <RouterView />
  </div>
</template>

<style scoped></style>
