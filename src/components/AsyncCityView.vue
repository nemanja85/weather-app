<script setup>
import axios from 'axios';
import { useRoute } from 'vue-router';

const route = useRoute();
const getWeatherData = async () => {
  try {
    const weatherData = await axios.get(
      `https://api.openweathermap.org/data/3.0/onecall?lat=${route.query.lat}&lon=${route.query.lng}&exclude={part}&appid=c162fe4f8aa2e1f78b033f17143a3c4a&units=imperial`
    );

    const localOffset = new Date().getTimezoneOffset() * 60000;
    const utc = weatherData.data.current.dt * 1000 + localOffset;
    weatherData.data.currentTime = utc + 1000 * weatherData.data.timezone_offset;

    weatherData.data.hourly.forEach((hour) => {
      const utc = hour.dt * 1000 + localOffset;
      hour.currentTime = utc + 1000 * weatherData.data.timezone_offset;
    });

    return weatherData;
  } catch (err) {
    console.error(err);
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
      <p class="mb-10 text-sm">
        {{
          newDate(weatherData.currentTime).toLocaleDataString('en-us', {
            weekday: 'short',
            day: '2-digit',
            month: 'long',
          })
        }}
        {{
          newDate(weatherData.currentTime).toLocaleDataString('en-us', {
            timeStyle: 'short',
          })
        }}
      </p>
    </div>
  </div>
</template>
