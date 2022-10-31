<script setup>
import axios from 'axios';
import { ref } from 'vue';

const mapboxAPIKey = 'pk.eyJ1IjoibmVtYW5qYTg1IiwiYSI6ImNsOXgzdjhubjA2am0zd3VyZ2NtdmJkcDIifQ.dzJeT_IoCyxP4OXbeK5FAA';
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
      <ul class="absolute w-full p-4 mt-3 bg-blue-700 shadow-md text-gray-light" v-if="mapboxSearchResults">
        <p v-if="searchError" class="text-base font-medium text-red-800">
          Sorry, something went wrong, please try again!
        </p>
        <p v-if="!serverError && mapboxSearchResults.value === 0" class="text-base font-medium text-red-800">
          No results match your query, please try a different term!
        </p>
        <template v-else>
          <li
            v-for="searchResult in mapboxSearchResults"
            :key="searchResult.id"
            class="py-2 text-sm font-normal cursor-pointer"
          >
            {{ searchResult.place_name }}
          </li>
        </template>
      </ul>
    </div>
  </main>
</template>
