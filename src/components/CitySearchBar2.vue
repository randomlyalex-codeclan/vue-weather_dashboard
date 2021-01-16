<template>
    <div>
        <input
            type="text"
            v-model="inputString"
            v-on:keyup.enter="submitSearch"
            v-on:keyup.up="hoverCity(-1)"
            v-on:keyup.down="hoverCity(+1)"
            autocomplete="off"
        />
        <!-- {{ hoverIndex }} -- {{ this.filterdCities[hoverIndex].name }} -->
        <ul v-if="inputString">
            <li
                v-for="(city, index) in filterdCities"
                v-bind:key="index"
                v-bind:class="{ highlighted: index === hoverIndex }"
            >
                {{ city.name }}, {{ city.country }}
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
            hoverIndex: 0,
            searchType: 'weather',
        }
    },
    methods: {
        hoverCity: function(num) {
            if (num === 0) {
                this.hoverIndex = 0
            } else {
                this.hoverIndex =
                    (this.hoverIndex + num) % this.filterdCities.length
            }
        },
        submitSearch: function() {
            this.$emit('queryApi', this.searchType, this.searchInput)
            this.inputString = null
        },
    },
    computed: {
        filterdCities: function() {
            var re = new RegExp(this.inputString, 'gi')
            this.hoverCity(0)
            //   console.log(this.allCities[0].name.match(re) != null)
            //   console.log(this.allCities[1].name.match(re) != null)

            return this.allCities.filter(city => city.name.match(re) != null)
        },
        searchInput: function() {
            return this.filterdCities[this.hoverIndex].name
        },
    },
}
</script>

<style lang="css" scoped>
.highlighted {
    background: tomato;
    color: white;
}
</style>
