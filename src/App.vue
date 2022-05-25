<template>

  <weather-app></weather-app>

</template>

<script>
import WeatherApp from './components/WeatherApp.vue'

export default {
  name: 'App',
  components: {
    WeatherApp
  }
}
</script>

<style>
  *{
    padding: 0;
    margin: 0;
    box-sizing: border-box;
  }
</style>
