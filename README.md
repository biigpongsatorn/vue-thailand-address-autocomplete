<p>
<a href="https://npmjs.com/package/vue-thailand-address-autocomplete"><img src="https://img.shields.io/npm/v/vue-thailand-address-autocomplete.svg" alt="NPM version"></a>
<a href="https://npmjs.com/package/vue-thailand-address-autocomplete"><img src="https://img.shields.io/npm/dm/vue-thailand-address-autocomplete.svg" alt="NPM downloads"></a>
<a href="https://www.npmjs.com/package/vue-thailand-address-autocomplete"><img src="https://img.shields.io/npm/l/vue-thailand-address-autocomplete.svg" alt="License"></a>
</p>

# Vue Thailand Address Autocomplete 🇹🇭

Auto complete ที่อยู่ในประเทศไทยแบบสำเร็จรูป, Base on [thai-address-database](https://github.com/Sellsuki/thai-address-database)

# ตัวอย่าง

> coming soon...

# ติดตั้ง

```sh
npm install vue-thailand-address-autocomplete --save
```
หรือ
```sh
yarn add vue-thailand-address-autocomplete
```

# การใช้งาน

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

# การนำไปใช้
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

    <ThailandAutoComplete v-model="amphoe" type="amphoe" @select="select" label="อำเภอ"/>

    <ThailandAutoComplete v-model="province" type="province" @select="select" label="จังหวัด"/>

    <ThailandAutoComplete v-model="zipcode" type="zipcode" @select="select" label="รหัสไปรษณีย์"/> 
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

# ทดลอง Run Demo.

``` sh
# clone this repository
git clone https://github.com/biigpongsatorn/vue-thailand-address-autocomplete.git

# cd to repository
cd vue-thailand-address-autocomplete

# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build
```
# License

[MIT](LICENSE)

Developed with ❤️ and ☕️  
Maintained by [@biigpongsatorn](https://github.com/biigpongsatorn)
