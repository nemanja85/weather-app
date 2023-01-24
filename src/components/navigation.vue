<script setup>
import { ref } from 'vue';
import { RouterLink, useRoute, useRouter } from 'vue-router';
import BaseModal from './baseModal.vue';

const modalActive = ref(null);
const toogleModal = () => {
  modalActive.value = !modalActive.value;
};

const savedCities = ref([]);
const route = useRoute();
const router = useRouter();

const addCity = () => {
  if (localStorage.getItem('savedCities')) {
    savedCities.value = JSON.parse(localStorage.getItem('savedCities'));
  }

  const location = {
    id: route.params.id,
    city: route.params.city,
    state: route.params.state,
    cords: {
      lat: route.query.lat,
      lng: route.query.lng,
    },
  };
  savedCities.value.push(location);
  localStorage.setItem('savedCities', JSON.stringify(savedCities.value));

  let query = Object.assign({}, route.query);
  delete query.preview;
  query.id = location.id;
  router.replace({ query });
};
</script>
<template>
  <header class="sticky top-0 bg-gray-800 shadow-lg">
    <nav class="container flex flex-col items-center gap-4 py-6 mx-auto text-white sm:flex-row">
      <RouterLink :to="{ name: 'home' }">
        <div class="flex items-center flex-1 gap-3">
          <i class="text-2xl fa-solid fa-sun"></i>
          <p>The Local Weather</p>
        </div>
      </RouterLink>
      <div class="flex justify-end flex-1 gap-3">
        <i
          class="text-xl duration-150 cursor-pointer fa-solid fa-circle-info hover:text-gray-700"
          @click="toogleModal"
        ></i>
        <i
          class="text-xl duration-150 cursor-pointer fa-solid fa-plus hover:text-gray-700"
          @click="addCity"
          v-if="route.query.preview"
        ></i>
      </div>
      <BaseModal :modalActive="modalActive" @close-modal="toogleModal">
        <div class="text-black">
          <h1 class="mb-1 text-2xl">About:</h1>
          <p class="mb-4">The Local Weather allows you to track the current weather of cities of your choosing.</p>
          <h2 class="text-2xl">How it works:</h2>
          <ol class="mb-4 list-decimal list-inside">
            <li>Search for your city by entering the name into the search bar.</li>
            <li>Select a city within the results, this will take you to the current weather for your selection.</li>
            <li>
              Track the city by clicking on the "+" icon in the top right. This will save the city to view at a later
              time on the home page.
            </li>
          </ol>

          <h2 class="text-2xl">Removing a city</h2>
          <p>
            If you no longer wish to track a city, simply select the city within the home page. At the bottom of the
            page, there will be am option to delete the city.
          </p>
        </div>
      </BaseModal>
    </nav>
  </header>
</template>
