<template>
  <div>
    <h1>Name: {{ selectedCity.name }}, {{ selectedCity.sys.country }}</h1>
    <h3>
      lat: {{ selectedCity.coord.lat }} long: {{ selectedCity.coord.lon }}
    </h3>
    <h3>Current Weather: {{ selectedCity.weather[0].description }}</h3>
    <h3>Current Temp: {{ roundToDp(selectedCity.main.temp - 273.15, 0) }}</h3>
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
