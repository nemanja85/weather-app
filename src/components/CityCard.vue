<script setup>
defineProps({
  city: {
    type: Object,
    default: () => {},
  },
});
</script>
<template>
  <div class="flex px-3 py-6 bg-gray-800 rounded-sm shadow-md cursor-pointer">
    <div class="flex flex-col flex-1">
      <h2 class="text-2xl font-bold">
        {{ city.city }}
      </h2>
      <h3 class="text-lg">
        {{ city.state }}
      </h3>
    </div>

    <div class="flex flex-col gap-2">
      <p class="self-end text-2xl font-bold">{{ Math.round((city.weather.main.temp - 32) * (5 / 9)) }}&degC</p>
      <div class="flex gap-2">
        <span class="text-xs">
          H:
          {{ Math.round((city.weather.main.temp_max - 32) * (5 / 9)) }}&degC
        </span>
        <span class="text-xs">
          L:
          {{ Math.round((city.weather.main.temp_min - 32) * (5 / 9)) }}&degC
        </span>
      </div>
    </div>
  </div>
</template>
