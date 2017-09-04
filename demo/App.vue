<template>
  <div id="app">
    <countdown :now-time="0" :end-time="60 + 1"></countdown><br>
    <countdown :now-time="0" :end-time="60 * 60 + 1"></countdown><br>
    <countdown :now-time="0" :end-time="60 * 60 * 24 + 1"></countdown><br>
    <countdown :now-time="0" :end-time="60 * 60 * 24 * 10 + 1"></countdown><br>
  </div>
</template>

<script>
import Countdown from '../src/countdown/index'

export default {
  name: 'app',
  components: {
    Countdown
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
