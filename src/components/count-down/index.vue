<template lang="html">
  <div class="count-down-container">
    <div class="left-time">
      <span class="counter">
        <template v-for="(value, index) in days">
          <number-counter :value="Number(value)"></number-counter>
        </template>
      </span>
      <span>天</span>
      <span class="counter">
        <template v-for="(value, index) in hours">
          <number-counter :value="Number(value)" :max-value="index === 0 ? 3 : 10"></number-counter>
        </template>
      </span>
      <span>:</span>
      <span class="counter">
        <template v-for="(value, index) in minutes">
          <number-counter :value="Number(value)" :max-value="index === 0 ? 6 : 10"></number-counter>
        </template>
      </span>
      <span>:</span>
      <span class="counter">
        <template v-for="(value, index) in seconds">
          <number-counter :value="Number(value)" :max-value="index === 0 ? 6 : 10"></number-counter>
        </template>
      </span>
    </div>
  </div>
</template>

<script>
import NumberCounter from './number-counter'
export default {
  props: {
    endTime: {
      type: Number,
      required: true
    },
    nowTime: {
      type: Number,
      default: Math.floor(Date.now() / 1000)
    }
  },
  data () {
    return {
      now: null,
      upsideDown: false,
      isClosed: true
    }
  },
  computed: {
    days () {
      let result = ''
      if (this.now !== null) {
        if (this.hours === 0 && this.minutes === 0) {
          result = Math.ceil((this.endTime - this.now) / (60 * 60 * 24))
        } else {
          result = Math.floor((this.endTime - this.now) / (60 * 60 * 24))
        }
      }
      return this.getPadZeroStr(result)
    },
    hours () {
      let result = ''
      if (this.now !== null) {
        if (this.minutes === 0) {
          result = Math.ceil((this.endTime - this.now) / (60 * 60)) % 24
        } else {
          result = Math.floor((this.endTime - this.now) / (60 * 60)) % 24
        }
      }
      return this.getPadZeroStr(result)
    },
    minutes () {
      let result = ''
      if (this.now !== null) {
        result = Math.floor((this.endTime - this.now) / 60) % 60
      }
      return this.getPadZeroStr(result)
    },
    seconds () {
      let result = ''
      if (this.now !== null) {
        result = Math.floor((this.endTime - this.now) % 60)
      }
      return this.getPadZeroStr(result)
    }
  },
  watch: {
    minutes () {
      this.upsideDown = !this.upsideDown
    }
  },
  mounted () {
    this.now = this.nowTime
    if (this.endTime > this.now) {
      this.isClosed = false
      this._interval = setInterval(() => {
        this.now = this.now + 1
        if (this.now >= this.endTime) {
          clearTimeout(this._interval)
          this.isClosed = true
          // NOTE
          // 因为是时间变了才开始变化动画的
          // 所以当时间已经为0，倒计时刚好要由1向0变化
          // 在感官体验上真实的结束时间略微早于倒计时的结束时间
          this.$emit('end')
        }
      }, 1000)
    }
  },
  beforeDestroy () {
    if (this._interval) clearTimeout(this._interval)
  },
  methods: {
    getPadZeroStr (value = '') {
      value = String(value).padStart(2, '0')
      return value
    }
  },
  components: {
    NumberCounter
  }
}
</script>

<style scoped>
.count-down-container {
  display: inline-block;
  height: 32px;
}
.left-time {
  display: inline-block;
}

.left-time .counter {
  position: relative;
  top: 4px;
}
.left-time span:not(.counter) {
  position: relative;
  top: -2px;
}

.counter {
  position: relative;
  top: 2px;
  display: inline-block;
  min-width: 24px;
  height: 24px;
  border: 1px solid rgba(0, 0, 0, 0.2);
  border-radius: 4px;
  background-color: #f8f8f8;
  text-align: center;
  line-height: 24px;
}
</style>
