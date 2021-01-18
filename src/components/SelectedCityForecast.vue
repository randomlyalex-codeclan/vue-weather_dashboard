<template>
    <div>
        <table>
            <tr>
                <th>Date/ Time</th>
                <th>Summary</th>
                <th>Cloud</th>
                <th>Wind -></th>
                <th>Wind Speed</th>
                <th>Min/Max</th>
                <th>Humidity</th>
            </tr>
            <tr></tr>
            <tr
                v-for="(hourly, index) in forecast.list"
                v-bind:key="index"
                v-bind:class="oddEven(index)"
            >
                <td>
                    {{ convertDateTime(hourly.dt, 0, 1) }}
                    {{ convertDateTimeISO(hourly.dt, 8, 10) }}
                    {{ convertDateTimeISO(hourly.dt, 11, 16) }}
                </td>
                <td>{{ hourly.weather[0].description }}</td>
                <td>{{ hourly.clouds.all }}</td>
                <!-- <span> {{ hourly.rain['3h'] }} </span> -->
                <td>{{ hourly.wind.deg }}</td>
                <td>{{ hourly.wind.speed }}</td>
                <td>
                    {{ roundToDp(hourly.main.temp_min - 273.15, 1) }} /
                    {{ roundToDp(hourly.main.temp_max - 273.15, 1) }}
                </td>
                <td>{{ hourly.main.humidity }}</td>
            </tr>
        </table>
    </div>
</template>

<script>
export default {
    name: 'SelectedCityForecast',
    data() {
        return {
            parsedDailyData: null,
        }
    },
    props: ['forecast'],
    methods: {
        convertDateTime: function(timestamp, start, end) {
            return new Date(timestamp * 1000).toString().substring(start, end)
        },
        convertDateTimeISO: function(timestamp, start, end) {
            return new Date(timestamp * 1000)
                .toISOString()
                .substring(start, end)
        },
        roundToDp: function(num, dp) {
            return Math.round((num + Number.EPSILON) * 10 ** dp) / 10 ** dp
        },
        oddEven: function(index) {
            if (index % 2 === 0) {
                return 'even'
            } else return 'odd'
        },
        // parseDailyData: function() {
        //     parsedDailyData = {
        //         mon: null,
        //         tue: null,
        //         wed: null,
        //         thu: null,
        //         fri: null,
        //         sat: null,
        //         sun: null,
        //     }
        // map with this: convertDateTimeISO(forcast.list[x].dt, 0, 3) === mon,tue,wed,thu,fri,sat,sun
        //     this.forecast.list.map() // figure out how to map mon, tue,
        // },
    },
    mounted: function() {},
}
</script>

<style lang="css" scoped>
table {
    border: 1px solid black;
}
th {
}
tr {
}
.odd {
}
.even {
}
</style>
