<template>
    <div>
        <input
            type="text"
            placeholder="Start typing city..."
            v-model="inputString"
            v-on:input="checkKeyCount"
            v-on:keyup.enter="submitSearch"
            v-on:keyup.up="hoverCity(-1)"
            v-on:keyup.down="hoverCity(+1)"
            v-on:keyup.esc="
                {
                    inputString = ''
                    allCitiesGeoDb = null
                }
            "
            autocomplete="off"
        />
        <ul v-if="filteredCities !== null && inputString">
            <li
                v-for="(city, index) in filteredCities"
                v-bind:key="index"
                v-bind:class="{ highlighted: index === hoverIndex }"
            >
                {{ city.city }}, {{ city.countryCode }} {{ city.population }}
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    name: 'CitySearchBar2',
    data() {
        return {
            allCitiesGeoDb: null,
            inputString: '',
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
                    (this.hoverIndex + num) % this.filteredCities.length
            }
        },
        submitSearch: function() {
            this.$emit('queryApi', this.searchType, this.searchInput)
            this.inputString = null
            this.allCitiesGeoDb = null
        },
        queryGeoDb: function(namePrefix) {
            fetch(
                `https://wft-geo-db.p.rapidapi.com/v1/geo/cities?limit=10&minPopulation=25000&namePrefix=${namePrefix}&sort=-population%2Cname`,
                {
                    method: 'GET',
                    headers: {
                        'x-rapidapi-key': process.env.VUE_APP_GEO_DB_XAPI_KEY,
                        'x-rapidapi-host': 'wft-geo-db.p.rapidapi.com',
                    },
                },
            )
                .then(response => response.json())
                .then(data => {
                    this.allCitiesGeoDb = data.data
                })
        },
        checkKeyCount() {
            if ([2, 4, 6].indexOf(this.inputString.length) > -1) {
                this.queryGeoDb(this.inputString)
                console.log('this would be an api call')
            }
        },
    },

    computed: {
        filteredCities: function() {
            var re = new RegExp(this.inputString, 'gi')
            this.hoverCity(0)
            if (this.inputString.length >= 2) {
                return this.allCitiesGeoDb.filter(
                    city => city.city.match(re) != null,
                )
            } else return null
        },
        searchInput: function() {
            if (
                !Array.isArray(this.filteredCities) ||
                !this.filteredCities.length
            ) {
                return this.inputString
            } else {
                return (
                    this.filteredCities[this.hoverIndex].city +
                    ',' +
                    this.filteredCities[this.hoverIndex].countryCode
                )
            }
        },
    },
}
</script>

<style lang="css" scoped>
.highlighted {
    background: rgb(255, 99, 71, 0.8);
    font-weight: bold;
    color: white;
}
li {
    list-style: none;
    font-weight: bold;
    width: 30vw;
    margin-left: 7vh;
    margin-right: 13vh;
    border-radius: 5px;
}
input {
    width: 40vw;
    text-align: center;
    background-color: rgb(59, 152, 233, 0.7);
}
</style>
