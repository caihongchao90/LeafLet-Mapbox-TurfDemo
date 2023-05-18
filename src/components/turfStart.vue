<template>
    <div id="map"></div>
</template>
<script>
import * as turf from '@turf/turf'
import mapboxgl from 'mapbox-gl'
export default {
    name:"turfStart",
    mounted(){
        let map = L.map('map').setView([51.5, -0.09], 14);
		L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
			maxZoom: 18,
			attribution: '© OpenStreetMap'
		}).addTo(map);
        console.log(mapboxgl)
    }
}
</script>
<style>
</style>