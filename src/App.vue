<template>
  <div class="w-app">
    <h1>Weather Forecast</h1>
    <SearchBox @location-selected="fetchWeather" />
    <WeatherForecast v-if="weatherData" :data="weatherData" />
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import SearchBox from './components/SearchBox.vue';
import WeatherForecast from './components/WeatherForecast.vue';

const weatherData = ref(null);

const fetchWeather = async (location: { name: string; latitude: number; longitude: number }) => {
  const now = new Date();
  const startDate = now.toISOString().split('T')[0];
  const endDate = new Date(now.getTime() + 24 * 60 * 60 * 1000).toISOString().split('T')[0];

  const url = `https://api.open-meteo.com/v1/forecast?latitude=${location.latitude}&longitude=${location.longitude}&hourly=temperature_2m&timezone=auto&start_date=${startDate}&end_date=${endDate}`;
  const res = await fetch(url);
  const data = await res.json();
  weatherData.value = {
    location: location.name,
    ...data,
  };
};
</script>


<style scoped>
body {
  font-family: sans-serif;
}

.app {
  max-width: 800px;
  margin: 0 auto;
  padding: 1rem;
}
</style>
