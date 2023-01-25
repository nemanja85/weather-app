<script setup>
import axios from 'axios';
import { useRoute, useRouter } from 'vue-router';

const route = useRoute();
const getWeatherData = async () => {
  try {
    const weatherData = await axios.get(
      `https://api.openweathermap.org/data/2.5/weather?lat=${route.query.lat}&lon=${route.query.lng}&exclude={part}&appid=4dbbb5fbef639a52415edc87fa9ef5a7&units=imperial`
    );

    // cal current date & time
    const localOffset = new Date().getTimezoneOffset() * 60000;
    const utc = weatherData.data.dt * 1000 + localOffset;

    return weatherData.data;
  } catch (err) {
    console.log(err);
  }
};

const weatherData = await getWeatherData();

const router = useRouter();
const removeCity = () => {
  const cities = JSON.parse(localStorage.getItem('savedCities'));
  const updatedCities = cities.filter((city) => city.id !== route.query.id);

  localStorage.setItem('savedCities', JSON.stringify(updatedCities));
  router.push({
    name: 'home',
  });
};
</script>

<template>
  <div class="flex flex-col items-center flex-1">
    <div v-if="route.query.preview" class="w-full p-4 text-center text-gray-100 bg-gray-800">
      <p>You are currently previewing this city , click the "+" icon to start tracking this city</p>
    </div>
    <div class="flex flex-col items-center p-10 text-gray-100">
      <h1 class="mb-2 text-4xl">
        {{ route.params.city }}
      </h1>
      <p class="mt-8 text-5xl">{{ Math.round((weatherData.main.temp - 32) * (5 / 9)) }}&degC</p>
      <p class="mt-4 capitalize text-md">
        Feels like: {{ Math.round((weatherData.main.feels_like - 32) * (5 / 9)) }}&degC
      </p>
      <p class="mt-4 text-xl capitalize">{{ weatherData.weather[0].main }}, {{ weatherData.weather[0].description }}</p>
      <img
        :src="`http://openweathermap.org/img/wn/${weatherData.weather[0].icon}@2x.png`"
        class="w-20 h-auto"
        :alt="`${weatherData.weather[0].description}`"
      />
      <div class="flex w-20 justify-evenly">
        <p class="text-md">{{ Math.round((weatherData.main.temp_max - 32) * (5 / 9)) }}</p>
        /
        <p class="text-md">{{ Math.round((weatherData.main.temp_min - 32) * (5 / 9)) }}&degC</p>
      </div>
      <p class="mt-2 text-lg">Humidity: {{ weatherData.main.humidity }}%</p>
      <p class="mt-2 text-lg">Pressure: {{ weatherData.main.pressure }} millibars</p>
      <p class="mt-2 text-lg">Visibility: {{ weatherData.visibility / 1000 }} km</p>
      <p class="mt-2 text-lg">Wind speed: {{ Math.round(weatherData.wind.speed) }} km/h</p>
    </div>
    <div
      class="flex items-center gap-2 py-10 text-gray-100 duration-150 cursor-pointer hover:text-red-500"
      @click="removeCity"
    >
      <i class="fa-solid fa-trash"></i>
      <p>Remove City</p>
    </div>
  </div>
  <hr class="w-full border border-white border-opacity-50" />
</template>
