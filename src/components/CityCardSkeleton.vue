<script setup>
import AnimatedPlaceholder from './AnimatedPlaceholder.vue';
</script>
<template>
  <div class="flex w-full px-3 py-6 bg-gray-800 rounded-sm shadow-md">
    <div class="flex flex-col flex-1 gap-2">
      <AnimatedPlaceholder class="max-w-[50%]" />
      <AnimatedPlaceholder class="max-w-[40%]" />
    </div>

    <div class="flex items-end flex-1 gap-2">
      <AnimatedPlaceholder class="w-full max-w-[50px]" />
      <AnimatedPlaceholder class="w-full max-w-[40px]" />
    </div>
  </div>
</template>
