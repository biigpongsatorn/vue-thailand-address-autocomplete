<template>
  <div class="container">
    <div class="label" v-if="label">{{ label }}</div>
    <input type="text"
    v-model="currentValue"
    class="input"
    @keydown.up="pressArrowUp()"
    @keydown.down="pressArrowDown()"
    @keydown.enter="pressEnter()">
    <div class="list-container" v-if="resultsFromSearch.length && isOpenListContainer">
      <div class="list"
      :style="{ 'backgroundColor': itemOnFocus === index ? currentColor : '#fff' }"
      :class="{ 'list-on-focus': itemOnFocus === index }"
      v-for="(item, index) in resultsFromSearch"
      :key="index"
      @mouseover="itemOnFocus = index"
      @mouseout="itemOnFocus = -1"
      @click="clickSelectItem(item)">
        {{item.district}} > {{item.amphoe}} > {{item.province}} > {{item.zipcode}}
      </div>
    </div>
  </div>
</template>

<script>
import { searchAddressByDistrict, searchAddressByAmphoe, searchAddressByProvince, searchAddressByZipcode } from 'thai-address-database'

export default {
  name: 'VueThailandAddressAutocomplete',
  props: {
    value: {
      type: [String, Number]
    },
    type: {
      type: String
    },
    label: {
      type: String
    },
    color: {
      type: String
    }
  },
  data () {
    return {
      currentValue: this.value,
      currentColor: this.color || '#0073ff',
      itemOnFocus: 0,
      isOpenListContainer: true
    }
  },
  computed: {
    resultsFromSearch () {
      if (this.type) {
        if (this.type === 'district') return this.resultsFromSearchByDistrict
        else if (this.type === 'amphoe') return this.resultsFromSearchByAmphoe
        else if (this.type === 'province') return this.resultsFromSearchByProvince
        else if (this.type === 'zipcode') return this.resultsFromSearchByZipcode
      } else {
        this._errorLog('type is undefined.')
        return []
      }
    },
    resultsFromSearchByDistrict () {
      return searchAddressByDistrict(this.currentValue)
    },
    resultsFromSearchByAmphoe () {
      return searchAddressByAmphoe(this.currentValue)
    },
    resultsFromSearchByProvince () {
      return searchAddressByProvince(this.currentValue)
    },
    resultsFromSearchByZipcode () {
      return searchAddressByZipcode(this.currentValue)
    }
  },
  watch: {
    /**
     * - Update v-model of user.
     * - Set ให้ List Container แสดงเมื่อ currentValue มีการเปลี่ยนแปลง
     * - Set itemOnFocus = first item.
     */
    currentValue () {
      this.$emit('input', this.currentValue)
      this.isOpenListContainer = true
      this.itemOnFocus = 0
    },
    /**
     * - When v-model is changed: set internal value.
     */
    value (value) {
      this.currentValue = value
    },
    /**
     * - When color is changed: set internal value.
     */
    color (val) {
      this.currentColor = val
    }
  },
  methods: {
    pressArrowUp () {
      if (this.itemOnFocus > 0) {
        this.itemOnFocus = this.itemOnFocus - 1
      } else {
        this.itemOnFocus = this.resultsFromSearch.length - 1
      }
    },
    pressArrowDown () {
      if (this.itemOnFocus < this.resultsFromSearch.length - 1) {
        this.itemOnFocus = this.itemOnFocus + 1
      } else {
        this.itemOnFocus = 0
      }
    },
    pressEnter () {
      this.setSelectedValue(this.resultsFromSearch[this.itemOnFocus])
    },
    clickSelectItem (address) {
      this.setSelectedValue(address)
    },
     /**
     * - emit ค่าที่เลือกกลับไปยังฟังชั่นของ user
     * - Set currentValue = ข้อมูลที่ user เลือกไว้
     * - ซ่อน List Container
     */
    setSelectedValue (address) {
      this.$emit('select', address)
      this.type ? this.currentValue = address[this.type] : this._errorLog('type is undefined.')
      this.$nextTick(() => this.isOpenListContainer = false)
    },
    _errorLog (text) {
      console.error(`[ERROR] vue-thailand-address-autocomplete : ${text}`)
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

.label {
  float: left;
  width: 100%;
  line-height: 1.25;
  letter-spacing: normal;
  text-align: left;
  color: #333333;
  margin-bottom: 10px; 
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
  top: 80px;
  left: 0;
  border-radius: 6px;
  box-shadow: 0 2px 4px 0 rgba(27, 18, 18, 0.1);
  background-color: #ffffff;
  border: solid 1px #d3d3d3;
  border-top: 0;
  overflow: hidden;
}

.list {
  float: left;
  width: 100%;
  padding: 10px;
  border-bottom: solid 2px #f1f1f1;
}
.list-on-focus {
  cursor: pointer;
  color: #fff;
}
</style>
