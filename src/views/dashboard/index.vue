<template>
  <div class="dashboard-container">
    <div class="dashboard-text card-wrapper">name: {{ name }}</div>
    <el-row class="card-wrapper" style="background:#fff;padding:16px 16px 0;margin-bottom:32px;">
      <line-chart :chart-data="lineChartData" />
    </el-row>
    <el-row :gutter="10">
      <PieChart :chart-data="tempature" />
      <PieChart :chart-data="tempature" />
      <PieChart :chart-data="humitiy" :color="'#AFEEEE'" />
      <PieChart :chart-data="tempature" />
      <PieChart :chart-data="tempature" />
    </el-row>
    <el-row :gutter="10">
      <MySwitch v-model="value" />
    </el-row>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
import LineChart from './LineChart'
import PieChart from '@/components/Charts/PieChart'
import MySwitch from '@/components/Switch'

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
      tempature: {
        name: '温度',
        value: 30
      },
      humitiy: {
        name: '湿度',
        value: 80
      },
      value: true
    }
  },
  computed: {
    ...mapGetters([
      'name'
    ])
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
