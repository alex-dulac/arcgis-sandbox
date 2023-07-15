<script setup>
import { onMounted, ref } from 'vue';
import config from '@arcgis/core/config.js';
import Map from '@arcgis/core/Map.js';
import MapView from '@arcgis/core/views/MapView.js';
import Graphic from "@arcgis/core/Graphic.js";
import RouteParameters from "@arcgis/core/rest/support/RouteParameters.js";
import FeatureSet from "@arcgis/core/rest/support/FeatureSet.js";
import * as route from "@arcgis/core/rest/route.js";

let view;
const mapDiv = ref();
const cheeseUrl = "https://services.arcgis.com/V6ZHFr6zdgNZuVG0/ArcGIS/rest/services/Single%20people%20and%20cheese/FeatureServer/0";

onMounted(() => {
  config.apiKey = import.meta.env.VITE_API_KEY;

  const map = new Map({
    basemap: 'arcgis-streets'
  });

  view = new MapView({
    map,
    container: mapDiv.value,
    center: [-70.25, 43.75],
    zoom: 10
  });

  // set up and analyze cheese.....

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
