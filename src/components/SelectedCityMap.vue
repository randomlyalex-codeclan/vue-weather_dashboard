<template>
    <div style="height: 500px; width: 100%">
        <div id="map">
            <button @click="showMap = !showMap">
                Toggle map
            </button>
        </div>
        <l-map
            v-if="showMap"
            :zoom="zoom"
            :center="center"
            :options="mapOptions"
            style="height: 80%"
            @update:center="centerUpdate"
            @update:zoom="zoomUpdate"
        >
            <l-tile-layer :url="transport" :attribution="attribution" />
            <l-marker :lat-lng="withPopup">
                <l-popup>
                    <div @click="innerClick">
                        I am a popup
                        <p v-show="showParagraph">
                            Lorem ipsum dolor sit amet, consectetur adipiscing
                            elit. Quisque sed pretium nisl, ut sagittis sapien.
                            Sed vel sollicitudin nisi. Donec finibus semper
                            metus id malesuada.
                        </p>
                    </div>
                </l-popup>
            </l-marker>
            <l-marker :lat-lng="withTooltip">
                <l-tooltip :options="{ permanent: true, interactive: true }">
                    <div @click="innerClick">
                        I am a tooltip
                        <p v-show="showParagraph">
                            Lorem ipsum dolor sit amet, consectetur adipiscing
                            elit. Quisque sed pretium nisl, ut sagittis sapien.
                            Sed vel sollicitudin nisi. Donec finibus semper
                            metus id malesuada.
                        </p>
                    </div>
                </l-tooltip>
            </l-marker>
        </l-map>
    </div>
</template>

<script>
import { latLng } from 'leaflet'
import { LMap, LTileLayer, LMarker, LPopup, LTooltip } from 'vue2-leaflet'

export default {
    name: 'SelectedCityMap',
    components: {
        LMap,
        LTileLayer,
        LMarker,
        LPopup,
        LTooltip,
    },
    props: ['selectedCity'],
    computed: {
        center() {
            if (this.selectedCity) {
                return latLng(
                    this.selectedCity.coord.lat,
                    this.selectedCity.coord.lon,
                )
            } else return latLng(55.9521, -3.1965)
        },
    },
    data() {
        return {
            zoom: 13,
            osmurl: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
            //make these one query with buttons later:
            transport: `https://tile.thunderforest.com/transport/{z}/{x}/{y}.png?apikey=${process.env.VUE_APP_THUNDERFOREST_API_KEY}`,
            OpenCycleMap: `https://tile.thunderforest.com/cycle/{z}/{x}/{y}.png?apikey=${process.env.VUE_APP_THUNDERFOREST_API_KEY}`,
            Transport: `https://tile.thunderforest.com/transport/{z}/{x}/{y}.png?apikey=${process.env.VUE_APP_THUNDERFOREST_API_KEY}`,
            Landscape: `https://tile.thunderforest.com/landscape/{z}/{x}/{y}.png?apikey=${process.env.VUE_APP_THUNDERFOREST_API_KEY}`,
            Outdoors: `https://tile.thunderforest.com/outdoors/{z}/{x}/{y}.png?apikey=${process.env.VUE_APP_THUNDERFOREST_API_KEY}`,
            TransportDark: `https://tile.thunderforest.com/transport-dark/{z}/{x}/{y}.png?apikey=${process.env.VUE_APP_THUNDERFOREST_API_KEY}`,
            SpinalMap: `https://tile.thunderforest.com/spinal-map/{z}/{x}/{y}.png?apikey=${process.env.VUE_APP_THUNDERFOREST_API_KEY}`,
            Pioneer: `https://tile.thunderforest.com/pioneer/{z}/{x}/{y}.png?apikey=${process.env.VUE_APP_THUNDERFOREST_API_KEY}`,
            MobileAtlas: `https://tile.thunderforest.com/mobile-atlas/{z}/{x}/{y}.png?apikey=${process.env.VUE_APP_THUNDERFOREST_API_KEY}`,
            Neighbourhood: `https://tile.thunderforest.com/neighbourhood/{z}/{x}/{y}.png?apikey=${process.env.VUE_APP_THUNDERFOREST_API_KEY}`,
            attribution:
                '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
            withPopup: latLng(47.41322, -1.219482),
            withTooltip: latLng(47.41422, -1.250482),
            currentZoom: 11.5,
            currentCenter: latLng(47.41322, -1.219482),
            showParagraph: false,
            mapOptions: {
                zoomSnap: 0.5,
            },
            showMap: true,
        }
    },
    methods: {
        zoomUpdate(zoom) {
            this.currentZoom = zoom
        },
        centerUpdate(center) {
            this.currentCenter = center
        },
        showLongText() {
            this.showParagraph = !this.showParagraph
        },
        innerClick() {
            alert('Click!')
        },
    },
}
</script>

<style type="scss" scoped>
.map {
    height: 100vh;
    width: 70vh;
}
</style>
