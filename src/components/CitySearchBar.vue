<template>
    <div>
        <div>
            <input
                type="text"
                placeholder="Guess a city..."
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

<style lang="css" scoped></style>
