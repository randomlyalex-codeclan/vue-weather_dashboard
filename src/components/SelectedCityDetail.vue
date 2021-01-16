<template>
  <div>
    <h1>Name: {{ selectedCity.name }}, {{ selectedCity.sys.country }}</h1>
    <h3>lat: {{ selectedCity.coord.lat }}</h3>
    <h3>long: {{ selectedCity.coord.lon }}</h3>
  </div>
</template>

<script>
export default {
  name: 'SelectedCityDetail',
  props: ['selectedCity'],
}
</script>

<style lang="scss" scoped></style>
