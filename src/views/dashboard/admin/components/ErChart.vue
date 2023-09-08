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
      // 如何设置样式 https://echarts.apache.org/zh/option.html#series-graph.data.label.fontSize
      this.chart = echarts.init(this.$el, 'macarons')
      this.chart.showLoading() // 开启 loading 效果
      this.chart.hideLoading() // 隐藏 loading 效果
      // 这里可以调用api，把数据设置到data里从而实现动态加载数据
      this.chart.setOption({
        title: {
          text: 'Basic Graph'
        },
        tooltip: {
          enterable: true, // 鼠标是否可进入提示框浮层中
          // triggerOn: 'click', // 提示框触发的条件,'click'鼠标点击时触发
          trigger: 'item'
          // 鼠标悬浮时，标签可视化
          // formatter: function(params) {
          //   var key
          //   let html = ''
          //   for (key in params.data) {
          //     html = html + '<br class="select">' + key + ':' + params.data[key] + '<br>'
          //   }
          //   return '<p class="select">' + html + '</p>'
          // }
        },
        animationDurationUpdate: 1500,
        animationEasingUpdate: 'quinticInOut',
        series: [
          {
            type: 'graph',
            layout: 'none',
            symbolSize: 50,
            roam: true,
            label: {
              show: true
            },
            edgeSymbol: ['circle', 'arrow'],
            edgeSymbolSize: [4, 10],
            edgeLabel: {
              fontSize: 20
            },
            data: [
              {
                name: 'Node 1',
                x: 300,
                y: 300
              },
              {
                name: 'Node 2',
                x: 800,
                y: 300
              },
              {
                name: 'Node 3',
                x: 550,
                y: 100
              },
              {
                name: 'Node 4',
                x: 550,
                y: 500
              }
            ],
            // links: [],
            links: [
              {
                source: 0,
                target: 1,
                symbolSize: [5, 20],
                label: {
                  show: true
                },
                lineStyle: {
                  width: 5,
                  curveness: 0.2
                }
              },
              {
                source: 'Node 2',
                target: 'Node 1',
                label: {
                  show: true
                },
                lineStyle: {
                  curveness: 0.2
                }
              },
              {
                source: 'Node 1',
                target: 'Node 3'
              },
              {
                source: 'Node 2',
                target: 'Node 3'
              },
              {
                source: 'Node 2',
                target: 'Node 4'
              },
              {
                source: 'Node 1',
                target: 'Node 4'
              }
            ],
            lineStyle: {
              opacity: 0.9,
              width: 2,
              curveness: 0
            }
          }
        ]
      })
    }
  }
}
</script>

<style>
// 可以选中悬浮框内的文本
.select {
  -webkit-user-select: text !important;
  user-select: text !important;
}
*{
  -webkit-user-select: text !important;
  user-select: text !important;
}
</style>
