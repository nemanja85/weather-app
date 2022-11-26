<script setup>
import axios from 'axios';
import { useRoute } from 'vue-router';

const route = useRoute();
const getWeatherData = async () => {
  try {
    const weatherData = await axios.get(
      `https://api.openweathermap.org/data/2.5/weather?lat=${route.query.lat}&lon=${route.query.lng}&exclude={part}&appid=4dbbb5fbef639a52415edc87fa9ef5a7&units=imperial`
    );

    // cal current date & time
    const localOffset = new Date().getTimezoneOffset() * 60000;
    console.log('weather data', weatherData);
    const utc = weatherData.data.dt * 1000 + localOffset;

    return weatherData.data;
  } catch (err) {
    console.log(err);
  }
};

const weatherData = await getWeatherData();
console.log(weatherData);
</script>

<template>
  <div class="flex flex-col items-center flex-1">
    <div v-if="route.query.preview" class="w-full p-4 text-center text-gray-100 bg-blue-600">
      <p>You are currently previewing this city , click the "+" icon to start tracking this city</p>
    </div>
    <div class="flex flex-col items-center p-10 text-gray-100">
      <h1 class="mb-2 text-4xl">
        {{ route.params.city }}
      </h1>
      <p class="mt-8 text-5xl">{{ Math.round(weatherData.main.temp) }}&deg</p>
      <p class="mt-4 text-xl capitalize">{{ weatherData.weather[0].description }}</p>
      <img
        :src="`http://openweathermap.org/img/wn/${weatherData.weather[0].icon}@2x.png`"
        class="w-20 h-auto"
        :alt="`${weatherData.weather[0].description}`"
      />
      <div class="flex w-20 justify-evenly">
        <p class="text-md">{{ Math.round(weatherData.main.temp_max) }}&deg</p>
        <p class="text-md">{{ Math.round(weatherData.main.temp_min) }}&deg</p>
      </div>
      <p class="mt-2 text-xl">Humidity: {{ weatherData.main.humidity }}%</p>
      <p class="mt-2 text-xl">Speed of wind: {{ Math.round(weatherData.wind.speed) }} m/s</p>
    </div>
  </div>
  <hr class="w-full border border-white border-opacity-50" />
</template>
