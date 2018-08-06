<template>
  <div class="container" v-click-outside="onClickOutside">
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
        'border': hasFocus && currentColor !== '#f5f5f5' ? 'solid 1px ' + currentColor : 'solid 1px #d3d3d3'
      }"
      ref="input"
      class="input"
      @focus="hasFocus = true"
      @blur="hasFocus = false"
      @keydown.up="pressArrow('up')"
      @keydown.down="pressArrow('down')"
      @keydown.enter="pressEnter()">
      <div v-if="resultsFromSearch.length && isOpenListContainer"
      ref="dropdown"
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
          <div class="box-item-top">
            <span class="item-first" :class="{ 'box-item-top-focused': itemOnFocus === index && currentColor !== '#f5f5f5' }">
              {{ itemFirst(item) }}
            </span>
            <div class="float-right">
              <span class="item-second" :class="{ 'box-item-top-focused': itemOnFocus === index && currentColor !== '#f5f5f5' }">
                {{ itemSecond(item) }}
              </span>
              <span class="item-third" :class="{ 'box-item-top-focused': itemOnFocus === index && currentColor !== '#f5f5f5' }">
                {{ itemThird(item) }}
              </span>
            </div>
          </div>
          <div class="box-item-bottom">
            <span class="item-first font-weight-bold" :style="{'color': itemOnFocus === index && currentColor !== '#f5f5f5' ? '#fff' : '#000'}">
              {{ itemFourth(item) }}
            </span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { searchAddressByDistrict, searchAddressByAmphoe, searchAddressByProvince, searchAddressByZipcode } from 'thai-address-database'
import vClickOutside from 'v-click-outside'

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
      currentColor: this.color || '#f5f5f5',
      itemOnFocus: 0,
      isOpenListContainer: false,
      hasFocus: false
    }
  },
  directives: {
    clickOutside: vClickOutside.directive
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
    currentValue (value) {
      this.$emit('input', this.currentValue)
      if (this.hasFocus) {
        this.isOpenListContainer = true
      }
      this.itemOnFocus = 0
    },
    /**
     * - ถ้า v-model เปลี่ยนค่าจากนอก Component
     * - ให้เปลี่ยนค่า Internal value ด้วย 
     * - และซ่อน Dropdown
     */
    value (value) {
      if (value !== this.currentValue) {
        this.currentValue = value
        this.$nextTick(() => this.isOpenListContainer = false)
      }
    },
    /**
     * - When color is changed: set internal value.
     */
    color (val) {
      this.currentColor = val
    }
  },
  methods: {
    onClickOutside (event) {
      this.isOpenListContainer = false
    },
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
      this.moveScrollOfListContainer()
    },
    /**
    * Enter button listener.
    */
    pressEnter () {
      this.setSelectedValue(this.resultsFromSearch[this.itemOnFocus])
    },
    /**
    * เลื่อน scroll bar ตาม Item ที่เลือก
    */
    moveScrollOfListContainer () {
      const list = this.$refs.dropdown
      const element = list.querySelectorAll('.list')[this.itemOnFocus]
      if (!element) return
      const visMin = list.scrollTop
      const visMax = list.scrollTop + list.clientHeight - element.clientHeight
      if (element.offsetTop < visMin) {
        list.scrollTop = element.offsetTop
      } else if (element.offsetTop >= visMax) {
        list.scrollTop = (
          element.offsetTop -
          list.clientHeight +
          element.clientHeight
        )
      }
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
     * - แสดงข้อมูลซ้ายบน
     */ 
    itemFirst (address) {
      return this.type === 'district' ? address['amphoe'] : address['district']
    },
    /**
     * - แสดงข้อมูลตรงกลางบน
     */ 
    itemSecond (address) {
      return this.type === 'amphoe' || this.type === 'district' ? address['province'] : address['amphoe']
    },
    /**
     * - แสดงข้อมูลขวาบน
     */ 
    itemThird (address) {
      return this.type === 'province' || this.type === 'amphoe' || this.type === 'district' ? address['zipcode'] : address['province']
    },
    /**
     * - แสดงข้อมูลล่างซ้าย
     */ 
    itemFourth (address) {
      return address[this.type]
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
  outline: none !important;
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
}
.box-item-top {
  color: rgba(0, 0, 0, 0.7);
  font-size: 16px;
  float: left;
  width: 100%;
  line-height: 14px;
}
.box-item-top-focused {
  color: rgba(255, 255, 255, 0.9);
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
  float: left;
  margin-right: 15px;
}
.item-third {
  float: left;
}
.float-right {
  float: right;
}
.font-weight-bold {
  font-weight: bold;
}
</style>
