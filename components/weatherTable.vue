<template>
  <div>
    <tr v-for="item in savedLocally" :key="item.location">
      <td class="border px-4 py-2">{{ item.location }}</td>
      <td class="border px-4 py-2">{{ item.timestamp }}</td>
      <td class="border px-4 py-2">{{ item.timezone }}</td>
      <td class="border px-4 py-2">{{ item.description }}</td>
      <td class="border px-4 py-2">{{ Math.floor(item.temp) }}&deg;C</td>
      <td class="border px-4 py-2">{{ item.windSpeed }}km/h</td>
      <td class="border px-4 py-2">{{ item.humidity }}%</td>
    </tr>
  </div>
</template>

<script>
export default {
  props: [
    'savedLocally',
    /* location: String,
    timestamp: String,
    timezone: String,
    description: String,
    temp: Number,
    windSpeed: Number,
    humidity: Number, */
  ],
  mounted() {
    setTimeout(() => {
      console.log(this.savedLocally)
    }, 5000)
  },
}
</script>

<style></style>
