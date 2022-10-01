<script setup lang="ts">
import { Map } from 'mapbox-gl';
import 'mapbox-gl/dist/mapbox-gl.css';

onMounted(() => {
  const map = new Map({
    accessToken: 'pk.eyJ1IjoiaW5vdWUyMDAyIiwiYSI6ImNramhjZXZkdjFhNmEycXFqd3NvNTFsMTIifQ.GEI7WRexsISWZjufT4wYsA',
    container: 'map', // <div id="map"></div>
    style: 'mapbox://styles/mapbox/streets-v11',
    center: [135.4, 34.6], // 梅田周辺
    zoom: 12,
  });
});
</script>

<template>
  <div>
    <div id="map" style="height: 100vh"></div>
  </div>
</template>
