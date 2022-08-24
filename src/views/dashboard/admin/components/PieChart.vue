<template>
  <div :class="className" :style="{height:height,width:width}" />
</template>

<script>
import echarts from 'echarts'
require('echarts/theme/macarons') // echarts theme
import resize from './mixins/resize'

export default {
  mixins: [resize],
  props: {
    className: {
      type: String,
      default: 'chart'
    },
    width: {
      type: String,
      default: '100%'
    },
    height: {
      type: String,
      default: '300px'
    }
  },
  data() {
    return {
      chart: null
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
      this.chart = echarts.init(this.$el, 'macarons')

      this.chart.setOption({

        color: ['#40c9c6', '#f4516c', '#f9c956', '#ef6567', '#f9c956', '#75bedc'],

        tooltip: {

          trigger: 'item'

        },

        legend: {

          orient: 'vertical',

          left: 'left'

        },

        series: [

          {

            name: 'Process',

            type: 'pie',

            radius: '50%',

            data: [

              { value: 1048, name: 'Succeed' },

              { value: 735, name: 'Failed' },

              { value: 580, name: 'Warning' }

            ],

            emphasis: {

              itemStyle: {

                shadowBlur: 10,

                shadowOffsetX: 0,

                shadowColor: 'rgba(0, 0, 0, 0.5)'

              }

            }

          }

        ]

      })
    }
  }
}
</script>
