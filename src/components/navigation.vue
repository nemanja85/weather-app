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
    city: route.params.name,
    cords: {
      lat: route.query.lat,
      lng: route.query.lng,
    },
  };
  savedCities.value.push(location);
  localStorage.setItem('savedCities', JSON.stringify(savedCities.value));

  let query = Object.assign({}, route.query);
  delete query.preview;
  router.replace({ query });
};
</script>
<template>
  <header class="sticky top-0 bg-blue-800 shadow-lg">
    <nav class="container flex flex-col items-center gap-4 py-6 mx-auto text-white sm:flex-row">
      <RouterLink :to="{ name: 'home' }">
        <div class="flex items-center flex-1 gap-3">
          <i class="text-2xl fa-solid fa-sun"></i>
          <p>The Local Weather</p>
        </div>
      </RouterLink>
      <div class="flex justify-end flex-1 gap-3">
        <i
          class="text-xl duration-150 cursor-pointer fa-solid fa-circle-info hover:text-blue-500"
          @click="toogleModal"
        ></i>
        <i class="text-xl duration-150 cursor-pointer fa-solid fa-plus hover:text-blue-500" @click="addCity"></i>
      </div>
      <BaseModal :modalActive="modalActive" @close-modal="toogleModal">
        <h1 class="text-black">Hello from Modal</h1>
      </BaseModal>
    </nav>
  </header>
</template>
