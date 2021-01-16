<template>
  <div id="app">
    <div>
      <city-search-bar></city-search-bar>
      <selected-city-detail></selected-city-detail>
      <button @click="currentWeatherData('Edinburgh', 'UK')">Get data</button>
    </div>
  </div>
</template>

<script>
import CitySearchBar from '.components/CitySearchBar'
import SelectedCityDetail from '.components/SelectedCityDetail'

export default {
  name: 'App',
  components: {
    'city-search-bar': CitySearchBar,
    'selected-city-detail': SelectedCityDetail,
  },
  data() {
    return {
      currentWeather: null,
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
