<template>
  <div id="app">
    <h1>vue-thailand-address-autocomplete</h1>
    input : {{ input }}
    <ThailandAutoComplete v-model="input" type="district" @select="select"/>
  </div>
</template>

<script>
import ThailandAutoComplete from '../src/vue-thailand-address-autocomplete'

export default {
  name: 'app',
  data () {
    return {
      input: ''
    }
  },
  methods: {
    select (address) {
      console.log(address)
    }
  },
  components: {
    ThailandAutoComplete
  }
}
</script>

<style scoped>
* {
  box-sizing: border-box;
}
h1 {
  font-family: Arial, Helvetica, sans-serif;
}
#app {
  width: 600px;
  margin: 0 auto;
}
.card {
  float: left;
  width: 100%;
  padding: 25px;
  border: 1px solid #ccc;
  text-align: center;
  margin-bottom: 25px;
}
</style>
