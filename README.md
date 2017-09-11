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

`nowTime`: (Number) unit is second, not required, set current time seconds as default

`endTime`: (Number) unit is second, required

`mode`: (String) `default` or `seconds`, seconds only show seconds counts

`dayText`: (Object) `{plural: 'days', singular: 'day'}`

`frames`: (Number) 1 - 24, default 16

## Events

`end`
