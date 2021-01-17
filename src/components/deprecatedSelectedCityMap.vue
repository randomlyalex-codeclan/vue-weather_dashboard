<template>
    <div id="container">
        <div id="mapContainer">
            <l-map
                @update:zoom="zoomUpdate"
                @update:center="centerUpdate"
                :zoom="zoom"
                :center="center"
            >
                <l-tile-layer
                    :url="url"
                    :attribution="attribution"
                ></l-tile-layer>
            </l-map>
        </div>
    </div>
</template>

<script>
import L from 'leaflet'
import { LMap, LTileLayer } from 'vue2-leaflet' //LMarker
export default {
    name: 'SelectedCityMap',
    data() {
        return {
            zoom: 13,
            center: [37, 7749, -122, 4194],
            url: `https://tile.thunderforest.com/transport/{z}/{x}/{y}.png?apikey=${process.env.VUE_APP_THUNDERFOREST_API_KEY}`,
            attribution:
                '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
            marker: L.latLng(47.41322, -1.219482),
            currentCenter: L.latLng(47.41322, -1.219482),
            currentZoom: 7,
            iconSize: [15, 15],
        }
    },
    components: {
        LMap,
        LTileLayer,
        // LMarker,
    },
    methods: {
        // setupLeafletMap: function() {
        //     const mapDiv = L.map('mapContainer').setView(this.center, 13)
        //     L.tileLayer(
        //         `https://api.mapbox.com/styles/v1/{id}/tiles/{z}/{x}/{y}?access_token={accessToken}`,
        //         {
        //             attribution:
        //                 'Map data (c) <a href="https://www.openstreetmap.org/">OpenStreetMap</a> contributors, <a href="https://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, Imagery (c) <a href="https://www.mapbox.com/">Mapbox</a>',
        //             maxZoom: 18,
        //             id: 'mapbox/streets-v11',
        //             accessToken: process.env.VUE_APP_MAPBOX_API_KEY,
        //         },
        //     ).addTo(mapDiv)
        // },
        latLng: function(lat, lng) {
            return L.latLng(lat, lng)
        },
        centerUpdate: function(center) {
            this.currentCenter = center
        },
        zoomUpdate: function(zoom) {
            this.currentZoom = zoom
        },
    },
    mounted() {
        // this.setupLeafletMap()
    },
    props: ['selectedCity'],
}
</script>

<style scoped>
#mapContainer {
    height: 50vh;
    width: 50vh;
}
</style>
