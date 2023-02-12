<script setup>
import AsyncCityView from '@/components/AsyncCityView.vue';
</script>

<template>
  <div>
    <Suspense>
      <AsyncCityView />
      <template #fallback>
        <CityViewSkeleton />
      </template>
    </Suspense>
  </div>
</template>
