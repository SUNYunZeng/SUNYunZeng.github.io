---
title: 'Vue + Express + MySQL驾驶行为分析全栈项目(五): Leftlet热力图实现'
comments: true
toc: true
date: 2019-12-14 17:13:58
categories: 教程
tags: Vue
---

# 背景

自己做了一点点的小尝试：基于前馈神经网络 LSTM 的个体出行目的地预测模型，基于个体历史出行数据，模型可以实现出行目的地的实时动态预测功能。

模型其实具有实际应用功能，为了对其应用场景进行探索，拟开发一个全栈的项目，在Web客户端实现用户出行的动态预测效果，同时能够提供数据可视分析等功能。

**[项目地址](https://github.com/SUNYunZeng/AIforDriving)**

**[可视化效果](http://geoai.sunyunzeng.cn/)**


# Leftlet底图模块构建

需要安装的依赖库与样式查看[Vue + Express + MySQL驾驶行为分析全栈项目(四): Leftlet组件与图层加载功能](http://sunyunzeng.cn/Vue-Express-MySQL%E9%A9%BE%E9%A9%B6%E8%A1%8C%E4%B8%BA%E5%88%86%E6%9E%90%E5%85%A8%E6%A0%88%E9%A1%B9%E7%9B%AE-%E5%9B%9B-Leftlet%E7%BB%84%E4%BB%B6%E4%B8%8E%E5%9B%BE%E5%B1%82%E5%8A%A0%E8%BD%BD%E5%8A%9F%E8%83%BD/)。

在 src/components/common 文件夹下添加 LeftletMap.vue文件，作为Leftlet地图的基础模块。

```javascript
// LeftletMap.vue

<template>
  <div class="map">
  </div>
</template>

<script>

  export default {
    name: 'LeftletMap',
    data: function () {
      return {
        L: null,
        map: null,
        map_config: this.store.state.map_config
      };
    },
    methods: {
      initMap () {
        this.L = L;
        let map = L.map(this.$el, {
          center: this.map_config.center,
          zoom: this.map_config.zoom
        });
        this.map = map;
        let baseLayer = L.tileLayer('https://api.tiles.mapbox.com/v4/{id}/{z}/{x}/{y}.png?' +
          'access_token=pk.eyJ1IjoibWFwYm94IiwiYSI6ImNpejY4NXVycTA2emYycXBndHRqcmZ3N3gifQ.rJcFIG214AriISLbB6B5aw',
          {
            id: this.store.state.layerItems['box_d'],
            minZoom: 3
          });
        map.addLayer(baseLayer);
      }
    },
    mounted () {
      this.initMap();
    }
  };
</script>

<style scoped>
  .map {
    width: 100%;
    height: calc(88vh);
    position: relative;
  }
</style>
```

其中 this.state 是用Vuex插件定义的全局状态变量，其本身是 src/store/index.js文件中定义的一个对象，在其中我们定义了各种地图的配置选项。具体内容可查看[Vue + Express + MySQL驾驶行为分析全栈项目(四): Leftlet组件与图层加载功能](http://sunyunzeng.cn/Vue-Express-MySQL%E9%A9%BE%E9%A9%B6%E8%A1%8C%E4%B8%BA%E5%88%86%E6%9E%90%E5%85%A8%E6%A0%88%E9%A1%B9%E7%9B%AE-%E5%9B%9B-Leftlet%E7%BB%84%E4%BB%B6%E4%B8%8E%E5%9B%BE%E5%B1%82%E5%8A%A0%E8%BD%BD%E5%8A%9F%E8%83%BD/)。

# 热力图实现

首先安装leftlet热力图插件 leaflet-heatmap。

```javascript
npm i leaflet-heatmap --save
```

然后在src/views/ 文件夹下创建 ODpoint.vue 文件，该组件是对轨迹的O-D(Origin-Destination)点进行热力图分析。

```html
<!-- ODpoint.vue -->
<template>
  <div>
    <Form ref="formInline" inline>
      <FormItem>
        测试用户: <Select v-model="user" clearable style="width:70px">
        <Option value="user1">User1</Option>
        <Option value="user2">User2</Option>
        <Option value="user3">User3</Option>
        <Option value="user4">User4</Option>
      </Select>
      </FormItem>
      <FormItem><b>出发时间:</b></FormItem>
      <FormItem>
        <DatePicker v-model="time_range" :start-date="new Date('2018-01-01 00:00:00')" type="datetimerange"
                    style="width: 300px"></DatePicker>
      </FormItem>
      <FormItem>
        <CheckboxGroup v-model="ODSelectoin">
          <Checkbox label="origin"></Checkbox>
          <Checkbox label="destination"></Checkbox>
        </CheckboxGroup>
      </FormItem>
      <FormItem>
        <Button type="primary" @click="show">展示</Button>
      </FormItem>
    </Form>
    <Map style="height: calc(78vh);" ref="leftletMap"></Map>
  </div>
</template>
```
然后定义展示的逻辑js代码。一些问题可以参考文章：https://blog.frytea.com/archives/41/。

```javascript
  // 引入LeftletMap子模块
  import Map from '@/components/commom/LeftletMap';
  import HeatmapOverlay from 'heatmap.js/plugins/leaflet-heatmap';
  import {post, get} from '@/utils/myAjax';
  import {points_factory} from  '@/utils/traj-handler'

  export default {
    name: 'od',
    data () {
      return {
        user: 'user1',
        time_range: ['2018-01-01 00:00:00', '2018-01-31:00:00:00'],
        ODSelectoin: ['origin'],
        heatmapLayer: null,
        cfg: {
          'radius': 0.005,
          'maxOpacity': 0.8,
          'scaleRadius': true,
          'useLocalExtrema': true,
          latField: 'lat',
          lngField: 'lng',
          valueField: 'eff'
        }
      };
    },
    mounted () {
      this.initHeatMap();
    },
    components: {
      Map
    },
    methods: {
      show () {
        if (this.ODSelectoin.length === 0 || this.user === undefined) {
          this.$Message.info('请配置参数');
          return null;
        }
        if (this.$isOnServer) {
          post('searchByRow', {
            rowName: ['origin', 'destination', 'norm_dict', 'o_eff', 'd_eff'],
            time: this.time_range,
            tableName: this.user
          }).then(data => {
            if (data.length > 0) {
              let res = points_factory(data, this.ODSelectoin);
              this.heatmapLayer.setData(res.data);
              this.$refs.leftletMap.map.setView(res.center, 10);
            } else {
              this.$Message.info('空数据');
            }
          });
        } else {
          get('../static/data/' + this.user + '.json').then(data => {
            if (data.RECORDS.length > 0) {
              let basket = [];
              let record = data.RECORDS;
              for (let item of record) {
                let tmp_date = new Date(item['time']);
                if (tmp_date >= this.time_range[0] && tmp_date <= this.time_range[1]) {
                  basket.push(item);
                }
              }
              let res = points_factory(basket, this.ODSelectoin);
              this.heatmapLayer.setData(res.data);
              this.$refs.leftletMap.map.setView(res.center, 10);
            } else {
              this.$Message.info('空数据');
            }
          });
        }
      },
      initHeatMap () {
        this.heatmapLayer = new HeatmapOverlay(this.cfg);
        this.heatmapLayer.addTo(this.$refs.leftletMap.map);
        L.control.scale({ maxWidth: 200, metric: true, imperial: false }).addTo(this.$refs.leftletMap.map);
      }
    }
  }
  ;
```

最后，预览效果：http://geoai.sunyunzeng.cn/#/od