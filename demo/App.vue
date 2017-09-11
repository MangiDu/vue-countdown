<template>
  <div id="app">
    <countdown v-show="!isEnd" :now-time="0" :end-time="12" @end="isEnd = true" :frames="24"></countdown><br>
    <span>left </span><countdown :now-time="0" :end-time="60 * 60" mode="seconds"></countdown><span> seconds</span><br>
    <countdown :now-time="0" :end-time="60 * 60 * 24 + 1"></countdown><br>
  </div>
</template>

<script>
import Countdown from '../src/countdown/index'

export default {
  name: 'app',
  components: {
    Countdown
  },
  data () {
    return {
      isEnd: false
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
