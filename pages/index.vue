<template>
  <v-main>
    <v-container
      class="min-h-full bg-gray-200 flex flex-col align-center"
      fluid
    >
      <input
        v-model="location"
        class="bg-white w-1/3 mb-12 mt-20 focus:outline-none focus:shadow-outline border border-gray-500 text-black rounded-lg py-2 px-4 block appearance-none leading-normal"
        type="city"
        placeholder="Etnry your location"
      />
      <button class="bg-blue-500 rounded py-1 px-2 mb-10" @click="showWeather">
        show weather
      </button>
      <template class="bg-white">
        <v-card class="mx-auto shadow" min-width="400" max-width="600">
          <v-list-item two-line>
            <v-list-item-content>
              <v-list-item-title class="headline">{{
                location
              }}</v-list-item-title>
              <v-list-item-subtitle>{{ info.timezone }}</v-list-item-subtitle>
              <v-list-item-subtitle>
                {{ info.description }}</v-list-item-subtitle
              >
            </v-list-item-content>
          </v-list-item>

          <v-card-text>
            <v-row align="center">
              <v-col class="display-3" cols="6">
                {{ Math.floor(info.temp) }}&deg;C
              </v-col>
            </v-row>
          </v-card-text>

          <v-list-item>
            <v-list-item-icon>
              <v-icon>mdi-send</v-icon>
            </v-list-item-icon>
            <v-list-item-subtitle
              >{{ info.windSpeed }} km/h</v-list-item-subtitle
            >
          </v-list-item>

          <v-list-item>
            <v-list-item-icon>
              <v-icon>mdi-cloud-download</v-icon>
            </v-list-item-icon>
            <v-list-item-subtitle>{{ info.humidity }}% </v-list-item-subtitle>
          </v-list-item>
        </v-card>
      </template>
    </v-container>
  </v-main>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      location: '',
      info: {
        timezone: null,
        temp: null,
        windSpeed: null,
        humidity: null,
        description: null,
        lat: null,
        lon: null,
      },
      time: 0,
    }
  },
  methods: {
    showWeather() {
      axios
        .get(
          `https://api.opencagedata.com/geocode/v1/json?q=${this.location}&key=d4068ba58b4745d8b650068739c713d3`
        )
        .then((response) => {
          this.info.lat = response.data.results[0].geometry.lat
          this.info.lon = response.data.results[0].geometry.lng
          axios
            .get(
              `https://api.openweathermap.org/data/2.5/onecall?lat=${this.info.lat}&lon=${this.info.lon}&units=metric&exclude=hourly,daily,minutely&appid=106c18242227a70ac3dc99786ba74228`
            )

            .then((response) => {
              this.info.timezone = response.data.timezone
              this.info.temp = response.data.current.temp
              this.info.windSpeed = response.data.current.wind_speed
              this.info.humidity = response.data.current.humidity
              this.info.description =
                response.data.current.weather[0].description
            })
        })
    },
  },
}
</script>

<style></style>
