<template>
  <div>
    <h2>Weather for {{ data.location }}</h2>
    <ul>
      <li v-for="(temp, idx) in data.hourly.temperature_2m" :key="idx">
        {{ formatTime(data.hourly.time[idx]) }} — {{ temp }}°C
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import { format, parseISO } from 'date-fns';

defineProps<{
  data: {
    location: string;
    hourly: {
      time: string[];
      temperature_2m: number[];
    };
  };
}>();

const formatTime = (time: string) => {
  const date = parseISO(time);
  return format(date, 'PPpp');
};
</script>

<style scoped>
h2 {
  margin-top: 1rem;
}

ul {
  padding: 0;
  list-style: none;
}

li {
  margin: 4px 0;
}
</style>
