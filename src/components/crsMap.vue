<template>
    <div id="map"></div>
</template>
<script>
export default {
	name:"crsMap",
    mounted(){
        let map = L.map('map', {
            crs: L.CRS.Simple,
            minZoom: -5
        });
        let wmsLayer = L.tileLayer.wms('http://ows.mundialis.de/services/service?', {
            layers: 'SRTM30-Colored-Hillshade'
        }).addTo(map);
        // let bounds = [[0,0], [1000,1000]];
        // let image = L.imageOverlay('./src/assets/uqm_map_400px.png', bounds).addTo(map);
        // map.fitBounds(bounds);
    }
}
</script>
<style>
</style>