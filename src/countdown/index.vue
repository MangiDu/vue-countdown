<template lang="html">
  <div class="count-down-container">
    <div class="left-time">
      <template v-if="mode !== 'seconds'">
        <span class="counter">
          <template v-for="(value, index) in days">
            <number-counter :value="Number(value)"></number-counter>
          </template>
        </span>
        <span class="day-time">
          <template v-if="dayText">
            {{ Number(days) > 1 ? dayText.plural : dayText.singular }}
          </template>
        </span>
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
      </template>
      <span class="counter">
        <template v-for="(value, index) in seconds">
          <number-counter :value="Number(value)" :max-value="(mode !== 'seconds' && index === 0) ? 6 : 10"></number-counter>
        </template>
      </span>
    </div>
  </div>
</template>

<script>
import NumberCounter from './number-counter'

const MINUTE_SECONDS = 60
const HOUR_MINUTES = 60
const DAY_HOURS = 24

const HOUR_SECONDS = MINUTE_SECONDS * HOUR_MINUTES
const DAY_SECONDS = HOUR_SECONDS * DAY_HOURS

const SECOND_MILIS = 1000

export default {
  props: {
    endTime: {
      type: Number,
      required: true
    },
    nowTime: {
      type: Number,
      default: Math.floor(Date.now() / SECOND_MILIS)
    },
    mode: {
      type: String,
      default: 'default' // seconds
    },
    dayText: {
      type: Object
    }
  },
  data () {
    return {
      modeArr: [],
      now: null,
      isClosed: true
    }
  },
  computed: {
    leftSeconds () {
      return Math.max(this.endTime - this.now, 0)
    },
    days () {
      let result = ''
      if (this.now !== null) {
        if (this.hours === 0 && this.minutes === 0) {
          result = Math.ceil(this.leftSeconds / DAY_SECONDS)
        } else {
          result = Math.floor(this.leftSeconds / DAY_SECONDS)
        }
      }
      return this.getPadZeroStr(result)
    },
    hours () {
      let result = ''
      if (this.now !== null) {
        if (this.minutes === 0) {
          result = Math.ceil(this.leftSeconds / HOUR_SECONDS)
        } else {
          result = Math.floor(this.leftSeconds / HOUR_SECONDS)
        }
        result %= DAY_HOURS
      }
      return this.getPadZeroStr(result)
    },
    minutes () {
      let result = ''
      if (this.now !== null) {
        result = Math.floor(this.leftSeconds / MINUTE_SECONDS)
        result %= MINUTE_SECONDS
      }
      return this.getPadZeroStr(result)
    },
    seconds () {
      let result = ''
      if (this.now !== null) {
        result = Math.floor(this.leftSeconds)
        if (this.mode !== 'seconds') result %= MINUTE_SECONDS
      }
      return this.getPadZeroStr(result)
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
  padding: 0 2px;
  border: 1px solid rgba(0, 0, 0, 0.2);
  border-radius: 4px;
  background-color: #f8f8f8;
  text-align: center;
  line-height: 24px;
}
.day-time {
  display: inline-block;
  min-width: 10px;
}
</style>
