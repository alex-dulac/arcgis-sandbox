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
const routeUrl = "https://route-api.arcgis.com/arcgis/rest/services/World/Route/NAServer/Route_World";

onMounted(() => {
  config.apiKey = import.meta.env.VITE_API_KEY;

  const map = new Map({
    basemap: 'arcgis-navigation'
  });

  view = new MapView({
    map,
    container: mapDiv.value,
    center: [-70.25, 43.75],
    zoom: 10
  });

  view.on("click", (event) => {
    if (view.graphics.length === 0) {
      addGraphic("origin", event.mapPoint);
    } else if (view.graphics.length === 1) {
      addGraphic("destination", event.mapPoint);
      getRoute(); // Call the route service
    } else {
      view.graphics.removeAll();
      addGraphic("origin", event.mapPoint);
    }
  })
});

function addGraphic(type, point) {
  const graphic = new Graphic({
    symbol: {
      type: "simple-marker",
      color: (type === "origin") ? "white" : "black",
      size: "8px"
    },
    geometry: point
  });
  view.graphics.add(graphic);
}

function getRoute() {
  const routeParams = new RouteParameters({
    stops: new FeatureSet({
      features: view.graphics.toArray()
    }),
    returnDirections: true
  });

  route.solve(routeUrl, routeParams).then((data) => {
      data.routeResults.forEach((result) => {
        result.route.symbol = {
          type: "simple-line",
          color: [5, 150, 255],
          width: 3
        };
        view.graphics.add(result.route);
      });

      // Display directions
      if (data.routeResults.length > 0) {
        const directions = document.createElement("ol");
        directions.classList = "esri-widget esri-widget--panel esri-directions__scroller";
        directions.style.marginTop = "0";
        directions.style.padding = "15px 15px 15px 30px";
        const features = data.routeResults[0].directions.features;

        // Show each direction
        features.forEach((feature) => {
          const direction = document.createElement("li");
          direction.innerHTML = feature.attributes.text + " (" + feature.attributes.length.toFixed(2) + " miles)";
          directions.appendChild(direction);
        })
        view.ui.empty("top-right");
        view.ui.add(directions, "top-right");
      }
    }).catch((error) => {
      console.log(error);
    });
}
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
