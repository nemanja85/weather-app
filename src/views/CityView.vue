<script setup>
import AsyncCityView from '@/components/AsyncCityView.vue';
</script>

<template>
  <div>
    <Suspense>
      <AsyncCityView />
      <template #fallback>
        <p>Loading data ....</p>
      </template>
    </Suspense>
  </div>
</template>
