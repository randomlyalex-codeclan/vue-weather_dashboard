<template>
  <div>
    <div>
      <button
        v-on:click="$emit('current-weather-query-api', 'Edinburgh', 'UK')"
      >
        Get data [payload: cityName: Edinburgh, countryCode:UK
      </button>
    </div>
    <div>
      <input
        type="text"
        placeholder="Enter city"
        v-model="searchInput"
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
    }
  },
}
</script>

<style lang="scss" scoped></style>
