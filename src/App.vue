<template>
  <div id="app">
    <TimezoneApp/>
  </div>
</template>

<script>
import TimezoneApp from './components/TimezoneApp.vue'

export default {
  name: 'App',
  components: {
    TimezoneApp
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
