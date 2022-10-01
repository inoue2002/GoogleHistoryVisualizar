<script setup lang="ts">
import { Map } from 'mapbox-gl';
import 'mapbox-gl/dist/mapbox-gl.css';

import mapJSON from '~/assets/test.json';

const getCurrentLocation = async () => {
  if (navigator.geolocation) {
    try {
      const result = await getCurrentPositionAsPromise();
      return result.coords;
    } catch (e) {
      // eslint-disable-next-line no-console
      console.error(e);
      throw e;
    }
  } else {
    // eslint-disable-next-line no-console
    console.log('can not get a location data in this browser.');
    throw new Error('can not get a location data in this browser.');
  }
};
const getCurrentPositionAsPromise = () => {
  const promise = new Promise((resolve, reject) => {
    navigator.geolocation.getCurrentPosition(
      (success) => {
        resolve(success);
      },
      (error) => {
        reject(error);
      }
    );
  });
  return promise;
};

onMounted(async () => {
  const currentLocation = await getCurrentLocation();
  const map = new Map({
    accessToken: 'pk.eyJ1IjoiaW5vdWUyMDAyIiwiYSI6ImNramhjZXZkdjFhNmEycXFqd3NvNTFsMTIifQ.GEI7WRexsISWZjufT4wYsA',
    container: 'map',
    style: 'mapbox://styles/mapbox/streets-v11',
    center: [currentLocation.longitude, currentLocation.latitude],
    zoom: 12,
  });

  map.on('load', () => {
    map.addSource('route', {
      type: 'geojson',
      data: {
        type: 'Feature',
        properties: {},
        geometry: {
          type: 'LineString',
          coordinates: [
            [130.4112421, 33.597952],
            [130.4100689, 33.5981205],
            [130.4115636, 33.5977976],
            [130.4093334, 33.5982304],
            [130.3981617, 33.5911897],
            [130.4024043, 33.5924419],
            [130.401897, 33.5918653],
          ],
        },
      },
    });
    map.addLayer({
      id: 'route',
      type: 'line',
      source: 'route',
      layout: {
        'line-join': 'round',
        'line-cap': 'round',
      },
      paint: {
        'line-color': 'rgba(255, 255, 255, 1)',
        'line-width': 15,
      },
    });

    // Add a data source containing GeoJSON data.
    map.addSource('maine', mapJSON);

    // Add a new layer to visualize the polygon.
    map.addLayer({
      id: 'maine',
      type: 'fill',
      source: 'maine', // reference the data source
      layout: {},
      paint: {
        'fill-color': '#0080ff', // blue color fill
        'fill-opacity': 0.5,
      },
    });
    // // Add a black outline around the polygon.
    // map.addLayer({
    //   id: 'outline',
    //   type: 'line',
    //   source: 'maine',
    //   layout: {},
    //   paint: {
    //     'line-color': '#000',
    //     'line-width': 2,
    //   },
    // });

    map.moveLayer('route', 'maine');
  });
});
</script>

<template>
  <div>
    <div id="map" style="height: 100vh"></div>
  </div>
</template>
