<template>
  <div class="line-smooth-wrapper">
    <div :id="id" :class="className" :style="{height:height,width:width}" />
  </div>
</template>

<script>
import * as echarts from 'echarts'
require('echarts/theme/macarons') // echarts theme
import resize from './mixins/resize'
import { randomNumber } from '@/utils'

export default {
  mixins: [resize],
  props: {
    className: {
      type: String,
      default: 'chart-line-smooth'
    },
    width: {
      type: String,
      default: '100%'
    },
    height: {
      type: String,
      default: '350px'
    },
    autoResize: {
      type: Boolean,
      default: true
    },
    chartData: {
      type: Object,
      required: true
    },
    formatter: {
      type: String,
      default: '{c} ℃'
    },
    maxValue: {
      type: Number,
      default: 100
    }
  },
  data() {
    return {
      chart: null,
      id: `chart-line-smooth-${randomNumber(1, 999999)}`,
      dialogVisible: false
    }
  },
  watch: {
    chartData: {
      deep: true,
      handler(val) {
        this.setOptions(val)
      }
    }
  },
  mounted() {
    this.$nextTick(() => {
      this.initChart()
    })
  },
  beforeDestroy() {
    if (!this.chart) {
      return
    }
    this.chart.dispose()
    this.chart = null
  },
  methods: {
    initChart() {
      this.chart = echarts.init(document.getElementById(this.id))
      this.setOptions({ ...this.chartData })
      console.log(this.chartData)
    },
    setOptions({ historyData } = {}) {
      this.chart.setOption({
        xAxis: {
          type: 'category',
          data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
        },
        yAxis: {
          type: 'value'
        },
        series: [{
          data: [34, 27, 30, 35, 33, 32, 31],
          type: 'line',
          smooth: true
        }]
    })
    },
    handleClose() {
      this.dialogVisible = false
    }
  }
}
</script>

<style lang="scss" scoped>
.line-smooth-wrapper {

}
</style>