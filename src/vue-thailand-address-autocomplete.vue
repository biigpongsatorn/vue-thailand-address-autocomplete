<template>
  <div>
    <input type="text" :value="type">
  </div>
</template>
<template>
  <div id="app">
    ตำบล
    <input v-model="q">
    <ul>
      <li v-for="item in result">
        {{item.district}} » {{item.amphoe}} » {{item.province}} » {{item.zipcode}}
      </li>
    </ul>
  </div>
</template>

<script>
import { searchAddressByDistrict } from 'thai-address-database'

export default {
  name: 'vue-thailand-address-autocomplete',
  props: {
    type: {
      type: String,
      required: true,
      default: 'district'
    }
  },
  data () {
    return {
      q: ''
    }
  },
  computed: {
    result () {
      return searchAddressByDistrict(this.q)
    }
  }
}
</script>
