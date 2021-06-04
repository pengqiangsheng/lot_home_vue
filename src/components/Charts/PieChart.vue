<template>
  <el-col :xs="24" :sm="12" :md="8" :lg="6">
    <div class="pie-wrapper card-wrapper" style="height: 220px">
      <div :id="id" :class="className" :style="{height:height,width:width}" @click="dialogVisible = true" />
      <el-dialog
        :title="chartData.name"
        :visible.sync="dialogVisible"
        :width="device === 'mobile' ? '100%' : '50%'"
        :before-close="handleClose"
        :show-close="device != 'mobile'"
        :close-on-click-modal="device === 'mobile'"
      >
        <LineSmoothChart :chart-data="chartData" :color="color" />
      </el-dialog>
    </div>
  </el-col>
</template>

<script>
import * as echarts from 'echarts'
require('echarts/theme/macarons') // echarts theme
import resize from './mixins/resize'
import { randomNumber } from '@/utils'
import LineSmoothChart from './LineSmoothChart'
import { mapGetters } from 'vuex'

export default {
  components: { LineSmoothChart },
  mixins: [resize],
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
    },
    color: {
      type: String,
      default: '#039BE5' // #AFEEEE
    }
  },
  data() {
    return {
      chart: null,
      id: `chart-pie-${randomNumber(1, 999999)}`,
      dialogVisible: false
    }
  },
  computed: {
    ...mapGetters(['device'])
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
    console.log(this.device)
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
      this.setOptions({ ...this.chartData, formatter: this.formatter, maxValue: this.maxValue, color: this.color })
      console.log(this.chartData)
    },
    setOptions({ name, value, formatter, maxValue, color } = {}) {
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
              },
              itemStyle: {
                color
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
