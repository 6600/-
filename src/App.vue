<template>
  <div class="my-chart">
    <Chart ref="myChart" :opt="mock" :w="800" :h="600"></Chart>
  </div>
</template>

<script>
import 'echarts'
import 'echarts-gl'
import Chart from 'echarts-middleware'
// const WebSocket = require('ws')
export default {
  name: 'app',
  components: {
    Chart
  },
  data () {
    return {
      mock: {
        "title": {
          "text": "2月份运营报告", 
          "textStyle": {
            "fontSize": 18, 
            "fontWeight": 600, 
            "fontFamily": "siyuanheiti_Thin"
          }, 
          "subtext": "100万", 
          "subtextStyle": {
            "fontSize": 16
          }
        }, 
        "tooltip": {
          "show": false
        }, 
        "visualMap": {
          "show": false, 
          "max": 20, 
          "inRange": {
            "color": [
              "#f7acbc", 
              "#ef5b9c", 
              "#f05b72", 
              "#f69c9f", 
              "#f58f98", 
              "#f391a9", 
              "#d71345"
            ]
          }
        }, 
        "xAxis3D": {
          "name": "", 
          "type": "category", 
          "data": [
            "新增收入", 
            "新增建档", 
            "打卡人数", 
            "扫楼数"
          ]
        }, 
        "yAxis3D": {
          "name": "", 
          "type": "category", 
          "data": [
            "星期一", 
            "星期二", 
            "星期三", 
            "星期四"
          ]
        }, 
        "zAxis3D": {
          "name": "", 
          "type": "value"
        }, 
        "grid3D": {
          "boxWidth": 120, 
          "boxDepth": 120, 
          "axisTick": {
            "show": false
          }, 
          "axisLine": {
            "lineStyle": {
              "color": "#ccc", 
              "width": 1
            }
          }, 
          "axisLabel": {
            "textStyle": {
              "color": "#000", 
              "fontSize": 14
            }
          }, 
          "light": {
            "main": {
              "intensity": 1.2, 
              "shadow": true
            }, 
            "ambient": {
              "intensity": 0.7, 
              "shadow": true
            }
          }, 
          "viewControl": {
            "alpha": 15, 
            "beta": 40, 
            "autoRotate": true, 
            "autoRotateAfterStill": 5, 
            "distance": 250
          }
        }, 
        "series": [
          {
            "type": "bar3D", 
            "name": "数量", 
            "data": [
              {
                "value": [
                  0, 
                  0, 
                  5
                ]
              }, 
              {
                "value": [
                  0, 
                  1, 
                  1
                ]
              }, 
              {
                "value": [
                  0, 
                  2, 
                  2
                ]
              }, 
              {
                "value": [
                  0, 
                  3, 
                  8
                ]
              }, 
              {
                "value": [
                  1, 
                  0, 
                  3
                ]
              }, 
              {
                "value": [
                  1, 
                  1, 
                  5
                ]
              }, 
              {
                "value": [
                  1, 
                  2, 
                  1
                ]
              }, 
              {
                "value": [
                  1, 
                  3, 
                  9
                ]
              }, 
              {
                "value": [
                  2, 
                  0, 
                  4
                ]
              }, 
              {
                "value": [
                  2, 
                  1, 
                  8
                ]
              }, 
              {
                "value": [
                  2, 
                  2, 
                  4
                ]
              }, 
              {
                "value": [
                  2, 
                  3, 
                  8
                ]
              }, 
              {
                "value": [
                  3, 
                  0, 
                  6
                ]
              }, 
              {
                "value": [
                  3, 
                  1, 
                  7
                ]
              }, 
              {
                "value": [
                  3, 
                  2, 
                  4
                ]
              }, 
              {
                "value": [
                  3, 
                  3, 
                  9
                ]
              }
            ], 
            "shading": "lambert", 
            "label": {
              "show": true, 
              "distance": 1, 
              "textStyle": {
                "color": "#333", 
                "fontSize": 18, 
                "borderWidth": 0, 
                "borderColor": "none", 
                "backgroundColor": "rgba(255,255,255,0)", 
                "fontFamily": "impact, Simhei"
              }
            }, 
            "itemStyle": {
              "opacity": 1
            }
          }
        ]
      }
    }
  },
  mounted () {
    const ws = new WebSocket('ws://140.143.207.13:2333')
    ws.onmessage = (evt) => {
      const data = JSON.parse(evt.data)
      console.log(data)
      if (data.err === 0) {
        const chart = this.$refs['myChart'].chart
        this.mock.series[0].data = data.data
        chart.setOption(this.mock, true, true)
      }
    }
    ws.onopen = () => {
      setInterval(() => {
        ws.send("needData")
      }, 10000)
    }
  }
}
</script>

<style>
#app, body, html {
  overflow: hidden;
  position: relative;
  width: 100%;
  height: 100%;
}
.my-chart {
  width: 800px;
  height: 600px;
  overflow: hidden;
}
</style>
