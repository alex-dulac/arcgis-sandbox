<script setup>
import { onMounted, ref } from 'vue';
import config from '@arcgis/core/config.js';
import Map from '@arcgis/core/Map.js';
import MapView from '@arcgis/core/views/MapView.js';
import BasemapToggle from '@arcgis/core/widgets/BasemapToggle.js'

const mapDiv = ref();

onMounted(() => {
  config.apiKey = import.meta.env.VITE_API_KEY;

  const map = new Map({
    basemap: 'arcgis-streets'
  });

  const view = new MapView({
    map,
    container: mapDiv.value,
    center: [-70.25, 43.75],
    zoom: 8
  });

  const toggle = new BasemapToggle({
    view: view,
    nextBasemap: "hybrid"
  });
  view.ui.add(toggle, "top-right");
});
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
