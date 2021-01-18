<template>
    <div id="app">
        <div class="top-half">
            <city-search-bar2
                class="search-container"
                v-on:queryApi="queryApi"
            ></city-search-bar2>
            <city-search-bar v-on:queryApi="queryApi"> </city-search-bar>
        </div>
        <div class="bottom-half">
            <div class="left-container">
                <selected-city-detail
                    v-bind:selectedCity="selectedCity"
                    v-bind:forecast="forecast"
                    v-on:queryApi="queryApi"
                >
                </selected-city-detail>
            </div>
            <div class="right-container">
                <div>
                    <form id="right-container-toggle-form">
                        <input
                            type="radio"
                            id="map-radio-button"
                            value="weather"
                            v-model="toggleViewSelect"
                        />
                        <label for="map-radio-button">Map</label>

                        <input
                            type="radio"
                            id="forecast-radio-button"
                            value="forecast"
                            v-model="toggleViewSelect"
                        />
                        <label for="forecast-radio-button">Forecast</label>

                        <input
                            type="radio"
                            id="historical-radio-button"
                            value="historical"
                            v-model="toggleViewSelect"
                        />
                        <label for="historical-radio-button">Historical</label>
                    </form>
                </div>
                <div v-if="toggleViewSelect === 'weather'">
                    <selected-city-map
                        class="weather"
                        v-bind:selectedCity="selectedCity"
                    ></selected-city-map>
                </div>
                <div v-if="toggleViewSelect === 'forecast'">
                    <selected-city-forecast
                        class="forecast"
                        v-bind:forecast="forecast"
                    ></selected-city-forecast>
                </div>
                <div v-if="toggleViewSelect === 'historical'">
                    <selected-city-historical
                        class="historical"
                        v-bind:historical="historical"
                    ></selected-city-historical>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import CitySearchBar from './components/CitySearchBar'
import CitySearchBar2 from './components/CitySearchBar2.vue'
import SelectedCityDetail from './components/SelectedCityDetail'
import SelectedCityMap from './components/SelectedCityMap.vue'
import SelectedCityForecast from './components/SelectedCityForecast.vue'
import SelectedCityHistorical from './components/SelectedCityHistorical.vue'

export default {
    name: 'App',
    components: {
        'city-search-bar': CitySearchBar,
        'selected-city-detail': SelectedCityDetail,
        'city-search-bar2': CitySearchBar2,
        'selected-city-map': SelectedCityMap,
        'selected-city-forecast': SelectedCityForecast,
        'selected-city-historical': SelectedCityHistorical,
    },
    data() {
        return {
            selectedCity: null,
            forecast: null,
            historical: null,
            toggleViewSelect: 'weather',
        }
    },
    mounted: function() {
        fetch(`./assets/city.list.json`)
            .then(response => response.json())
            .then(data => (this.allCities = data))
    },
    computed: {},
    methods: {
        //this query function takes the place of all of the below, so long as i can figure out how to make it safe
        queryApi: function(call, query) {
            const choices = ['weather', 'forecast', 'historical']
            this.toggleViewSelect = call
            if (choices.indexOf(call) === -1) {
                return null
            } else {
                fetch(
                    `https://api.openweathermap.org/data/2.5/${call}?q=${query}&appid=${process.env.VUE_APP_OW_API_KEY}`,
                )
                    .then(response => response.json())
                    .then(data => {
                        if (call === 'weather') {
                            this.selectedCity = data
                        } else if (call === 'forecast') {
                            this.forecast = data
                        } else if (call === 'historical') {
                            this.historical = data
                        }
                    })
            }
        },
        queryOneCallApi: function() {
            // https://api.openweathermap.org/data/2.5/onecall?lat={lat}&lon={lon}&exclude={part}&appid={API key}
        },

        //     currentWeatherData: function(cityName, countryCode) {
        //       fetch(
        //         // more details https://openweathermap.org/current
        //         `https://api.openweathermap.org/data/2.5/weather?q=${cityName},${countryCode}&appid=${process.env.VUE_APP_OW_API_KEY}`,
        //       )
        //         .then(response => response.json())
        //         .then(data => (this.currentWeather = data))
        //     },

        //     hourlyForecast4Days: function(cityName, countryCode) {
        //       fetch(
        //         //more details https://openweathermap.org/api/hourly-forecast
        //         `pro.openweathermap.org/data/2.5/forecast/hourly?q=${cityName},${countryCode}&appid=${process.env.VUE_APP_OW_API_KEY}`,
        //       )
        //     },

        //     dailyForecast16Days: function(cityName, countryCode, count) {
        //       fetch(
        //         //more details https://openweathermap.org/forecast16
        //         `api.openweathermap.org/data/2.5/forecast/daily?q=${cityName},${countryCode}&cnt=${count}&appid=${process.env.VUE_APP_OW_API_KEY}`,
        //       )
        //     },

        //     historicalWeatherAPI: function(cityName, countryCode, unixStart, unixEnd) {
        //       fetch(
        //         //more details https://openweathermap.org/history
        //         `http://history.openweathermap.org/data/2.5/history/city?q=${cityName},${countryCode}&type=hour&start=${unixStart}&end=${unixEnd}&appid=${process.env.VUE_APP_OW_API_KEY}
        // `,
        //       )
        //     },
    },
}
</script>

<style>
/* main viewport */
#app {
    z-index: 1;
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 30px;
    height: 100vh;
    width: 100vw;
    background-color: rgb(223, 229, 235);
}

.top-half {
    z-index: 1;
    width: 100vw;
    background-color: rgb(139, 201, 255);
}

.search-container {
    position: absolute;
    z-index: 100;
    top: 0;
    left: 25%;
    right: 25%;
    margin: 0;
    border: none;
    border-bottom-left-radius: 1em;
    border-bottom-right-radius: 1em;

    background-color: rgb(59, 152, 233, 0.85);
}

.bottom-half {
    z-index: 1;
    display: flex;
    width: 100vw;
    height: 100vh;

    background-color: rgb(139, 201, 255);
}

.left-container {
    z-index: 1;
    width: 35vw;
}

.right-container {
    z-index: 1;
    width: 65vw;
}

/* apps within */
.map {
    z-index: 1;
    width: 100vw;
    height: 100vh;
}
</style>
