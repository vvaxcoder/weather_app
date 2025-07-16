<template>
  <div>
    <input v-model="query" placeholder="Enter location..." @keyup.enter="search" />
    <ul v-if="results.length">
      <li v-for="loc in results" :key="loc.id" @click="selectLocation(loc)">
        {{ loc.name }} <small v-if="loc.country">({{ loc.country }})</small>
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const query = ref('');
const results = ref([]);

const emit = defineEmits<{
  (e: 'location-selected', loc: { name: string; latitude: number; longitude: number }): void;
}>();

const search = async () => {
  if (!query.value) return;

  const url = `https://geocoding-api.open-meteo.com/v1/search?name=${encodeURIComponent(query.value)}&count=10&language=en&format=json`;
  const res = await fetch(url);
  const data = await res.json();
  results.value = data.results || [];
};

const selectLocation = (loc: any) => {
  emit('location-selected', {
    name: loc.name,
    latitude: loc.latitude,
    longitude: loc.longitude,
  });
  results.value = [];
  query.value = '';
};
</script>

<style scoped>
ul {
  list-style: none;
  padding: 0;
}

li {
  cursor: pointer;
  padding: 4px;
}

li:hover {
  background-color: #f0f0f0;
}
</style>
