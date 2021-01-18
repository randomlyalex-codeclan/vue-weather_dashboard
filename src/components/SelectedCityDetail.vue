<template>
    <div v-if="selectedCity === null">
        <!-- blank start page -->
    </div>
    <div class="city-day-result" v-else-if="selectedCity.cod != '404'">
        <h1>{{ selectedCity.name }}, {{ selectedCity.sys.country }}</h1>
        <p>
            {{ getNiceDate(selectedCity.dt) }}
        </p>
        <p>Currently: {{ selectedCity.weather[0].description }}</p>
        <p>
            Current Temp: {{ roundToDp(selectedCity.main.temp - 273.15, 1) }}°
        </p>
        <p>
            Feels Like:
            {{ roundToDp(selectedCity.main.feels_like - 273.15, 1) }}°
        </p>
        <p>
            Min / Max: {{ roundToDp(selectedCity.main.temp_min - 273.15, 0) }}°
            / {{ roundToDp(selectedCity.main.temp_max - 273.15, 0) }}°
        </p>
        <button
            v-on:click="
                submitSearch(
                    'forecast',
                    selectedCity.name + ',' + selectedCity.sys.country,
                )
            "
        >
            5 day / 3 hour
        </button>
        <button>Map Overlay</button>
    </div>
    <div v-else>
        <h1>No city found</h1>
    </div>
</template>

<script>
export default {
    name: 'SelectedCityDetail',
    props: ['selectedCity', 'forecast'],
    methods: {
        roundToDp: function(num, dp) {
            return Math.round((num + Number.EPSILON) * 10 ** dp) / 10 ** dp
        },
        submitSearch: function(searchType, searchInput) {
            this.$emit('queryApi', searchType, searchInput)
        },
        getNiceDate: function(timestamp) {
            return new Date(timestamp * 1000).toString().substring(0, 10)
        },
    },
    computed: {},
}
</script>

<style lang="css" scoped>
.city-day-result {
    text-align: right;
    padding-right: 5vw;
}
</style>
