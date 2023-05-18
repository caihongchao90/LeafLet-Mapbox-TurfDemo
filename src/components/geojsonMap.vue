<template>
	<div id="map"></div>
</template>
<script>
import statesData from "../assets/us-states.js"
export default {
	name:"geojsonMap",
    mounted(){
        let map = L.map('map').setView([37.8, -96], 4);
		L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
			maxZoom: 18,
			attribution: '© OpenStreetMap'
		}).addTo(map);
        function getColor(d) {
			return d > 1000 ? '#800026' :
				d > 500  ? '#BD0026' :
				d > 200  ? '#E31A1C' :
				d > 100  ? '#FC4E2A' :
				d > 50   ? '#FD8D3C' :
				d > 20   ? '#FEB24C' :
				d > 10   ? '#FED976' :
							'#FFEDA0';
		}
		function style(feature) {
			return {
				fillColor: getColor(feature.properties.density),
				weight: 2,
				opacity: 1,
				color: 'white',
				dashArray: '3',
				fillOpacity: 0.7
			};
		}
		let geojson = L.geoJson(statesData, {style: style}).addTo(map);
		
        // 右上角信息控件
        let info = L.control();
		info.onAdd = function (map) {
			this._div = L.DomUtil.create('div', 'info'); // create a div with a class "info"
			this.update();
			return this._div;
		};
		// method that we will use to update the control based on feature properties passed
		info.update = function (props) {
			this._div.innerHTML = '<h4>US Population Density</h4>' +  (props ?
				'<b>' + props.name + '</b><br />' + props.density + ' people / mi<sup>2</sup>'
				: 'Hover over a state');
		};
		info.addTo(map);

        // 右下角图例控件
		let legend = L.control({position: 'bottomright'});
		legend.onAdd = function (map) {
			let div = L.DomUtil.create('div', 'info legend'),
				grades = [0, 10, 20, 50, 100, 200, 500, 1000],
				labels = [];

			// loop through our density intervals and generate a label with a colored square for each interval
			for (let i = 0; i < grades.length; i++) {
				div.innerHTML +=
					'<i style="background:' + getColor(grades[i] + 1) + '"></i> ' +
					grades[i] + (grades[i + 1] ? '&ndash;' + grades[i + 1] + '<br>' : '+');
			}

			return div;
		};
		legend.addTo(map);

        // 鼠标事件处理
		function highlightFeature(e) {
			let layer = e.target;
			layer.setStyle({
				weight: 5,
				color: '#666',
				dashArray: '',
				fillOpacity: 0.7
			});
			layer.bringToFront();
			info.update(layer.feature.properties);
		}
		function resetHighlight(e) {
			geojson.resetStyle(e.target);
			info.update();
		}
		function zoomToFeature(e) {
			map.fitBounds(e.target.getBounds());
		}
		function onEachFeature(feature, layer) {
			layer.on({
				mouseover: highlightFeature,
				mouseout: resetHighlight,
				click: zoomToFeature
			});
		}
		geojson = L.geoJson(statesData, {
			style: style,
			onEachFeature: onEachFeature
		}).addTo(map);
		
		
    }
}
</script>
<style>
#map {
	height: 100%; 
	width: 100%;

}
.info {
    padding: 6px 8px;
    font: 14px/16px Arial, Helvetica, sans-serif;
    background: white;
    background: rgba(255,255,255,0.8);
    box-shadow: 0 0 15px rgba(0,0,0,0.2);
    border-radius: 5px;
}
.info h4 {
    margin: 0 0 5px;
    color: #777;
}
.legend {
    line-height: 18px;
    color: #555;
}
.legend i {
    width: 18px;
    height: 18px;
    float: left;
    margin-right: 8px;
    opacity: 0.7;
}
</style>
  