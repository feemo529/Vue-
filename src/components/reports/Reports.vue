<template>
  <div>
    <!-- 面包屑导航区域 -->
    <Breadcrumb name1="数据统计" name2="数据报表" />
    <!-- 卡片 -->
    <el-card>
      <div id="main" style="width: 760px;height:400px;"></div>
    </el-card>
  </div>
</template>

<script>
import Breadcrumb from '../Breadcrumb.vue'
import * as echarts from 'echarts'
import _ from 'lodash'
export default {
  name: 'Reports',
  components: { Breadcrumb },
  data() {
    return {
      // 需要合并的数据
      options: {
        title: {
          text: '用户来源'
        },
        tooltip: {
          trigger: 'axis',
          axisPointer: {
            type: 'cross',
            label: {
              backgroundColor: '#E9EEF3'
            }
          }
        },
        grid: {
          left: '3%',
          right: '4%',
          bottom: '3%',
          containLabel: true
        },
        xAxis: [
          {
            boundaryGap: false
          }
        ],
        yAxis: [
          {
            type: 'value'
          }
        ]
      }
    }
  },
  created() {},
  async mounted() {
    // 初始化图表
    var myChart = echarts.init(document.getElementById('main'))

    const { data: res } = await this.$http.get('reports/type/1')
    if (res.meta.status !== 200) return this.$message.error('获取折线图失败!')

    // 准备数据和配置项(深拷贝)
    const result = _.merge(res.data, this.options)
    myChart.setOption(result)
  },
  methods: {}
}
</script>

<style></style>
