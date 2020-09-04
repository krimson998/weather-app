<template>
  <v-app id="inspire">
    <v-app-bar app color="indigo" dark>
      <v-toolbar-title>Weather App</v-toolbar-title>
    </v-app-bar>
    <Nuxt />
    <v-footer color="indigo" app>
      <span class="white--text">&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {}
</script>
