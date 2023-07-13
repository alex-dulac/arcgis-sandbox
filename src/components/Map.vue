<script setup>
import { onMounted, ref } from 'vue';
import config from '@arcgis/core/config.js';
import Map from '@arcgis/core/Map.js';
import MapView from '@arcgis/core/views/MapView.js';

const mapDiv = ref();

onMounted(() => {
  config.apiKey = import.meta.env.VITE_API_KEY;

  const map = new Map({
    basemap: 'arcgis-nova'
  });

  new MapView({
    map,
    container: mapDiv.value,
    center: [-70, 43],
    zoom: 8
  })
})
</script>

<template>
  <div class="map-container" ref="mapDiv"></div>
</template>

<style scoped>
.map-container {
  padding: 0;
  margin: 0;
  height: 100vh;
}
</style>
