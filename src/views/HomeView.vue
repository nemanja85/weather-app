<script setup>
import CityCardSkeleton from '@/components/CityCardSkeleton.vue';
import CityList from '@/components/CityList.vue';
import axios from 'axios';
import { ref } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter();
const previewCity = (searchResult) => {
  const [city, state] = searchResult.place_name.split(',');
  router.push({
    name: 'cityView',
    params: { state: state.replaceAll(' ', ''), city: city },
    query: {
      lat: searchResult.geometry.coordinates[1],
      lng: searchResult.geometry.coordinates[0],
      preview: true,
    },
  });
};
const mapboxAPIKey = import.meta.env.VITE_MAPBOX_API_KEY;
const searchQuery = ref('');
const queryTimeout = ref(null);
const mapboxSearchResults = ref(null);
const searchError = ref(null);

const getSearchResults = () => {
  clearTimeout(queryTimeout.value);
  queryTimeout.value = setTimeout(async () => {
    if (searchQuery.value !== '') {
      try {
        const result = await axios.get(
          `https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${mapboxAPIKey}&types=place`
        );
        mapboxSearchResults.value = result.data.features;
      } catch {
        searchError.value = true;
      }

      return;
    }
    mapboxSearchResults.value = null;
  }, 300);
};
</script>

<template>
  <main class="container mx-auto text-gray-100">
    <div class="relative px-6 py-6 md:px-0">
      <input
        v-model="searchQuery"
        @input="getSearchResults"
        type="text"
        placeholder="Search for a city or state"
        class="w-full p-2 bg-transparent border-b focus:outline-none focus:shadow-md"
      />
      <ul class="absolute w-full p-4 mt-3 bg-gray-800 shadow-md text-gray-light" v-if="mapboxSearchResults">
        <p v-if="searchError" class="text-xl font-medium text-red-800">
          Sorry, something went wrong, please try again!
        </p>
        <p v-if="!serverError && mapboxSearchResults.length === 0" class="text-base font-medium text-red-800">
          No results match your query, please try a different term!
        </p>
        <template v-else>
          <li
            v-for="searchResult in mapboxSearchResults"
            :key="searchResult.id"
            @click="previewCity(searchResult)"
            class="py-2 text-sm font-normal cursor-pointer"
          >
            {{ searchResult.place_name }}
          </li>
        </template>
      </ul>
    </div>
    <div class="flex flex-col gap-4">
      <Suspense>
        <CityList />
        <template #fallback>
          <div class="flex items-center justify-center w-full h-screen">
            <div role="status">
              <CityCardSkeleton />
            </div>
          </div>
        </template>
      </Suspense>
    </div>
  </main>
</template>
