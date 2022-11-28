<script setup>
import { RouterView } from 'vue-router';
import Navigation from './components/navigation.vue';
</script>

<template>
  <div class="flex flex-col min-h-screen bg-gray-900 font-Roboto">
    <Navigation />
    <RouterView />
  </div>
</template>

<style scoped></style>
