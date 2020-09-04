<template>
  <div>
    <v-main>
      <v-container
        class="min-h-full bg-gray-200 flex flex-col align-center"
        fluid
      >
        <input
          v-model.lazy="info.location"
          class="bg-white w-2/3 md:w-1/3 mb-6 mt-8 lg:mt-20 focus:outline-none focus:shadow-outline border border-gray-500 text-black rounded-lg py-2 px-4 block appearance-none leading-normal"
          type="city"
          placeholder="Entry your location"
        />
        <div class="flex flex-col lg:w-1/3 lg:flex-row justify-space-around">
          <button
            class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded mb-6"
            @click="showWeather"
          >
            Find and save
          </button>
          <button
            class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded mb-6"
            @click="saveLocally"
          >
            Find and save locally
          </button>
        </div>
        <template class="bg-white">
          <v-card class="mx-auto shadow" min-width="300" max-width="600">
            <v-list-item two-line>
              <v-list-item-content>
                <v-list-item-title class="headline">{{
                  info.location
                }}</v-list-item-title>
                <v-list-item-subtitle>{{
                  info.timestamp
                }}</v-list-item-subtitle>
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
    <div class="mt-6 pb-10 lg:mb-20 lg:w-4/6 overflow-x-auto" min-width="375">
      <table class="table-auto">
        <thead>
          <tr>
            <th class="px-4 py-2">Index</th>
            <th class="px-4 py-2">Location</th>
            <th class="px-4 py-2">Timestamp</th>
            <th class="px-4 py-2">Timezone</th>
            <th class="px-4 py-2">Weather description</th>
            <th class="px-4 py-2">Temperature</th>
            <th class="px-4 py-2">Wind Speed</th>
            <th class="px-4 py-2">Humidity</th>
          </tr>
        </thead>
      </table>
      <tbody>
        <!-- Передача данных компоненту -->
        <weatherTable :saved-locally="savedLocally" />
      </tbody>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import weatherTable from '../components/weatherTable.vue'
export default {
  components: {
    weatherTable,
  },
  data() {
    return {
      savedLocally: [],
      info: {
        location: 'Kyiv',
        timezone: null,
        timestamp: null,
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
  /* Слежение за значением массива */
  watch: {
    localArray(newArray) {
      localStorage.savedLocally = newArray
    },
  },
  /* Управление загрузкой значения из localStorage */
  mounted() {
    this.showWeather()
    if (localStorage.getItem('savedLocally')) {
      try {
        this.savedLocally = JSON.parse(localStorage.getItem('savedLocally'))
      } catch (e) {
        localStorage.removeItem('savedLocally')
      }
    }
  },

  methods: {
    /* Функция для  просмотра погоды и сохранения данных в таблице */
    showWeather() {
      /* Запрос с получением координат локации, по её названию */
      axios
        .get(
          `https://api.opencagedata.com/geocode/v1/json?q=${this.info.location}&key=d4068ba58b4745d8b650068739c713d3`
        )
        .then((response) => {
          this.info.lat = response.data.results[0].geometry.lat
          this.info.lon = response.data.results[0].geometry.lng
          this.info.timestamp = response.data.timestamp.created_http
          /* Запрос с получением данных о погоде по координатам */
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
              /* Проверка на пустое поле ввода */
              if (!this.info.location) {
                return
              }
              /* Сохранение и обновления данных в массиве */
              this.savedLocally = [
                ...this.savedLocally,
                Object.assign({}, this.info),
              ]
            })
        })
    },
    /* Функция для  просмотра погоды и локального сохранения данных в таблице */
    saveLocally() {
      /* Запрос с получением координат локации, по её названию */
      axios
        .get(
          `https://api.opencagedata.com/geocode/v1/json?q=${this.info.location}&key=d4068ba58b4745d8b650068739c713d3`
        )
        .then((response) => {
          this.info.lat = response.data.results[0].geometry.lat
          this.info.lon = response.data.results[0].geometry.lng
          this.info.timestamp = response.data.timestamp.created_http
          /* Запрос с получением данных о погоде по координатам */
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
              /* Проверка на пустое поле ввода */
              if (!this.info.location) {
                return
              }
              /* Сохранение и обновления данных в массиве */
              this.savedLocally = [
                ...this.savedLocally,
                Object.assign({}, this.info),
              ]
              /* Локальное сохранение */
              const parsed = JSON.stringify(this.savedLocally)
              localStorage.setItem('savedLocally', parsed)
            })
        })
    },
  },
}
</script>

<style></style>
