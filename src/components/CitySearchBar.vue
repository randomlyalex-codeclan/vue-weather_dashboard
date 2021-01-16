<template>
  <div>
    <div>
      <button v-on:click="$emit('currentWeatherQuery-Api', 'Edinburgh', 'UK')">
        Get data [payload: cityName: Edinburgh, countryCode:UK
      </button>
    </div>
    <div>
      <input
        type="text"
        placeholder="Enter city"
        v-model="searchInput"
        v-on:keyup.enter="$emit('queryApi', searchType, searchInput)"
        autocomplete="off"
      />
    </div>
  </div>
</template>

<script>
export default {
  name: 'CitySearchBar',
  data() {
    return {
      searchInput: null,
      searchType: 'weather',
    }
  },
}
</script>

<style lang="scss" scoped></style>
