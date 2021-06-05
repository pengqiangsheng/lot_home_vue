<template>
  <el-col :xs="24" :sm="12" :md="8" :lg="6">
    <div class="pie-wrapper card-wrapper" style="height: 220px">
      <div :id="id" :class="className" :style="{height:height,width:width}" @click="dialogVisible = true" />
      <div class="icon-wrapper">
        <i class="el-icon-edit" @click="editSwitch = true" />
        <i class="el-icon-delete" @click="handleDel" />
      </div>
    </div>
    <!-- 折线图 -->
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
    <!-- 编辑 -->
    <!-- <el-dialog
      :title="'编辑'"
      :visible.sync="editSwitch"
      :width="device === 'mobile' ? '100%' : '50%'"
      :before-close="handleClose"
      :show-close="device != 'mobile'"
      :close-on-click-modal="device === 'mobile'"
    >
      <el-form ref="form" :model="form" label-width="80px">
        <el-form-item label="名称">
          <el-input v-model="form.name" />
        </el-form-item>
        <el-form-item label="类型">
          <el-select v-model="form.type" placeholder="请选择卡片类型">
            <el-option
              v-for="item in typeList"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            />
          </el-select>
        </el-form-item>
      </el-form>
    </el-dialog> -->
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
      dialogVisible: false,
      editSwitch: false
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
    },
    handleEdit() {

    },
    handleDel() {
      this.$emit('del')
    }
  }
}
</script>
<style lang="scss" scoped>
.icon-wrapper {
  position: absolute;
  top: 10px;
  right: 5px;
  i {
    font-size: 20px;
    margin-right: 5px;
    &:hover {
      cursor: pointer;
    }
  }
}
</style>
