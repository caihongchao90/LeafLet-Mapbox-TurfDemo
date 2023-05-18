<template>
  <!-- <leafMap></leafMap> -->
  <geojsonMap></geojsonMap>
  <!-- <groupMap></groupMap> -->
  <!-- <crsMap></crsMap> -->
  <!-- <turfStart></turfStart> -->
  <!-- <MapboxglStart></MapboxglStart> -->
  <!-- <navigation></navigation> -->
  <!-- <turfAndMapbox></turfAndMapbox> -->
  
</template>
<script>
import leafMap from "./components/leafMap.vue"
import geojsonMap from "./components/geojsonMap.vue";
import groupMap from "./components/groupMap.vue";
import crsMap from "./components/crsMap.vue";

import turfStart from "./components/turfStart.vue";
import MapboxglStart from "./components/mapbox/mapboxglStart.vue";
import navigation from "./components/mapbox/navigation.vue";
import turfAndMapbox from "./components/mapbox/turfAndMapbox.vue";
export default {
  name:"app",
  components:{
    leafMap,
    geojsonMap,
    groupMap,
    crsMap,
    turfStart,
    MapboxglStart,
    navigation,
    turfAndMapbox
}
}
</script>

<style>

</style>
