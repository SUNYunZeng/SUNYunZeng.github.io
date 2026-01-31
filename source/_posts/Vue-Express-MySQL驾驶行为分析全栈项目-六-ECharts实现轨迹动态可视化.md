---
title: 'Vue + Express + MySQL驾驶行为分析全栈项目(六): ECharts实现轨迹动态可视化'
comments: true
toc: true
date: 2019-12-23 13:46:46
categories: 教程
tags: Vue
---

# 背景

自己做了一点点的小尝试：基于前馈神经网络 LSTM 的个体出行目的地预测模型，基于个体历史出行数据，模型可以实现出行目的地的实时动态预测功能。

模型其实具有实际应用功能，为了对其应用场景进行探索，拟开发一个全栈的项目，在Web客户端实现用户出行的动态预测效果，同时能够提供数据可视分析等功能。

**[项目地址](https://github.com/SUNYunZeng/AIforDriving)**

**[可视化效果](http://geoai.sunyunzeng.cn/)**

轨迹的动态可视化预期效果可以参考: https://www.echartsjs.com/examples/zh/editor.html?c=lines-bmap-effect

# ECharts准备

首先安装EChart组件

```javascript
npm install echarts --save
```
然后在 Vue 的 **main.js** 文件中引入百度地图依赖

```javascript
require('echarts/extension/bmap/bmap');
```

# 封装百度地图轨迹可视化模块

我们将百度地图轨迹可视化单独包装成一个模块 EChartsMap.vue，该模块作用是接收父模块传来的可视化数据及设置参数，实现对应的可视化效果。

首先创建界面模版：

```html
<template>
    <div class="bmap"></div>
</template>

<style scoped>
.bmap{
  width: 100%;
  height: calc(78vh);
  position: relative;
}
</style>
```

然后在 script 标签中引入逻辑代码

```javascript
 import store from '@/store';
 // 引入ECharts组件
  let echarts = require('echarts');
  export default {
    name: 'EChartsMap',
    props: ['option'],
    watch:{
      option: function (option) {
        this.myChart.setOption(option);
      }
    },
    data(){
      return {
        myChart: null
      }
    },
    mounted(){
      this.draw();
    },
    methods: {
      draw () {
        this.myChart = echarts.init(this.$el);
        this.myChart.setOption({
          bmap:{
            center: [114.03, 22.32],
            zoom: 10,
            roam: true,
            mapStyle: store.state.mapStyle
          }});
      },
    }
  };
```

这样子，我们父组件通过子组件的 option 属性进行 ECharts 可视化的配置。

然后子模块通过设置watch监听，实现可视化效果的随父模块数据的动态更新。

我们的ECharts需要在模版渲染成HTML界面后引入，所以在 **mounted()** 函数中使用。

# 轨迹动态可视化

首先我们通过样例：https://www.echartsjs.com/examples/zh/editor.html?c=lines-bmap-effect 知道轨迹动态可视化的数据组织形式:

```javascript
    busLines = [{coords: [[114,25],[115,26], [117,28]...[lng, lat]],
            lineStyle: {
                normal: {
                    color: echarts.color.modifyHSL('#5A94DF')
                }
            }},
            {coords: [[114,25],[115,26], [117,28]...[lng, lat]],
            lineStyle: {
                normal: {
                    color: echarts.color.modifyHSL('#5A94DF')
                }
            }}]
    myChart.setOption(option = {
        // 百度底图设置
        bmap: {
            center: [116.46, 39.92],
            zoom: 10,
            roam: true,
            mapStyle: {
              'styleJson': [
                ...]
            }
        },
        series: [
            // 基础线条设置
            {
            type: 'lines',
            coordinateSystem: 'bmap',
            polyline: true,
            data: busLines,
            silent: true,
            lineStyle: {
                normal: {
                    opacity: 0.2,
                    width: 1
                }
            },
            progressiveThreshold: 500,
            progressive: 200
        }, 
        // 线上动态点效果设置
        {
            type: 'lines',
            coordinateSystem: 'bmap',
            polyline: true,
            data: busLines,
            lineStyle: {
                normal: {
                    width: 0
                }
            },
            effect: {
                constantSpeed: 20,
                show: true,
                trailLength: 0.1,
                symbolSize: 1.5
            },
            zlevel: 1
        }]
    });
});
```

通过数据分析发现，我们的轨迹数据只要组织成 busLines (其中设定了轨迹点序列、及轨迹颜色)，然后在ECharts 底图设定 **bmap** 及数据设定 **series** 里面将 **基础线条设置** 与 **线上动态点效果设置** 按照自己喜欢的样式设计好即可。

然后在父模块中先引入EChartMap.vue模块

```html
<template>
  <div>
    <Button type="primary" @click="handleSubmit">展示</Button>
    <EChartsMap :option="option"></EChartsMap>
  </div>
</template>

<script>
  import EChartsMap from '@/components/commom/EChartsMap';

  export default {
    name: 'prediction',
    data () {
      return {
        bmap: {
          center: this.$store.state.mapconfig.center,
          boundingCoords: [],
          zoom: 15,
          roam: true,
          mapStyle: store.state.mapStyle
        },
        option: {
          bmap: this.bmap,
          tooltip: {
            trigger: 'item'
          },
          series: []
        },
        myChart: null,
        lines: {
          coords: [],
          lineStyle: {
            normal: {
              color: 'orange'
            }
          }
        },
        lines_option: {
          type: 'lines',
          coordinateSystem: 'bmap',
          polyline: true,
          data: [],
          silent: true,
          lineStyle: {
            normal: {
              opacity: 0.75,
              width: 3
            }
          },
          progressiveThreshold: 500,
          progressive: 200
        },
        lines_state: {
          type: 'lines',
          coordinateSystem: 'bmap',
          polyline: true,
          data: [],
          lineStyle: {
            normal: {
              width: 0
            }
          },
          effect: {
            constantSpeed: 40,
            show: true,
            trailLength: 0.3,
            symbolSize: 3.5
          },
          zlevel: 1
        }
    },
    mounted () {
    },
    methods: {
        // 异步数据处理
        handleSubmit () {
            this.$axios.post().then((data) => {
                draw(data);
            })
        },
        // 数据可视化函数
        draw (trajectory) {
        this.lines.coords = trajectory;
        this.lines_option.data = [this.lines];
        this.lines_state.data = [this.lines];
        let series = [this.lines_option, this.lines_state];
        this.option = {
          bmap: this.bmap,
          tooltip: {
            trigger: 'item'
          },
          series: series
        };;
      },
    },
    components: {
      EChartsMap
    }
  };
</script>
```

完整代码请查看：https://github.com/SUNYunZeng/AIforDriving/blob/master/src/views/Prediction.vue