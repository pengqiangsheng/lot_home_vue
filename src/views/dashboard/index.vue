<template>
  <div class="dashboard-container">
    <div class="dashboard-text">name: {{ name }}</div>
    <el-row style="background:#fff;padding:16px 16px 0;margin-bottom:32px;">
      <line-chart :chart-data="lineChartData" />
    </el-row>
    <el-row :gutter="10">
      <el-col :xs="12" :sm="12" :md="8" :lg="6">
        <PieChart :chart-data="tempature" />
      </el-col>
      <el-col :xs="12" :sm="12" :md="8" :lg="6">
        <PieChart :chart-data="tempature" />
      </el-col>
      <el-col :xs="12" :sm="12" :md="8" :lg="6">
        <PieChart :chart-data="tempature" />
      </el-col>
      <el-col :xs="12" :sm="12" :md="8" :lg="6">
        <PieChart :chart-data="tempature" />
      </el-col>
      <el-col :xs="12" :sm="12" :md="8" :lg="6">
        <PieChart :chart-data="tempature" />
      </el-col>
    </el-row>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
import LineChart from './LineChart'
import PieChart from '@/components/Charts/PieChart'

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
  components: { LineChart, PieChart },
  data() {
    return {
      lineChartData: lineChartData.newVisitis,
      tempature: {
        name: '温度',
        value: 30
      }
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
  .card-wrapper {
    position: relative;
    display: block;
    height: 200px;
    width: 350px;
    background: var( --ha-card-background, var(--card-background-color, white) );
    border-radius: var(--ha-card-border-radius, 4px);
    box-shadow: var( --ha-card-box-shadow, 0px 2px 1px -1px rgba(0, 0, 0, 0.2), 0px 1px 1px 0px rgba(0, 0, 0, 0.14), 0px 1px 3px 0px rgba(0, 0, 0, 0.12) );
  }
.dashboard {
  &-container {
    margin: 30px;
  }
  &-text {
    font-size: 30px;
    line-height: 46px;
  }
}
</style>
