<a href="https://www.buymeacoffee.com/biigpongsatorn" target="_blank"><img src="https://www.buymeacoffee.com/assets/img/custom_images/yellow_img.png" alt="Buy Me A Coffee" style="height: auto !important;width: auto !important;" ></a>

<p>
<a href="https://npmjs.com/package/vue-thailand-address-autocomplete"><img src="https://img.shields.io/npm/v/vue-thailand-address-autocomplete.svg" alt="NPM version"></a>
<a href="https://npmjs.com/package/vue-thailand-address-autocomplete"><img src="https://img.shields.io/npm/dm/vue-thailand-address-autocomplete.svg" alt="NPM downloads"></a>
<a href="https://www.npmjs.com/package/vue-thailand-address-autocomplete"><img src="https://img.shields.io/npm/l/vue-thailand-address-autocomplete.svg" alt="License"></a>
<a href="https://www.npmjs.com/package/vue-thailand-address-autocomplete"><img src="https://travis-ci.org/biigpongsatorn/biigpongsatorn.github.io.svg?branch=dev" alt="Demo Build Status"></a>
</p>


# Vue Thailand Address Autocomplete 🇹🇭

Auto complete ที่อยู่ในประเทศไทยแบบสำเร็จรูป

Base on
[jquery.Thailand.js](https://github.com/earthchie/jquery.Thailand.js) and [thai-address-database](https://github.com/Sellsuki/thai-address-database)

Very well database by [@earthchie](https://github.com/earthchie)

# Demo Page

[Documentation & Demo Page](https://biigpongsatorn.github.io/#/vue-thailand-address-autocompleter)

# Install

```sh
npm install vue-thailand-address-autocomplete --save
```
หรือ
```sh
yarn add vue-thailand-address-autocomplete
```

# Usage

```javascript
import ThailandAutoComplete from 'vue-thailand-address-autocomplete'

Vue.component('ThailandAutoComplete', ThailandAutoComplete)
```
หรือ
```javascript
import ThailandAutoComplete from 'vue-thailand-address-autocomplete'

export default {
  components: {
    ThailandAutoComplete
  }
}
```

# Example
<img src="https://raw.githubusercontent.com/biigpongsatorn/vue-thailand-address-autocomplete/HEAD/static/example/ex1.png"/>

```html
<template>
  <div>
    <ThailandAutoComplete
      v-model="district"
      type="district"
      @select="select"
      label="ตำบล"
      color="#42b883"
      size="default"
      placeholder="ตำบล..."/>

    <ThailandAutoComplete v-model="amphoe" type="amphoe" @select="select" label="อำเภอ" placeholder="อำเภอ..."/>

    <ThailandAutoComplete v-model="province" type="province" @select="select" label="จังหวัด" size="medium" color="#35495e" placeholder="จังหวัด..."/>

    <ThailandAutoComplete v-model="zipcode" type="zipcode" @select="select" size="large" color="#00a4e4" placeholder="รหัสไปรษณีย์..."/> 
  </div>
</template>

<script>
export default {
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
  }
}
</script>
```

# Options

## Props
| Props       | Type          | Default   | Description                                                             |
| ----------- |:--------------| ----------|-------------------------------------------------------------------------|
| v-model     | String        | -         | Binding value                                                           |
| type        | String        | -         | ประเภทของ Field ประกอบด้วย `district`, `amphoe`, `province`, `zipcode`   |
| label       | String        | -         | คำอธิบายของ Field                                                        |
| placeholder | String        | -         | placeholder ของ Field                                                   |
| color       | String        | -         | สี border ของ Field ตอน Focus และสี background ของ List                   |
| size        | String        |`default`  | ขนาดของ Field ประกอบด้วย `small`, `default`, `medium`, `large`           |

## Events

| Events      | Type          | Default           | Description                                                                                      |
| ----------- |:--------------| ------------------|--------------------------------------------------------------------------------------------------|
| select      | Function      | (address) => {}   | Return address data (type object) when select address, คืนค่า Object ที่อยู่เมื่อเลือกที่อยู่จาก Autocomplete |

# Contributing
1. Fork this repository.
2. Create new branch with feature name.
3. Run `npm install` and `npm run dev`.
4. Create your feature.
5. Commit and set commit message with feature name.
6. Push your code to your fork repository.
7. Create pull request. 🙂

# Support

```
If you like this project, You can support me with starring ⭐ this project.
```

# License

[MIT](LICENSE)

Developed with ❤️ and ☕️ 
