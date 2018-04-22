<template>
  <div class="container">
    <input type="text" v-model="subDistrict" class="input">
    <div class="list-container" v-if="resultsFromSearchBySubDistrict.length">
      <div class="list" v-for="(item, index) in resultsFromSearchBySubDistrict" :key="index">
        {{item.district}} » {{item.amphoe}} » {{item.province}} » {{item.zipcode}}
      </div>
    </div>
  </div>
</template>

<script>
import { searchAddressByDistrict, searchAddressByAmphoe, searchAddressByProvince, searchAddressByZipcode } from 'thai-address-database'

export default {
  name: 'vue-thailand-address-autocomplete',
  props: {
    type: {
      type: String,
      required: true
    }
  },
  data () {
    return {
      subDistrict: '',
      district: '',
      province: '',
      zipcode: ''
    }
  },
  computed: {
    resultsFromSearchBySubDistrict () {
      return searchAddressByDistrict(this.subDistrict)
    },
    resultsFromSearchByDistrict () {
      return searchAddressByAmphoe(this.q)
    },
    resultsFromSearchByProvince () {
      return searchAddressByProvince(this.q)
    },
    resultsFromSearchByZipcode () {
      return searchAddressByZipcode(this.q)
    }
  }
}
</script>

<style scoped>
* {
  box-sizing: border-box;
  font-family: 'Thonburi';
  font-size: 16px;
  font-weight: normal;
  font-style: normal;
  font-stretch: normal;
}

.container {
  float: left;
  width: 100%;
  position: relative;
}

.input {
  float: left;
  width: 100%;
  height: 50px;
  border-radius: 6px;
  background-color: #ffffff;
  border: solid 1px #d3d3d3;
  padding-left: 10px;
  line-height: 1.25;
  letter-spacing: normal;
  text-align: left;
  color: #333333;
}
.input:focus{
  outline: none;
  border: solid 1px #007ef9;
}

.list-container {
  z-index: 999;
  width: 100%;
  position: absolute;
  top: 50px;
  left: 0;
  border-radius: 6px;
  box-shadow: 0 2px 4px 0 rgba(27, 18, 18, 0.1);
  background-color: #ffffff;
  border: solid 1px #d3d3d3;
}

.list {
  float: left;
  width: 100%;
  padding: 10px;
  border-bottom: solid 2px #f1f1f1;
}
.list:hover {
  cursor: pointer;
  background-color: #0073ff;
  color: #fff;
}
</style>
