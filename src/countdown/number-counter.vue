<template lang="html">
  <div class="counter-wrapper">
    <div class="place-wrapper">
      <ul class="number-list" :style="{transform: `translateY(${calculatedTop}%)`}">
        <li class="number-item">{{ prev }}</li>
        <li class="number-item">{{ current }}</li>
        <li class="number-item">{{ next }}</li>
      </ul>
    </div>
  </div>
</template>

<script>
// 单个数字
export default {
  props: {
    value: {
      type: Number,
      default: 0
    },
    bit: {
      type: Number,
      default: 1
    },
    maxValue: {
      type: Number,
      default: 10
    },
    frequency: { // 定时器频率
      type: Number,
      default: 16
    }
  },
  data () {
    return {
      calculatedTop: 0,
      startFrom: 0,
      entTo: 0,
      current: 0,
      base: 100, // 百分比，与number-item的height保持一致
      duration: 500,
      speed: 2
    }
  },
  computed: {
    next () {
      if (this.current >= (this.maxValue - 1)) return 0
      return this.current + 1
    },
    prev () {
      if (this.current <= 0) return (this.maxValue - 1)
      return this.current - 1
    }
  },
  watch: {
    value (newValue, oldValue) {
      newValue = Math.floor(newValue / Math.pow(10, this.bit - 1))
      oldValue = Math.floor(oldValue / Math.pow(10, this.bit - 1))
      let getNum = function (number) {
        if (number % 10 === 0 && number !== 0) {
          return 10
        }
        return number % 10
      }
      newValue = getNum(newValue)
      oldValue = getNum(oldValue)
      this.current = oldValue % 10
      this.startFrom = oldValue
      this.endTo = newValue
      this.startTick()
    }
  },
  mounted () {
    if (this.value !== undefined) {
      let value = Math.floor(this.value / Math.pow(10, this.bit - 1))
      this.endTo = value % 10
    } else {
      this.endTo = 0
    }
    // 防止出现设置了值后不tick直接显示目标值的情况
    this.startTick()
  },
  beforeDestroy () {
    if (this._interval) clearTimeout(this._interval)
  },
  methods: {
    startTick () {
      let distance = this.endTo - this.current
      if (distance === 0 || distance === this.maxValue) return
      let direction = 1
      // if (Math.abs(distance) <= 1) {
      //   direction = distance >= 0 ? -1 : 1 // top正值是向下
      // } else {
      //   // 间距不为1的时候就始终从上往下滚
      //   direction = 1
      // }
      if (this._interval) {
        // 为了安全保证，先清掉setInterval
        clearTimeout(this._interval)
      }
      this.calculatedTop = 0
      // 通过容器高度、计时器频率和速度计算步距
      //
      let extent = (this.base / this.frequency) * this.speed
      this._interval = setInterval(() => {
        this.calculatedTop += direction * extent
        if (Math.abs(this.calculatedTop) > this.base) {
          this.current = Math.abs(this.maxValue + this.current + -1 * direction) % this.maxValue
          this.calculatedTop = 0
          clearTimeout(this._interval)
          this.startTick()
        }
      }, Math.round(this.duration / this.frequency / (Math.abs(distance) >= (this.maxValue - 1) ? 1 : Math.abs(distance))))
    }
  }
}
</script>

<style>
.counter-wrapper {
  display: inline-block;
  width: 10px;
  height: 100%;
  margin: 0 auto;
}
.counter-wrapper .place-wrapper {
  position: relative;
  width: 100%;
  height: 100%;
  overflow: hidden;
}
.counter-wrapper .number-list {
  position: absolute;
  display: inline-block;
  top: -100%;
  left: 0;
  height: 100%;
  margin: 0;
  padding: 0;
  list-style: none;
}
.counter-wrapper .number-item {
  display: inline-block;
  width: 100%;
  height: 100%;
  text-align: center;
}
</style>
