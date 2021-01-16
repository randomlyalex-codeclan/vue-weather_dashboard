<template>
  <div>
    <input
      type="text"
      v-model="inputString"
      v-on:keyup.enter="filterCities"
      autocomplete="off"
    />
    <ul>
      <li v-for="(city, index) in filtedCities" v-bind:key="index">
        {{ index }} {{ city.name }}
      </li>
    </ul>
  </div>
</template>

<script>
import AllCities from '../static/world-cities.json'
export default {
  name: 'CitySearchBar2',
  data() {
    return {
      allCities: AllCities,
      inputString: null,
    }
  },
  computed: {
    filtedCities: function() {
      var re = new RegExp(this.inputString, 'gi')
      //   console.log(this.allCities[0].name.match(re) != null)
      //   console.log(this.allCities[1].name.match(re) != null)

      return this.allCities.filter(city => city.name.match(re) != null)
    },
  },
}
</script>

<style lang="css" scoped></style>
