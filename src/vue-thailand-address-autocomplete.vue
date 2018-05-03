<template>
  <div class="container">
    <div class="label" v-if="label">{{ label }}</div>
    <div class="input-container">
      <input type="text"
      v-model="currentValue"
      :placeholder="placeholder"
      :class="{
        'input-size-small': size === 'small',
        'input-size-default': size === 'default',
        'input-size-medium': size === 'medium',
        'input-size-large': size === 'large'
      }"
      :style="{
        'border': hasFocus ? 'solid 1px ' + currentColor : 'solid 1px #d3d3d3'
      }"
      ref="input"
      class="input"
      @focus="hasFocus = true"
      @blur="hasFocus = false"
      @keydown.up="pressArrow('up')"
      @keydown.down="pressArrow('down')"
      @keydown.enter="pressEnter()">
      <div v-if="resultsFromSearch.length && isOpenListContainer"
      class="list-container"
      :style="{'top': findListContainerPosition() }">
        <div class="list"
        :class="{ 'list-on-focused': itemOnFocus === index }"
        :style="{
          'background-color': itemOnFocus === index ? currentColor : '#fff'
        }"
        v-for="(item, index) in resultsFromSearch"
        :key="index"
        @mouseover="itemOnFocus = index"
        @mouseout="itemOnFocus = -1"
        @click="clickSelectItem(item)">
          <div class="box-item-top" :class="{ 'box-item-top-focused': itemOnFocus === index }">
            <span class="item-first">
              {{item.district}}
            </span>
            <span class="item-second">
              {{item.zipcode}}
            </span>
            <span class="item-third">
              {{item.province}}
            </span>
          </div>
          <div class="box-item-bottom">
            <span class="item-first font-weight-bold">
              {{item.amphoe}}
            </span>
          </div>
        </div>
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
    placeholder: {
      type: String
    },
    color: {
      type: String
    },
    size: {
      type: String,
      default: 'default'
    }
  },
  data () {
    return {
      currentValue: this.value,
      currentColor: this.color || '#0073ff',
      itemOnFocus: 0,
      isOpenListContainer: true,
      hasFocus: false
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
    /**
    * Arrows keys listener.
    */
    pressArrow (direction) {
      if (direction === 'up') {
        this.setInputCursorToLastChar()
        this.itemOnFocus = this.itemOnFocus > 0 ? this.itemOnFocus - 1 : 0
      } else {
        this.itemOnFocus = this.itemOnFocus < this.resultsFromSearch.length - 1 ? this.itemOnFocus + 1 : this.resultsFromSearch.length - 1
      }
    },
    /**
    * Enter button listener.
    */
    pressEnter () {
      this.setSelectedValue(this.resultsFromSearch[this.itemOnFocus])
    },
    /**
    * Event on click item.
    */
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
     /**
     * - หาค่า top ของ Dropwon Container
     */ 
    findListContainerPosition () {
      let top = '36px'
      if (this.size === 'small') {
        top = '27px'
      } else if (this.size === 'medium') {
        top = '45px'
      } else if (this.size === 'large') {
        top = '54px'
      }
      return top
    },
    /**
     * - ทำให้ Cursor ของ input อยู่หลังสุดเสมอ
     */ 
    setInputCursorToLastChar () {
      const len = this.currentValue.length * 2
      setTimeout(() => { this.$refs.input.setSelectionRange(len, len) }, 1)
    },
    /**
     * - Error Log
     */ 
    _errorLog (text) {
      console.error(`[ERROR] vue-thailand-address-autocomplete : ${text}`)
    }
  }
}
</script>

<style scoped>
* {
  box-sizing: border-box;
  font-family: 'Thonburi', Arial;
  font-weight: normal;
  font-style: normal;
  font-stretch: normal;
}
.container {
  float: left;
  width: 100%;
  margin-bottom: .75rem;
}
.input-container {
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
  color: #363636;
  margin-bottom: .75rem;
}
.input {
  float: left;
  width: 100%;
  border-radius: 3px;
  background-color: #ffffff;
  padding: calc(.475em - 1px) .75rem;
  line-height: 1.25;
  letter-spacing: normal;
  text-align: left;
  color: #363636;
  border: solid 1px #d3d3d3;
  box-shadow: inset 0 1px 2px hsla(0,0%,4%,.1);
}
.input:focus{
  outline: none;
  border-radius: 3px;
}
.input-size-small {
  font-size: .75rem;
}
.input-size-default {
  font-size: 1rem;
}
.input-size-medium {
  font-size: 1.25rem;
}
.input-size-large {
  font-size: 1.5rem;
}
.list-container {
  z-index: 999;
  width: 100%;
  position: absolute;
  left: 0;
  padding-top: 4px;
  max-height: 300px;
  overflow: auto;
  background-color: #ffffff;
  border-top: solid 1px #f1f1f1;
  border-radius: 3px;
  box-shadow: 0 2px 3px hsla(0,0%,4%,.1), 0 0 0 1px hsla(0,0%,4%,.1);
}
.list {
  float: left;
  width: 100%;
  padding: 12px;
}
.list-on-focused {
  cursor: pointer;
  color: #fff;
}
.box-item-top {
  color: rgba(0, 0, 0, 0.7);
  font-size: 16px;
  float: left;
  width: 100%;
  line-height: 14px;
}
.box-item-top-focused {
  color: rgba(255, 255, 255, 0.7);
}
.box-item-bottom {
  float: left;
  width: 100%;
  margin-top: 5px;
}
.item-first {
  float: left;
}
.item-second {
  float: right;
}
.item-third {
  float: right;
  margin-right: 15px;
}
.font-weight-bold {
  font-weight: bold;
}
</style>
