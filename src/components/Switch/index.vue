<template>
  <el-col :xs="24" :sm="12" :md="8" :lg="6">
    <div class="switch-wrapper card-wrapper">
      <div class="title-wrapper">灯</div>
      <el-row v-for="item in switchList" :key="item.name"  type="flex" justify="space-between" class="row-wrapper">
        <span>{{ item.name }}</span>
        <el-switch
          v-model="item.value"
          active-color="#039BE5"
          inactive-color="#A9A9A9"
          v-on="$listeners"
        />
      </el-row>
    </div>
  </el-col>
</template>

<script>
export default {
  inheritAttrs: true,
  data: () => ({
    switchList: [
      {
        name: '卧室灯',
        value: true
      },
      {
        name: '客厅灯',
        value: true
      }
    ]
  }),
  watch: {
  },
  mounted() {
    console.log(this.$attrs)
  },
  beforeDestroy() {
    if (!this.chart) {
      return
    }
    this.chart.dispose()
    this.chart = null
  },
  methods: {
  }
}
</script>
<style lang="scss" scoped>
.switch-wrapper {
  padding: 10px;
  .title-wrapper {
    font-size: 30px;
    margin-bottom: 10px;
  }
  .row-wrapper {
    margin: 0 10px 10px 10px;
  }
}
</style>
