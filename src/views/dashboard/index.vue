<template>
  <div class="dashboard-container">
    <el-row :gutter="10">
      <div v-for="item in list" :key="item.key" v-dragging="{ item, list, group: 'list' }">
        <pie-chart
          v-if="item.type === 'pie'"
          :chart-data="item.data"
          :color="item.data.color"
          :formatter="item.data.formatter"
          @del="handleDel(item.key)"
        />
        <my-switch v-if="item.type === 'switch'" />
        <el-col v-if="item.type === 'line'" :span="24">
          <line-chart :chart-data="item.data" />
        </el-col>
        <el-col v-if="item.type === 'span'" :xs="24" :sm="12" :md="8" :lg="6">
          <div class="dashboard-text card-wrapper">name: {{ name }}</div>
        </el-col>
      </div>
      <el-col :xs="24" :sm="12" :md="8" :lg="6">
        <div class="el-upload el-upload--picture-card" @click="dialogVisible = true">
          <i class="el-icon-plus" />
        </div>
      </el-col>
    </el-row>
    <el-dialog
      title="添加卡片"
      :visible.sync="dialogVisible"
      width="50%"
      :before-close="handleClose"
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
        <el-form-item label="值">
          <el-input v-model="form.value" />
        </el-form-item>
        <template v-if="form.type === 'pie'">
          <el-form-item label="单位">
            <el-input v-model="form.formatter" />
          </el-form-item>
          <el-form-item label="颜色">
            <el-color-picker v-model="form.color" />
          </el-form-item>
        </template>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="handleAdd">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
import LineChart from './LineChart'
import PieChart from '@/components/Charts/PieChart'
import MySwitch from '@/components/Switch'
import { randomNumber } from '@/utils'

const lineChartData = {
  newVisitis: {
    expectedData: [100, 120, 161, 134, 105, 160, 165],
    actualData: [120, 82, 91, 154, 162, 140, 145]
  },
  messages: {
    expectedData: [200, 192, 120, 144, 160, 130, 140],
    actualData: [180, 160, 151, 106, 145, 150, 130]
  },
  purchases: {
    expectedData: [80, 100, 121, 104, 105, 90, 100],
    actualData: [120, 90, 100, 138, 142, 130, 130]
  },
  shoppings: {
    expectedData: [130, 140, 141, 142, 145, 150, 160],
    actualData: [120, 82, 91, 154, 162, 140, 130]
  }
}

export default {
  name: 'Dashboard',
  components: { LineChart, PieChart, MySwitch },
  data() {
    return {
      lineChartData: lineChartData.newVisitis,
      list: [
        {
          type: 'span',
          key: 'span1',
          data: this.$store.getters.name
        },
        {
          type: 'line',
          key: '0',
          data: lineChartData.newVisitis
        },
        {
          type: 'pie',
          key: '1',
          data: {
            name: '温度',
            value: 30,
            color: '#039BE5',
            formatter: '{c} ℃'
          }
        },
        {
          type: 'pie',
          key: '2',
          data: {
            name: '温度',
            value: 35,
            color: '#039BE5',
            formatter: '{c} ℃'
          }
        },
        {
          type: 'pie',
          key: '3',
          data: {
            name: '湿度',
            value: 80,
            color: '#AFEEEE',
            formatter: '{c} %'
          }
        },
        {
          type: 'switch',
          key: '4',
          data: {
            name: '灯',
            value: true
          }
        }
      ],
      tempature: {
        name: '温度',
        value: 30
      },
      humitiy: {
        name: '湿度',
        value: 80
      },
      dialogVisible: false,
      form: {
        name: '',
        type: '',
        formatter: '',
        value: '',
        color: '#AFEEEE'
      },
      typeList: [
        {
          label: '扇形图表',
          value: 'pie'
        },
        {
          label: '标签',
          value: 'span'
        },
        {
          label: '开关',
          value: 'switch'
        }
      ]
    }
  },
  computed: {
    ...mapGetters([
      'name'
    ])
  },
  mounted() {
    this.$dragging.$on('dragged', ({ value }) => {
      // console.log(value.item)
      console.log(value.list)
      // console.log(value.otherData)
    })
    this.$dragging.$on('dragend', () => {
      console.log('拖完了')
    })
  },
  updated() {
    // console.log(this.list)
  },
  methods: {
    handleAdd() {
      console.log(this.form)
      this.dialogVisible = false
      this.list.push({
        type: this.form.type,
        key: `${this.form.type}-${this.form.name}-${randomNumber(10, 1000)}`,
        data: {
          name: this.form.name,
          value: this.form.value,
          color: this.form.color,
          formatter: `{c} ${this.form.formatter}`
        }
      })
    },
    handleClose() {

    },
    handleDel(key) {
      console.log(key)
      const index = this.list.findIndex(item => item.key === key)
      console.log(index)
      this.list.splice(index, 1)
    }
  }
}
</script>

<style lang="scss" scoped>
.dashboard {
  &-container {
    margin: 30px;
  }
  &-text {
    font-size: 30px;
    line-height: 46px;
    padding: 10px;
  }
}
</style>
