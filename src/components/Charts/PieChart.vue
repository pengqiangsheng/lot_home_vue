<template>
  <div class="pie-wrapper">
    <div @click="dialogVisible = true" :id="id" :class="className" :style="{height:height,width:width}" />
    <el-dialog
      :title="chartData.name"
      :visible.sync="dialogVisible"
      width="50%"
      :before-close="handleClose">
      <LineSmoothChart :chartData="chartData"/>
    </el-dialog>
  </div>
</template>

<script>
import * as echarts from 'echarts'
require('echarts/theme/macarons') // echarts theme
import resize from './mixins/resize'
import { randomNumber } from '@/utils'
import LineSmoothChart from './LineSmoothChart'

export default {
  mixins: [resize],
  components: { LineSmoothChart },
  props: {
    className: {
      type: String,
      default: 'chart-pie'
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
      id: `chart-pie-${randomNumber(1, 999999)}`,
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
      this.setOptions({ ...this.chartData, formatter: this.formatter, maxValue: this.maxValue })
      console.log(this.chartData)
    },
    setOptions({ name, value, formatter, maxValue } = {}) {
      this.chart.setOption({
        legend: {
          top: '5%',
          left: '10%'
        },
        series: [{
          type: 'pie',
          radius: ['40%', '70%'],
          startAngle: 0,
          data: [
            {
              value: maxValue,
              itemStyle: {
                normal: {
                  color: 'rgba(0,0,0,0)'
                }
              }
            },
            {
              value,
              name,
              label: {
                show: true,
                formatter,
                textStyle: {
                  color: '#000',
                  bottom: '5%'
                },
                fontSize: '30',
                position: 'center'
              }
            },
            {
              value: maxValue - value,
              itemStyle: {
                normal: {
                  color: '#eeeeee'
                }
              }
            }
          ]
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
.pie-wrapper {
  position: relative;
  display: block;
  height: 220px;
  width: 100%;
  margin-bottom: 10px;
  background: var( --ha-card-background, var(--card-background-color, white) );
  border-radius: var(--ha-card-border-radius, 4px);
  box-shadow: var( --ha-card-box-shadow, 0px 2px 1px -1px rgba(0, 0, 0, 0.2), 0px 1px 1px 0px rgba(0, 0, 0, 0.14), 0px 1px 3px 0px rgba(0, 0, 0, 0.12) );

}
</style>
<style>
  .el-dialog__header {
    padding: 20px;
    border-bottom: 1px solid #eee;
  }
  .el-dialog__body {
    padding: 0 20px 20px 20px;
  }
</style>