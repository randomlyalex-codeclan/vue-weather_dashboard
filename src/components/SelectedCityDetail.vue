<template>
    <div v-if="selectedCity === null">
        <!-- blank start page -->
    </div>
    <div v-else-if="selectedCity.cod != '404'">
        <h1>{{ selectedCity.name }}, {{ selectedCity.sys.country }}</h1>
        <p>
            lat: {{ selectedCity.coord.lat }} long: {{ selectedCity.coord.lon }}
        </p>
        <p>Current Weather: {{ selectedCity.weather[0].description }}</p>
        <p>
            Current Temp: {{ roundToDp(selectedCity.main.temp - 273.15, 0) }}°
        </p>
        <button>4 hours</button>
        <button>Daily</button>
        <button>History</button>
    </div>
    <div v-else>
        <h1>No city found</h1>
    </div>
</template>

<script>
export default {
    name: 'SelectedCityDetail',
    props: ['selectedCity'],
    methods: {
        roundToDp: function(num, dp) {
            return Math.round((num + Number.EPSILON) * 10 ** dp) / 10 ** dp
        },
    },
}
</script>

<style lang="css" scoped></style>
