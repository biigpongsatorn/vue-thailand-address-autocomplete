<p>
<a href="https://npmjs.com/package/vue-thailand-address-autocomplete"><img src="https://img.shields.io/npm/v/vue-thailand-address-autocomplete.svg" alt="NPM version"></a>
<a href="https://npmjs.com/package/vue-thailand-address-autocomplete"><img src="https://img.shields.io/npm/dm/vue-thailand-address-autocomplete.svg" alt="NPM downloads"></a>
<a href="https://www.npmjs.com/package/vue-thailand-address-autocomplete"><img src="https://img.shields.io/npm/l/vue-thailand-address-autocomplete.svg" alt="License"></a>
</p>

# Vue Thailand Address Autocomplete 🇹🇭

ออโต้คอมพลีทที่อยู่ในประเทศไทยแบบสำเร็จรูป.

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
   <ThailandAutoComplete v-model="district" type="district" @select="select" label="ตำบล/แขวง" color="#42b883" size="default" placeholder="ตำบล/แขวง"/>
  </div>
</template>
```

# Options

## Props

> coming soon...

## Events

> coming soon...

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
