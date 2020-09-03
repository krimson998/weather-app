<template>
  <div class="flex flex-col">
    <tr v-for="(item, index) in savedLocally" :key="item.location" class="flex">
      <div>
        <td class="border px-4 py-2">{{ index + 1 }}</td>
        <td class="border px-4 py-2">{{ item.location }}</td>
        <td class="border px-4 py-2">{{ item.timestamp }}</td>
        <td class="border px-4 py-2">{{ item.timezone }}</td>
        <td class="border px-4 py-2">{{ item.description }}</td>
        <td class="border px-4 py-2">{{ Math.floor(item.temp) }}&deg;C</td>
        <td class="border px-4 py-2">{{ item.windSpeed }}km/h</td>
        <td class="border px-4 py-2">{{ item.humidity }}%</td>
      </div>

      <button class="ml-8" @click="removeLocation(index)">Remove</button>
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
  methods: {
    removeLocation(x) {
      this.savedLocally.splice(x, 1)
    },
  },
}
</script>

<style></style>
