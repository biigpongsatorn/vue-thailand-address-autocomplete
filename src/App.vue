<template>
  <div id="app">
    <VueThailandAddressAutocomplete/>
  </div>
</template>

<script>
import VueThailandAddressAutocomplete from './components/VueThailandAddressAutocomplete'

export default {
  name: 'App',
  components: {
    VueThailandAddressAutocomplete
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
