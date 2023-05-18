<template>
	<div id="map"></div>
	<div id="instructions"></div>
</template>
<script>

export default {
	name:"navigation",
    mounted(){
			mapboxgl.accessToken = 'pk.eyJ1IjoiY2FpaG9uZ2NoYW8iLCJhIjoiY2xmcTkzZ3oyMWQ3ZjQycXp3dnJ3cnlyZSJ9.xAE7egcnXDHWU2hdbgShpQ';
			const map = new mapboxgl.Map({
				container: 'map', //id
				style:'mapbox://styles/mapbox/streets-v12', // mapbox官方的底图和样式文件
				preserveDrawingBuffer: true,//允许地图导出为图片,
				center: [-122.662323, 45.523751], 
				zoom: 12
			});
					// set the bounds of the map
			const bounds = [
				[-123.069003, 45.395273],
				[-122.303707, 45.612333]
			];
			map.setMaxBounds(bounds);

			// an arbitrary start will always be the same
			// only the end or destination will change
			const start = [-122.662323, 45.523751];

			// this is where the code for the next step will go
			// create a function to make a directions request
			async function getRoute(end) {
			// make a directions request using cycling profile
			// an arbitrary start will always be the same
			// only the end or destination will change
			const query = await fetch(
				`https://api.mapbox.com/directions/v5/mapbox/cycling/${start[0]},${start[1]};${end[0]},${end[1]}?steps=true&geometries=geojson&access_token=pk.eyJ1IjoiY2FpaG9uZ2NoYW8iLCJhIjoiY2xmcTkzZ3oyMWQ3ZjQycXp3dnJ3cnlyZSJ9.xAE7egcnXDHWU2hdbgShpQ`,
				{ method: 'GET' }
			);
			const json = await query.json();
			const data = json.routes[0];
			const route = data.geometry.coordinates;
			const geojson = {
				type: 'Feature',
				properties: {},
				geometry: {
					type: 'LineString',
					coordinates: route
				}
			};
			// if the route already exists on the map, we'll reset it using setData
			if (map.getSource('route')) {
				map.getSource('route').setData(geojson);
			}
			// otherwise, we'll make a new request
			else {
				map.addLayer({
					id: 'route',
					type: 'line',
					source: {
						type: 'geojson',
						data: geojson
					},
					layout: {
						'line-join': 'round',
						'line-cap': 'round'
					},
					paint: {
						'line-color': '#3887be',
						'line-width': 5,
						'line-opacity': 0.75
					}
				});
			}
			// add turn instructions here at the end
			// get the sidebar and add the instructions
			const instructions = document.getElementById('instructions');
			const steps = data.legs[0].steps;

			let tripInstructions = '';
			for (const step of steps) {
				tripInstructions += `<li>${step.maneuver.instruction}</li>`;
			}
			instructions.innerHTML = `<p><strong>Trip duration: ${Math.floor(
				data.duration / 60
			)} min 🚴 </strong></p><ol>${tripInstructions}</ol>`;

			}

			map.on('load', () => {
			// Add starting point to the map
			map.addLayer({
				id: 'point',
				type: 'circle',
				source: {
					type: 'geojson',
					data: {
							type: 'FeatureCollection',
							features: [
							{
								type: 'Feature',
								properties: {},
								geometry: {
									type: 'Point',
									coordinates: start
								}
							}
							]
					}
				},
				paint: {
				'circle-radius': 10,
				'circle-color': '#3887be'
				}
			});
			// this is where the code from the next step will go
		});
		map.on('click', (event) => {
			const coords = Object.keys(event.lngLat).map((key) => event.lngLat[key]);
			const end = {
				type: 'FeatureCollection',
				features: [
					{
						type: 'Feature',
						properties: {},
						geometry: {
							type: 'Point',
							coordinates: coords
						}
					}
				]
			};
			if (map.getLayer('end')) {
				map.getSource('end').setData(end);
			} else {
				map.addLayer({
					id: 'end',
					type: 'circle',
					source: {
						type: 'geojson',
						data: {
							type: 'FeatureCollection',
							features: [
								{
									type: 'Feature',
									properties: {},
									geometry: {
										type: 'Point',
										coordinates: coords
									}
								}
							]
						}
					},
					paint: {
						'circle-radius': 10,
						'circle-color': '#f30'
					}
				});
			}
			getRoute(coords);
		});
	}
}
</script>
<style scoped>
#instructions {
  position: absolute;
  margin: 20px;
  width: 25%;
  top: 0;
  bottom: 20%;
  padding: 40px;
  background-color: #fff;
  overflow-y: scroll;
  font-family: sans-serif;
}
</style>