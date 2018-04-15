<template>
  <div>
    <input type="text" :value="type">
  </div>
</template>

<script>
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
    }
  },
  mounted () {
  },
  watch: {
    type (val) {
      console.log(val)
    }
  },
  methods: {
  }
}
</script>
