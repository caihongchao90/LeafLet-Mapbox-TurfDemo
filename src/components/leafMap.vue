<template>
	<div id="map"></div>
</template>
<script>
import statesData from "../assets/us-states.js"
export default {
	name:"leafMap",
	mounted(){
		let map = L.map('map').setView([51.5, -0.09], 14);
		L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
			maxZoom: 18,
			attribution: '© OpenStreetMap'
		}).addTo(map);
		// let marker = L.marker([51.5, -0.09]).addTo(map);
		// let circle = L.circle([51.508, -0.11], {
		// 	color: 'red',
		// 	fillColor: '#f03',
		// 	fillOpacity: 0.5,
		// 	radius: 500
		// }).addTo(map);
		// var polygon = L.polygon([
		// 	[51.509, -0.08],
		// 	[51.503, -0.06],
		// 	[51.51, -0.047]
		// ]).addTo(map);
		// marker.bindPopup("<b>Hello world!</b><br>I am a popup.").openPopup();
		// circle.bindPopup("I am a circle.");
		// polygon.bindPopup("I am a polygon.");
		// let popup = L.popup()
		// 	.setLatLng([51.513, -0.09])
		// 	.setContent("I am a standalone popup.")
		// 	.openOn(map);
		// function onMapClick(e) {
		// 	alert("You clicked the map at " + e.latlng);
		// }
		// map.on('click', onMapClick);
		let greenIcon0 = L.icon({
			iconUrl: './src/assets/leaf-green.png',
			shadowUrl: './src/assets/leaf-shadow.png',

			iconSize:     [38, 95], // size of the icon
			shadowSize:   [50, 64], // size of the shadow
			iconAnchor:   [22, 94], // point of the icon which will correspond to marker's location
			shadowAnchor: [4, 62],  // the same for the shadow
			popupAnchor:  [-3, -76] // point from which the popup should open relative to the iconAnchor
		});
		// L.marker([51.5, -0.09], {icon: greenIcon0}).addTo(map);
		let LeafIcon = L.Icon.extend({
			options: {
					shadowUrl: './src/assets/leaf-shadow.png',
					iconSize:     [38, 95],
					shadowSize:   [50, 64],
					iconAnchor:   [22, 94],
					shadowAnchor: [4, 62],
					popupAnchor:  [-3, -76]
			}
		});
		let greenIcon = new LeafIcon({iconUrl: './src/assets/leaf-green.png'}),
			redIcon = new LeafIcon({iconUrl: './src/assets/leaf-red.png'}),
			orangeIcon = new LeafIcon({iconUrl: './src/assets/leaf-orange.png'});
		L.icon = function (options) {
    		return new L.Icon(options);
		};
		L.marker([51.5, -0.09], {icon: greenIcon}).addTo(map).bindPopup("I am a green leaf.");
		L.marker([51.495, -0.083], {icon: redIcon}).addTo(map).bindPopup("I am a red leaf.");
		L.marker([51.49, -0.1], {icon: orangeIcon}).addTo(map).bindPopup("I am an orange leaf.");

		
	},
}
</script>
  
<style>
#map {
	height: 100%; 
	width: 100%;
}
</style>
  