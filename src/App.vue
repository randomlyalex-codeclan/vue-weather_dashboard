<template>
  <div id="app">
    <div>
      <city-search-bar
        v-on:current-weatherQueryApi="currentWeatherData"
        v-on:queryApi="queryApi"
      >
      </city-search-bar>
      <selected-city-detail
        v-bind:selectedCity="currentWeather"
      ></selected-city-detail>
    </div>
  </div>
</template>

<script>
import CitySearchBar from './components/CitySearchBar'
import SelectedCityDetail from './components/SelectedCityDetail'

export default {
  name: 'App',
  components: {
    'city-search-bar': CitySearchBar,
    'selected-city-detail': SelectedCityDetail,
  },
  data() {
    return {
      currentWeather: null,
      selectedCity: {
        coord: { lon: -3.1965, lat: 55.9521 },
        weather: [
          { id: 801, main: 'Clouds', description: 'few clouds', icon: '02d' },
        ],
        base: 'stations',
        main: {
          temp: 280.78,
          feels_like: 275.1,
          temp_min: 279.82,
          temp_max: 281.48,
          pressure: 1006,
          humidity: 87,
        },
        visibility: 10000,
        wind: { speed: 6.69, deg: 250 },
        clouds: { all: 20 },
        dt: 1610806774,
        sys: {
          type: 1,
          id: 1442,
          country: 'GB',
          sunrise: 1610785941,
          sunset: 1610813543,
        },
        timezone: 0,
        id: 2650225,
        name: 'Edinburgh',
        cod: 200,
      },
    }
  },
  mounted: function() {},
  computed: {},
  methods: {
    //this query function takes the place of all of the below, so long as i can figure out how to make it safe
    queryApi: function(call, query) {
      fetch(
        // more details https://openweathermap.org/api
        // work out how to validate  call = [
        //   'weather',
        //   'hourly',
        //   'daily',
        //   'history'
        // ].indexOf(value) !== -1
        // }
        // consider how to keep query safe from injection
        //
        `https://api.openweathermap.org/data/2.5/${call}?q=${query}&appid=${process.env.VUE_APP_OW_API_KEY}`,
      )
        .then(response => response.json())
        .then(data => (this.currentWeather = data))
    },

    currentWeatherData: function(cityName, countryCode) {
      fetch(
        // more details https://openweathermap.org/current
        `https://api.openweathermap.org/data/2.5/weather?q=${cityName},${countryCode}&appid=${process.env.VUE_APP_OW_API_KEY}`,
      )
        .then(response => response.json())
        .then(data => (this.currentWeather = data))
    },

    hourlyForecast4Days: function(cityName, countryCode) {
      fetch(
        //more details https://openweathermap.org/api/hourly-forecast
        `pro.openweathermap.org/data/2.5/forecast/hourly?q=${cityName},${countryCode}&appid=${process.env.VUE_APP_OW_API_KEY}`,
      )
    },

    dailyForecast16Days: function(cityName, countryCode, count) {
      fetch(
        //more details https://openweathermap.org/forecast16
        `api.openweathermap.org/data/2.5/forecast/daily?q=${cityName},${countryCode}&cnt=${count}&appid=${process.env.VUE_APP_OW_API_KEY}`,
      )
    },

    historicalWeatherAPI: function(cityName, countryCode, unixStart, unixEnd) {
      fetch(
        //more details https://openweathermap.org/history
        `http://history.openweathermap.org/data/2.5/history/city?q=${cityName},${countryCode}&type=hour&start=${unixStart}&end=${unixEnd}&appid=${process.env.VUE_APP_OW_API_KEY}
`,
      )
    },
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
