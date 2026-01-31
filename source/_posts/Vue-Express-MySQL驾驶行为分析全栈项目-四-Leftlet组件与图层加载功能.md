---
title: 'Vue + Express + MySQL驾驶行为分析全栈项目(四): Leftlet组件与图层加载功能'
comments: true
toc: true
date: 2019-12-14 16:45:07
categories: 教程
tags: Vue
---

# 背景

自己做了一点点的小尝试：基于前馈神经网络 LSTM 的个体出行目的地预测模型，基于个体历史出行数据，模型可以实现出行目的地的实时动态预测功能。

模型其实具有实际应用功能，为了对其应用场景进行探索，拟开发一个全栈的项目，在Web客户端实现用户出行的动态预测效果，同时能够提供数据可视分析等功能。

**[项目地址](https://github.com/SUNYunZeng/AIforDriving)**

**[可视化效果](http://geoai.sunyunzeng.cn/)**

# 准备工作

1. 首先下载安装 leftlet。

```javascript
npm install leaflet --save
```

2. 然后在全局入口 main.js 文件中引入leftlet，包括样式文件

```javascript
// 引入插件与样式
import * as L from 'leaflet';
import 'leaflet/dist/leaflet.css';

// 全局注册
Vue.L = Vue.prototype.$L = L;
```

3. 安装leftlet地图图层的加载插件-- leaflet.chinatmsproviders

```javascript
npm i leaflet.chinatmsproviders --save
```

```javascript
import * as Provider from 'leaflet.chinatmsproviders';

Vue.Provider = Vue.prototype.$Provider = Provider;
```

然后引入

```javascript
npm i leaflet.chinatmsproviders
```

5. 再安装iView插件。

```javascript
npm install iview --save
```

然后在全局入口 main.js 文件中引入iview插件及样式

```javascript
import iView from 'iview';

Vue.use(iView);
```

6. main.js文件

```javascript
// The Vue build version to load with the `import` command
// (runtime-only or standalone) has been set in webpack.base.conf with an alias.
import Vue from 'vue';
import App from '@/App';
import router from '@/router';
import iView from 'iview';
import store from '@/store';
import 'leaflet/dist/leaflet.css';
import 'iview/dist/styles/iview.css'
import '@/permission';
import * as L from 'leaflet';
import * as Provider from 'leaflet.chinatmsproviders';


// 全局注册
Vue.use(iView);
Vue.L = Vue.prototype.$L = L;
Vue.Provider = Vue.prototype.$Provider = Provider;

/* eslint-disable no-new */
new Vue({
  el: '#app',
  router,
  store,
  render: h => h(App) // 相当于 components: { App }  vue1.0的写法
});
```
# 界面展示逻辑

然后定义我们的页面展示逻辑。

首先在 src/views/ 文件夹下创建 Home.vue 文件。然后定义界面组件，包括返回home按钮与下拉图层选择选项。

```html
<!-- Home.vue -->
<template>
  <div>
    <div id="map"></div>
    <Button  id="map_home" class="rightmenu" @click="returnHome"  icon="ios-home" type="primary" shape="circle" title="home"></Button>
    <Dropdown class="rightmenu" style="margin-left: 20px" placement="bottom-end">
      <Button type="primary">
        {{msg}}
        <Icon type="ios-arrow-down"></Icon>
      </Button>
      <DropdownMenu slot="list">
        <DropdownItem @click.native="mapSet('gd')">高德地图</DropdownItem>
        <DropdownItem @click.native="mapSet('glg')">谷歌地图</DropdownItem>
        <DropdownItem @click.native="mapSet('box')">MapBox</DropdownItem>
        <DropdownItem @click.native="mapSet('osm')">OSM地图</DropdownItem>
        <DropdownItem @click.native="mapSet('geo')">智图</DropdownItem>
      </DropdownMenu>
    </Dropdown>
  </div>
</template>

<style scoped>
  #map {
    width: 100%;
    height: calc(88vh);
    position: relative;
  }
  .rightmenu{
    position: absolute;
    top: 30px;
    right: 70px;
    z-index: 1000;
  }
  #map_home{
    right: 30px;
  }
</style>
```

# 功能实现逻辑

```javascript
// Home.vue 

<script>
  import store from '@/store';
  export default {
    name: 'home',
    data () {
      return {
        map: null,
        L: null,
        baseLayer: null,
        msg: '高德地图',
        map_config: {
          zoom: 15,
          center: [30.541093, 114.360734],
          minZoom: 2,
          maxZoom: 18
          // key: "eb35dddb3aa33c7a8fb4218b39d1e424"  天地图key
        }
      };
    },
    mounted () {
      this.initMap();
    },
    methods: {
      initMap () {
        this.L = L;
        let map = L.map("map", {
          center: this.map_config.center,
          zoom: this.map_config.zoom
        });
        this.map = map;
        this.addLayer(L, map, store.state.layerItems['gd_n_a'],18, 3);
      },
      addLayer (L, map, layer_name, maxZoom, minZoom) {
        if (this.baseLayer !== null) {
          map.removeLayer(this.baseLayer);
        }
        this.baseLayer = L.tileLayer.chinaProvider(layer_name, {
          maxZoom: maxZoom === undefined ? this.map_config.maxZoom : maxZoom,
          minZoom: minZoom === undefined ? this.map_config.minZoom : minZoom
        });
        map.addLayer(this.baseLayer);
      },
      returnHome () {
        this.map.setView(this.map_config.center, 15);
      },
      mapSet (map_name, L = this.L, map = this.map) {
        switch (map_name) {
          case 'gd':
            this.addLayer(L, map, store.state.layerItems['gd_n_a'], 18, 3);
            this.msg = '高德地图 ';
            break;
          case 'glg':
            this.addLayer(L, map, store.state.layerItems['glg_n']);
            this.msg = '谷歌地图 ';
            break;
          case 'geo':
            this.addLayer(L, map, store.state.layerItems['geo_n_g'], 16);
            this.msg = '   智图   ';
            break;
          case 'osm':
            this.addLayer(L, map, store.state.layerItems['osm']);
            this.msg = 'OSM地图';
            break;
          case 'box':
            if (this.baseLayer !== null) {
              map.removeLayer(this.baseLayer);
            }
            this.baseLayer = L.tileLayer('https://api.tiles.mapbox.com/v4/{id}/{z}/{x}/{y}.png?' +
              'access_token=pk.eyJ1IjoibWFwYm94IiwiYSI6ImNpejY4NXVycTA2emYycXBndHRqcmZ3N3gifQ.rJcFIG214AriISLbB6B5aw',
              {
                id: store.state.layerItems['box'],
                minZoom: 3
              });
            map.addLayer(this.baseLayer);
            this.msg = 'Mapbox';
            break;
          default:
            break;
        }
      },
    },
  }
</script>
```

其中 this.state 是用Vuex插件定义的全局状态变量，其本身是 src/store/index.js文件中定义的一个对象，在其中我们定义了各种地图的配置选项。

```javascript
// src/store/index.js
import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(Vuex);

const store = new Vuex.Store({
  state: {
    // 底图数据
    layerItems: {
      'tdt_n': 'TianDiTu.Normal.Map',
      'tdt_n_a': 'TianDiTu.Normal.Annotion',
      'tdt_s': 'TianDiTu.Satellite.Map',
      'tdt_s_a': 'Satellite.Annotion',
      'gd_n_a': 'GaoDe.Normal.Map',
      'gd_s': 'GaoDe.Satellite.Map',
      'gd_s_a': 'GaoDe.Satellite.Annotion',
      'glg_n': 'Google.Normal.Map',
      'glg_s': 'Google.Satellite.Map',
      'geo_n': 'Geoq.Normal.Map',
      'geo_n_p': 'Geoq.Normal.PurplishBlue',
      'geo_n_g': 'Geoq.Normal.Gray',
      'geo_n_w': 'Geoq.Normal.Warm',
      'geo_n_h': 'Geoq.Normal.Hydro',
      'osm': 'OSM.Normal.Map',
      'box_d': 'mapbox.dark',
      'box_l': 'mapbox.light',
      'box': 'mapbox.streets'
    },
    //地图配置
    mapconfig: {
      zoom: 15,
      center: [30.541093, 114.360734],
      minZoom: 2,
      maxZoom: 18
    }
  },
  mutations: {
    setMap(state, key, value) {
      state.mapconfig[key] = value;
    }
  }
});

export default store;
```

然后基于Leftlet的图层切换与返回指定位置功能就实现啦，预览：http://geoai.sunyunzeng.cn/#/home
