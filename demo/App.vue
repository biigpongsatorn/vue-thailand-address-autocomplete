<template>
  <div id="app">
    <a href="https://github.com/biigpongsatorn/vue-thailand-address-autocomplete">
      <h1>vue-thailand-address-autocomplete</h1>
    </a>
    <ThailandAutoComplete
      v-model="district"
      type="district"
      @select="select"
      label="ตำบล"
      color="#42b883"
      size="small"
      placeholder="ตำบล..."/>
    <ThailandAutoComplete v-model="amphoe" type="amphoe" @select="select" label="อำเภอ" placeholder="อำเภอ..."/>
    <ThailandAutoComplete v-model="province" type="province" @select="select" label="จังหวัด" size="medium" color="#35495e" placeholder="จังหวัด..."/>
    <ThailandAutoComplete v-model="zipcode" type="zipcode" @select="select" size="large" color="#00a4e4" placeholder="รหัสไปรษณีย์..."/> 
  </div>
</template>

<script>
import ThailandAutoComplete from '../src/vue-thailand-address-autocomplete'

export default {
  name: 'app',
  data () {
    return {
      district: '',
      amphoe: '',
      province: '',
      zipcode: ''
    }
  },
  methods: {
    select (address) {
      this.district = address.district
      this.amphoe = address.amphoe
      this.province = address.province
      this.zipcode = address.zipcode
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
  width: 50vw;
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

@media only screen and (max-width: 768px) {
  #app {
    width: 80vw;
    margin: 0 auto;
  }
}
</style>
