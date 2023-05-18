<template>
	<div id="map"></div>
</template>
<script>
// import mapboxgl from 'mapbox-gl'
// import "mapbox-gl"
export default {
	name:"mapboxglStart",
	mounted(){
		//导入mapbox-gl
		mapboxgl.accessToken = 'pk.eyJ1IjoiY2FpaG9uZ2NoYW8iLCJhIjoiY2xmcTkzZ3oyMWQ3ZjQycXp3dnJ3cnlyZSJ9.xAE7egcnXDHWU2hdbgShpQ';
		const map = new mapboxgl.Map({
			container: 'map', //id
			style: 'mapbox://styles/examples/clg45vm7400c501pfubolb0xz', 
			preserveDrawingBuffer: true,//允许地图导出为图片,
			center: [-122.662323, 45.523751], 
			zoom: 12
		});
		map.on('click', (event) => {
			const features = map.queryRenderedFeatures(event.point, {
				layers: ['chicago-parks']
			});
			console.log(features)
			if (!features.length) {
				return;
			}
			const feature = features[0];

			const popup = new mapboxgl.Popup({ offset: [0, -15] })
				.setLngLat(feature.geometry.coordinates)
				.setHTML(
					`<h3>${feature.properties.title}</h3><p>${feature.properties.description}</p>`
				)
				.addTo(map);
	  });
		const geojson = {
			type: 'FeatureCollection',
			features: [
				{
					type: 'Feature',
					geometry: {
						type: 'Point',
						coordinates: [-77.032, 38.913]
					},
					properties: {
						title: 'Mapbox',
						description: 'Washington, D.C.'
					}
				},
				{
					type: 'Feature',
					geometry: {
						type: 'Point',
						coordinates: [-122.414, 37.776]
					},
					properties: {
						title: 'Mapbox',
						description: 'San Francisco, California'
					}
				}
			]
		};
		// add markers to map
		for (const feature of geojson.features) {
			// create a HTML element for each feature
			const el = document.createElement('div');
			el.className = 'marker';

			// make a marker for each feature and add to the map
			new mapboxgl.Marker(el)
				.setLngLat(feature.geometry.coordinates)
				.setPopup(
					new mapboxgl.Popup({ offset: 25 }) // add popups
						.setHTML(
							`<h3>${feature.properties.title}</h3><p>${feature.properties.description}</p>`
					)
				).addTo(map);
		}
	}
}
</script>
<style>
#map {
	height: 100%; 
	width: 100%;
	overflow: hidden;
}
.marker {
  background-image: url('../../assets/mapbox-icon.png');
  background-size: cover;
  width: 50px;
  height: 50px;
  border-radius: 50%;
  cursor: pointer;
}

.mapboxgl-popup {
  max-width: 200px;
}

.mapboxgl-popup-content {
  text-align: center;
  font-family: 'Open Sans', sans-serif;
}
</style>