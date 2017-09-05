# vue-countdown

> A Vue.js countdown component

## Install

``` bash
npm install vue-countdown-component
```

## Usage
```vue
<template>
  <div>
    <vue-countdown :now-time="0" :end-time="200" @end="onEnd"></vue-countdown>
  </div>
</template>

<script>
import VueCountdown from 'vue-countdown-component'

export default {
  components: {
    VueCountdown
  },
  data () {
    return {}
  },
  methods: {
    onEnd () {
      // do something when countdown ended
    }
  }
}
</script>

<style>
@import "vue-countdown-component/dist/vue-countdown.min.css";
</style>
```

## Props

`nowTime`: unit is second, not required, set current time seconds as default

`endTime`: unit is second, required

## Events

`end`
