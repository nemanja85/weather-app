<script setup>
import CityCard from '@/components/CityCard.vue';
import axios from 'axios';
import { ref } from 'vue';

const savedCities = ref([]);
const getCities = async () => {
  if (localStorage.getItem('savedCities')) {
    savedCities.value = JSON.parse(localStorage.getItem('savedCities'));
    const request = [];
    console.log('saved cities', savedCities.value);
    savedCities.value.forEach((city) => {
      request.push(
        axios.get(
          `https://api.openweathermap.org/data/2.5/weather?lat=${city.cords.lat}&lon=${city.cords.lng}&appid=4dbbb5fbef639a52415edc87fa9ef5a7&units=imperial`
        )
      );
    });

    const weatherData = await Promise.all(request);

    weatherData.forEach((value, index) => {
      savedCities.value[index].weather = value.data;
    });
  }
};

await getCities();
</script>
<template>
  <div v-for="city in savedCities" :key="city.id">
    <CityCard :city="city" />
  </div>
</template>
