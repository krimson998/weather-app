<template>
  <tr>
    <div
      v-for="(item, index) in savedLocally"
      :key="index"
      class="flex align-center"
    >
      <div class="ml-4">
        <td class="border px-5 py-2">{{ index + 1 }}</td>
        <td class="border px-5 py-2">{{ item.location }}</td>
        <td class="border px-5 py-2">{{ item.timestamp }}</td>
        <td class="border px-5 py-2">{{ item.timezone }}</td>
        <td class="border px-5 py-2">{{ item.description }}</td>
        <td class="border px-5 py-2">{{ Math.floor(item.temp) }}&deg;C</td>
        <td class="border px-5 py-2">{{ item.windSpeed }}km/h</td>
        <td class="border px-5 py-2">{{ item.humidity }}%</td>
      </div>
      <div>
        <button class="mx-8 hover:underline" @click="removeLocation(index)">
          Remove
        </button>
      </div>
    </div>
  </tr>
</template>

<script>
export default {
  props: {
    savedLocally: Array,
  },

  methods: {
    removeLocation(x) {
      this.savedLocally.splice(x, 1)
      const parsed = JSON.stringify(this.savedLocally)
      localStorage.setItem('savedLocally', parsed)
    },
  },
}
</script>

<style></style>
